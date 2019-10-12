
# Hierarchical Model
We have assumed that all the observations were independent so far, but there is often a natural grouping to our data points which leads us to believe that some observation pairs should be more similar to each other than to others. For example, let's say a company plan to sample 150 test products for quality check, but they do 30 from your location and then 30 from 4 other factory locations(30 from each of 5 locations). We might expect a product from your location to be more similar to another product from your batch than to a product from another locations' batch. And we might be able to account for the likely differences between products in our Poisson model by making it a hierarchical model. That is, your lambda is not only estimated directly from your 30 products, but also indirectly from the other 120 products leveraging this hierarchical structure. Being able to account for relationships in the data while estimating everything with the single model is a primary advantage of using hierarchical models. And most Bayesian Models are hierarchical. 
<img src="https://user-images.githubusercontent.com/31917400/48874302-7ff9af00-edea-11e8-835e-ff0b7ff2f098.jpg" />

How we might use hierarchical modeling to extend a linear model? 
<img src="https://user-images.githubusercontent.com/31917400/48876484-3911b680-edf6-11e8-892b-ec6e8ed8b284.jpg" />

> Application
 - **Mixture models** provide a nice way to build nonstandard probability distributions from simper distributions, as well as to identify unlabeled clusters/populations in the data. Mixture models can be formulated **hierarchically**, allowing us to estimate unobserved(latent) variables in a technique called `data augmentation`.
 - **GLM** generalize normal linear regression models in the sense that the likelihood belongs to a more general class of distributions. `Data augmentation` techniques similar to those used for mixture models make GLMs amenable to **hierarchical modeling**. 
 - Observations from distinct **spatial locations** can exhibit dependence(just as observations collected across time are often correlated) For example, we might expect a measurement at location x to be more similar to `measurement y` 5 meters away than to `measurement z` 100 meters away. `State-space models` and `Non-parametric models` for response surfaces are common for spatial data. 
 - **DeepLearning models** involve layers of “neurons” that separate inputs from outputs, allowing nonlinear relationships. These **intermediate nodes** can be thought of as `latent variables` in a **hierarchical probabilistic model**, although Bayesian inference of neural networks is uncommon. 
 - **Bayesian Non-parametric models** move beyond inference for `parameters` to inference for `functions and distributions`. Finite-dimensional representations of the necessary priors often appear as **hierarchical models**. 2 of the most popular non-parametric priors are the `Gaussian process prior`(typically used as a prior on continuous functions), and `Dirichlet process prior`(as a prior on probability distributions). 








# Latent Variable Model
### What is Latent Variable?
Latent variable is just a random variable which is unobservable to you nor in training nor in test phase. This is the variable you can't just measure with some quantitative scale. 
<img src="https://user-images.githubusercontent.com/31917400/48974117-ebd85380-f046-11e8-913b-f788ec6bf63f.jpg" />

> __[Note]:__  
 - Latent variable can make things **easily interpretable** as we, for example, can estimate his intelligence on the scale from one to 100 although you don't know what the "scale" means. However, you can compare each data point according to this scale. Training latent variable model relies on a lot math.
 - Sometimes adding latent variables **restrict(overly simplify) your model** too much. If, for example, a student is doing 2 tests in the same day, it doesn’t make sense to assume that these two grades are caused only by his intelligence and doesn’t influence each other directly. Even if we know that he is very smart, if he failed the first test he is more likely to fail the second one because he may have a headache or maybe he didn’t have time to prepare the day before.

## Latent Variable Modeling_01> Probabilistic Clustering with GMM
__Hard/Soft Clustering:__ Usually clustering is done in a hard way, so for each data point we assign its **membership**. But sometimes, people do soft clustering. So instead of assigning each data point a particular membership, we will assign each data point from **probability** distributions over clusters(for example, 40% probability to belong to the A_cluster, and 60% probability to belong to the B_cluster, and 0% to the C_cluster). That is, instead of just assigning each data point a particular cluster, we assume that each data point belongs to every cluster, but with some **different probabilities**: `P(cluster_idx|x)` instead of `cluster_idx = f(x)`. Whyyyyy?  
 - 1. For handling **missing data**
 - 2. For tunning **hyperparameters**(meta-parameters...it can be the No.of clusters of course)
 <img src="https://user-images.githubusercontent.com/31917400/51439273-b69b5b00-1caf-11e9-99ee-a39f00c652bc.jpg" /> 
 
 - 3. For building a **generative model** of our data.. treating everything probabilistically, we may sample new data points from our model of the data.

