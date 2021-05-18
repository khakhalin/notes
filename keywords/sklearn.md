# Sklearn

Parents: [[python]]
See also: [[numpy]], [[pandas]]

#tools

# Linear regression 

See: [[linear_regression]]

```python
from sklearn.linear_model import LinearRegression
model = LinearRegression().fit(df.drop(columns='y'), df.y)
coefs = model.coef_
out = model.predict(df.drop(columns='y'))
```
Remember that when working with [[pandas]], to run a `predict`, the argument should have the same exact structure as the original dataset (it feels as if pandas was cast at numpy array at this point, but I'm not sure). So in practice it may make sense to always make sure the testing dataset is fashioned after a training dataset, or another way around. Put columns in order; add zeros for missing columns.

# PCA and similar techniques

See: [[embedding]] for a list of dimensionality reduction techniques.

```python
import sklearn.decomposition as decomposition
pca = decomposition.PCA(n_components=20)
pca.fit(X) # Where X is some sort of matrix we're trying to deconstruct
scores = pca.transform(X)
loadings = pca.components_ # These are all unit vectors
eigenvectors = pca.components_.T * np.sqrt(pca.explained_variance_) # Scaled loadings
```

Other manifold methods:

```python
import sklearn.manifold as manifold
???
???
```