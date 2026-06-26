### BBO Optimisation Model Card



#### Overview



**Model name:** Hybrid Neural Trust-Region Optimisation (HN-TRO)

**Type:** Sequential black-box optimisation framework

**Version:** Stage 10 (final iteration of 10-round capstone process)



This approach combines surrogate modelling (Gaussian Processes → Neural Networks), uncertainty estimation (Monte Carlo Dropout), and structured sampling strategies to optimise unknown 10-dimensional black-box functions.



The method evolved from heuristic local search into a hybrid model-based optimisation pipeline integrating exploration–exploitation balancing mechanisms.



#### Intended Use



Suitable tasks:

* Black-box optimisation in bounded continuous spaces
* Sequential decision-making under uncertainty
* Benchmarking surrogate-based optimisation methods
* Exploration–exploitation trade-off analysis
* High-dimensional function optimisation (up to 10D in this project setting)



Out-of-scope / not suitable:

* Real-world causal inference
* Deterministic function reconstruction
* Low-noise analytical optimisation problems (where gradient methods are available)
* Domains requiring exact global guarantees (method is probabilistic and heuristic)





#### Details (Method Evolution Across 10 Rounds)



The optimisation strategy evolved in five main phases:



**Weeks 1–3: Heuristic Local Search**

* Random perturbations around best observed points
* Strong exploitation bias
* High boundary concentration
* Limited global exploration



**Weeks 4–5: Gaussian Process + Uncertainty Sampling**

* Introduction of surrogate modelling (GP regression)
* Acquisition function: mean + uncertainty
* First structured exploration–exploitation balance



**Weeks 6–7: Hybrid GP + SVM + Neural Networks**

* SVM used for coarse classification (high vs low regions)
* Neural networks introduced for non-linear regression
* Ensemble-style decision-making
* Early signs of overfitting in sparse regions



**Weeks 8–9: Neural Surrogate + Dropout Uncertainty**

* Transition to neural network as primary model
* Monte Carlo Dropout for uncertainty estimation
* Trust-region sampling introduced
* Boundary penalty added to reduce collapse



**Week 10 - 13: Stabilised Hybrid Framework**



Final optimisation strategy:

* Neural network surrogate model
* MC Dropout uncertainty estimation
* Trust-region + uniform + interior-biased sampling
* Adaptive acquisition function:

  * predicted mean (exploitation)
  * uncertainty (exploration)
  * boundary regularisation (stability)
* Reduced boundary collapse (especially Functions 2 and 8)
* Improved coverage in high-dimensional space





#### Performance



8 independent black-box functions

* 10-dimensional input space
* Sequential iterative optimisation over 10 rounds
* Dataset size per function:

  * Initial stage: 10 observations per function
  * Final stage: 20 observations per function (Stage 10)

    * Note: Data will be updated on 15th June, currently data contains 19 observations



Metrics used:

* Best observed function value per function
* Stability across iterations (variance of improvements)
* Coverage of input space (qualitative)
* Reduction of boundary collapse (frequency of extreme values near 0 or 1)



Summary of results:

* Consistent improvement across all 8 functions over iterations
* Strongest gains observed after introduction of hybrid neural + uncertainty framework
* Early rounds showed instability and local convergence
* Final model improved exploration balance, especially in higher-dimensional functions (F7, F8)
* Remaining limitation: incomplete global coverage in 10D space due to sparse sampling



#### Assumptions and Limitations



Key assumptions:

* Black-box functions are locally smooth enough for neural surrogate approximation
* High-performing regions are partially clustered, enabling trust-region optimisation
* Monte Carlo Dropout provides a reasonable approximation of uncertainty
* Historical observations remain relevant for guiding future queries



Limitations / failure modes:

* High-dimensional sparsity (10D space) limits global accuracy
* Boundary bias can still emerge if penalty is insufficient
* Surrogate models may overfit in low-data regimes
* Uncertainty estimates are approximate and may be miscalibrated
* No formal guarantee of global optimum discovery



#### Ethical Considerations



Although this is a synthetic optimisation task, the methodology reflects principles relevant to real-world decision systems

* Transparency improves reproducibility - clearly defined acquisition rules allow others to replicate the optimisation path.
* Interpretability supports trust - breaking down decisions into mean/uncertainty/boundary components makes model behaviour explainable.
* Bias awareness - identifying boundary bias and correcting it improves robustness and avoids systematic sampling errors.
* Real-world adaptation - structured documentation allows transfer of the method to real applications such as resource allocation or experimental design.



#### Reflection on Decision-Making



The optimisation process makes decisions using a hybrid scoring mechanism combining:

* predicted performance (exploitation),
* predictive uncertainty (exploration),
* structural constraints (boundary penalty),
* and stochastic sampling for robustness.



Strengths:

* Adaptive and robust across different function behaviours
* Reduces premature convergence
* Handles uncertainty explicitly
* Scales reasonably to higher dimensions compared to GP-only methods



Limitations:

* Still sensitive to sparse data in high-dimensional spaces
* Uncertainty is model-dependent (approximation error)
* Can underexplore rare but high-value regions



