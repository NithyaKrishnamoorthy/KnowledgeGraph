# Movie Recommendation Engine

### Introduction and background
During the past few decades, with the rise of YouTube, Amazon, Netflix and many other such e-Commerce or streaming services, recommendation systems have taken gained a lot of importance in the digital landscape.
It is imperative for the service providers to implement an efficient recommender system, as it can produce significant revenue uplift and differentiate the service provider from its competitors, thus gaining stronger customer engagement.

### Motivation
The objective is to build a movie recommendation system leveraging on knowledge graph tools / resources.
The knowledge graph tools and resources can represent inter-connected relationships in an intuitive manner and help to mine insights from the underlying network. This would be valuable to recommendation engine, as many times a recommendation is not solely based on the target user’s own behaviour or attributes but also the community he/she is associated with.
This project aims to explore various paradigms of implementing recommendation system for movie recommendation and eventually build a mini application for such purpose.

### Data
MovieLens dataset is obtained from https://grouplens.org/datasets/movielens/ <br>
(F. Maxwell Harper and Joseph A. Konstan. 2015. The MovieLens Datasets: History and Context. ACM Transactions on Interactive Intelligent Systems (TiiS) 5, 4: 19:1–19:19. https://doi.org/10.1145/2827872)

The dataset includes 9000 movies, 600 users, 100,000 ratings and 3600 tags. Details of files as per below table:

<table>
<th><td>File</td><td>Info</td></th>
<tr><td>Movies.csv</td><td>Movie ID, title and genre (multiple)</td></tr>
<tr><td>Ratings.csv</td><td>User ID, Movie ID and rating (5-star scale)</td></tr>
<tr><td>Tags.csv</td><td>User-generated metadata about movies</td></tr>
<tr><td>Links.csv</td><td>Links to Imdb, tmdb</td></tr>
</table>


![Movies by genre](https://github.com/NithyaKrishnamoorthy/KnowledgeGraph/blob/master/images/movies_genre.PNG)
![Users and ratings](https://github.com/NithyaKrishnamoorthy/KnowledgeGraph/blob/master/images/user_ratings.PNG)

### Methodology

<b>Content-based filtering: </b> Pairwise similarity for all movies is determined by the following heuristics:
<ol>
	<li>Similarity based on movie genre</li> 
	<li>Similarity based on viewership and ratings</li> 
	<li>Similarity based on tag genome</li> 
</ol>

The output of the models will be fed into a neo4j database using the py2Neo driver. The recommendation will be performed by querying the neo4j database using cypher queries.

### Evaluation
Evaluation for content-based filtering:<br>
Top N results were considered for evaluation of the recommendation system.
Firstly, test dataset was prepared by the leave-one-out portioning. From the original dataset, a random record was selected from each user to be treated as movie that the user has not watched yet, totaling 610 records. The remaining dataset forms the training dataset.
The idea is, each user is given 5 recommendations by the recommender system. The recommendations are compared against the test dataset that was left out from the original dataset to examine for each user, if the left-out record is a part of the top 5 recommendations. If so, this would be considered as one hit. The presence of a hit means the recommendation is relevant to certain degree, as the user eventually watched the recommended movie later. 
For each hit, the rating given by the user about the left-out record is also considered. If the rating is high (>=4), this indicates that the recommendation is effective given that user has rated the movie favorably.
This evaluation method is applied for the top 5 recommendations across all 610 users. This generated 0 hits. This is a non-conclusive result, as this can either indicate the recommendation system is not effective or it is simply because the information on whether the user has watched the recommended movie is not available at the time the dataset is released.
Following this, the distribution of the test dataset is examined. About 44% of these records are rated lower than 4, signaling that these movies are not favored by the user and intuitively these should not appear in the recommended list. Also, 56% of these records are rated with 4 and above, and none of these was found in the recommended list. 
The original dataset is examined for data sparsity. Out of the 610 users, the mean value of the number of movies rated per user is 165 and the value at the upper quartile (75%) is 168, while there are more than 9,000 movies in the system. 
In the movies dataset, the upper quartile value for the number of reviews per movie is 9. This shows that there are much more movies than users in the system and may have attributed to the results of 0 hits.

![Results](https://github.com/NithyaKrishnamoorthy/KnowledgeGraph/blob/master/images/Result.PNG)
