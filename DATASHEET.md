### Datasheet – BBO Capstone Project Dataset 



#### 1\. Motivation



This dataset was created as part of a black-box Bayesian optimisation (BBO) capstone project. The main purpose is to iteratively explore and optimise eight unknown objective functions under limited query budgets.



The dataset supports a sequential decision-making task where each new query is chosen based on previously observed input–output pairs. The goal is not only to maximise function outputs but also to study how different optimisation strategies evolve over time under uncertainty.



In particular, the dataset was constructed to support:

* black-box function optimisation under limited evaluations,
* exploration–exploitation trade-off analysis,
* surrogate model learning (Gaussian Processes and neural networks),
* investigation of uncertainty-driven sampling strategies.



#### 2\. Composition



The dataset consists of sequential query–response pairs for 8 independent black-box functions.



Each function dataset contains:

* Inputs: real-valued vectors in a bounded space (typically \[0,1])
* Outputs: scalar function evaluations
* Size: 19 data points per function at the final stage (Stage 10)



Overall structure: 8 functions × 19 observations each = 152 total samples



Format

* Input vectors vary in dimensionality:

  * Function 1–2: 2D inputs
  * Function 3–4: 3–4D inputs
  * Function 5–6: 4–5D inputs
  * Function 7–8: 6–8D inputs
* Outputs are continuous real values



Gaps / limitations:

* Sparse sampling in high-dimensional spaces (especially Functions 7 and 8)
* Uneven exploration of interior vs boundary regions
* Over-representation of boundary-adjacent points in earlier rounds
* Non-uniform coverage of the input space due to trust-region sampling bias





#### 3\. Collection Process



The dataset was constructed iteratively over 10 sequential optimisation rounds. Each round built on previously observed data.



**Strategy evolution**



Early stage (Rounds 1–3):

* Heuristic local search around best observed points
* Small perturbations + random exploration
* Strong exploitation bias



Middle stage (Rounds 4–7):

* Gaussian Process regression models
* Support Vector Machine classification (high vs low regions)
* Acquisition functions combining mean + uncertainty



Late stage (Rounds 8–10):

* Neural network surrogate model with Monte Carlo dropout
* Trust-region sampling + uniform exploration
* Boundary penalty to reduce edge collapse
* Interior-biased sampling introduced (to correct boundary bias)



Time frame:

* Each week added new query points for all 8 functions, dataset grew gradually from small initial set (10 samples) to final 19 points per function
* Data was collected across sequential modules (approximately 10 weeks)
* Each iteration corresponds to one optimisation round per function



**Key decision logic**



Query points were selected using a hybrid pipeline:

* Model training on previous observations
* Candidate generation (trust-region + uniform + interior bias)
* Scoring using predicted mean, uncertainty, and boundary regularisation
* Selection of highest scoring candidate



#### 4\. Preprocessing and Intended Uses



Preprocessing steps applied:

* Output normalisation (z-score standardisation in neural model training)
* Input clipping to ensure all values remain within valid bounds \[0,1]
* Monte Carlo dropout used to estimate predictive uncertainty
* Ensemble-like sampling through stochastic forward passes
* Deduplication of repeated or near-identical candidate points (implicit via jitter noise)



Intended uses:

* Sequential black-box optimisation research
* Study of exploration–exploitation strategies
* Benchmarking surrogate modelling approaches under limited data
* Educational demonstration of Bayesian optimisation concepts



Inappropriate uses:

* Direct inference of true underlying function structure (functions are not fully sampled)
* Statistical generalisation beyond observed domain
* High-stakes decision-making (dataset is synthetic and low-sample)
* Treating model outputs as ground truth



#### 5\. Distribution and Maintenance



**Availability**

The dataset is maintained within the BBO capstone project portal and is not publicly distributed. Access is restricted to course participants.



**Terms of use:**

* Data is strictly for educational and research purposes within the capstone project
* Redistribution outside the course context is not permitted
* No commercial use is allowed
* Data should be interpreted as synthetic black-box evaluations, not real-world measurements



**Maintenance**

The dataset is maintained by the project pipeline (student-driven generation process). There is no external curator; instead, data evolves dynamically through iterative query submissions.



Updates are generated each round based on:

* model performance,
* observed output patterns,
* exploration–exploitation adjustments,
* correction of biases (e.g. boundary over-sampling).



#### 6\. Additional notes

#### 

Across iterations, a key structural bias emerged: early over-exploration of boundary regions led to a skewed sampling distribution. This influenced later modelling decisions, particularly the introduction of boundary penalties and interior-biased sampling.



Another important insight is that model complexity did not scale linearly with performance. While Gaussian Processes and neural networks improved local decision quality, diminishing returns appeared as data size remained small. This led to a shift toward better sampling strategy design rather than more complex modelling.



Overall, the dataset reflects an adaptive optimisation process where data is not static but co-evolves with the strategy used to generate it.inforce biases if not properly regularised

