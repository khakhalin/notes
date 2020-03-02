# LA for signal processing
#linalg

## Convolution

$c_i = \sum_{k=1}^n a_{i-k+1} b_k$ . For example, for n = len(b) = 3, c3 = a3b1 + a2b2 + a1b3 , so b goes backwards in time. On edges (where b doesn't fit), it's logical to just crop it (so use min(n,legal) for upper target)

As it's linear, it may be represented as c = Ta where T = almost diagonal matrix with b written in columns, and sliding down. T_ij = b_{i-j+1} if legal subscript, and 0 otherwise. Apparently it's called a _Toeplitz matrix_.

In practice conv isn't calculated via direct matrix multiplication though, but via a fast convolution algorithm, somehow based on FFT. Wow.

2D convolution is similar, VMLS denotes it as $A\star B$ ; as it's 1D sister it's commutative, associative, and linear for one matrix if the other one is fixed.