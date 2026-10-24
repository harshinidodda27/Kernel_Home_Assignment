
# Kestrel Home — AI Support Request Routing

## Overview
This project builds a machine-learning classifier to predict the initial support team for customer requests. It uses request text, product family, channel, and warranty status to classify requests into seven standardized teams.

## Model and Results
- **Model:** TF-IDF + One-Hot Encoding + Logistic Regression
- **Validation method:** Stratified 80/20 train-validation split (`random_state=42`)
- **Validation accuracy:** 94.78%
- **Macro F1-score:** Approximately 0.95
- **Prediction output:** 2,178 unlabelled requests

These results measure agreement with standardized historical labels. They are not a measured score on the hidden test outcomes.

## Data Preparation
Historical team names changed on 15 January 2026. To maintain consistency:
- `Consumables` → `Filters & Consumables`
- `Installations` → `Installs & Demo`

The target is `team_label`, which represents the initial queue assignment, not `final_team`, which represents the team that resolved the request.

## Business Impact Analysis
Using the supplied cost assumptions:
- Transfer costs: ₹11,90,110 over the historical period
- Estimated extra-contact costs: ₹6,42,460
- Combined historical cost estimate: ₹18,32,570
- Annualized estimated routing cost: approximately ₹14,66,056
- Existing routing-bot licence: ₹3,20,000 per year

Illustrative 10–40% routing-cost reductions correspond to approximately ₹1.47–₹5.86 lakh in potential annual savings, before AI operating costs.

These are hypothetical scenarios, not measured savings. Differences between initial and final teams are only a proxy for misrouting.

## Experiments and Error Analysis
A keyword-based override for selected purifier-malfunction requests was tested but rejected because validation accuracy declined from 94.78% to approximately 92.98%.

Requests mentioning payment can also concern installation, repair, or warranty issues. A broad rule sending every payment mention to Billing was therefore not adopted.

## Limitations
- Historical routing labels may be inconsistent.
- Random validation may overestimate performance on future requests.
- Hidden-set accuracy is not yet known.
- AI API, infrastructure, monitoring, and maintenance costs have not been measured.
- The model has not been tested in a live routing workflow.

## Recommended Next Steps
1. Run a controlled pilot.
2. Review ambiguous and low-confidence predictions.
3. Measure actual transfers and additional customer contacts.
4. Estimate total AI operating costs.
5. Compare verified savings with the existing licence cost.

## Privacy
Do not publish client-provided datasets, customer request text, resolution logs, or confidential operational information in a public repository. Only upload code and documentation approved for public sharing.
