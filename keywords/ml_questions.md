# ML interview questions

#lifehack #interview

1. Know what a p-value is and its limitations in decisions.
2. Linear regression and its assumptions.
3. When to use different statistical distributions.
4. How an effect size impacts results/decisions.
5. Mean, variance for Normal, Uniform, Poisson.
6. Sampling techniques and common designs (e.g. A/B).
7. Bayes' theorem (applied calculations).
8. Common conjugate priors (Bayesian stats).
9. Logistic regression and ROC curves.
10. Resampling (Cross validation + bootstrapping).
11. Dimensionality reduction.
12. Tree-based models (particularly how to prune)
13. Ridge and Lasso for regression.

https://www.linkedin.com/posts/eric-weber-060397b7_data-datascience-statistics-activity-6690747775702888448-9Pnq/

What is supervised machine learning? 
What is regression? Which models can you use to solve a regression problem? 
What is linear regression? When do we use it? 
What’s the normal distribution? Why do we care about it? 
Which metrics for evaluating regression models do you know? 
What are MSE and RMSE? 
What is overfitting? 
How to do you validate your models? 
Why do we need to split our data into three parts: train, validation, and test? 
Can you explain how cross-validation works? 
What is K-fold cross-validation? 
How do we choose K in K-fold cross-validation? What’s your favourite K? 
What is classification? Which models would you use to solve a classification problem? 
What is logistic regression? When do we need to use it? 
What is regularization? Why do we need it? 
Is logistic regression a linear model? Why? 
What is sigmoid? What does it do? 
How do we evaluate classification models? 
What is accuracy? 
Is accuracy always a good metric? 
What is the confusion table? What are the cells in this table? 
What is precision, recall, and F1-score? 
What are the main parameters of the decision tree model? 
What is random forest? 
How do we select the right regularization parameters? 
What is feature selection? Why do we need it? 
What are the decision trees? 
How do we check if a variable follows the normal distribution? 
What if we want to build a model for predicting prices? Are prices distributed normally? Do we need to do any pre-processing for prices? 
What are the methods for solving linear regression do you know? 
What is gradient descent? How does it work? 
What is the normal equation? 
What is SGD - stochastic gradient descent? What’s the difference with the usual gradient descent? 
What happens to our linear regression model if we have three columns in our data: x, y, z - and z is a sum of x and y? 
What happens to our linear regression model if the column z in the data is a sum of columns x and y and some random noise? 
Which regularization techniques do you know? 
Precision-recall trade-off 
What is the ROC curve? When to use it? 
What is AUC (AU ROC)? When to use it? 
How to interpret the AU ROC score? 
What is the PR (precision-recall) curve? 
What is the area under the PR curve? Is it a useful metric? 
In which cases AU PR is better than AU ROC? 
What do we do with categorical variables? 
Why do we need one-hot encoding? 
What kind of regularization techniques are applicable to linear models? 
How does L2 regularization look like in a linear model? 
What’s the effect of L2 regularization on the weights of a linear model? 
How L1 regularization looks like in a linear model? 
What’s the difference between L2 and L1 regularization? 
Can we have both L1 and L2 regularization components in a linear model? 
What’s the interpretation of the bias term in linear models? 
How do we interpret weights in linear models? 
If a weight for one variable is higher than for another - can we say that this variable is more important? 
When do we need to perform feature normalization for linear models? When it’s okay not to do it? 
Is feature selection important for linear models? 
Which feature selection techniques do you know? 
Can we use L1 regularization for feature selection? 
Can we use L2 regularization for feature selection? 
How do we train decision trees? 
How do we handle categorical variables in decision trees? 
What are the benefits of a single decision tree compared to more complex models? 
How can we know which features are more important for the decision tree model? 
Why do we need randomization in random forest? 
What are the main parameters of the random forest model? 
How do we select the depth of the trees in random forest? 
How do we know how many trees we need in random forest? 
Is it easy to parallelize training of random forest? How can we do it? 
What are the potential problems with many large trees?
What if instead of finding the best split, we randomly select a few splits and just select the best from them. Will it work?