### A. Gaussian Mixture Model: Soft Clustering with EM-Algorithm
__General form of EM-Algorithm:__ This is an algorithm for obtaining MLE/MAP of θ **`when some data is missing`**(or data is latent variables). When the likelihood(data) is a member of Exponential family, it is particularly applicable. For example, HMM(Hidden Markov) for ?, or GMM(Gaussian Mixture) for clustering? so it's a point estimate at the end? 
 - Setting:
   - Given `X` = {x1,x2,...xn} 
   - Given `Z` is **latent** variable(never observed)
   - Joint `X, Z` likelihood ~ some distribution(exponential family) with unknown θ
 - Problem: <img src="https://user-images.githubusercontent.com/31917400/66665785-25a64e80-ec47-11e9-91c2-924afe6ede25.jpg" />
 
   - But the issue here is that maximizing the marginal of X is difficult coz the joint distribution can be "multimodal"!!! 
 - EM can address this by iteratively improving our parameter estimate.
     - 1> Initialize θ_knot 
       - give the initial values...whatever..
     - 2> For t=0,1,2,... **E-Step** 
       - **Get the Q-function** by a) computing the **Conditional Expectation** of the log(joint distribution) under current θ and from simply feeding current data, and b) estimating next parameters. 
       
       a)
       <img src="https://user-images.githubusercontent.com/31917400/66671143-542a2680-ec53-11e9-886e-87dbaedf6adf.jpg" />
       
       b) for example..
       <img src="https://user-images.githubusercontent.com/31917400/66701345-45934c00-ecf3-11e9-87d6-3c21f9e088f0.jpg" />
       
     - 3> For t=... don't stop until convergence... **M-Step**
       - **Get the maximizer** from maximizing the Q-function and update... (but sometimes you cannot get the maximizer..)  
       <img src="https://user-images.githubusercontent.com/31917400/66701362-89865100-ecf3-11e9-946b-de8c491c782c.jpg" />

__Example:__ Wow, several datasets were hacked and mixed up..How to retrieve the originals?
 - [Assumption]: Each cluster follows a certain statistical distribution











### B. Gaussian Mixture Model: Soft Clustering with Latent Variable
 - **How to fit it?**(How to find the model parameters?) 
   - The simplest way to fit a probability distribution is to use **maximum likelihood**. Find the parameters maximizing the likelihood(density)! 
 <img src="https://user-images.githubusercontent.com/31917400/51492177-c3e84080-1da8-11e9-8386-e1ce3e4eb595.jpg" /> 

 - **What if we introduce a latent variable?**
<img src="https://user-images.githubusercontent.com/31917400/51533344-71576480-1e3a-11e9-8570-c0128a7cc197.jpg" /> 

[Note]: 
 - When choosing the best run (highest training log-likelihood) among several training attempts with different random initializations, we can suffer from **local maxima**. and its solution would be **NP-Hard**. 
 - We use EM-Algorithm to fit the distribution(such as Gaussian) to the **multi-dimensional** data(estimating the mean vector `μ_` and the covariance matrix `σ_`), not to the one-dimensional data. 
 - We can treat `missing values` as **latent variables** and still estimate the Gaussian parameters. But with one-dimensional data, if a data point has missing values, it means that we don’t know anything about it (its only dimension is missing), so the only thing that is left is to throw away points with missing data.
 - Note that we also don’t need EM to estimate the mean vector (e.i. we need it only for the covariance matrix) in the multi-dimensional case: since each coordinate of the mean vector can be treated independently, we can treat each coordinate as one-dimensional case and just throw away missing values. 

























