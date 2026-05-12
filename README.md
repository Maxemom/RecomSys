Short Project and Assignment Description

Overall assignment

The goal of this assignment is to define the methodology for our recommender systems research project. We do not need to present final experimental results yet. Instead, we need to design a rigorous, feasible, and well-structured experimental plan.

The paper should explain:

* which research questions we want to answer,
* which datasets and models we will use,
* how we define and construct edge-case users,
* how we evaluate model behavior,
* what challenges we expect,
* and how the full experimental pipeline will work.

The submission should be written in ACM two-column format and include a short disclosure of LLM assistance.

⸻

Refined Project Focus

Our project investigates how different recommender system model families behave when evaluated on edge-case user profiles at test time.

Instead of only measuring average recommendation performance over all users, we want to test whether models show hidden weaknesses for users whose preferences are unusual, niche, or slightly modified in controlled ways.

The proposed setup should remain manageable:

* 1 main dataset: MovieLens
* 3 model types: for example Popularity Baseline, Matrix Factorization / Neural Collaborative Filtering, and SASRec or another sequential model
* 2 edge-case user types:
    1. Niche users found inside the dataset
    2. Perturbation-based users created from real user profiles through realistic, controlled modifications

The project focuses on test-time evaluation, meaning that the edge-case users are used to test trained recommender models, not necessarily to train them.

⸻

Research Questions

RQ1: keep mostly as it is

RQ1 is already strong and clear:

RQ1: How do different recommender model families behave when evaluated on edge-case user profiles at test time?

This is the main research question.

It asks whether different model families, such as classical collaborative filtering, neural recommenders, and sequential recommenders, show different strengths or weaknesses when serving edge-case users.

⸻

RQ2 and RQ3: need rework

The feedback showed that the previous RQ2 and RQ3 were too broad and abstract.

The words “unified,” “standardized,” and “benchmark” need to be used carefully. If we keep them, we must explain exactly what remains fixed and what can be adapted.

A better direction would be to make RQ2 and RQ3 more concrete and directly connected to the experimental setup.

Possible direction:

RQ2: How can niche users be identified in a recommender dataset in a way that is reproducible and suitable for model comparison?

This would focus on the niche-user-search strategy.

Another possible direction:

RQ3: How stable are recommender model outputs when real user profiles are modified through small, realistic perturbations?

This would focus on perturbation-based profiles and model behavior changes.

Together, the RQs would then be much more manageable:

RQ	Focus
RQ1	Compare model families on edge-case users
RQ2	Define and identify niche users
RQ3	Test behavior changes under realistic perturbations

This is much clearer than trying to build a universal benchmark for all recommender systems.

⸻

Main Open Methodological Question

One important issue is whether we should:

Option A: train the models ourselves

This gives us full control over:

* train/test split,
* validation data,
* model inputs,
* user histories,
* edge-case generation,
* data leakage prevention,
* and fair comparison.

This is methodologically cleaner.

Option B: use existing pretrained models

This may save time, but it creates major problems:

* We may not know which data was used for training.
* We may not know whether test users or test interactions leaked into training.
* We may not be able to define niche users consistently.
* Different models may have been trained on different data.
* Fair comparison becomes difficult.

Therefore, if we use existing models, we must explain exactly how we verify that no data leakage happens. If that is not possible, training our own simpler models is probably the safer methodological choice.

⸻

Work Package 1: Dataset, Model Selection, and Experimental Setup

Main responsibility

This person defines the general experimental setup:

* Which dataset do we use?
* Which model types do we compare?
* Do we train models ourselves or use existing implementations/models?
* How do we avoid data leakage?
* What is the train/validation/test strategy?

⸻

Questions to answer

Dataset

* Why is MovieLens suitable for this project?
* Which MovieLens version should be used?
* Does the dataset contain:
    * user-item interactions?
    * ratings?
    * timestamps?
    * genres or item metadata?
* Is the dataset large enough for niche-user analysis?
* Is it small enough to be feasible?

Models

* Which three model types should be used?
* Why are these model families meaningfully different?
* Which model is the simple baseline?
* Which model is the classical recommender?
* Which model is the neural or sequential recommender?
* Are all models compatible with MovieLens?
* Can all models be trained within the project timeframe?

Training or pretrained models

* Do we train all models ourselves?
* If yes:
    * Which library or implementation do we use?
    * How do we keep training comparable?
    * Which hyperparameters are fixed?
* If no:
    * How do we verify the original training data?
    * How do we avoid data leakage?
    * Are the models actually comparable?

Splitting strategy

* Do we use a temporal split?
* Do we split per user?
* How much data goes to train, validation, and test?
* Are edge-case users only used at test time?
* How do we ensure that test interactions do not leak into training?

⸻

