---
title: "Next Word Wizard: LSTM Based Next Word Predictor"
excerpt: "Conducted an Ablation Study to optimize hyperparameters for an LSTM Model trained on a corpus of News Articles for predicting the next word on a given input sequence of words"
collection: portfolio
---


## GitHub Repository:
[Repository](https://github.com/sameerprasadkoppolu/Next-Word-Wizard-Group-18-NLP-Project-?tab=readme-ov-file)


## Introduction:
Next Word Prediction is an indispensable application of Natural Language Processing (NLP) that enables machines to predict the most likely word to follow a given sequence of words. This model utilizes both statistical and neural network techniques to analyze vast amounts of text data to identify patterns and relationships between words, enabling machines to make accurate predictions. With numerous practical applications such as auto-completion of text, machine translation, and content creation, Next Word Prediction has become an essential tool for people who are looking to improve their writing skills and communication capabilities.


## Methodology:
Four different next-word prediction models/strategies were utilized for this ablation study. These included - LSTM, Bi-LSTM, Transformers, and Markov Model. LSTM and Bi-LSTM models are recurrent neural networks that can capture long-term dependencies in the input sequence, while Transformers use self-attention mechanisms to process the entire sequence of words at once. The Markov Model is a probabilistic model that assumes that the probability of a word depends only on the preceding n-words. By evaluating and comparing the performances of these models, we can gain insights into their strengths and weaknesses and determine their suitability for different use cases.  

The flowchart below showcases the methodology of the project.  
  
![image](https://github.com/sameerprasadkoppolu/portfolio/assets/40263744/fd84d74f-f137-45fe-aa2a-960d06fd701b)


## Dataset and Preprocessing:
The dataset New York Times comments obtained from Kaggle. was used to train and test the LSTM and BiLSTM models, as well as the Markov model. The dataset consists of comments on articles from The New York Times published in April 2017. The dataset contains a total of 1,411,956 comments. 
* The entire set of articles was used for the Markov model.
* The LSTM and BiLSTM models were trained and tested on a portion of the dataset (specifically the News Articles from April 2017) due to processor limitations.
* For the Transformer models, the same dataset was used for training and the Yelp dataset from the Kaggle2. was used for testing. The Yelp dataset contains a total of 5,261,668 reviews. The Transformer models used in this project include DistilBert, AlBert, Bert, and RoBerta.  

In our data preprocessing phase, we cleaned the text data by converting it to lowercase and splitting it into individual lines. The Keras Tokenizer function was then used to create a tokenizer object that converted the text data into sequences of integers. N-gram sequences were generated from the in- teger sequences using a nested loop, and these sequences were padded and split into input data and labels to train the next word prediction model. We used pre-trained GloVe embeddings, and a dictionary called ’embeddings index’ was created to store the word and its embedding coefficients as key-value pairs. We then created a numpy matrix called ’embeddings matrix’ with the same number of rows as the total number of unique words in the input data and the same number of columns as the embedding dimension. Finally, the embeddings matrix was populated with the GloVe embeddings for the words in the input data using the embeddings index dictionary to look up the embedding coefficients for each word.  


## Modelling:
* **Markov Chain Model:** We implemented the Markov Model to generate new text based on a given corpus by analysing sequences of words. This model is based on modelling the transition between states in a stochastic process. We created methods for suggesting the next word in a sentence based on the number of preceding words given as input. If a word or sentence outside the corpus is encountered, the Markov chain model will not have a probability value assigned to it.
* **LSTM:** For the LSTM model, we specified a hidden layer with 128 units, a dropout rate of 0.5, and a learning rate of 0.01. We then trained the model on the input data for 50 epochs, with a batch size of 256 and a verbosity level of 1, meaning that progress updates were displayed during training. The hidden layer units determine the number of memory cells in the LSTM, which can help the model capture long-term dependencies in the input sequence. The dropout rate is a regularization technique that randomly drops out connections between neurons during training to prevent overfitting. The learning rate determines the step size taken in the direction of the gradient during training. By adjusting these hyperparameters and evaluating the resulting model performance, we can tune the LSTM to achieve optimal re- sults. LSTM can suffer from vanishing gradients or exploding gradients during training.
* **BiLSTM:** For the BiLSTM model, we used similar hyperparameters to the LSTM model, including a hid- den layer with 128 units, a dropout rate of 0.5, and a learning rate of 0.01. We trained the model on the input data for 50 epochs, with a verbosity level of 1. The key difference between the BiLSTM and the LSTM is that the BiLSTM processes the input sequence in both directions, allowing the model to capture not only previous but also future context. This can be particularly useful in next word prediction tasks, where the context surrounding a given word is crucial for accurate predictions. By adjusting the hyperparameters and comparing the performance of the BiLSTM to other models, we can evaluate its effectiveness for the given task. However, BiLSTM is computationally expensive, to address the shortcoming we have used Transformers.
* **Transformers:** Finally, we utilized attention mechanisms to process sequential data and employed different transformer models such as DistilBert, AlBert, Bert, and RoBerta. These models were trained on a dataset of 10,000 news articles over 10 epochs and evaluated on an external test dataset of 5,000 Yelp reviews. The evaluation process was done to measure the accuracy and effectiveness of the transformer models in predicting the next word in a sequence. By using these transformer models, we aimed to improve the performance of our next word prediction model and achieve better results in various NLP applications such as content creation and machine translation.


## Results:
Our evaluation metrics were perplexity and mean test score. The figure below shows the ablation study on LSTM and BiLSTM Models. The LSTM model had a hidden layer with 128 units, a dropout rate of 0.5, and a learning rate of 0.01, and it achieved a perplexity of 6.86 and a mean test score of -8.836. The BiLSTM model also had a hidden layer with 128 units, a dropout rate of 0.5, and a learning rate of 0.01, and it achieved a perplexity of 15.29 and a mean test score of -9.635. The transformer models that we used in this study were DistilBert, AlBert, Bert, and RoBerta. DistilBert achieved a perplexity of 30.14 and a loss of 3.4, AlBert achieved a perplexity of 15.78 and a loss of 2.75, Bert achieved a perplexity of 19.86 and a loss of 2.98, and RoBerta achieved the best results with a perplexity of 4.24 and a loss of 1.44.  
  
![image](https://github.com/sameerprasadkoppolu/portfolio/assets/40263744/d74acdc2-0153-4790-8fd0-4339e519e9eb)  
  
The results shown above indicate that the LSTM model outperformed the BiLSTM model in terms of perplexity and mean test score. However, the transformer models performed better than both LSTM and BiLSTM models, with RoBerta achieving the best results in terms of perplexity and loss. These findings suggest that transformer models are more effective than traditional recurrent neural network models in predicting the next word in a sequence of text.
