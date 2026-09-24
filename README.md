# Minor-Saver-Conversion-Model
Predictive model to identify former Minor Savers with a high probability of becoming full members after reaching adulthood using Weight of Evidence (WOE) and logistic regression.

## Predicting Membership Conversion After Reaching Adulthood

### Overview

Savings cooperatives can establish long-term relationships with customers from an early age through **Minor Saver** programs, where children can save under the relationship of a parent or legal guardian who is already a member.

Once a Minor Saver reaches legal adulthood, they can no longer remain under the Minor Saver scheme and must transition out of the program. This creates a customer-retention opportunity: identifying former Minor Savers who are more likely to become full members.

This project develops a predictive model to estimate the probability of membership conversion among former Minor Savers.

The resulting propensity score can be used to rank individuals according to their estimated likelihood of conversion and provide a quantitative baseline for future retention strategies.

---

> **Data Confidentiality**
>
> This project is based on real-world professional data. Due to confidentiality requirements, the original dataset and proprietary information are not included in this repository.

---

## Business Problem

The customer lifecycle can be summarized as:

**Minor Saver → Reaches adulthood → Leaves Minor Saver scheme → Potential full member**

The institution needs to identify which former Minor Savers are more likely to complete this transition.

The key business question is:

> **How can the institution identify former Minor Savers who are more likely to become full members after reaching adulthood?**

A predictive model can provide a structured way to estimate this propensity and prioritize future customer-retention efforts.

---

## Analytical Objective

The objective is to estimate:

$$
P(Y=1 \mid X)
$$

where:

* \(Y=1\): the former Minor Saver became a full member.
* \(Y=0\): the former Minor Saver did not become a full member.
* \(X\): customer and guardian characteristics available around the transition period.

The model generates an estimated probability for each individual, which can be used to rank former Minor Savers according to their likelihood of conversion.

This is a **propensity model**, not a causal model or a deterministic decision rule.

---

## Methodology

The analytical workflow consisted of:

1. Target definition
2. Exploratory data analysis
3. Weight of Evidence (WOE) transformation
4. Information Value (IV) analysis
5. Logistic regression
6. Statistical significance assessment
7. Model discrimination evaluation
8. Propensity score generation

### WOE and IV

Weight of Evidence was used to transform the explanatory variables before fitting the logistic regression model.

Information Value was used to evaluate the univariate predictive strength of the candidate variables.

The final model included ten predictors:

| Variable                       |     IV | Predictive Strength |
| ------------------------------ | -----: | ------------------- |
| `savings`                      | 0.8452 | Very High           |
| `guardian_savings`             | 0.3312 | Strong              |
| `district`                     | 0.1585 | Medium              |
| `guardian_checking_account`    | 0.1538 | Medium              |
| `guardian_certificate_deposit` | 0.1499 | Medium              |
| `guardian_ebank`               | 0.0743 | Weak                |
| `certificate_deposit`          | 0.0552 | Weak                |
| `opening_age`                  | 0.0515 | Weak                |
| `guardian_credit_application`  | 0.0333 | Weak                |
| `guardian_housing_tenure`      | 0.0085 | Insignificant       |

These classifications reflect the IV thresholds applied in the analysis.

The complete WOE tables and variable-level analysis are available in the accompanying Quarto analysis.

---

## Logistic Regression

The WOE-transformed variables were used as predictors in a logistic regression model.

The model estimates the probability that a former Minor Saver becomes a full member.

All ten explanatory variables were statistically significant in the fitted model, with positive coefficients on the WOE-transformed predictors.

The complete regression output, coefficient estimates, statistical tests, and interpretation are presented in the Quarto analysis.

An important distinction is made between **univariate predictive strength (IV)** and **multivariate regression effects**. A variable with low standalone IV can still provide incremental information when included in a multivariate model.

---

## Model Performance

The final model achieved:

| Metric      |     Result |
| ----------- | ---------: |
| **AUC-ROC** | **0.8023** |
| **KS**      | **0.4760** |
| **Gini**    | **0.6046** |

### AUC-ROC

