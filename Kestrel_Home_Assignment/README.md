# Kestrel Home — AI Support Request Routing

## Project overview
This project trains a machine-learning classifier to predict the initial support team for a customer request. It uses request text together with product family, channel, and warranty status.

The project includes model evaluation, an analysis of historical routing-cost estimates, and predictions for an unlabelled set of 2,178 requests.

## Model
- **Text features:** TF-IDF with unigrams and bigrams
- **Categorical features:** product family, channel, and warranty status, encoded with one-hot encoding
- **Classifier:** Logistic Regression
- **Validation:** stratified 80/20 train-validation split, using `random_state=42`
- **Validation accuracy:** 94.78%
- **Macro F1:** approximately 0.95

These scores are measured against standardized historical labels. They are not a measured score on the hidden/unlabelled requests and do not guarantee live operational performance.

## Data preparation
Two historical team names were updated effective 15 January 2026. For consistent modelling, the historical labels were standardized as follows:
- `Consumables` → `Filters & Consumables`
- `Installations` → `Installs & Demo`

The model predicts the initial `team_label`, not `final_team` (the team that eventually resolved a request).

## Output
The prediction file contains 2,178 rows and two columns:
- `request_id`
- `team`

The output was checked for missing values, duplicate request IDs, and consistency with the selected model's predictions.

## Business-cost analysis
Using the supplied operational cost assumptions:
- Transfer events: 3,902 × ₹305 = ₹11,90,110
- Requests where first team differed from final team: 2,471 × ₹260 = ₹6,42,460 (proxy estimate)
- Combined estimate over the historical period: ₹18,32,570
- Annualized estimate over 12 months from a 15-month period: approximately ₹14,66,056
- Existing routing-bot licence: ₹3,20,000 per year

Illustrative 10%–40% reductions imply approximately ₹1.47–₹5.86 lakh in annual potential savings before AI operating costs. These are scenarios, not measured savings. A first-team/final-team difference is only a proxy for misrouting and does not prove that every associated cost is avoidable.

## Experiment and error analysis
A keyword-based override for selected purifier-malfunction cases was tested and rejected because validation accuracy declined from 94.78% to approximately 92.98%. Payment-related wording can also appear in requests whose main issue is installation, repair, or warranty; therefore, a broad rule that sends every payment mention to Billing was not adopted.

## Limitations and next steps
- Historical labels may contain inconsistent routing decisions.
- A random validation split may overestimate performance on future requests.
- The unlabelled prediction set cannot be scored until ground-truth labels become available.
- AI API, infrastructure, monitoring, and maintenance costs have not been measured.
- The model has not been evaluated in a live routing workflow.

Recommended next step: run a controlled pilot, review ambiguous cases, measure actual transfers and customer contacts, and compare verified savings with the full cost of operating the AI system.

## Reproducibility and privacy
The notebook contains the implementation and analysis. To reproduce the workflow, use the required source datasets in a private, authorized environment and run the notebook from top to bottom.

**Privacy:** Do not commit client-provided datasets, request text, resolution logs, or other confidential operational data to a public repository. Include only code and documentation that are permitted to be shared.
