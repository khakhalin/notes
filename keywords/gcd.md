# Greatest Common Divisor

#algo

Aka **GCD**. Can be found using an Euclidean Algorithm:

```python
def gcd(a,b):
    if b==0: return a
    return gcd(b, a % b)
```

How does it even work? First of all, k = gcd(a,b) = gcd(a mod b, b). That's because if a<b, then a mod b = a; and if a>b, then a = xb + (a mod b). Therefore, if k divides a and b, then it also has to divide (a mod b) (proving that k is both a divisor), but also if something divides (a mod b)  and a, then it has to divide b (proving that k has to remain the greatest common divisor of b and a mod b). And it means that you can always simplify a task, and go from gcd(a,b) to gcd(a mod b, b), if a>b, or gcd(a, b mod a) if b>a. If you repeat that long enough, at some point you'll get down to k = gcd(a,b) on one side, and at the next step (xk mod k) = 0.

The formula above uses all three points. It performs a mod b recusrively, but also it changes the order every time (sometimes it will mean a blank, empty loop, but that's OK), it catches the moment immediately after gcd was reached, to break the recursion.

# Refs

* Proof: https://www.whitman.edu/mathematics/higher_math_online/section03.03.html
* https://www.geeksforgeeks.org/c-program-find-gcd-hcf-two-numbers/