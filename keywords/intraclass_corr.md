# Intraclass correlation

#stats

How strongly units in the same group resemble each other.

> So like ANOVA, but as a −1..1 coefficient?

Originally proposed by Fisher, with the following logic:
1. Consider all possible pairs of values (x1,x2) where both x1 and x2 belong to the same class
2. Calculate a gross mean across everything: m
3. Σ(x1-m)(x2-m)
4. Normalize appropriately (formulas for normalization coeffs are weird tho)

How people use it now:
1. Use ANOVA framework, and calculate all variances
2. ICC = var_within / var_total

> So basically, the "explained variance", huh? Except that it's now never negative. Which is probably for the best.

# Refs

https://en.wikipedia.org/wiki/Intraclass_correlation