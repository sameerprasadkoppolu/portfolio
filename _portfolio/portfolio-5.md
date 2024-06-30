---
title: "Keystroke Behavioral Anomaly Detection"
excerpt: "An Anomaly Detection Algorithm to detect mimickers among typists' clusters utilizing Dimensionality Reduction using t-SNE, and Spectral Clustering."
collection: portfolio
---


## GitHub Repository:
[Repository](https://github.com/sameerprasadkoppolu/Keystroke-Behavioral-Anomaly-Detection/tree/main)  


## Introduction:
One effective technique for thwarting cybercrime is anomaly detection in user authentication. Analyzing the keystroke dynamics of people typing the same text—a seemingly routine action that has minute individual differences—is one intriguing method. This method can produce a distinct profile of each user's typing habit by recording and evaluating characteristics like dwell time, flight time, and keystroke sequence. Even in cases where the attacker has the right credentials, this profile serves as the benchmark for identifying deviations and may point to abnormalities that could be signs of attempted illegal access. This continuous, non-intrusive authentication technique has a lot of promise to improve online environment security, especially when paired with additional security measures.  

While keystroke behavior can vary depending on the user, it can also vary depending on the text being typed out. This is because the sequence of characters also plays an important role along with the timing information. Hence, for this project, we consider the typing and timing information of 51
users all typing the same text 400 times across 8 sessions (50 per session). This data was collected as part of a research paper (Killourhy, Maxion, 2009) done at Carnegie Mellon University (CMU).  

While the authors of the paper showcased 14 different approaches to detecting anomalous users (i.e. users who were trying to gain illegitimate access to other users’ accounts by mimicking their keystroke behavior), this project aims at showcasing an additional method towards solving the same problem. The key difference is that the solution to the problem taken in this project makes use of Unsupervised Machine Learning – specifically clustering.


## Data Collection:
The data consist of keystroke-timing information from 51 subjects (typists), each typing a password (.tie5Roanl) 400 times. The data are arranged as a table with 34 columns. Each row of data corresponds to the timing information for a single repetition of the password by a single subject. The first column, subject, is a unique identifier for each subject (e.g., s002 or s057). Even though the data set contains 51 subjects, the identifiers do not range from s001 to s051; subjects have been assigned unique IDs across a range of keystroke experiments, and not every subject participated in every experiment. For instance, Subject 1 did not perform the password typing task and so s001 does not appear in the data set. The second column, sessionIndex, is the session in which the password was typed (ranging from 1 to 8). The third column, rep, is the repetition of the password within the session (ranging from 1 to 50).  
  
The remaining 31 columns present the timing information for the password. The name of the column encodes the type of timing information. Column names of the form H.key designate a hold time for the named key (i.e., the time from when key was pressed to when it was released). Column names of the form DD.key1.key2 designate a keydown-keydown time for the named digraph (i.e., the time from when key1 was pressed to when key2 was pressed). Column names of the form UD.key1.key2 designate a keyup-keydown time for the named digraph (i.e., the time from when key1 was released to when key2 was pressed). Note that UD times can be negative, and that H times and UD times add up to DD times.  
  
Prior to applying any clustering or dimensionality reduction algorithms, the data was standardized by subtracting the mean from every observation and dividing by the standard deviation.  


## Methodology:
As stated before, the aim of the project was to detect mimickers and non-mimickers based on the fact that every user would have their own unique keystroke behavior. Hence, the project was divided into two parts – clustering and anomaly detection.  
  
For clustering, our project was focused on the idea that since every user had their own unique keystroke behavior, they would have their own cluster. Hence, as the number of sessions increased, the clusters would become more and more well separated. Three different clustering methods were implemented – K-Means, GMM, and Spectral Clustering. They were chosen as they allow for the decision of the number of clusters (51 in this case as there are 51 unique users in each session). The same clustering algorithms were implemented with dimensionality reduction algorithms such as PCA and t-SNE. A point to note is that the clustering is performed for every session individually. Additionally, the dimensionality reduction as well as the standardization of the data was done for every session individually.  

Once the clustering is performed, the test observations need to be classified as to whether they are mimickers or not. As clustering with t-SNE gave the best results, the same t-SNE model used prior to clustering the data, is fit on the test observations to reduce the dimension of the test observations to 2 dimensions. After reducing the dimensions of the test observations, Anomaly Detection is performed as follows.
  1. First, the centroid of every cluster is calculated as the mean of all the points of that cluster. Additionally, a threshold value equal to 2 standard deviations is also calculated for each cluster. Furthermore, in each cluster, the ‘subject’ that has the maximum number of observations (majority class user) is determined, along with whether the ‘subject’ is a true majority of the cluster or not (i.e. at least 50% of the observations of the cluster belong to the majority class user).
  2. Next, for each test observation, the distance to each centroid is calculated to determine the closest centroid. Once the closest centroid is determined, it is determined whether this centroid belongs to a cluster where there is a true majority.
     * If a test observation’s closest centroid is part of a cluster that has a true majority, and if the distance between the test observation and its closest centroid is more than 2 standard deviations, then the test observation is a mimicker who is mimicking the majority class user.
     * If the above condition is not met, then the test observation is not mimicking any other user.
     * Additionally, to replicate a real scenario where a user may type out a password multiple times for confirmation, every test observation has 2 additional copies that have noise added to it to account for the minor fluctuations in a user’s typing.

Here is the algorithm's pseudocode.  
![image](https://github.com/sameerprasadkoppolu/portfolio/assets/40263744/27d15b38-1cf3-4d7d-b770-9b34cbad601c)  

A point to note is that the test observations were created by bootstrapping the data of the 8th session and adding noise to each observation. Like mentioned before, each test observation had 2 additional copies created with noise added to each. The size of the test set was 20% of the size of the data in the 8th session.  
  
Additional Analysis using labeled test data was also performed. This test data was that of the 6th session. No additional noise-boosted copies were created in this case. All observations were classified as mimickers and non-mimickers. This approach was done to check whether the anomaly detection approach identifies false negatives (i.e. legitimate users identified as mimickers) and false positives (i.e. illegitimate users identified as non-mimickers).  
  

## Results:
For all of the Clustering Algorithms, the number of clusters chosen was 51. The clustering was performed for each ses-
sion individually and the internal measures (Silhouette Score and Calinski-Harabasz Score) and external measures (Homogeneity, Completeness, and V-Measure) were reported.  

The internal measures of the Clustering Algorithms in the 8th session were as follows.  
![image](https://github.com/sameerprasadkoppolu/portfolio/assets/40263744/ca0ba43c-c3cc-4f3a-ae14-2079b321ec2d)  

The external measures of the Clustering Algorithms in the 8th session were as follows.  
![image](https://github.com/sameerprasadkoppolu/portfolio/assets/40263744/0e1a6cc0-b0a7-4601-b057-9e398b694c9f)  

The best clustering result obtained was from t-SNE + Spectral Clustering. Thereafter, the anomaly detection was performed on unlabeled test data. The test data was generated by bootstrapping the data of the 8th session to get 510 observations (equivalent to 20% of the 8th session’s data). Each test observation was then classified as ‘Mimicker’ or ‘Not a Mimicker’ as described in the Project Description section.  

In the additional analysis, the data from the 6th session was taken as test data with the ‘subject’ field being the true
label. Thereafter, each test observation was again classified as ‘Mimicker’ or ‘Not a Mimicker’. After classification, the False Positive Rate and the False Negative Rate were calculated which in turn was used to calculate the Equal Error Rate where the Equal Error Rate is defined as the sum of the False Positive and False Negative Rates divided by 2. 

Here's a comparison of the anomaly detection approach implemented in this project with respect to the other approachesin the paper (Killourhy, Maxion, 2009).  
![image](https://github.com/sameerprasadkoppolu/portfolio/assets/40263744/c96638fb-4229-4770-86cd-b4b58d07780c)  
  
