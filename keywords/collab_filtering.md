# Collaborative filtering

#distributed #systems

See also: [[music]], [[svd]], [[04_Features]]
Potential in the future: Federated Learning

**Recommendation systems**; important for digital marketing, but also media recommendation (movies, music); Netflix, Pandora etc. Also social networks.

In essence: a matrix of ratings, user × item. The matrix is very sparse. Ratings may be multimodal (explicit ratings and various implicit "use history"). The goal is to fill the matrix.

Simplest idea:
* User-based nearest neighbors: a weighted mean of users with ratings most similar to that given by this user in the past, then look at what they like
* Item-based nearest neighbors: a weighted mean of items with ratings most alike to those rated by the user in the past. (Negative weights are also probably possible, if the user hated something?). _More direct, but potentially less insightful, no? At least based on common sense, I wold expect the similarity and dissimilarity of various music pieces (for example) would lie in very different dimensions, and only some of the dimensions would be important for any given user at any given time..._

Simplest approach: matrix factorization, like SVD ([[svd]]), where we make R = XᵀY. Potentially better approaches: hierarchical (Koenigstein 2011), mixing implicit and explicit data (Hu 2008).

But neither of thease captures things like context and goal (the same person wants different things in different situations). Also there are intrinsic properties of the sound that we potentially have access to (rhythm, harmony etc.). We may also have access to external info (like, for music, cover image, lyrics, discussion forums).

Interestingly, in practical applications, often accuracy is a bad measure, as we are not trying to reinforce patterns; we are often trying to create a better experience for the user, and it may mean broadening, even slightly challenging their recommendation (to break the hegemony of simply "popular"). Novelty.

Another curious tidbit: if you make far-reachign assumptions, it's good to try to explain them to the user (transparency). But then they will want to correct them every now and then, and it can make the product so much more complicated.

# Refs

Lecture slides on music recommender systems:
https://www.slideshare.net/FabienGouyon/music-recommendation-2018-116102609

Books:
* Peter Knees. Music similarity and retrieval.
* Ricci, Shapira. Recommender Systems Handbok