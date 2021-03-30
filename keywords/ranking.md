# On ranking from pairwise comparisons

Parents: ??
Related: [[centrality]], [[logreg]]

#centrality #bib


**Problem:** we have a bunch of pairwise comparisons (say, plays within players, that can end in a win for one or another). We want to create a "natural" ranking system. I think the most logical ways to define it is so that the Δrank would be a predictor of the win/loss ratio, but not sure how universal this assumption is.

Existing methods, subjectively ranked by meaningfulness)
* Bradley–Terry model (most general approach)
* ELO (practical IRL for chess)
* de Vries I&SI ranking	 (Inconsistency & strength of inconsistency)
* David's score (from 1987)
* Clutton-Brock (from 1979) - old, bad

Now descriptions, from older and simpler, to fancier:

### Clutton-Brock

Old and seems completely random and useless. Like, why one would even? I would guess, that's for computational reasons, in those old cave-times. Doesn't look at actual probabilities of defeat; makes seemingly arbitrary assumptions about reaching into the graph (length=2). There's a bit of prophetic centrality-like feeling to it though.

$R_i =(B+Σb+1)/(L+Σl+1)$
Where: B = how many other players this player_i defeated at least once, Σb = how many other players were defeated by those players that i defeated (like, grand-kids defeated; excluding i, if they had a mixed record); L = same but in the opposite direction; the number of players that beat i; Σl = the number of players that beat those that beat i.

Apparently, it's really easy to come with illogical counter-examples where CBI produces ridiculous results (see Gammell 2003 below).

### David's score

Let $p_{ij}$ is the probability of i winning over j (the share of wins among all interactions they had).

$\displaystyle R_i = \sum_j p_{ij} + \sum_j \sum_k p_{ij} p_{jk} - \sum_j p_{ji} - \sum_j \sum_k p_{ji} p_{kj}$, where sums only include those pairs {ij} where an interaction actually happened.

Relatively simple; relatively easy to calculate; and relies on p_ij values, and not just whether there was at least a single win or not, but still penetration into the graph of only length=2, and the fact that we are only summing over some interactions (that feels a bit arbitrary, although I don't know if it has weird mathematical consequences).

### Elo system

Used in chess. The difference in rating is a predictor of win ratio (or win probability). As ratings are updated all the time, essentially most rating agencies do something like "live rating propagation", when if i wins over j, some part of j's rating is added to i's rating, and then both rating are renormalized. And the opposite for the loss.

In practice, after a game between i and j they change R_i by te value of R_j + δ∙400, where δ is 1 if i won, −1 if they lost, and 0 if it was a draw. And they also limit the maximal change of R_i during each tournament by a value called a K-factor, to make sure ratings don't change too fast, and to improve real-life convergence. And then there are lots of other practical considerations, like how to discourage top-ranked students from just sitting on their rating (they don't have rating decay, but conceivably they could try it, no?).

The ratings are linked to win probabilities with logit linkage (see [[logreg]]):
$P(i>j)=1/(1+e^{-(R_i-R_j)})$, where ">" is my dirty shortcut for i winning over j.
IRL they use some norming coefficients: it's base 10, not e, and they divide ΔR by 400, but other than that, it's a logistic function.

### Bradley–Terry model

Same idea of $\text{logit}(P(i>j)) = R_i / (R_i + R_j)$, but now we are trying optimize all R for this formula actually work as good as possible, in terms of the max likelihood of {R} producing the observed data.

The way we do it, we summarize all win numbers of i over j (not rates, actual numbers) as $w_{ij}$. It gives a matrix W (positive, asymmetric). Now we init Rs at some random values, and perform update:

$\displaystyle R_i ← \left(\sum_j w_{ij}\right)\cdot \left( \sum_{j≠i} \frac{w_{ij} + w_{ji}}{R_i + R_j}\right)^{-1}$

Here left brackets is the total number of wins for this player, and the numerator in the 2nd bracket is the total number of games… But I kinda struggle to relate to this formula beyond that.

Once all R_i are calculated, renormalize them $R_i ← R_i / \sum_j R_j$. Supposedly, this nicely converges to a unique maximum.

### Rank centrality

If I get it right, it's another way to calculate a stable set of {Ri}, similar to how [[pagerank]] centrality is calculated. Let w_ij again be the number of wins. Then $w_{ij}/(w_{ij} + w_{ji})$ is the win ratio. Now scale these win ratios down by the max degree in a graph of games, and we get an oriented graph with a weight matrix P where each weight P_ij can serve as a probability of transition along the edge j→i, but also is proportional to the probability of i winning over j. To make them serve as real transition probabilities, let's just add loops P_ii, equal to $1-\sum_{j≠i}P_{ij}$.

Now applying P to a vector x performs a transition proportional to win rate. There's a stable state for x = Px (Negahban 2017), and for this situation players who win often get higher values of x. Moreover, this vector x is very closer to a true BTL solution. But apparently can be calculated much faster (big-O estimations in the paper).

### de Vries I&SI

Similar, but instead of win-rate it assumes that either i>j or not (so real binary value), just it is observed noisly. Not sure if helpful (haven't read the paper in detail; return if necessary!)

# Refs

Gammell, M. P., Vries, H. D., Jennings, D. J., Carlin, C. M., & Hayden, T. J. (2003). David's score: a more appropriate dominance ranking method than Clutton-Brock et al.'s index. Animal behaviour, 66(3), 601-605.
https://www.reed.edu/biology/342_old/assets/readings/gammell_2003.pdf

Balasubramaniam, K. N., Berman, C. M., De Marco, A., Dittmar, K., Majolo, B., Ogawa, H., ... & De Vries, H. (2013). Consistency of dominance rank order: a comparison of David's scores with I&SI and Bayesian methods in macaques. American journal of primatology, 75(9), 959-971.

De Vries, H. A. N. (1998). Finding a dominance order most consistent with a linear hierarchy: a new procedure and review. Animal Behaviour, 55(4), 827-843.
Contains a nice review of other methods.

https://en.wikipedia.org/wiki/Elo_rating_system

https://en.wikipedia.org/wiki/Bradley%E2%80%93Terry_model
(not a very good explanation)

Hunter, D. R. (2004). MM algorithms for generalized Bradley-Terry models. The annals of statistics, 32(1), 384-406. http://personal.psu.edu/drh20/papers/bt.pdf
The paper describes the BTM algorithm in a bit more detail, provides some generalizations, and also proofs, that I skipped.

Negahban, S., Oh, S., & Shah, D. (2017). Rank centrality: Ranking from pairwise comparisons. Operations Research, 65(1), 266-287.
https://devavrat.mit.edu/wp-content/uploads/2017/10/Rank-Centrality-Ranking-from-pair-wise-comparisons.pdf