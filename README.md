# Credit Card Fraud Detection

Exploratory risk analysis of 555,719 simulated credit card transactions, framed as a report to a retail bank's fraud risk management team. The analysis locates where fraud concentrates across time, merchant category, geography, and customer demographics, and translates that into prioritised control recommendations.

## Headline finding

Fraud looks negligible by transaction count and severe by transaction value.

| Metric | Value |
| --- | --- |
| Fraudulent transactions | 2,145 of 555,719 (0.39%) |
| Fraud loss rate by value | 293.89 bps |
| Reported U.S. industry benchmark | ~6.81 bps |

Measured by value rather than count, the portfolio sits roughly 43x the cited national average. This is the signature of a low-frequency, high-severity fraud problem, which is harder to catch and more costly per incident than high-frequency small-amount fraud.

## What the analysis covers

1. **Overall exposure** - fraud rate by count and by value in basis points, the metric U.S. regulators and the payments industry use.
2. **Time series** - fraud value share by quarter, month, and time of day. September and October are the elevated-risk months; the 18:00 to 23:59 evening window is the largest intraday vulnerability, when human oversight and customer responsiveness are lowest.
3. **Merchant category** - `shopping_net` alone carries roughly 44% of fraudulent transaction value, more than double the next category. Combined with `misc_net`, card-not-present channels account for over 63% of total fraud losses.
4. **Geography** - New York, Pennsylvania, and Texas rank highest by fraud value. New York's exposure runs about 59% above Pennsylvania and 90% above Texas. Category mix is broken out within each of the three states to test whether the online concentration is national or local.
5. **Demographics** - fraud distribution by gender and by age group (young adult, adult, elderly), by both count and value.
6. **Logistic regression setup** - log-transformed amount, encoded gender, and quarter dummies prepared as model inputs.

## Recommended controls

- Step-up authentication (for example OTP) on `shopping_net` and `misc_net` transactions above a defined value threshold, since card-not-present channels dominate losses in every geography examined.
- Concentrated monitoring capacity in New York, Pennsylvania, and Texas, weighted toward online merchant transactions.
- Extra review capacity staffed for the evening window and for the September to October seasonal peak.
- A review of whether existing card-not-present controls meet current industry standards, given the 43x gap to the benchmark loss rate.

## Data

Simulated Credit Card Transaction dataset from Kaggle (`fraudTest.csv`, 555,719 rows, 23 columns, target `is_fraud`). The file is not committed here because it exceeds GitHub's file size limit.

To run the notebook, download `fraudTest.csv` from Kaggle and place it at `data/fraudTest.csv`. The loading cell falls back to a Google Drive mount when run in Colab, so the notebook works in both environments without edits.

## Repository layout

```
.
├── notebooks/
│   └── credit_card_fraud_detection.ipynb   Full analysis, outputs and charts included
├── data/
│   └── README.md                           Dataset acquisition instructions
├── requirements.txt
└── .gitignore
```

## Running it

```bash
git clone https://github.com/<your-username>/credit-card-fraud-detection.git
cd credit-card-fraud-detection
pip install -r requirements.txt
# place fraudTest.csv in data/
jupyter notebook notebooks/credit_card_fraud_detection.ipynb
```

## Known limitations and next steps

- The dataset is simulated, so absolute loss rates should be read as directional rather than as a real portfolio benchmark.
- Customer age is derived from `dob` against the current system date, so age group boundaries shift slightly depending on when the notebook is run. Pinning a fixed reference date would make the demographic split fully reproducible.
- The logistic regression is set up but not yet fitted. Fitting it, reporting coefficients and odds ratios, and evaluating with precision, recall, and AUC on a held-out split is the natural next step given the 0.39% class imbalance, where accuracy is not a usable metric.
- Geographic analysis uses raw fraud value by state and is not normalised by transaction volume or population. New York's lead may partly reflect exposure rather than elevated per-transaction risk.

## Tools

Python, pandas, NumPy, matplotlib, Jupyter.
