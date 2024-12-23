---
title: "Finetuning T5-Small For Dialogue/Chat Summarization"
excerpt: "Finetuned Google T5-Small Pretrained model for Dialogue Summarization using Samsung's SAMsum corpus of dialogues to resulting in an increase of 16 percentage points in the Test Sets' ROUGE-LSum score as compared to the Pre-Finetuned Model's Summaries."
collection: portfolio
---


## Links:
[Repository](https://github.com/sameerprasadkoppolu/T5-Finetuning-Chat-Summarization)  
[HuggingFace Model Card](https://huggingface.co/koppolusameer/t5-finetuned-summarization-samsum)

## Introduction:
[Google T5-Small](https://huggingface.co/google-t5/t5-small) is Sequence-2-Sequence model involving an encoder and decoder and pretrained with the objective of denoising a span corrupted text. 
This model has 66.5 million parameters and can be finetuned for a specific downstream task using input transformations and a task-specific dataset.
The task-specific dataset used here is Samsung's [SAMSum Corpus](https://huggingface.co/datasets/Samsung/samsum) which contains about 16k messenger-like conversations 
with summaries for each conversation/dialogue/chat.

## Data Preprocessing:
To preprocess the data, the 'AutoTokenizer' class from HuggingFace is used within the 'map' function to preprocess the dialogues and their summaries.
Additionally, dynamic padding is also achieved using the 'DataCollatorForSeq2Seq' class. The training set, evaluation set, and test set is 
the SAMsum corpus' training, validation, and test splits respectively that can be obtained when the data is loaded.  
  
It is important to note that every dialogue is prefixed with the string 'summarize: ' prior to tokenization which helps T5 in distinguishing between
tasks.

## Training:
Prior to training, the dialogues and their summaries were batched into batches of 8 inputs. For Training, HuggingFace's 'Trainer API' is used.
The trainer is instantiated several training arguments - the most important of which are shown below.  
  
![image](https://github.com/sameerprasadkoppolu/portfolio/assets/40263744/dbf9001b-5733-448d-b2dc-13e8348ba0aa)  
  
Another important training argument is the metrics function that is used along with the Cross Entropy Loss to evaluate the training process 
with respect to the actual summary of each dialogue. The metric used here are the ROUGE scores - ROUGE1, ROUGE2, ROUGE-L, ROUGE-LSum. These
metrics allow the model to evaluate how many of the facts in the actual summary of the dialogue are present in the model generated summary.  

Model parameters are updated after every epoch but only the parameters of the previous 3 epochs are saved. Finally, once training is done, the best
model is loaded.  

##  Pushing to Model Hub:
Once the model is finetuned via the Trainer API, it is pushed to the Model Hub on HuggingFace where it can be interacted with. 
Furthermore, the model's results on the evaluation set can also be viewed here. The tokenizer of this model is also saved to the same repository
so that whenever the model needs to be loaded from this checkpoint, the tokenizer can also be loaded from the same checkpoint.

## Test Set Evaluation:
The finetuned model is then used to generate summaries of the test set's dialogues and it's results are reported as follows.  

![image](https://github.com/sameerprasadkoppolu/portfolio/assets/40263744/f9f603c4-495a-4755-9103-88f384894cab)  

When compared with the pre-finetuned model's summaries, there is a noticeable difference in summary quality.
Here are the pre-finetuned model's summaries' ROUGE scores.  

![image](https://github.com/sameerprasadkoppolu/portfolio/assets/40263744/52e2aaab-e41a-49bc-9ba4-9a23fefad77b)  

**NOTE:** The ROUGE scores reported during training and evaluation are scaled by a factor of 100 for a percentage represntation. 
By default, the ROUGE scores obtained from the 'evaluate' library are all < 1, which is why the ROUGE scores on the test set are all less than 1. 
(i.e. a ROUGE-LSum score of 0.3782 is the same as 37.82)  

## LoRA vs Full Finetuning:  
For the purpose of comparison on finetuning procedures - the Base version of the [Flan T5](https://huggingface.co/docs/transformers/model_doc/flan-t5) model was finetuned using LoRA and full-finetuning  on the [DialogSum](https://huggingface.co/datasets/knkarthick/dialogsum) dataset for ROUGE score improvements on dialogue summarization.  

Both LoRA and full-finetuning were performed for a single epoch. The improvement in ROUGE scores via LoRA when compared to the pre-finetuned model were as follows
![image](https://github.com/user-attachments/assets/d10430ce-5777-41bb-b646-517333b53564)  

The ROUGE score improvements via full-finetuning were then compared to the results obtained from finetuning via LoRA  
![image](https://github.com/user-attachments/assets/8f2a1107-c4a0-4f03-b930-6ca208186a1a)  

The actual ROUGE scores for the pre-finetuned model, the LoRA model, and the fully-finetuned model are as follows  
![image](https://github.com/user-attachments/assets/fdd70cbe-50cf-444d-8564-74c1b4ea1c6d)  

**NOTE:** The 'instruct model' indicated above is the fully-finetuned model and the 'PEFT model' indicated above the LoRA model




