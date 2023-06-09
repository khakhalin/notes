# Anomaly detection

Parents: [[03_Classification]]
See also: [[time-series]], [[augmentation]], [[embedding]]

#classification


Essentially, a binary classification problem. We assume that most of the data is produced by a certain process, but every now and then we have a point produced by a different process. We need to identify these "stray points" (outliers, aka "surprise"). But then on top of this binary classifier we may of course be intrested in a probability score; multi-class classification labels, etc.

Most approaches try to approximate the distribution for the "main set of data", and then see if each point is likely to come from this distribution. This can be done parametrically (e.g. with Z-scores), using Bayesian statistics, unsupervised methods such as [[clustering]], supervised classifiers, such as [[05_Ensembles]], or fancy semi-supervised approaches, such as [[autoencoder]]s or discriminators from a [[gan]] network.

Examples: fraud detection (banks, credit cards, phones etc.), attack detection (in cybersecurity), insider training, novelty detection (a closely related concept, important for some forms of self-supervised learning), public health anomalies, industrial damage and fault detection, data validation etc.

Some practical complications:
* Behavior drift. The distribution of normal behavior often changes, evolves over time
* Typically, the data is unlabeled, so supervised techniques may be tricky, as labeleing ooften requires human intervention, and thus is very expensive.
* Individual variables may be distributed very differently (continuous, categorical, etc.).
* Contextual anomalies: Often it's not the value (vector) itself that is sketchy, but rather the value in the context (time, spatial). [[time-series]] contextual anomalies are the most common, and both neighboring values of the signal, and values of other signals for this period of time provide the context. (Example: a sudden increase in credit card spending that is normal on Xmas, but maybe not outside)
* Collective anomalies: when the values themsevelves are ok and changing smoothly, but a collection of points together is weird (say, a cardiogram missing a beat, or a pathological sequence of events in a stream of events).

Tips and tricks:
* Semi-supervised learning, including synthetic data and data [[augmentation]]
* For a mix of continuous and categorical values, we may either try an [[embedding]] (see below), or switch to a pairwise distance representation (using Manhatten distance for example), and then either project from distances to a low-D space, or to keep working with distances (see KNN-like methods below)
* Instead of explicitly modeling a distribution, we can train an [[autoencoder]] to learn an [[embedding]] for valid states, and then either classifying in the space of this embedding, or directly relying on the encoding-decoding error for anomalous points, under an assumption that anomalous points cannot be represented by this network as well as valid points. (This approach also appears to be called "replicator networks")
    * Back in the 1990s people tried to even use Kolmgorov komplexity for sequences of events with the same motivation (that "legal" sequences have some structure, and so can be zipped to smaller files! See Chandola2009 for refs)
* As for anomaly detection not only the values of valid learning points, but also their concentration is important, one can do something like a reversed [[knn]] method: to look at the number of good valid points in the immediate vicinity of the point of interest, or the size of the vicinity (hypersfhere) necessary to catch a certain number (share) of training points. This group of approaches used to be called "Peer group analysis", "relative density" approach, or "Local Outlier Factor".
* Partitions and kernels: instead of defining areas as a decision boundary or in a KNN way, optimize "safe space" description by placing some archetypical values in it, and looking at the distance to the closest archetype. Reduces the problem to a bunch of local classifications for the vicinity of each archetype (here "archetype" is my word, not something I read; ppl in the 2000s seem to have mostly called it "parititions")
* A very simplified variant of partitioning is just calculating probabilities on a grid (hypergrid)
* In early 2000s it was trendy to play with manually constructed metrics, like Connectivity-based Outlier Factor (COF), Outlier Detecton using In-Degree (ODIN), Multi-Granuality Deviation (MDEF), Cluster-Based Local Outlier Factor (CBLOF) etc, but I would expect these to become less valuable today
* For weird data, the key is either in creating an [[embedding]], or in starting from distances. Say, for protein sequences, people used some "Probabilistic Suffix Trees"

# References

https://en.wikipedia.org/wiki/Anomaly_detection

Chandola, V., Banerjee, A., & Kumar, V. (2009). Anomaly detection: A survey. ACM computing surveys (CSUR), 41(3), 1-58. https://conservancy.umn.edu/bitstream/handle/11299/215731/07-017.pdf?sequence=1
The most cited review paper ever. Provides an interesting (but of course dated) summary of domain-specific tricks and preferences.



