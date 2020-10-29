# Numpy

#tools

Parent: [[python]]
See also: [[pandas]], [[tensorflow]]

# Syntax innovations

* `a @ b` is the same as `np.matmult(a,b)`
* **Ellipsis**: a short for a sequence of `:,:`..., so `a[...,1]`, for a 3D variable a, means the same as `a[:,:,1]`

# Array manipulation

* For array creation, outermost bracket becomes axis0 dimension: `np.array([[1,2],[3,4]])` becomes `[1 2 ; 3 4]` in Matlab notation.
* Array properties: `a.shape` gives shape; `size` - number of elements; `dtype` - type.
* `linspace(start, stop, n)` - start and stop are _included_ (by default). Making it very different from `arange(start,stop,step)` that doesn't include the right border (similar to core Python `range`)
* Array  methods: 
    * Conversion methods: `.astype(dtype)`, possibly followed by `.tolist()`
    * `.T` - transpose and output. Doesn't modify the array. Also this is more of a property, not a method (adding parentheses results in an error)
    * `.reshape(2,3)` - reshape to dimensions and output. Doesn't modify the array.
        * For a wildcard (to skip stating one of the dimensions), use `-1` for this dimension.
        * By default reads by row (last index changes the fastest.)
    * `.sort()` - sort in-place: DOES modify the array!!! ⚠️Gotcha! Returns a None. 
    * `.flatten()` - turn a >1D array into a 1D array. Last axis runs first.
* `np.sort(a)` - sort before output (doesn't modify the array)
* `np.append(a, 7)` - append element to an array and returns the result.
    * Trying to append more than one element just appends them all one by one.
    * Trying to add a non-1D array doesn't raise an error, but makes the array 1D suddenly.

**Stacking and concatenation**: several options here
* `np.concatenate((ar1,ar2), axis=0)` - stacking along an existing dimensions (e.g. to get a 2D array as a result, you need to give it 2D arrays as inputs), `axis=0` is down (adding rows), `1` is to the right (adding columns). Doesn't expand dimensions automatically (raises an error).
* `np.stack((ar1,ar2), axis=0)` - stacking along a new dimension.
* `np.vstack((a1,a2))` and `np.hstack` - not sure it's a correct explanation, but it seems that if axis0 (for vstack) or axis1 (for hstack) exists, it acts as `concatenate` (splicing along the existing axis), while if it doesn't exist, it behaves as `stack` (and creates an axis).
* If you need a new dimension, and the number of items to stack is known upfront and is small, you can also just do `c = [a,b]`, which is obviously equivalent to just writing `c` explicitly, like `[[1,2],[3,4]]` or something. It would create a new axis0, and shift axes of `a` and `b` by one.

The stacking rule above leads to an interesting behavior. `hstack` of two 1D arrays is a 1D array, presumably because dimension 0 exists for 1D array, making `hstack(a,b)` the same as `np.concatenate((a,b), axis=0)`. But for 2D arrays, `hstack` operates along the 2nd dimension (axis1), because the 1st dimension (axis0) is vertical, so `hstack(a,b)` is the same as `np.concatenate((a,b), axis=1)`. In other words, 1D arrays seem to be technically columns, but hstack works on them as if they were rows, as that's how they are written and outputted. That's obviously a ⚠️Gotcha!!! 
    
**Expanding dimensions:**
* Option 1: `np.expand_dims(a, axis=0)` - inserts a new axis at position (0 here)
* Option 2: `a[np.newaxis, ...]` - same result. To insert in places other than leftest and rightest, can also do explicit `a[:,np.newaxis,:]` instead of ellipsis.
* Note that **slicing** with fixed coordinates on at least some of the axes reduces dimensions (so `a[:,1]` from a 2D array is a 1D array), but slicing with `:` doesn't (so `a[:,1:2]` from a 2D array gives a 2D result, even though the values themselves are the same in these two expressions).
* `np.split(arr, n, axis=0)` - returns a list of arrays achieved after splitting the input array along its axis (axis0 by default) in equal lengths (like automated slicing of sorts). The result has the same ndim as the original array (e.g. even if you slice a 2D array by row, you still get 1×n arrays)

# Open questions

* `a.view()` - apparently a big thing for performance, about creating a view of an array without rewriting it in memory. Not sure yet how it works.

# More ⚠️Gotchas

* Unlike in Matlab, **1D arrays and 1×n arrays are two different objects**. 1D arrays cannot be transposed (or rather, which is actually worse, transposing them doesn't change them, but also doesn't return an error).
* Unlike in Matlab, **different array-generating commands treat the first argument differently**:
    * `zeros` and `ones` assume the first arg is a length of a 1D array (unless you provide a tuple),
    * `eye` thinks it's rank (you cannot provide a tuple),
    * `np.random.rand` wants the dimensions be the 1st and the 2nd positional arguments (cannot take a tuple)
    * `np.random.normal` treats 1st ad 2nd arguments as μ and σ, and if you provide a tuple it's assumes it's a vector of μs. `np.random.randit` is similar in spirit (except first 2 arguments are low and high, setting the range of numbers it returns).
    * Moreover, the name for the dimension argument is different for core np and np.random: the core ones call it `shape=(2,2)`, while randoms call it `size=(2,2)`. (Despite the fact that the resulting array has a property `shape` of course, and not size, as all numpy arrays do)
* For 3D arrays, `print()` loops through axis0, and then plots axes1-2 "as if they were" 0 and 1 (as 2D matrices). Somehow I assumed that it would loop through axis2, trying to preserve 0 and 1 as "true" 0 and 1. But no.
* By default, both `ones` and `zeros` create floats (but it may be changed with `, dtype=int`). An alternative way to create a matrix of all ints: use `np.full((shape),value)`

# Refs

* https://s3.amazonaws.com/assets.datacamp.com/blog_assets/Numpy_Python_Cheat_Sheet.pdf
* https://s3.amazonaws.com/dq-blog-files/numpy-cheat-sheet.pdf