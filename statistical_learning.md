**clustering analysis**
is a form of exploratory data analysis in which observations are divided into different groups that share
common characteristics.

the purpose of cluster analysis is to construct groups(clusters) while ensuring the following property: 
within a group, the observations must be similar as possible, while observations belonging to different groups
must be different as possible.

clustering algorithms use distance to separate observations into different groups. therefore, before
diving into some random classification methods, a reminder exercise on how to compute distances between points is presented.

*application 1*: computing distances
the that we're going to use to calculate the distance between 2 points is with the Pythagorean theorem, known as:
$$\sqrt{(x_a - x_b)^2 + (y_a - y_b)^2}$$

for example, if we got 2 points: 'b' and 'c' with some coordinates known as: $b(1,0)'$ and $c(5,5)'$. so imagine that we need
to calculate the distance between those points. what should we do now?

$$\sqrt{(x_b - x_c)^2 + (y_b - y_c)^2} = \sqrt{(1 - 5)^2 + (0 - 5)^2} = 6.403$$

in python, we have a module that can help us to calculate this distance, named as math module. it's also nice to remember that the
distance between those points is called euclidean distance. 

*note*: we do have some other ways to calculte the distance between those points, like:
- minkowski;
- manhatan;
- chebyshev;
- mahalanobis;
- cosine.

Topics that are importante to understand before presenting any type of clustering:
**Distortion**
distortion is the sum of square errors and its basically the sum of the squared differences between each observation and its group's mean. it can be used as a measure of variation within a cluster. if all cases within a cluster are identical, the SSE would then be equal to 0.
$$\sum_{i=1}^n (x_i - \bar{x})^2$$

**Inertia**
inertia or within cluster of sum of squares value gives an indication of of how coherent the different clusters are. 
$$\sum_{i=0}^N (x_i - C_k)^2$$

- *N* is the number of samples within the data set;
- *C* is the center of a cluster

So the Inertia simply computes the squared distance of each sample in cluster to its cluster center and sums them up. This process
is done for each cluster and all samples within that data set. 

*note*: The smaller the inertia value, the more coherent are the diffent clusters.

When as many clusters are added as there are samples in the data set, then the inertia value would be zero. So there's a problem. To
solve it we should use a method called the *Elbow Method* 

**Elbow Method**
~~~python
from sklearn.datasets import make_bloobs
from sklearn.cluster import KMeans
import seaborn as sns
import matplotlib.pyplot as plt
plt.rcParams['figure.figsize'] = [8,8]
sns.set_style("whitegrid")
colors = plt.rcParams['axes.prop_cycle'].by_key()['color']

n_samples = 2000
n_features = 2
centers= 3
cluster_std = 2.5

X, y = make_bloobs(n_samples = n_samples, n_features = n_features, centers = centers,
                    cluster_std = cluster_std, random_state = 42)
~~~

There are three clusters in this data set, so the optimal number of clusters for K-Means should be three. But let???s assume we don???t know that yet.

Let???s now train a K-Means model for a different amount of clusters and store the Inertia value for each trained model. Afterwards, the Inertia-Curve can be plotted in order to use the Elbow-Method for finding the optimal number of clusters. 

~~~python
inertia_list = []
for num_clusters in range(1, 11):
    kmeans_model = KMeans(n_clusters=num_clusters, init="k-means++")
    kmeans_model.fit(X)
    inertia_list.append(kmeans_model.inertia_)
    
# plot the inertia curve
plt.plot(range(1,11),inertia_list)
plt.scatter(range(1,11),inertia_list)
plt.scatter(3, inertia_list[3], marker="X", s=300, c="r")
plt.xlabel("Number of Clusters", size=13)
plt.ylabel("Inertia Value", size=13)
plt.title("Different Inertia Values for Different Number of Clusters", size=17)
~~~

The elbow point gives the optimal number of clusters, which is three here. This makes totally sense, because the data set is created such that there are three different clusters. When adding more clusters, the Inertia value decreases, but also the information contained in a cluster decreases further. Having to many clusters leads to a performance decrease and also to a not optimal clustering result.

**$Types$ $of$ $R-Squared$**

**$R^2$** is a statistical measure of how close the date are to the fitted regression line. And is defined as:

**$R^2$** = $\frac{Var_{Explained}}{Var_{Total}}$

**$R^2$** is always between 0 and 100% 
- 0% indicates hat the model explains none of the variability of the response data around its mean
- 100% indicates that the model explains all the variability of the response data around its mean

$R^2 adjusted$
The adjusted R-squared compares the explanatory power of regression models that contain different numbers of predictors.

$Predicted$ $R-Squared$
The predicted R-squared indicates how well a regression model predicts responses for new observations.

*application 2*: k-means clustering 
k-means is a good method to learn about how clustering works. so, we're going to study some particularities about it. 
the first thing we should know about it is that k-means clustering uses $n$ observations into $k$ clusters in which each observation belongs 
to the cluster with the closest average, serving a prototype of the cluster. 



**two-step clustering**
it's a type of clustering that give the number of clusters that we can work with.
*pros* 
- determinate the number of clusters;

*cons*
- we cant work with a large sample 


**partitional clustering** 
divides data objects in non overlapping. in other words, no object can be a member of more than one cluster, and every
cluster must have at least one object.

*pros*
- they work well when clusters have spherical shape;
- they're scalable with respect to algorithm complexity.

*cons*
- they're not well suited for clusters with complex shapes and different sizes;
- they break down whem used with clusters of diferent densities.

**hierarchical clustering** 
determines cluster assignments by building a hierarchy. basically, hierarchical clustering starts by treating each observation as a separate cluster. Then, it repeatedly executes the following two steps:
- identify the two clusters that are closest together;
- merge the two most similar clusters. 

*this process continues until all the clusters are merged together. 

**Measure of distances between clusters**
the distance between two clusters has been computed based on the length of the straight line drawn from one cluster to another. this is commonly referred to as the euclidean distance. there is no theoretical justification for an alternative, the euclidean should generally be preferred, as it is usually the appropriate measure of distance in the physical world.

the choice of distance metric should be made based on theoretical concerns from the domain of study. that is, a distance metric needs to define similarity in a way that is sensible for the field of study.

**Linkage criteria**
after selecting a distance metric, it is necessary to determine from where distance is computed. as with distance metrics, the choice of linkage criteria should be made based on theoretical considerations from the domain of application. there are no clear theoretical justifications for the choice of linkage criteria, ward???s method is the sensible default. this method works out which observations to group based on reducing the sum of squared distances of each observation from the average observation in a cluster. 


