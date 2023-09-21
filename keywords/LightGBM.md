# LightGBM

Parent: [[05_Ensembles]], [[gbm]]
See also: [[attribution]]

#ensemble #trees


The most popular implementation of boosted trees as of 2023; fast and works out of the box. Allegedly, may not be an ideal method for small datasets (hundreds to thousands of points), but I'm not sure what is the rationale here.

# Typical use

```python
from scipy import stats
from sklearn.model_selection import train_test_split
from sklearn.metrics import mean_squared_error

import lightgbm as lgb

X = dfx.values # Numpy array of input values
y = dfy.values # Numpy vector of outputs
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.25, random_state=1)
lgb_train = lgb.Dataset(X_train, y_train)
lgb_eval  = lgb.Dataset(X_test, y_test, reference=lgb_train)

params = {'boosting_type': 'gbdt', 'objective': 'regression', 'verbose': 0} # Config
gbm = lgb.train(params, lgb_train,
                num_boost_round=20, valid_sets=lgb_eval,
                callbacks=[lgb.early_stopping(stopping_rounds=5)])

gbm.save_model('my_model.txt') # Save model to file
y_pred = gbm.predict(X_test, num_iteration=gbm.best_iteration) # Predict
rmse_test = mean_squared_error(y_test, y_pred) ** 0.5 # Evaluate
```

Other possible parameters (as pairs in a dict):
* `categorical_feature` - multi-int (column numbers) or string, to mark which features need to be treated as categorical (even if they look like integers)
* `metric` - supposedly can take strings 'l1' and 'l2' as values, or both (as a set)
* To regulate the size of the tree (these two would typically go together):
    * `num_leaves` - maximal number of leaves in every node. Higher number - more overfitting
    * `max_depth` - branch depth
* Alternative (or additional) way to fight overfitting; reducing information at each tree. Allegedly, for really large datasets, these values also affect comptutaional complexity 🔥.
    * `feature_fraction` - typically large, like 0.9, but reducing it should increase "averaging".
    * `bagging_fraction` - similar idea.
* `lambda_l2` and `lambda_l1` - control regularization (but seem to be 0 by default? 🔥)
* `learning_rate` - typical value of about 0.1 (?🔥)
* `bagging_freq` - ? 🔥

Categorical values need to be pre-packaged as such before they are passed to the model:
```python
categorical_columns = ['GROUP', 'REGION'] # For example
df[categorical_columns] = (
    df[categorical_columns]
    .apply(lambda x: x.astype("category").cat.codes.astype('category'))
)
```
or better yet something like this:
```python
def df_to_x(df, categorical_columns, mapping=None):
    if mapping is None:
        mapping = {}
        for cat in categorical_columns:
            temp_series = df[cat].astype('category')
            df[cat] = temp_series.cat.codes.astype('category')
            mapping[cat] = {a:b for b,a in enumerate(temp_series.cat.categories)} # Reverse mapping
    else:
        for cat in categorical_columns:
            df[cat] =dfdfx[cat].map(mapping[cat]).astype('category')
    return df.values, mapping
```

## As a linear model

Is it possible to imitate a linear model in LGBM? Not sure it is worth it, but is it possible? Is a linear model a limit case for some set of parameters? Say, would we essentially get a linear model if we set `feature_fraction` to exactly `1/n_features`? 🔥
    
* https://stats.stackexchange.com/questions/517835/do-xbgoost-and-lightgbm-only-use-trees-as-base-learners

# Shap values

See [[attribution]]

# Refs

Basics: https://en.wikipedia.org/wiki/LightGBM

Official documentation:
* https://lightgbm.readthedocs.io/en/latest/Parameters.html
* https://lightgbm.readthedocs.io/en/latest/Advanced-Topics.html

On parameters and hypertuning: 
https://neptune.ai/blog/lightgbm-parameters-guide