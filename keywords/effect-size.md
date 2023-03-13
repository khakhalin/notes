# Effect size

Parent: [[stats]]
See also: anova

#stats


**Eta-sqared**: $η^2 =$ explained variance / total variance, or $SS_B / SS_T$ (sum of squares between, or treatment, divided by sum of squares total).

If number of data points / degrees of freedom is known, η² can be recalculated from the F-value in [[anova]]:

$F = V_B/V_W$ (variances, between and within)

$V_B = SS_B/(K-1) = SS_B/d_1$, where K is the number of groups, (K-1) is the degrees of freedom 1 (d_1), and $SS_B$ is calculated by replacing points with group means, and calculating squares of differences vs the global mean

$V_W = SS_W/(N-K) = SS_W/d_2$

Therefore: $SS_B/SS_W = F\frac{d_1}{d_2}$. Let's call it a. 
Then $SS_B/SS_T = SS_B/(SS_B+SS_W) = a/(1+a)$.

Substitute a with a combo of F and degrees of freedom, get the final formula:

$\displaystyle η^2 = \frac{F d_1}{F d_1 + d_2}$

# Refs

Some summaries:
* https://en.wikipedia.org/wiki/Effect_size
* https://www.spss-tutorials.com/effect-size/

On F-values (for )

Very old blog post by Lakens:
https://sites.google.com/site/lakens2/blog/thefirstruleofnotunderstandingeffectsizesisyoudon%E2%80%99ttalkaboutnotunderstandingeffectsizes