Deliverables

This person should produce:

1. A short dataset justification section
2. A model selection table
3. A train/validation/test split proposal
4. A short argument about training models ourselves vs. using existing models
5. A first experimental pipeline overview

⸻

Work Package 2: Niche User Identification

Main responsibility

This person defines how to find real niche users inside the dataset.

This is one of the most important parts of the project.

The goal is not just to say “niche users are rare users,” but to create a reproducible method for identifying users with non-mainstream preferences.

⸻

Key idea

Niche users should be identified based on their interaction patterns.

A possible high-level strategy:

1. Compute item popularity from the training set.
2. Define long-tail items using a relative popularity threshold.
3. Compute each user’s long-tail interaction ratio.
4. Classify users with high long-tail ratios as niche users.
5. Compare them against mainstream users.

⸻

Questions to answer

Definition of niche

* What does “niche” mean in our project?
* Is it based on item popularity?
* Is it based on genre concentration?
* Is it based on distance from mainstream users?
* Do we use one definition or multiple definitions?

Long-tail item definition

* How do we define item popularity?
* Is popularity the number of interactions in the training set?
* Do we use absolute thresholds or percentiles?
* Example:
    * bottom 20% of items by popularity,
    * or items below median popularity,
    * or items with fewer than X interactions.

A percentile-based definition is more transferable across datasets than an absolute threshold.

User-level niche score

Possible score:

NicheScore(user) = number of long-tail interactions / total number of interactions

Questions:

* Which interactions count?
* Only training history?
* Full observed user history before test?
* Do ratings matter?
* Do low ratings count as preferences?

Avoiding confusion with sparse users

Important:

A niche user is not the same as a user with very few interactions.

Therefore, this work package must define:

* minimum number of interactions per user,
* minimum number of training interactions,
* minimum number of test interactions,
* and possibly separate sparse users from niche users.

Mainstream comparison group

To measure gaps, we also need a comparison group.

Questions:

* How do we define mainstream users?
* Low niche score?
* High interaction with popular items?
* Random sample of non-niche users?
* Same history length as niche users?

This is important because otherwise differences may be caused by different profile lengths, not niche preferences.

⸻

Possible outputs to prepare

* Niche-user definition
* Long-tail-item definition
* NicheScore formula
* Mainstream-user definition
* Inclusion/exclusion rules
* Discussion of limitations

⸻

Deliverables

This person should produce:

1. A clear definition of niche users
2. A reproducible search strategy for finding them in MovieLens
3. A formula for measuring user nicheness
4. A strategy for selecting a mainstream control group
5. A short discussion of risks, especially sparsity vs. nicheness

⸻

Work Package 3: Perturbation-Based User Profiles

Main responsibility

This person defines how to create perturbed user profiles from real users.

The goal is to test how model recommendations change when user profiles are slightly modified.

Important:

The perturbations must remain realistic. We do not want adversarial attacks or unrealistic fake users.

⸻

Core idea

Start with real user profiles from the dataset.

Then create modified versions by changing a small number of interactions.

Example:

Original user:
popular action movie, popular action movie, popular thriller
Perturbed user:
popular action movie, less popular action movie, popular thriller

The profile remains plausible, but becomes slightly more niche.

⸻

Questions to answer

Which users are perturbed?

* Mainstream users?
* Niche users?
* All eligible users?
* Users with enough history length?
* Users with clear genre preferences?

What type of perturbation?

Possible perturbation types:

Type A: Popular-to-long-tail replacement

Replace one or two popular items with less popular but related items.

Questions:

* How many items are replaced?
* Which items are eligible for replacement?
* How do we choose the replacement item?
* Must it share a genre with the original?
* Must it have lower popularity?

Type B: Minor irrelevant perturbation

Make a small change that should not strongly change the user profile.

Purpose:

The model should stay stable.

Type C: Meaningful preference shift

Add or replace an item that signals a possible new preference.

Purpose:

The model should adapt.

⸻

Realism constraints

This is very important.

The perturbation should follow rules such as:

* change only a small percentage of the profile,
* keep most of the original history unchanged,
* replacement item should be semantically plausible,
* replacement item should share genre/category with original item or user history,
* avoid random unrelated item insertion,
* avoid replacing too many recent interactions unless specifically testing recency sensitivity,
* do not insert items the user has already interacted with.

⸻

Test-time behavior

The perturbation should be applied at test time.

Questions:

* Is the model retrained after perturbation?
* Or do we only modify the input profile and observe changed recommendations?
* For sequential models, do we perturb the last interaction, middle interaction, or early interaction?
* For non-sequential models, how is the modified profile represented?

This is a major methodological detail.

⸻

What should be measured?

Perturbed profiles are mainly useful for measuring:

