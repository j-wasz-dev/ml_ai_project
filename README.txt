Black-Box Optimisation (BBO) Capstone Project

Project Overview

This project focuses on Black-Box Optimisation (BBO), where the goal is to maximise unknown functions using only observed input-output variables. The internal structure of these functions is hidden, so decisions must be made iteratively based on feedback from each query, received from tutor.

The challenge are similar to real-world scenarios where evaluations are expensive or limited, such as hyperparameter tuning, A/B testing, etc. Rather than exhaustive search, the emphasis is on making efficient, informed sampling decisions.

From a career perspective, this project strengthens skills in:

optimisation under uncertainty
iterative model improvement
balancing exploration vs exploitation
applying machine learning techniques under real-world constraints
These skills are directly relevant for roles in data science, ML engineering, and decision intelligence systems.

Inputs and Outputs

Inputs

Continuous vectors of real numbers in the range [0, 1]
Dimensionality varies per function (8 outputs given, 2D, 2D, 3D, 4D, 4D, 5D, 6D, 8D)
Each input must match the function’s dimensionality.

Example input (for 3D function):

[0.123456, 0.654321, 0.987654]

Outputs

Single scalar value representing function performance
Objective: to maximise this value

Example output:

0.842367

No additional information (gradients, functional form, or noise characteristics) is provided, only input-output observations. Every week tutor send outputs of proposed inputs.

Challenge Objectives

The project aims to maximise each unknown function.

Key constraints:

Strict limit on number of queries per function
Unknown and potentially irregular function surfaces
Sequential feedback (one new output per query)
Different dimensionality across functions
Success requires balancing exploration (sampling uncertain or untested regions) and exploitation (refining around high-performing regions)

The challenge is not just to find high values but to improve performance efficiently under uncertainty.

Technical Approach

The approach evolves across the rounds, with three rounds completed so far:

Week 1 – heuristic Local Search
- identify best-performing points in observed data
- apply small perturbations nearby (exploitation)
- limitation - risk of getting stuck in local optima

(Code 1: simple exploit/explore strategy with 70/30 ratio)

Week 2 – Gaussian Process (Bayesian Optimisation)
- introduced a Gaussian Process (GP) surrogate model
- used acquisition functions to combine predicted mean + uncertainty
- balance between exploration and exploitation
- improved guidance for selecting promising regions

(Code 2: GP-based acquisition using mu + 0.1 * sigma as scoring)

Week 3 – Hybrid GP + SVM Approach
- Gaussian Process models the unknown function
- Support Vector Machine (SVM) classifies regions as high- or low-performing
- combined score: mu + 0.15 * sigma + 0.15 * prob_good
- prioritises areas expected to give good results, while keeping a check on model overconfidence
- balances exploration in uncertain areas and exploitation in strong regions

(Code 3: hybrid approach integrating GP predictions and SVM probabilities)

Exploration vs Exploitation Strategy

The strategy balances exploitation, exploration, and adaptation. Exploitation focuses on refining areas predicted to perform well, while exploration checks untested regions to prevent early convergence. The strategy adapts based on model uncertainty and observed performance trends.

The main strengths of this approach are that it combines model-based reasoning with human intuition, uses both the initial and newly collected data, and adapts its strategy for each function instead of following a one-size-fits-all rule. It carefully balances exploring new areas and exploiting known high-performing regions, creating a flexible framework where heuristics, Bayesian models, and classification techniques can evolve as more data comes in.