# Car-Insurance-Claim-Prediction
Insurance premium prediction project exploring regression models and actuarial pricing concepts.

## Overview 
Insurers need to estimate a drivers's likelihood of filing a claim in order to price policies fairly and manage risk. This project develops and compares two models that predict claim probability using applicant data available at underwriting, interpreting the main factors influencing risk, providing insights that could support risk-scoring or pricing decisions.


### Dataset
Car Isurance Data (Kaggle, via sagnik1511), containing 10,000 rows covering driver demographics, driving history, vehicle details, and whether a claim was filed (OUTCOME).

### Design Decisions
- **Race and Gender were excluded as predictors** under the Equality Act 2010 and the 2012 EU Test-Achats ruling with no appliance exemptions. 

- **Postal code was excluded** as postcode based pricing can act as a proxy for ethnicity. 

- **Missing values** in CREDIT_SCORE and ANNUAL_MILEAGE (~10% each) were filled with the column mean, given both distributions were close to symmetric.

- **Ordinal features** (age bracket, driving experience, eductaion, income) were mapped to ordered integers rater than one-hot encoded, to preserve their natural ranking.

- **Nominal features** (gender excluded per above; vehicle year, vehicle type) were one-hot encoded.

### Models
Two predictive models were trained and compared: 

1. **Logistic Regression** (scaled features) - The standard, interpretable baseline used throughout actuarial pricing and underwriting, chosen for its transparency.
2. **Random Forest** (200 trees, max depth 8) - A more flexible model included as the benchmark, to check whether a non-linear model could meaningfully outperform the interpretable baseline.

### Results
| Metric              | Logistic Regression | Random Forest |
| --------------------|:-------------------:|--------------:|
| Accuracy            |        0.81         |     0.81      |
|Claim-class precision|        0.69         |     0.71      |
|Claim-class recall   |        0.72         |     0.70      |
|AUC                  |        0.876        |     0.869     |

**The Random Forest did not outperform the Logistic Regression.** <br> 
Given comparable performance, the logistic regression is the more defensible choice for a real pricing or underwriting context, since its coefficients can be directly explained to a regulator, underwriter, or policyholder, whereas the random forests predictions cannot. 

### Key Findings
- **Driving experience was the strongest predictor of claim risk in both models**. Greater driving experience is linked to substantially fewer insurance claims.

- **Speeding violations, annual mileage and DUI's increased predicted risk**, aligning with current motor-insurer risk assesments. 

- **Vehicle ownership and a newer vehicle (post 2015) were both associated with lower claim risk**.

- The models concurred on the main predictors of risk but **diverged on secondary factors**. Most notably, age ranked as one of the most important features in the random forest but had almost no influence in the logistic regression. The discrepancy implies that the underlying age-risk relationship may be non-linear (e.g. elevated risk among both the yougest and oldest drivers) a pattern poorly representd by linear logistic regression but better captured by the tree-based model. This should be trested as s hypothedis for further investigation, not a confirmed result.

- **Past accidents showed a small negative coeffcient in the logistic regression**. The direction is counterintuitive, as a greater number of past incidents would normally suggest higher future claim risk. This likely reflects confounding with driving experience (longer exposure increases both past accidents and experience), a multicollinearity issue that should be investigated rather than accepting the coefficient at face value.

### Tools
Python, pandas, scikit-learn, matplotlib

### Possible Extensions
- Adress the age non-linearity directly, e.g. by binning age more graduarly or adding an interaction term.

- Investigate the past accidents coefficient with correlation/VIF analysis.

- Try a Gamma or Tweedie GLM if claim severity data were available, rather than just claim occurence.