* recommendation stability,
* performance drop,
* popularity shift,
* sensitivity to small changes,
* whether the model reacts too much or too little.

⸻

Deliverables

This person should produce:

1. A perturbation strategy
2. Rules for realistic perturbations
3. A distinction between minor perturbation and meaningful shift
4. A plan for how perturbations are applied at test time
5. A short discussion of how this differs from adversarial attacks

⸻

Work Package 4: Evaluation Metrics and Analysis Protocol

Main responsibility

This person defines how we evaluate the models.

This is not only about standard accuracy. The main point is to evaluate behavior differences across user groups and before/after perturbation.

⸻

Evaluation levels

The evaluation should happen on multiple levels:

1. Overall performance on all test users
2. Performance on niche users
3. Performance on mainstream users
4. Performance gap between mainstream and niche users
5. Behavior change under perturbation
6. Popularity bias of recommendations
7. Stability of recommendation lists

⸻

Standard ranking metrics

These measure normal recommender performance.

Possible metrics:

* Recall@K
* Precision@K
* NDCG@K
* HitRate@K
* MAP@K

Questions:

* Which K values do we use?
* K = 5, 10, 20?
* Do we evaluate over all candidate items?
* Or over sampled negative candidates?
* Are already-seen items filtered out?

⸻

Edge-case subgroup metrics

The same ranking metrics should be computed separately for:

* all users,
* niche users,
* mainstream users,
* perturbed users.

Important metric:

PerformanceGap = Metric(mainstream users) - Metric(niche users)

Or relative:

RelativeGap = (Metric(mainstream) - Metric(niche)) / Metric(mainstream)

This directly measures whether niche users are underserved.

⸻

Robustness metrics

For perturbation-based profiles:

ΔNDCG@K = NDCG@K_original - NDCG@K_perturbed

or:

RelativeDrop = (Metric_original - Metric_perturbed) / Metric_original

This measures whether small profile changes cause large performance drops.

⸻

Stability metrics

These compare recommendation lists before and after perturbation.

Possible metrics:

Overlap@K = |TopK_original ∩ TopK_perturbed| / K

Also possible:

* Jaccard Similarity
* Kendall Tau
* Spearman Rank Correlation

These answer:

Did the recommendation list change too much after a small profile perturbation?

⸻

Popularity bias metrics

These are especially important for niche users.

Possible metrics:

Average Recommendation Popularity

For each recommended item, compute its popularity in the training set.

Then average across recommendations.

Question:

Does the model recommend mostly popular items even to niche users?

Long-Tail Recommendation Ratio

LongTailRatio = number of recommended long-tail items / K

Question:

Does the model recommend long-tail items to users who prefer long-tail items?

Popularity Mismatch

Compare the long-tail ratio in the user profile with the long-tail ratio in recommendations.

Example:

User history: 70% long-tail items
Recommendations: 10% long-tail items

This indicates poor niche alignment.

⸻

Metric generality question

This person should also address:

Which metrics remain stable across different model types and datasets?

For example:

Generally reusable:

* Recall@K
* NDCG@K
* HitRate@K
* Overlap@K
* Jaccard Similarity
* Performance Gap

Dataset-dependent:

* genre calibration,
* category-based diversity,
* metadata-based niche definitions.

This is important for RQ2.

The evaluation protocol can be consistent even if some dataset-specific metrics must be adapted.

⸻

Deliverables

This person should produce:

1. A metric table
2. A description of standard and edge-case evaluation
3. Formulas for performance gap, robustness drop, and stability
4. A popularity bias evaluation plan
5. A discussion of which metrics are dataset-independent and which are dataset-specific

⸻

How the Four Work Packages Fit Together

The four parts should connect like this:

WP1: Dataset + Models + Split
        ↓
WP2: Find real niche users
        ↓
WP3: Create perturbation-based user profiles
        ↓
WP4: Evaluate model behavior across normal, niche, and perturbed users

Together, these answer the main research direction:

How do different recommender model families behave on test-time edge-case user profiles, and how can this behavior be evaluated in a reproducible and manageable way?

⸻

Recommended Final Group Structure

Work Package	Person Focus	Main Output
WP1	Dataset, models, training setup	Experimental setup
WP2	Niche users	Search and definition strategy
WP3	Perturbation users	Realistic perturbation design
WP4	Metrics	Evaluation protocol

⸻

Important Shared Decisions

Even though the work packages are separate, the team must agree on these points together:

1. Final RQ1, RQ2, RQ3
2. MovieLens version
3. Final model list
4. Whether models are trained from scratch or taken from existing implementations
5. Exact train/test split
6. Definition of long-tail items
7. Minimum user-history length
8. Perturbation strength
9. K values for evaluation metrics
10. Whether a second dataset is core or optional
