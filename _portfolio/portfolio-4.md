---
title: "Soccer Player Recommendation System"
excerpt: "Created content-based recommendation system web application to recommend the top 10 most similar players for an input player using PCA and Self-Organizing Maps with the stats of the 2022-
23 season in Europe"
collection: portfolio
---


## GitHub Repository:
[Repository](https://github.com/sameerprasadkoppolu/Soccer-Player-Recommendation-System)  
[App](https://soccer-player-recommendation-system-app.streamlit.app/)  
[Medium Blog](https://medium.com/@sameerkoppolu/building-a-soccer-player-recommendation-system-94673091307e)


## Introduction:
With Soccer (or Football) fans so large in number all across the globe, the world tunes in whenever a major event in the sport takes place. Be it at the club or international level, fans throng in support of their favorite teams, and a big part of these teams are the players who represent them. Therefore, it is with no surprise that when the transfer window opens, the world is curious to know what the new player-team pairings will be.  

But what if a team isn’t able to sign their primary target player? What are their alternative options that are similar to their primary target? It is in light of this that I have built a simple player recommendation system that suggests the Top 10 most similar players for a given input player using Principal Component Analysis (PCA) and a Self Organizing Map (SOM) using Cosine Distance and Bray-Curtis Distance.


## Data Collection:
The data used to build this Recommendation System is obtained from fbref. However, the data was obtained using the worldfootballR library, which extracts data from the fbref website. The data that I used consists of player statistics from the 2022–23 season from players belonging to Europe’s Top 5 leagues, the Liga Primeira, and the Eredivisie. The players’ stats included their Standard Stats, Shooting, Passing, Defensive Actions, Pass Types, Goal and Shot Creation, Possession, and Miscellaneous Stats. All of these can be seen on the fbref website.  


## Data Preprocessing:
As part of the Data Preprocessing step, the most important task is to sum up the stats of players who have played across multiple clubs within the same season. This ensures that every observation is a unique player. To do this, we create a new variable that holds the concatenated value of the Player’s name, Birth Year, Nation, and Age, and then perform a group by using this variable while summing up the stats. This ensures that every player is represented as a unique observation. We perform this activity to take care of players who were transferred within the same league or across leagues in a given season.  
  
The second important step is more related to specific to Liga Primeira and Eredivisie. Here, we need to rename the columns so that they match the column names of the dataframe containing Europe’s Top 5 Leagues. Additionally, the Liga Primeira and Eredivisie dataframes don’t have the league names for all the observations. This has to be fixed.  
  
Finally, prior to modeling, we need to make sure that every player statistic except for Matches Played and Matches Played is converted to their Per 90 statistical form. Here is the formula.  
![image](https://github.com/sameerprasadkoppolu/portfolio/assets/40263744/6c28ade5-4dd4-4aba-970f-8b53da33fd41)  


## Methodology:
* Post the data preprocessing step, the final dataset has around 103 dimensions that we wish to base the recommendation system on. And since the system works on the principle of content-based filtering, it is important to find points, or in this case players, that are close to each other. However, the number of dimensions are very large to compute the neighborhood of a point. Hence, we need to perform dimensionality reduction, which is achieved using Principal Component Analysis (abbreviated as PCA).
* Next clustering is performed using a Self Organizing Map (SOM) with combination of Bray Curtis distance and Cosine distance metrics. Let’s look at the steps involved in SOM.  
  1. First we initialize the weights in the connections randomly. Remember that a connection is a weight vector representing a neuron.  
  2. Next is the competition step. For every input observation, we find the neuron that is most similar to an input observation using a specific distance metric. In the case of this project, I’m using a combination of Cosine Distance and Bray-Curtis Distance. Remember that this step will most likely result in a specific neuron being similar to more than just 1 observation. This most-similar neuron is also called the Best Matching Unit (BMU).
  3. Now we arrive at the collaboration step. Here we have to understand that every neuron has a neighborhood of neurons identified by a specific function. You can visualize this as a circle with its centre on the BMU and a radius extending from the centre resulting in a circle, and whatever neurons are in that circle are in the neighborhood of the BMU. As the word collaboration says, the aim is to try and make every neighborhood neuron similar to the input vector. This is where the weight update step comes into play. A point to note is that, over multiple iterations, the size of the neighborhood gets smaller. Moreover, the weight update step involves the use of a learning rate which also decreases as the number of iterations increase.
  4. Step 4 is to repeat the steps 2 and 3 for a certain number of iterations.
* At the end of it, we will have clusters of input observations where each cluster’s centre is a neuron in the map. Some important hyperparameters in the SOM are the number of neurons, the parameter that controls the size of a BMU, and the learning rate. For this project, I have made use of the ‘minisom’ library. The link here provides the details of the library. One more point to note in the case of this project is the grid size I have chosen. I have picked a grid size of 2x3 which is basically a plane of 6 neurons arranged in 2 rows and 3 columns. This resulted in 6 clusters of reasonable sizes. Moreover, I noticed that even if I increased the grid size, the topographic error and the quantization error were small and did not change much.
* To obtain the Recommendations we do the following.
  1. Prepare a distance matrix for every cluster obtained from SOM. The distance metric is the sum of the Cosine Distance and Bray-Curtis Distance.
  2. For a given input player, identify the player’s cluster and thereby the distance matrix. Now once we have the distance matrix, we simply look for the 10 closest observations (i.e. players) for the input player using the distances in the distance matrix calculated for the cluster of observations that the given input player belongs to.
  3. Once we get the 10 closest players for the given input player, we can visualize them using a radar plot. I have built the radar plot using a few of the features from the original dataset to create a plot that contains 2 radars — one for the input player and the other for the recommended player. This results in 10 radar plots where each plot contains 2 radars. For this, I made use of the ‘mplsoccer’ and ‘matplotlib’ libraries. The link here refers to a documentation from ‘mplsoccer’ that explains how to build the radar plot.  
Here's an example of one of the recommendations for Luka Modrić, the star midfielder of Real Madrid.
![image](https://github.com/sameerprasadkoppolu/portfolio/assets/40263744/6d7ce5c6-d307-4534-8d5c-b3fbcfa0ef94)  
