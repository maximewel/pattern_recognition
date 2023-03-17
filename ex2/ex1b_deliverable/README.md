# Assignement 1_b: Kmeans

## Introduction

### Assignement
Create your own implementation of the K-Means algorithm (see lecture notes). Use it to cluster the contents of the training set.\
Apply at least two of the following cluster validation methods: 
* C-Index, 
* Goodman-Kruskal-Index
* Dunn-Index
* Davis-Bouldin-Index

### Expected output
• Source Code of your implementation
• Validation Values for K = {5, 7, 9, 10, 12, 15}
• Short report / text file with results

## Report
### Features implemented
The clustering works with the basis of the euclidian function, seeing as this is the most performing functions on the basis of the K-means algorithm (1_a).\
As this is an unsupervised learning algorithm, the testing set is never used during the clustering itself.\
Two validation methods are implemented:
* Dunn index
* Davis Bouldin index

The algorithm tracks when a point is re-clustered and when a center is re-computed and has changed. It can therefore stop when the max iteration is reached or on the basis of a lack of change of the centers or of the clusters.
\
Furthermore, running the notebook yields some data visualisation of the clustering that has been performed. As the images have a dimension of (28*28), they are very difficult to display - and the clustering (step by step, for example) that would have been very interesting to observe is very difficult to plot.\
Some methods to display N-dimensional data have been tried: Parallel display and spider web plot, but as the dataset is very large, the results were inconclusive. Reducing the features via PCA is not interesting as the predictors are pixel value intensities that we do a distance over, which is unadapted for reduction via predictor corellations. Maybe a display of the images via the distance could have been done, but time was lacking to find a proper implementation and visualisation if it even exists.
\
But some other data visualisation is possible. During the run, the algorithm is executed on each K and a set of interesting plots is generated:
* A pie chart showing the data repartition between the class versus a pie chart showing the data repartition on the clusters. This is to be viewed in parallel with the cluster center visualisation as multiple labels could be partitioned on multiple cluster if there are more clusters than classes (so noticable when k > n, with k the cluster count, and n the label/classes count).
* A data repartition that simply shows the initial [index, class] mix with the [index, cluster] mix. This helps see if the overall repartition is similar, although the Y axis don't necessarily match. The class 0 is the number 0, but the cluster 0 can represent any class. But in theory and if the clustering did its work, the overall repartition should more or less make similar clustering appear. Of course, this is also dependend on the n(cluster)/n(classes) ratio
* A clusters map which shows the center of each generated cluster
* A label_repartition map showing which cluster is effective for each number with its center each time. This shows how was each number classified by displaying the most represented cluster center for each class.

### Results
Each execution will not yield the exact same results. Furthermore, two validations methods are implemented - Dunn index and davis bouldin - meaning we might have two good candidates (and two orderings). Each K has the full output bundle located in "out/K/(...)". The following section briefly discuss the two best results.
\

#### All-k
* Best clusters according to Dunn: (9, 0.31896578773824047) 
    * ordered: [(9, 0.31896578773824047), (7, 0.3107420575114801), (5, 0.29668221804482436), (15, 0.18592989299361065), (12, 0.184168330352228), (10, 0.13779595363279395)]
* Best clusters according to davis bouldin: (12, 2.7442819239040905)
    * ordered: [(12, 2.7442819239040905), (15, 2.8424627385849495), (9, 2.8488637719221934), (7, 2.853872159188472), (10, 2.9031705472169804), (5, 2.989056368970224)]
The Dunn index is the ratio between the max distance of two clustered points and and the min (single linkage) distance between clusters. According to this ratio, the best Ks are 9, 7, 5, 15, 12, 10.\
The Davis boulder index is the mean of the max ratio between the cluster diameter and the distance between cluster centers. According to this index, the best Ks are 12, 15, 9, 7, 10, 5.\
We can see that the index agree to put the clusters 9,7 in the top3 and k=10 as being very bad. This is again in a very limited sample with only one experiment.
\
##### k = 9
The index k=9 is the one we could expect as being a top performer as it is close to the initial number of different classes of the dataset (10). The clusters's centers represent the numbers:
* 9 9 0 1 2 5 6 3 8
It is interesting to see that the first clusters has very high difficulties discerning the '4', the '7' and the '9'. In other cases, it happens that '8' and '0' are also closely related.\
It is interesting to see that indeed, the datapoints classified as "4" and "9" are both routed to cluster 0 which has this "4/9" center.\
It is also interesting to see on the "label repartition" that the cluster representing class 7 is also ambiguous with respect to 9 with the cluster 0. We can also assume from the piecharts that some '7' are routed to '1' as the cluster for '1' is over-represented. We clearly see that here are over-and-under-represented clusters in term of volume.

##### k = 12
The bouldin index prefers the K=12 on this run. With more clusters than classes, we could expect some classes as having multiple clusters representing them: For example, if a "0" has two main ways of being written, then multiple clusters could be usefull to catch all there representations. This is not a show stopper and it seems like the validation index indicated that this can lead to better clusters.
The clusters centers represent:
* 9 ~5 ~9,7 3 6 ~9,4 8 6 0 1 2 ~1,7
There are some clear clusters: 9, 7, 3, 6, 8, 6, 0, 1, 2. Some are confused betweem choices, with the second cluster ressembling confused 5, the third ands seventh being a merged 4 and 9... A very interesting phenomena is the last cluster being a diagonal "1": A good mix between a 1 and a seven.\
Indeed, the label_repartition shows a better representation of the centers of each cluster being close to the classes. But this representation is limited as there are more clusters than classes. Adding more cluster than classes seem to have removed some confusions between close numbers as each number seem to be representer in the centers, compared to the K=9 that had some clear confusions.\
Looking at the pie charts indicate that the volume is more evenly balanced betweem the clusters.

## Conclusion
In conclusion, the two indexes should be used together as they represent different validation values. The results of this clustering are very encouraging and the cluster centers converge to the initial classes of the problem. Of course, the clustering results are less clear and harder to use compared to the KNN but this is an other kind of problem. In this exercice, we would use the result of the clustering to understand the dataset distribution and see what classes seem to be present and different enough to be classified separately. Both 'best results' final cluster centers represent the numbers and having more clusters than classes is not a show stoper as it can help identify the relevant / well separated informations and ignore some more confuse one into less relevant clusters.