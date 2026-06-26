#### Black-Box Optimisation (BBO) Capstone Project



##### Project Overview



This project focuses on Black-Box Optimisation (BBO), where the goal is to maximise unknown functions using only observed input-output variables. The internal structure of these functions is hidden, so decisions must be made iteratively based on feedback from each query, received from tutor.



The challenge are similar to real-world scenarios where evaluations are expensive or limited, such as hyperparameter tuning, A/B testing, etc. Rather than exhaustive search, the emphasis is on making efficient, informed sampling decisions.



From a career perspective, this project strengthens skills in:

* optimisation under uncertainty
* iterative model improvement
* balancing exploration vs exploitation
* applying machine learning techniques under real-world constraints
* These skills are directly relevant for roles in data science, ML engineering, and decision intelligence systems.



##### Inputs and Outputs



**Inputs**



Continuous vectors of real numbers in the range \[0, 1]

Dimensionality varies per function (8 outputs given, 2D, 2D, 3D, 4D, 4D, 5D, 6D, 8D)

Each input must match the function’s dimensionality.



Example input (for 3D function):



\[0.123456, 0.654321, 0.987654]



**Outputs**



Single scalar value representing function performance

Objective: to maximise this value



Example output:



0.842367



No additional information (gradients, functional form, or noise characteristics) is provided, only input-output observations. Every week tutor send outputs of proposed inputs.



##### Challenge Objectives



The project aims to maximise each unknown function.



Key constraints:

* Strict limit on number of queries per function
* Unknown and potentially irregular function surfaces
* Sequential feedback (one new output per query)
* Different dimensionality across functions
* Success requires balancing exploration (sampling uncertain or untested regions) and exploitation (refining around high-performing regions)



The challenge is not just to find high values but to improve performance efficiently under uncertainty.



##### Technical Approach



The approach evolves across the rounds, with three rounds completed so far:



**Week 1 – heuristic Local Search**

\- identify best-performing points in observed data

\- apply small perturbations nearby (exploitation)

\- limitation - risk of getting stuck in local optima



(Code 1: simple exploit/explore strategy with 70/30 ratio)



**Week 2 – Gaussian Process (Bayesian Optimisation)**

\- introduced a Gaussian Process (GP) surrogate model

\- used acquisition functions to combine predicted mean + uncertainty

\- balance between exploration and exploitation

\- improved guidance for selecting promising regions



(Code 2: GP-based acquisition using mu + 0.1 \* sigma as scoring)



**Week 3 – Hybrid GP + SVM Approach**

\- Gaussian Process models the unknown function

\- Support Vector Machine (SVM) classifies regions as high- or low-performing

\- combined score: mu + 0.15 \* sigma + 0.15 \* prob\_good

\- prioritises areas expected to give good results, while keeping a check on model overconfidence

\- balances exploration in uncertain areas and exploitation in strong regions



(Code 3: hybrid approach integrating GP predictions and SVM probabilities)



**Week 4 – Neural Network Surrogate Model**

&#x20;- replaced GP with a neural network surrogate model for better scalability in higher dimensions (10D space)

&#x20;- used backpropagation to learn non-linear relationships between inputs and outputs

&#x20;- introduced gradient-based intuition for guiding search direction

&#x20;- added small stochastic noise to avoid boundary collapse and premature convergence

&#x20;- improved flexibility compared to GP, but reduced interpretability



(Code 4: feedforward NN with dropout + MSE loss, gradient-informed candidate selection)



**Week 5 – Neural Network + Gradient-Guided Search**

&#x20;- extended neural network approach with explicit gradient-based optimisation

&#x20;- used gradients of predicted output with respect to inputs to guide sampling

&#x20;- shifted from random exploration to directional improvement steps

&#x20;- added clipping and noise injection to prevent instability near boundaries

&#x20;- improved efficiency of search in high-dimensional regions



(Code 5: NN + gradient ascent with jitter + input clipping)



**Week 6 – Ensemble Stabilisation + Exploration Control**

&#x20;- introduced ensemble-style modelling behaviour (multiple runs / reinitialisations)

&#x20;- reduced variance in predictions through averaging across models

&#x20;- controlled exploration-exploitation trade-off more explicitly

&#x20;- reduced overly aggressive local search behaviour from previous weeks

&#x20;- improved stability across all 8 functions



(Code 6: NN ensemble averaging + probabilistic candidate selection)



**Week 7 – Hyperparameter Tuning + Random Search**

&#x20;- introduced hyperparameter optimisation for neural network

&#x20;- tuned architecture depth, learning rate, and training epochs

&#x20;- used random search due to small dataset constraints

&#x20;- improved robustness and reduced overfitting risk

&#x20;- observed that smaller models performed more consistently than larger ones



(Code 7: random search over NN configurations + validation-based selection)



**Week 8 – Trust Region + Structured Sampling**

