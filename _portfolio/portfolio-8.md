---
title: "VisaWise - FAQ Chatbot for USCIS Policies and Procedures"
excerpt: "Augmented the Instruction tuned Llama 3 8B LLM to develop a Retrieval Augmented Generation System (RAG) based Chatbot that answers F-1 Visa related queries by utilizing USCIS policy and procedure information, thereby achieving a 24% improvement in factual accuracy as compared to using only the LLM by itself."
collection: portfolio
---


## Links:
[Repository](https://github.com/sameerprasadkoppolu/VisaWise_DS5983)  

## Done in Collaboration With:  
* [Divya Hegde](https://www.linkedin.com/in/hegdedivya/)
* [Raghav Gali](https://www.linkedin.com/in/raghavgali/)

## Introduction:
This project addresses the challenge faced by international students on F-1/J-1 visas in the US, who often struggle with understanding the complex rules and procedures set forth by the United Stats Citizenship and Immigration Services (USCIS). To alleviate this issue, we developed a chatbot using Retrieval-Augmented Generation (RAG). The chatbot provides accurate and accessible information, simplifying the understanding of visa regulations. The results show improved user comprehension and reduced anxiety, enhancing the overall experience for international students navigating the US visa system. Furthermore, the RAG based chatbot is able to answer questions on topics such as Green Card, H-1B, Request for Evidence (RFE), and the O Visa.

## Data Sources and Processing:
* **Data Sources:**
  * **USCIS Policy Manual:** The comprehensive manual was downloaded from the official USCIS website, containing detailed information on immigration policies, including guidelines for F-1 and J-1 visas.
  * **CFR Volume 8:** Relevant sections from the Code of Federal Regulations, specifically Volume 8, were collected. This volume provides the legal framework governing visa statuses and procedures.
  * **Title 8 of Immigration and Nationality Act (INA):** This is collection of laws in the United States that pertains to “Aliens and Nationality.”
    
* **Data Format and Organization:**
  * **Format:** All data were converted into PDF format for consistency.
  * **Directory Structure:** The documents were organized into a single directory to streamline management and retrieval processes.

* **Data Parsing:**
  * **Tool:** We use Llamaindex's SimpleDirectoryReader to parse and extract the text from the PDFs


## Architecture:
* **Vector Only RAG:**  
  * Uses the standard RAG architecture implemented with vector based retrieval. An embedding model converts a chunk of tokens into a vector of fixed length. The vectorized tokens are stored in a vector database. When a query is entered by the user, the embedding model retrieves the `top_k` most similar vectors from the vector database, reranks them and takes only the `top_n` most similar out of the retrieved vectors, and puts them along with the query vector as context into a prompt that goes the LLM. The LLM then utilizes the context to generate an answer.
  * Embedding Models used - [`BAAI/bge-base-en-v1.5`](https://huggingface.co/BAAI/bge-base-en-v1.5) and [`Snowflake/snowflake-arctic-embed-m-v1.5`](https://huggingface.co/Snowflake/snowflake-arctic-embed-m-v1.5)
  * Embedding Dimension - 768
  * Chunk Sizes used - 1024, 2048
  * Chunk Overlaps used - 50, 100, 200
  * `top_k` - 10, 20
  * Reranker - [`BAAI/bge-reranker-base`](https://huggingface.co/BAAI/bge-reranker-base)
  * `top_n` - 3, 5
  * Best Combination of Hyperparameters - `Snowflake/snowflake-arctic-embed-m-v1.5` embedding model, Chunk Size = 1024, Chunk Overlap = 100, `top_k` = 10, `top_n` = 3
    
* **Hybrid/Fusion RAG:**  
  * Uses the standard Vector based retrieval as described above. Additionally also uses the BM25 Keyword based retriever. Thereafter the retreived vectors are reranked using Mean Reciprocal Reranking, fed along with query vector into prompt, and an output is generated
  * Embedding Models used - [`BAAI/bge-base-en-v1.5`](https://huggingface.co/BAAI/bge-base-en-v1.5) and [`Snowflake/snowflake-arctic-embed-m-v1.5`](https://huggingface.co/Snowflake/snowflake-arctic-embed-m-v1.5)
  * Embedding Dimension - 768
  * Chunk Size used for Vector Retrieval - 1024
  * Chunk Overlap used for Vector Retrieval - 50
  * Chunk Size used for Keyword Retrieval - 1024
  * Chunk Overlap used for Keyword Retrieval - 200
  * `top_k` for Vector Retriever - 8, 10
  * `top_k` for BM25 Retriever - 2, 8
  * Reranker - Mean Recirprocal Reranking
  * `top_n` - 8, 10
  * Best Combination of Hyperparameters - `BAAI/bge-base-en-v1.5` embedding model, Vector Retrieval Chunk Size = 1024, Vector Retrieval Chunk Overlap = 50, BM25 Keyword Retreival Chunk Size = 1024, BM25 Keyword Retrieval Chunk Overlap = 200, `top_k` for Vector Retrieval = 10, `top_k` for BM25 Keyword Retrieval = 2, Final `top_k` = 10, and Reranking = Mean Reciprocal Reranking


## Testing and Evaluation:  
* **Test Set Creation:**  
  * Prepared a set of 100 questions with ground truth answers related to five categories - F-1, Green Card, H-1B, O Visa, and RFE.
  * The same questions were answered by the LLM alone and the RAG system to compare performance.
  * Also prepared a set of context questions to evaluate compare responses of LLM, Vector RAG, and Hybrid RAG.
  * Synthetically prepared questions using an LLM to quantitatively evaluate Vector RAG and Hybrid RAG.

* **Qualitative Evaluation Criteria:**
  * Accuracy: How correctly the chatbot answered the queries.
  * Relevance: The degree to which the responses were relevant to the user queries.
  * Completeness: Whether or not the responses cover all of the necessary aspects asked in the query.
  * Clarity: Ease of understanding responses.
  * Conciseness: Whether or not the responses contain unnecessary information

* **Quantitative Evaluation Critera:**  
  * Faithfulness: Measures the amount of factual consistency between the generated answer and the context.
  * Answer Relevancy: The degree to which the responses were relevant to the user queries.
  * Context Precision: Provides insight on how much of the ground truth is covered by highly ranked retrieved vectors.
  * Context Recall: Quantifies the degree to which the retrieved context aligns with the ground truth answer.
  * Harmfulness: Use a critique LLM to evaluate whether responses are harmful. Decisions are made based on a majority vote of responses from successive LLM calls.

* **Qualitative Evaluation Results:**
  * For the 100 questions containing ground truth answers, qualitative evaluation using GPT-4o-mini indicated that Vector only RAG gave the best responses and achieved an average score of 7.77 on 10 as shown below.  
    ![image](https://github.com/user-attachments/assets/c79ad855-467a-4320-89fe-dd73339bf4b3)
    
* **Quantitative Evaluation Results:**  
  * Coming to Quantitative Analysis, the Hybrid RAG outperformed the Vector RAG across most of the RAGAS metrics on the synthetically generated questions.
    ![image](https://github.com/user-attachments/assets/643224c3-81ca-44d2-9fd0-0456268fae54)

  * Human Evaluation done on the Context question showed strong indication of higher performance by Hybrid RAG. However, there were a few instances in which the LLM was the best performer out of the 3, and a few instances where the Vector only RAG was the best performer out of the 3.
    * *Example Question where LLM performs best*:
      ![image](https://github.com/user-attachments/assets/679c1c2c-9055-4f90-9588-c2f28cf8d6e2)
      ![image](https://github.com/user-attachments/assets/90d353a0-bef7-499e-9015-2419c8d4f1d3)
      ![image](https://github.com/user-attachments/assets/5b6d7210-17b7-4d48-8bad-0ec0fca9e202)  
      **Conclusion:** *LLM is doing a much better job here as the documents have little to or no information on purchasing a house on F1 visa.*

    * *Example Question where Vector Only RAG performs best*:
      ![image](https://github.com/user-attachments/assets/324aa77c-b1a1-4aa5-bd3e-80506705c2cf)
      ![image](https://github.com/user-attachments/assets/b7357e86-b215-47d0-afd6-d29aae1c0ba8)
      ![image](https://github.com/user-attachments/assets/f34138b2-9f7d-4357-b799-b9deaa7d4cf9)
      ![image](https://github.com/user-attachments/assets/9c5cee71-6b9b-4365-b33c-8cc5c5702eb1)  
      **Conclusion:** *While both Vector based and Hybrid provide detailed results, the Vector based provides more pertinent steps in terms of a timeline thereby focusing on the ‘10 year plan’ of getting a Visa*


## Tools:
* **Framework**: Llama Index
* **Vector Database**: Pinecone DB
* **LLM for Response Synthesis**: [`llama3:instruct`](https://ollama.com/library/llama3:instruct) with 8B parameters
* **Qualitative Evaluation done using**: [`GPT-4o-mini`](https://openai.com/index/gpt-4o-mini-advancing-cost-efficient-intelligence/)
* **Quantitative Evaluation Framework**: [`RAGAS`](https://docs.ragas.io/en/stable/index.html)
* **Critic LLM for Quantitative Evaluation**: [`mistral-nemo`](https://ollama.com/library/mistral-nemo)
* **Generator LLM for Quantitative Evaluation**: [`llama3:instruct`](https://ollama.com/library/llama3:instruct) with 8B parameters
* **Evaluator LLM for Quantitative Evaluation**: [`llama3.1`](https://ollama.com/library/llama3.1) with 8B parameters
