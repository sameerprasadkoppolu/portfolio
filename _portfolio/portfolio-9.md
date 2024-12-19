---
title: "SemantiScout – Product Search by Capturing Visual Semantics "
excerpt: "Configured a multimodal product search engine using CLIP embeddings, GCP Vertex AI, and Pinecone to integrate text and image data of ~1 M Amazon Products, enabling faster and highly relevant product recommendations for user queries focused on describing product features or appearances."
collection: portfolio
---


## Links:
[Repository](https://github.com/sameerprasadkoppolu/SemantiScout/tree/main)

## Done In Collaboration With:
* [Aswath Sundar Sundaram Ramasubramanian](https://www.linkedin.com/in/aswathsundar1709/)
* [Divya Hegde](https://www.linkedin.com/in/hegdedivya/)
* [Raghav Gali](https://www.linkedin.com/in/raghavgali/)

## Introduction:
The goal of this project is to develop a robust product search engine that leverages multimodal data (text and images) to enhance the understanding of user preferences and deliver highly relevant and personalized product recommendations. Traditional search systems often fall short in capturing the combined semantic impact of visual and textual features, which can lead to suboptimal search results. Our solution addresses these limitations by integrating and processing multimodal data for more accurate and context-aware recommendations.

## Data Sources and Processing:
* **Data Sources:**
  * [Amazon Product Reviews Dataset](https://huggingface.co/datasets/McAuley-Lab/Amazon-Reviews-2023)  
    * A large-scale collection of 575 million reviews and ~50 million products across diverse product categories, offering rich insights for sentiment analysis, recommendation systems, and customer behavior modelling.
    * Composed of Product Information that includes product titles, features, and images. Products are organized into categories and individual product categories can also be retrieved.
    * Also includes User Reviews for products - but this is not being used for this project.
    
* **Data Availability and Format:**
  * Data is available on HuggingFace (using the link above) for 35 product categories in the JSONL format for ~50 M products.

* **Data Parsing:**
  * **Tool:** We use the `datasets` library from HugginFace to access the dataset.
 

## Methodology:  
At an overall level, the workflow of the product search engine is as follows:  
![image](https://github.com/user-attachments/assets/265efa35-96ff-4c78-ae26-324e39f01d39)  

* **Data Ingestion and Preprocessing:**
  * The project uses a subset of the McAuley Amazon dataset, which contains product metadata and user reviews across 35 categories.
  * For this project, we focus on three categories (pet supplies, office products, health and household) and utilize only the product metadata, which includes titles, images, descriptions, and other product-related features. The selected subset provides a rich dataset for understanding the interplay of textual and visual features in product searches.
  * To ensure data relevance, products without images, those with very short titles (two words or fewer), and those with mismatched titles and images were removed. CLIP was used to identify mismatches by comparing text and image embeddings, with products scoring below a cosine similarity of 0.2 excluded.

* **Embedding Generation with [CLIP](https://huggingface.co/openai/clip-vit-base-patch16):**  
  * The dataset, originally present as a JSONL file on HuggingFace was loaded using the ‘datasets’ Python Library. The products in each of the three chosen categories - Health and Household, Office Products, and Pet Supplies were loaded separately. The loaded data was pre-processed and filtered as mentioned above.
  * For each product category, CLIP was used to generate text and image embeddings using the product’s title and the first large resolution image of the product respectively. This processing step was compute intensive - especially due to the fact that CLIP had to process pixel values for the images on the CPU. The processing of the text involved tokenization using Byte Pair Encoding (BPE) while the processing of the images involved obtaining pixel values of the images. This tokenization of text and obtaining pixel values of images was done across multiple CPU cores in parallell with the help of [DASK](https://docs.dask.org/en/stable/)
  * The tokenized text and pixel values were passed to the CLIP model to generate embeddings. We used Google Cloud Platform's (GCP) Vertex AI along with DASK to speed up the process of generating embeddings using an NVIDIA L4 GPU, 16 vCPUs, and 64GB RAM on a Vertex AI instance which handled the intense computing needs of embedding generation. This setup on GCP helped reduce runtime and computational load while keeping things scalable and efficient for our large dataset.

* **Vector Storage and Similarity Score:**  
  * The obtained text and image embeddings for each product were combined using different techniques - addition and using the image only embeddings.  
  * These embeddings are stored in [Pinecone](https://www.pinecone.io/), a vector database, with two separate vector databases created for each category: one containing only image embeddings and the other a linear combination of text and image embeddings.

* **Zero Shot Classification of User Query:**  
  * For query processing, a user query undergoes zero-shot classification to identify its category, effectively narrowing the search space and minimizing latency.
  * This method relies on pre-trained language models like BERT, DeBERTa, or bart-large, which understand the semantic relationships between words and sentences. The models predict the relevance of a user query to a given set of labels by scoring the query-label alignment based on their contextual embeddings.
  * Titles and descriptions of 50 products from each of the three categories (Health and Household, Pet Supplies, and Office Products) were extracted from the Amazon Product Review dataset. These product attributes were used as input to ChatGPT to generate diverse queries, including straightforward queries, overlapping product queries, queries for non-existent products, and ambiguous queries.
  * This allowed for a more comprehensive evaluation of zero-shot classification models. Four models - `bart-large`, `MoritzLaurer/mDeBERTa-v3`, `typeform/distilbert`, and `Cross-Encoder/DeBERTa` were tested on these generated queries, and their performance was assessed using the Top-1 Evaluation metric.
  * The results of the performance of each model on the queries are provided below.
    ![image](https://github.com/user-attachments/assets/040a1328-eb66-462a-b985-028af19fec99)

* **Inclusion of Text Embeddings using Zero-Shot Classification of Words in User Query:**  
  * Once the user query's category is identified, the top-10 most similar vectors to the user query are retrieved from the vector databases containing image-only embeddings and text+image embeedings pertraining to the relavant product category.
  * For example consider the user query **"red travel-sized electric toothbrush kit"**. This query exemplifies CLIP's effectiveness in integrating contextual understanding when processing image features. The initial results accurately identify travel-sized electric toothbrushes but lack sensitivity to the color context, specifically the red hue present in the images. In contrast, embeddings based solely on image features capture the color context effectively, avoiding any interference from irrelevant textual information.
    ![image](https://github.com/user-attachments/assets/e230420a-d0d0-463c-ab36-1406c26782f2)
  * Now consider the example user query **"Pet collar with hearts"**. Image only embeddings outperforms in capturing shape-related visual cues. CLIP's ability to understand nuances in shape helps identify subtle variations, such as differentiating "square" from "rounded square" products. This is especially useful for applications like furniture or fashion, where small changes in shape can significantly impact style or functionality. This is shown below.
    ![image](https://github.com/user-attachments/assets/8e5284f6-6915-4e23-8102-48ff8f5f04bb)
  * Now consider the user query **"anti-UV sunglasses with reader lens for women"**. Here image-only embeddings has challenges with abstract concepts. Functional attributes often depend on sensory qualities (like smell or taste) or protective features (like UV-blocking properties) that are not visually observable. CLIP is unable to directly encode these qualities because they are not embedded in the image itself and are often only communicated through text descriptions.
    ![image](https://github.com/user-attachments/assets/97da966a-7113-4ee9-8f53-a68a69a764d3)
  * It is observed that when the user query contained visualizable words or phrases like objects, colors, shapes, and animals, the image only embeddings showed much better results in comparison to the combination of text and image embeddings. However, when the user query contained words or phrases that could not be visualized or were difficult to visualize, the combination of text and image embeddings generally provided better results. These non-visualizable or hard to visualize words or phrases were typically feelings, adjectives, or product qualities. Therefore, it became imperative to implement a strategy that would prioritize the combination of text and image embeddings versus using the image embeddings alone.
  * This prioritization was achieved through zero-shot classification of each word in the user query into 2 classes - ‘feeling or adjective or quality’ or ‘animal or color or shape or object’. Once each word was classified, the majority class of the entire user query was determined by identifying which class label occurred the most.
    * If the most frequently occurring label was ‘animal or color or shape or object’, only the image embeddings are used and the top 10 recommendations are displayed based on the cosine similarity between the user query embedding the image embedding of each product.
    * If the most frequently occurring label was ‘feeling or adjective or quality’ then we re-rank the recommendations provided by the combination of text and image embeddings and image only embeddings - we define a weight for each recommendation from the combination of text and image embeddings and for each recommendation from the image only embeddings. Thereafter, the top 10 recommendations are displayed to the user. The weighting strategy is as follows
      * Text + Image embeddings: Weight = 0.25 × cosine similarity between user query embedding and combined embeddings.
      * Image-only embeddings: Weight = 0.75 × cosine similarity between user query embedding and image only embeddings.
  * Zero-Shot Classification here was achieved using the [`Knowledgegator Comprehend-It-Base`](https://huggingface.co/knowledgator/comprehend_it-base) model. This model is a fine tuned version of the `DeBERTaV3-base` model. Comprehend-It-Base has been fine tuned extensively on several Natural Language Inference (NLI) and Text Classification datasets which allow it to be used in a zero-shot setting for text classification with custom class labels.
  * Example results of this prioritization method are given below
    * Consider the example query **"Pet Collar With Hearts"**, the results for zero-shot classification of each word in this query after stop words are removed is shown below.
      ![image](https://github.com/user-attachments/assets/f9bee7f8-082e-4b7f-9b6d-e82d0a8b3938)
      It is observed that the most frequently occurring class is ‘animal or color or shape or object’ and there only the image embeddings are used to retrieve products. The recommendations for this query are displayed below.
      ![image](https://github.com/user-attachments/assets/ac29a165-bd98-4d1c-9c63-042870b7b3e0)
    * Now consider the example query **"Anti-UV Sunglasses"**, the results for zero-shot classification of each word in this query after stop words are removed is shown below.
      ![image](https://github.com/user-attachments/assets/dc0accca-df07-4bd4-bd0f-0c5714ec151f)
       It is observed that the most frequently occurring class is ‘feeling or adjective or quality’ and therefore, the 20 product recomendations are re-ranked using the strategy mentioned above and only the top-10 after re-ranking are displayed.
      ![image](https://github.com/user-attachments/assets/0ed26fb5-b0f0-4b57-a03f-88ea18d870ed)


## References:  
* Amazon Reviews Dataset. Available at: [https://amazon-reviews-2023.github.io/](https://amazon-reviews-2023.github.io/)
* Ni, J., Li, J., & McAuley, J. (2021). Justifying Recommendations using Distantly-Labeled Reviews and Fine-Grained Aspects. arXiv preprint arXiv:2103.00020.
* Hou, Y., Li, J., He, Z., Yan, A., Chen, X., & McAuley, J. (2024). Bridging language and items for retrieval and recommendation. arXiv preprint arXiv:2403.03952.
* Patel, S. (2016). Sentiment Analysis on Amazon Product Reviews Using Recurrent Neural Networks. Stanford University. Available at: [https://cs224d.stanford.edu/reports/PatelShabaz.pdf](https://cs224d.stanford.edu/reports/PatelShabaz.pdf)
* Patel, S. (2023, March 28). Building a semantic book search – Part 2: Scaling the embedding pipeline with Apache Spark and AWS. Towards Data Science. Retrieved from [https://towardsdatascience.com/building-a-semantic-book-search-part-2-scaling-the-embedding-pipeline-with-apache-spark-and-aws-1d074ee9cb55](https://towardsdatascience.com/building-a-semantic-book-search-part-2-scaling-the-embedding-pipeline-with-apache-spark-and-aws-1d074ee9cb55)  
* Radford, A., Kim, J. W., Hallacy, C., Ramesh, A., Goh, G., Agarwal, S., Sastry, G., Askell, A., Mishkin, P., Clark, J., Krueger, G., & Sutskever, I. (2021). Learning Transferable Visual Models From Natural Language Supervision. arXiv preprint arXiv:2103.00020
* CLIP: Connecting text and images - [https://openai.com/index/clip/](https://openai.com/index/clip/)
 