An AUC of **0.8023** indicates good discriminatory ability.

Under the standard interpretation of AUC, the model assigns a higher score to a randomly selected converter than to a randomly selected non-converter approximately 80.23% of the time.

AUC should not be interpreted as classification accuracy.

### KS

The model achieved a **KS statistic of 0.4760**, indicating substantial separation between the score distributions of converters and non-converters.

### Gini

The Gini coefficient was **0.6046**, consistent with the AUC:

$$
Gini = 2 \times AUC - 1
$$

---

## Operational Implementation and Validation

Following model development, the model was implemented on a subsequent population of former Minor Savers identified after reaching legal adulthood during the 2025 fiscal year.

A total of **11,320 former Minor Savers** were evaluated by the model and classified according to their estimated propensity to become full members.

Their model classifications were subsequently compared with membership records as of **December 2025**.

### Implementation Results

| Metric                                          |     Result |
| ----------------------------------------------- | ---------: |
| Former Minor Savers evaluated                   | **11,320** |
| Former Minor Savers who became full members     |  **4,259** |
| Observed conversion rate                        |  **37.6%** |
| Actual converters identified as high propensity |   **>75%** |

Among the 11,320 former Minor Savers evaluated, **4,259 subsequently became full members**, corresponding to an observed conversion rate of **37.6%**.

More than **75% of the individuals who ultimately became full members had been previously classified by the model as having a high propensity to convert**.

This result corresponds to a **recall/sensitivity above 75%** when the high-propensity classification is evaluated against the observed converters:

$$
Recall =
\frac{\text{Actual converters classified as high propensity}}
{\text{All actual converters}} >75\%
$$

This operational result complements the model-development metrics by showing that the model was subsequently applied to a real population and that its high-propensity segment captured a substantial proportion of the individuals who ultimately converted.

Importantly, the **37.6% conversion rate is an observed outcome for the evaluated population**, not an estimate of the causal effect of the model or of any intervention based on it.

---

## Key Findings

### Financial engagement

The strongest univariate predictor was `savings`, with an IV of **0.8452**.

Its WOE values increased consistently across savings-balance ranges, indicating a strong relationship between savings behavior and subsequent membership conversion in the analyzed population.

### Guardian financial relationship

Variables associated with the guardian's relationship with the institution also contributed predictive information, including:

* `guardian_savings`
* `guardian_checking_account`
* `guardian_certificate_deposit`
* `guardian_credit_application`
* `guardian_ebank`

This indicates that the broader financial relationship surrounding the Minor Saver contains useful information for predicting future membership conversion.

### Product engagement

Variables related to savings and financial products held by the Minor Saver also contributed to the model.

Together, these results suggest that both the Minor Saver's relationship with the institution and the guardian's financial engagement provide predictive signal for the transition to full membership.

---

## Business Application

The model produces an estimated conversion probability for each former Minor Saver.

Potential applications include:

* Ranking former Minor Savers by estimated propensity.
* Segmenting customers according to predicted conversion likelihood.
* Supporting customer-retention prioritization.
* Establishing a baseline for future retention strategies.
* Identifying populations for controlled experimentation.

The model is intended to **support decision-making rather than determine customer treatment**.

Importantly, predictive association does not imply causation. A high predicted probability does not mean that a particular intervention will cause an individual to become a member.

---

## Conclusion

This project applies **Weight of Evidence, Information Value, and logistic regression** to a customer-lifecycle problem: identifying former Minor Savers with a higher estimated probability of becoming full members after reaching adulthood.

The model achieved:

* **AUC-ROC: 0.8023**
* **KS: 0.4760**
* **Gini: 0.6046**

The analysis highlights the predictive relevance of savings behavior, guardian financial engagement, and product relationships.

Rather than functioning as a deterministic classification system, the model provides a **propensity score that can be used to rank former Minor Savers** and serve as a baseline for future retention strategies and controlled experiments.

The project demonstrates the application of interpretable predictive modeling techniques to a real-world customer-retention problem while maintaining a clear distinction between prediction and causality.
