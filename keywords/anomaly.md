# Anomaly detection

Parents: [[03_Classification]]
See also:

#classification


Essentially, a binary classification problem. We assume that most of the data is produced by a certain process, but every now and then we have a point produced by a different process. We need to identify these "stray points".

Most approaches try to approximate the distribution for the "main set of data", and then see if each point is likely to come from this distribution. This can be done parametrically (e.g. with Z-scores), using Bayesian statistics, unsupervised methods such as [[clustering]], supervised classifiers, such as [[05_Ensembles]], or fancy DL approaches, such as [[autoencoder]]s or discriminators from a [[gan]] network.

# References

https://en.wikipedia.org/wiki/Anomaly_detection