&#x20;- introduced trust-region based sampling around elite solutions

&#x20;- combined local exploitation with controlled global exploration

&#x20;- added structured noise (jitter) to prevent collapse into narrow regions

&#x20;- observed improved stability but persistent boundary bias in some functions

&#x20;- began focusing more on sampling strategy than model complexity



(Code 8: trust-region sampling + mixed uniform/exploit ratio)



**Week 9 – MC Dropout Uncertainty + Boundary Penalty**

&#x20;- introduced Monte Carlo Dropout for uncertainty estimation

&#x20;- used uncertainty to guide exploration in under-sampled regions

&#x20;- added explicit boundary penalty to reduce extreme-value collapse (0 or 1 regions)

&#x20;- improved exploration of interior regions, especially in higher-dimensional functions

&#x20;- reduced overconfidence in surrogate predictions



(Code 9: MC dropout UCB-style acquisition + boundary penalty term)



**Week 10 – Final Hybrid Optimisation Framework**

&#x20;- final model combines neural surrogate + MC Dropout + structured sampling

&#x20;- acquisition function includes:

&#x20;- predicted mean (exploitation)

&#x20;- uncertainty (exploration)

&#x20;- boundary penalty (regularisation)

&#x20;- sampling strategy includes:

&#x20;- trust-region sampling (local refinement)

&#x20;- uniform sampling (global exploration)

&#x20;- interior-biased sampling (Beta-like behaviour)

&#x20;- significantly reduced boundary collapse (especially Functions 2 and 8)

&#x20;- achieved best overall balance between exploration and exploitation across all 8 functions



(Code 10: hybrid NN + MC dropout + mixed sampling + UCB-style scoring)



**Week 11 – Cluster-Aware Optimisation + Structured Ensemble Search**

&#x20;- introduced K-Means clustering to identify multiple high-performing regions instead of relying only on a single elite point

&#x20;- applied StandardScaler to improve numerical stability and neural network convergence

&#x20;- shifted from single-mode optimisation to multi-region exploitation using clustered elite samples

&#x20;- candidate generation performed around cluster centres with controlled variance (cluster-specific local search)

&#x20;- scoring influenced by cluster structure, encouraging exploration of under-explored high-performing regions

&#x20;- improved robustness on multimodal functions with multiple local optima

&#x20;- reduced premature convergence and improved diversity of explored solutions



(Code 11: KMeans clustering + StandardScaler + cluster-based candidate generation + surrogate scoring)



**Week 12 –  PCA-Guided Search + Directional Exploration**



&#x20;- introduced PCA to extract dominant directions of variation in elite solutions

&#x20;- used principal components to guide sampling in lower-dimensional meaningful subspaces

&#x20;- combined PCA-driven exploration with local perturbations and uniform sampling

&#x20;- reduced isotropic randomness by introducing structured directional bias

&#x20;- improved efficiency in higher-dimensional problems by focusing search on principal directions

&#x20;- enhanced convergence stability by aligning sampling with data-driven geometry of the search space

&#x20;- improved performance in cases where objective structure lies in low-dimensional manifolds

(Code 12: PCA-based sampling + directional perturbations + mixed exploration strategy)



**Week 13 –  Bandit-Inspired Adaptive Sampling + Final Unified Strategy**



&#x20;- introduced bandit-style allocation of different sampling strategies within a single framework

&#x20;- combined multiple candidate generation methods: trust-region sampling, uniform sampling, and beta-distributed interior sampling

&#x20;- replaced fixed sampling ratios with adaptive strategy selection

&#x20;- used MC Dropout uncertainty to improve robustness of surrogate predictions

&#x20;- final scoring combined mean prediction, uncertainty, and structured exploration effects

&#x20;- achieved highest stability across all benchmark functions by avoiding dependence on a single search heuristic

&#x20;- resulted in a fully adaptive optimisation framework combining surrogate modelling, uncertainty estimation, and multi-strategy search

(Code 13: bandit-style multi-sampler + MC Dropout + hybrid UCB scoring + adaptive candidate generation)


#### Exploration vs Exploitation Strategy


The strategy balances exploitation, exploration, and adaptation. Exploitation focuses on refining areas predicted to perform well, while exploration checks untested regions to prevent early convergence. The strategy adapts based on model uncertainty and observed performance trends.



The main strengths of this approach are that it combines model-based reasoning with human intuition, uses both the initial and newly collected data, and adapts its strategy for each function instead of following a one-size-fits-all rule. It carefully balances exploring new areas and exploiting known high-performing regions, creating a flexible framework where heuristics, Bayesian models, and classification techniques can evolve as more data comes in.



Model Card: https://github.com/j-wasz-dev/ml\_ai\_project/blob/main/MODEL\_CARD.md



Datasheet: https://github.com/j-wasz-dev/ml\_ai\_project/blob/main/DATASHEET.md

