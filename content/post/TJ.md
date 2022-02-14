---
title: "On the Trail of Trader Joe's Warehouses"
date: 2022-02-14T10:19:48+08:00
keywords: ["K-means"]
description: "Finding the location of TJ's warehouses using its stores by the help go K-means clustering"
tags: ["Clustering"]
cover: "img/traderjoes.jpeg"
---

Recently, I came across with a 2015 project by Sam Burer via hackernews archive. In his project, Sam mainly tries to find the location of warehouses/distribution centers of a well-known retailer called Trader Joe's by using the location of the stores in US. He does it very creatively by exploiting centroids of the clusters suggestd by the K-means algorithm, a basic algorithm that is primarly used to tackle unsupervised, clustering, problems in the Machine learning domain. 

In this post, I try to -somehow- update his project as I encouraged by oldness of his post (june 2015)...

Trader Joe's is reluctant to make the location of its warehouses public, however one can reach information about some of its warehouses by digging deeper the internet. Sam manages to find the location of six of them from various internet sources and shares them with their evidences in his original post .  I managed to increase the number of his findings to twelve, thanks to this blog post. These "true" locations of the warehouses will be used to compare validity of our findings at the end.

The number of Trader Joe's has increased from 451 to 539 since Sam's post. Besides, they have omitted the definitive list of their stores in the mean time. Hence, I needed to build a pipline that systematically scrapes their web site and creates a data set of their inclusive list of stores and their corresponding addresses. The data set can be downloaded from here. Feel free to use it for your own purpose.

As one can imagine, the maintaned addresses are string type and needed to be transformed in a way that our algortihm would make sense of. In order to succeed that, the process called geocoding which resolves addresses into coordinates pairs comes into play. Coordinate pairs are actually nothing but a float type tuples which of a numerical algorithm would make sense without any problem.

The K-means algorithm requires a pre-defined number of clusters, k, where observations are partitioned into. Thus, one needs to guess the number of clusters in prior to algorithm starts running. In the original post, Sam guesses it as 22, approximately square root of the number of Trader Joe's at the time (451), in a sense that each warehouse would serve on average about 22 stores. I experimented with numbers between 21 and 23 for k in the line of his logic, however I couldnt come up with valid results; given that one-third of the Trader Joe's are located in California, some warehouses in other states would end up serving only up to 5-6 stores according to this scenario. Hence, I decided to approach the problem slightly more analytically and apply a quick silhouette analysis.

Silhuette score of the final configuration of the data is calculated by averaging  silhoutte value of all the observations. A silhuette value of an observation can be taught as its degree of similarity to its own cluster compared to other clusters. The score ranges from -1 to +1, where a high value indicates a more appropriate clustering configuration.  


<center>
![Alt text](/img/sit.png){width=40%}
</center>




Silhuette scores of different k values ranging from 13 to 20 is calculated, and k=19 is found to be the most appropriate.  Besides number of stores that each warehouses serve is more homogenously distributed this time. It should be noted that K-means algorithm only converges to a local minimum rather than a global minimum, hence the results are strongly influenced by the initial conditions. In other words, cluster configurations may seem difference from run to run unless it's random state is seeded, even as k stays constant.

The following map shows all 539 TJ's in yellow, while showing the  19 guessed warehouses in red. Also, the 12 'known' warehouses which are listed above are depicted in green. It can be seen that the actual warehouses are not too far from guessed warehouses. It is very fascinating to have not that bad guesses with such a basic algortihm.\


<center>
![Alt text](/img/tj.png)
</center>





