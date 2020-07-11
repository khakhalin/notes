# Collaborative filtering

#distributed #systems

See also: 
Potential in the future: Federated Learning

**Recommendation systems**; important for digital marketing, but also media recommendation (movies, music); Netflix, Pandora etc. Also social networks.

In essence: a matrix of ratings, user × item. The matrix is very sparse. Ratings may be multimodal (explicit ratings and various implicit "use history"). The goal is to fill the matrix.

Simplest methods:
* User-based nearest neighbors: a weighted mean of users with ratings most similar to that given by this user in the past, then look at what they like
* Item-based nearest neighbors: a weighted mean of items with ratings most alike to those rated by the user in the past. (Negative weights are also probably possible, if the user hated something?). _More direct, but potentially less insightful, no? At least based on common sense, I wold expect the similarity and dissimilarity of various music pieces (for example) would lie in very different dimensions, and only some of the dimensions would be important for any given user at any given time..._

# Refs

On music recommender systems:
https://www.slideshare.net/FabienGouyon/music-recommendation-2018-116102609