How will you implement dropout during forward and backward pass?
What do you do if Neural network training loss/testing loss stays constant?
Why do RNNs have a tendency to suffer from exploding/vanishing gradient? How to prevent this? (Talk about LSTM cell which helps the gradient from vanishing, but make sure you know why it does so. Talk about gradient clipping, and discuss whether to clip the gradient element wise, or clip the norm of the gradient.)
Do you know GAN, VAE, and memory augmented neural network? Can you talk about it?
Does using full batch means that the convergence is always better given unlimited power?
What is the problem with sigmoid during backpropagation?
Given a black box machine learning algorithm that you can’t modify, how could you improve its error? (you can transform the input for example.)
How to find the best hyper parameters? (Random search, grid search, Bayesian search (and what it is?))
What is transfer learning?
Compare and contrast L1-loss vs. L2-loss and L1-regularization vs. L2-regularization.
Can you state Tom Mitchell's definition of learning and discuss T, P and E?
What can be different types of tasks encountered in Machine Learning?
What are supervised, unsupervised, semi-supervised, self-supervised, multi-instance learning, and reinforcement learning?
Loosely how can supervised learning be converted into unsupervised learning and vice-versa?
Consider linear regression. What are T, P and E?
Derive the normal equation for linear regression.
What do you mean by affine transformation? Discuss affine vs. linear transformation.
Discuss training error, test error, generalization error, overfitting, and underfitting.
Compare representational capacity vs. effective capacity of a model.
Discuss VC dimension.
What are nonparametric models? What is nonparametric learning?
What is an ideal model? What is Bayes error? What is/are the source(s) of Bayes error occur?
What is the no free lunch theorem in connection to Machine Learning?
What is regularization? Intuitively, what does regularization do during the optimization procedure? (expresses preferences to certain solutions, implicitly and explicitly)
What is weight decay? What is it added?
What is a hyperparameter? How do you choose which settings are going to be hyperparameters and which are going to be learnt? (either difficult to optimize or not appropriate to learn - learning model capacity by learning the degree of a polynomial or coefficient of the weight decay term always results in choosing the largest capacity until it overfits on the training set)
Why is a validation set necessary?
What are the different types of cross-validation? When do you use which one?
What are point estimation and function estimation in the context of Machine Learning? What is the relation between them?
What is the maximal likelihood of a parameter vector $theta$? Where does the log come from?
Prove that for linear regression MSE can be derived from maximal likelihood by proper assumptions.
Why is maximal likelihood the preferred estimator in ML? (consistency and efficiency)
Under what conditions do the maximal likelihood estimator guarantee consistency?
What is cross-entropy of loss? (trick question)
What is the difference between an optimization problem and a Machine Learning problem?
How can a learning problem be converted into an optimization problem?
What is empirical risk minimization? Why the term empirical? Why do we rarely use it in the context of deep learning?
Name some typical loss functions used for regression. Compare and contrast. (L2-loss, L1-loss, and Huber loss)
What is the 0-1 loss function? Why can't the 0-1 loss function or classification error be used as a loss function for optimizing a deep neural network?
Write the equation describing a dynamical system. Can you unfold it? Now, can you use this to describe a RNN? (include hidden, input, output, etc.)
What determines the size of an unfolded graph?
What are the advantages of an unfolded graph? (arbitrary sequence length, parameter sharing, and illustrate information flow during forward and backward pass)
What does the output of the hidden layer of a RNN at any arbitrary time t represent?
Are the output of hidden layers of RNNs lossless? If not, why?
RNNs are used for various tasks. From a RNNs point of view, what tasks are more demanding than others?
Discuss some examples of important design patterns of classical RNNs.
Write the equations for a classical RNN where hidden layer has recurrence. How would you define the loss in this case? What problems you might face while training it? (Discuss runtime)
What is backpropagation through time? (BPTT)
Consider a RNN that has only output to hidden layer recurrence. What are its advantages or disadvantages compared to a RNNhaving only hidden to hidden recurrence?
What is Teacher forcing? Compare and contrast with BPTT.
What is the disadvantage of using a strict teacher forcing technique? How to solve this?
Explain the vanishing/exploding gradient phenomenon for recurrent neural networks. (use scalar and vector input scenarios)
Why don't we see the vanishing/exploding gradient phenomenon in feedforward networks? (weights are different in different layers - Random block intialization paper)
What is the key difference in architecture of LSTMs/GRUs compared to traditional RNNs? (Additive update instead of multiplicative)
What is the difference between LSTM and GRU?
Explain Gradient Clipping.
Adam and RMSProp adjust the size of gradients based on previously seen gradients. Do they inherently perform gradient clipping? If no, why?
Discuss RNNs in the context of Bayesian Machine Learning.
Can we do Batch Normalization in RNNs? If not, what is the alternative? (BNorm would need future data; Layer Norm)
What is an Autoencoder? What does it "auto-encode"?
What were Autoencoders traditionally used for? Why there has been a resurgence of Autoencoders for generative modeling?
What is recirculation?
What loss functions are used for Autoencoders?
What is a linear autoencoder? Can it be optimal (lowest training reconstruction error)? If yes, under what conditions?
What is the difference between Autoencoders and PCA?
What is the impact of the size of the hidden layer in Autoencoders?
What is an undercomplete Autoencoder? Why is it typically used for?
What is a linear Autoencoder? Discuss it's equivalence with PCA. (only valid for undercomplete) Which one is better in reconstruction?
What problems might a nonlinear undercomplete Autoencoder face?
What are overcomplete Autoencoders? What problems might they face? Does the scenario change for linear overcomplete autoencoders? (identity function)
Discuss the importance of regularization in the context of Autoencoders.
Why does generative autoencoders not require regularization?
What are sparse autoencoders?
What is a denoising autoencoder? What are its advantages? How does it solve the overcomplete problem?
What is score matching? Discuss it's connections to DAEs.
Are there any connections between Autoencoders and RBMs?
What is manifold learning? How are denoising and contractive autoencoders equipped to do manifold learning?
What is a contractive autoencoder? Discuss its advantages. How does it solve the overcomplete problem?
Why is a contractive autoencoder named so? (intuitive and mathematical)
What are the practical issues with CAEs? How to tackle them?
What is a stacked autoencoder? What is a deep autoencoder? Compare and contrast.
Compare the reconstruction quality of a deep autoencoder vs. PCA.
What is predictive sparse decomposition?
Discuss some applications of Autoencoders.
What is representation learning? Why is it useful? (for a particular architecture, for other tasks, etc.)
What is the relation between Representation Learning and Deep Learning?
What is one-shot and zero-shot learning (Google's NMT)? Give examples.
What trade offs does representation learning have to consider?
What is greedy layer-wise unsupervised pretraining (GLUP)? Why greedy? Why layer-wise? Why unsupervised? Why pretraining?
What were/are the purposes of the above technique? (deep learning problem and initialization)
Why does unsupervised pretraining work?
When does unsupervised training work? Under which circumstances?
Why might unsupervised pretraining act as a regularizer?
What is the disadvantage of unsupervised pretraining compared to other forms of unsupervised learning?
How do you control the regularizing effect of unsupervised pretraining?
How to select the hyperparameters of each stage of GLUP?
What are deterministic algorithms? (nothing random)
What are Las vegas algorithms? (exact or no solution, random resources)
What are deterministic approximate algorithms? (solution is not exact but the error is known)
What are Monte Carlo algorithms? (approximate solution with random error)
Discuss state-of-the-art attack and defense techniques for adversarial models.
Describe bias and variance with examples.
What is Empirical Risk Minimization?
What is Union bound and Hoeffding's inequality?
Write the formulae for training error and generalization error. Point out the differences.
State the uniform convergence theorem and derive it.
What is sample complexity bound of uniform convergence theorem?
What is error bound of uniform convergence theorem?
What is the bias-variance trade-off theorem?
From the bias-variance trade-off, can you derive the bound on training set size?
What is the VC dimension?
What does the training set size depend on for a finite and infinite hypothesis set? Compare and contrast.
What is the VC dimension for an n-dimensional linear classifier?
How is the VC dimension of a SVM bounded although it is projected to an infinite dimension?
Considering that Empirical Risk Minimization is a NP-hard problem, how does logistic regression and SVM loss work?
Model and feature selection
Why are model selection methods needed?
How do you do a trade-off between bias and variance?
What are the different attributes that can be selected by model selection methods?
Why is cross-validation required?
Describe different cross-validation techniques.
What is hold-out cross validation? What are its advantages and disadvantages?
What is k-fold cross validation? What are its advantages and disadvantages?
What is leave-one-out cross validation? What are its advantages and disadvantages?
Why is feature selection required?
Describe some feature selection methods.
What is forward feature selection method? What are its advantages and disadvantages?
What is backward feature selection method? What are its advantages and disadvantages?
What is filter feature selection method and describe two of them?
What is mutual information and KL divergence?
Describe KL divergence intuitively.
Curse of dimensionality
Describe the curse of dimensionality with examples.
What is local constancy or smoothness prior or regularization?
Universal approximation of neural networks
State the universal approximation theorem? What is the technique used to prove that?
What is a Borel measurable function?
Given the universal approximation theorem, why can't a MLP still reach a arbitrarily small positive error?
Deep Learning motivation
What is the mathematical motivation of Deep Learning as opposed to standard Machine Learning techniques?
In standard Machine Learning vs. Deep Learning, how is the order of number of samples related to the order of regions that can be recognized in the function space?
What are the reasons for choosing a deep model as opposed to shallow model? (1. Number of regions O(2^k) vs O(k) where k is the number of training examples 2. # linear regions carved out in the function space depends exponentially on the depth. )
How Deep Learning tackles the curse of dimensionality?
How can the SVM optimization function be derived from the logistic regression optimization function?
What is a large margin classifier?
Why SVM is an example of a large margin classifier?
SVM being a large margin classifier, is it influenced by outliers? (Yes, if C is large, otherwise not)
What is the role of C in SVM?
In SVM, what is the angle between the decision boundary and theta?
What is the mathematical intuition of a large margin classifier?
What is a kernel in SVM? Why do we use kernels in SVM?
What is a similarity function in SVM? Why it is named so?
How are the landmarks initially chosen in an SVM? How many and where?
Can we apply the kernel trick to logistic regression? Why is it not used in practice then?
What is the difference between logistic regression and SVM without a kernel? (Only in implementation – one is much more efficient and has good optimization packages)
How does the SVM parameter C affect the bias/variance trade off? (Remember C = 1/lambda; lambda increases means variance decreases)
How does the SVM kernel parameter sigma^2 affect the bias/variance trade off?
Can any similarity function be used for SVM? (No, have to satisfy Mercer’s theorem)
Logistic regression vs. SVMs: When to use which one? ( Let's say n and m are the number of features and training samples respectively. If n is large relative to m use log. Reg. or SVM with linear kernel, If n is small and m is intermediate, SVM with Gaussian kernel, If n is small and m is massive, Create or add more fetaures then use log. Reg. or SVM without a kernel)
What are the differences between “Bayesian” and “Freqentist” approach for Machine Learning?
Compare and contrast maximum likelihood and maximum a posteriori estimation.
How does Bayesian methods do automatic feature selection?
What do you mean by Bayesian regularization?
When will you use Bayesian methods instead of Frequentist methods? (Small dataset, large feature set)
Regularization
What is L1 regularization?
What is L2 regularization?
Compare L1 and L2 regularization.
Why does L1 regularization result in sparse models? here
Evaluation of Machine Learning systems
What are accuracy, sensitivity, specificity, ROC?
What are precision and recall?
Describe t-test in the context of Machine Learning.
Describe the k-means algorithm.
What is distortion function? Is it convex or non-convex?
Tell me about the convergence of the distortion function.
What is the Gaussian Mixture Model?
Describe the EM algorithm intuitively.
What are the two steps of the EM algorithm
Compare GMM vs GDA.
Why do we need dimensionality reduction techniques?
What do we need PCA and what does it do?
What is the difference between logistic regression and PCA?
What are the two pre-processing steps that should be applied before doing PCA?
What is WORD2VEC?
What is t-SNE? Why do we use PCA instead of t-SNE?
What is sampled softmax?
Why is it difficult to train a RNN with SGD?
How do you tackle the problem of exploding gradients? (By gradient clipping)
What is the problem of vanishing gradients for RNNs? How to tackle it?
Explain the memory cell of a LSTM.
What type of regularization do one use in LSTM?
What is Beam Search?
How to automatically caption an image?
What is the difference between loss function, cost function and objective function?

# Math

What is broadcasting in connection to Linear Algebra?
What are scalars, vectors, matrices, and tensors?
What is Hadamard product of two matrices?
What is an inverse matrix?
If inverse of a matrix exists, how to calculate it?
What is the determinant of a square matrix? How is it calculated (Laplace expansion)? What is the connection of determinant to eigenvalues?
Discuss span and linear dependence.
What is Ax = b? When does Ax =b has a unique solution?
In Ax = b, what happens when A is fat or tall?
When does inverse of A exist?
What is a norm? What is L1, L2 and L infinity norm?
What are the conditions a norm has to satisfy?
Why is squared of L2 norm preferred in ML than just L2 norm?
When L1 norm is preferred over L2 norm?
Can the number of nonzero elements in a vector be defined as L0 norm? If no, why?
What is Frobenius norm?
What is a diagonal matrix? (D_i,j = 0 for i != 0)
Why is multiplication by diagonal matrix computationally cheap? How is the multiplication different for square vs. non-square diagonal matrix?
At what conditions does the inverse of a diagonal matrix exist? (square and all diagonal elements non-zero)
What is a symmetrix matrix? (same as its transpose)
What is a unit vector?
When are two vectors x and y orthogonal? (x.T * y = 0)
At R^n what is the maximum possible number of orthogonal vectors with non-zero norm?
When are two vectors x and y orthonormal? (x.T * y = 0 and both have unit norm)
What is an orthogonal matrix? Why is computationally preferred? (a square matrix whose rows are mutually orthonormal and columns are mutually orthonormal.)
What is eigendecomposition, eigenvectors and eigenvalues?
How to find eigen values of a matrix?
Write the eigendecomposition formula for a matrix. If the matrix is real symmetric, how will this change?
Is the eigendecomposition guaranteed to be unique? If not, then how do we represent it?
What are positive definite, negative definite, positive semi definite and negative semi definite matrices?
What is SVD? Why do we use it? Why not just use ED?
Given a matrix A, how will you calculate its SVD?
What are singular values, left singulars and right singulars?
What is the connection of SVD of A with functions of A?
Why are singular values always non-negative?
What is the Moore Penrose pseudo inverse and how to calculate it?
If we do Moore Penrose pseudo inverse on Ax = b, what solution is provided is A is fat? Moreover, what solution is provided if A is tall?
Which matrices can be decomposed by ED? (Any NxN square matrix with N linearly independent eigenvectors)
Which matrices can be decomposed by SVD? (Any matrix; V is either conjugate transpose or normal transpose depending on whether A is complex or real)
What is the trace of a matrix?
How to write Frobenius norm of a matrix A in terms of trace?
Why is trace of a multiplication of matrices invariant to cyclic permutations?
What is the trace of a scalar?
Write the frobenius norm of a matrix in terms of trace?
What is underflow and overflow?
How to tackle the problem of underflow or overflow for softmax function or log softmax function?
What is poor conditioning?
What is the condition number?
What are grad, div and curl?
What are critical or stationary points in multi-dimensions?
Why should you do gradient descent when you want to minimize a function?
What is line search?
What is hill climbing?
What is a Jacobian matrix?
What is curvature?
What is a Hessian matrix?
Compare "Frequentist probability" to "Bayesian probability".
What is a random variable?
What is a probability distribution?
What is a probability mass function?
What is a probability density function?
What is a joint probability distribution?
What are the conditions for a function to be a probability mass function?
What are the conditions for a function to be a probability density function?
What is a marginal probability? Given the joint probability function, how will you calculate it?
What is conditional probability? Given the joint probability function, how will you calculate it?
State the Chain rule of conditional probabilities.
What are the conditions for independence and conditional independence of two random variables?
What are expectation, variance and covariance?
Compare covariance and independence.
What is the covariance for a vector of random variables?
What is a Bernoulli distribution? Calculate the expectation and variance of a random variable that follows Bernoulli distribution?
What is a multinoulli distribution?
What is a normal distribution?
Why is the normal distribution a default choice for a prior over a set of real numbers?
What is the central limit theorem?
What are exponential and Laplace distribution?
What are Dirac distribution and Empirical distribution?
What is mixture of distributions?
Name two common examples of mixture of distributions? (Empirical and Gaussian Mixture)
Is Gaussian mixture model a universal approximator of densities?
Write the formulae for logistic and softplus function.
Write the formulae for Bayes rule.
What do you mean by measure zero and almost everywhere?
If two random variables are related in a deterministic way, how are the PDFs related?
Define self-information. What are its units?
What are Shannon entropy and differential entropy?
What is Kullback-Leibler (KL) divergence?
Can KL divergence be used as a distance measure?
Define cross-entropy.
What are structured probabilistic models or graphical models?
In the context of structured probabilistic models, what are directed and undirected models? How are they represented? What are cliques in undirected structured probabilistic models?
What is population mean and sample mean?
What is population standard deviation and sample standard deviation?
Why population s.d. has N degrees of freedom while sample s.d. has N-1 degrees of freedom? In other words, why 1/N inside root for pop. s.d. and 1/(N-1) inside root for sample s.d.?
What is the formula for calculating the s.d. of the sample mean?
What is confidence interval?
What is standard error?

What is the difference between supervised and unsupervised Machine Learning
What is bias, variance trade-off?
What is exploding gradients?
What is a confusion matrix?
Explain how a ROC curve works.
What is selection bias?
Explain SVM machine learning algorithm in detail.
What are support vectors in SVM?
What are the different kernel functions in SVM?
Explain decision tree algorithm in detail.
What is Entropy and Information gain in a Decision tree algorithm?
What is pruning in a decision tree?
What is Ensemble learning?
What is random forest? How does it work?
What cross-validation technique would you use on a time series data set?
What is logistic regression? Or State an example when you have used logistic regression recently.
What do you understand by the term Normal Distribution?
What is a Box-Cox Transformation?
How will you define the number of clusters in a clustering algorithm?
What is deep learning?
What are Recurrent Neural Networks(RNNs)?
What is the difference between Machine Learning and Deep Learning?
What is reinforcement learning?
What is selection bias?
Explain what regularisation is and why is it useful
What is TF/IDF vectorization?
What are recommender systems?
What is the difference between regression and classification of ML techniques?

Sources:
* https://twitter.com/Al_Grigor via [this summary](https://www.reddit.com/r/datascience/comments/f7cdwg/data_science_and_machine_learning_interview/)
* https://github.com/Sroy20/machine-learning-interview-questions