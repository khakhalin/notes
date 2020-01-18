# Lagrangian
#coremath

A way to solve equations with constraints.

**Statement:** to minimize f(x) given a consraint c(x)=0, just consider f(x)-λ∙c(x), and solve for ∂L/∂x=0 and ∂L/∂λ = 0 (like, unconstrained in dimensionality p+1).

**Some intuitive story:**

Say, c(x) is nice enough that we managed to solve it, and explicitly express some subset of X coordinates (let's call it y) through the rest of X (let's keep calling it x). 

Let's say the solution for c(x,y)=0 is y=g(x).

So now instead of f(x,y) given c(x,y)=0, we get f(x,g(x)), without constraints, that we can solve directly:

$\displaystyle \frac{df}{dx} = \frac{∂f}{∂x} + \frac{∂f}{∂y} \frac{dg}{dx} = 0$

On the other hand, let's look at what Lagrange proposed. That we take f(X)-λc(X), and set partial derivatives to 0. As in this case we (supposedly) found an explicit form for y = g(x), so we can replace c(X) with (y-g(x)), and f(X) by f(x,y):

L = f(x,y) - λ(y-g(x))

Differentiating, we have a system:

$\displaystyle \frac{∂L}{∂x} = \frac{∂f}{∂x} + λ \frac{dg}{dx} = 0$

$\displaystyle \frac{∂L}{∂y} = \frac{∂f}{∂y} - λ = 0$

From the second one here, we see that λ = ∂f/∂y, which can be put in the 1st equation, giving us the same exact equation that we got above! So at least with the assumptions we made, and with this particular type of c(X) constraint, Langrangian leads to the same formulas, and thus the same optimum, as the direct calculation. Except it doesn't really need an explicit form of y=g(x), and so can handle more arbitrary constaint.

> It's not a proof tho, is it? More of an illustration.