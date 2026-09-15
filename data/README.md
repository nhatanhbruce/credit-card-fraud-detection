# Data

The dataset is not committed to this repository. `fraudTest.csv` is roughly 140 MB, above GitHub's 100 MB per-file limit.

## Getting the file

1. Download the Simulated Credit Card Transaction dataset from Kaggle (Kartik Shenoy, "Credit Card Transactions Fraud Detection Dataset").
2. Place `fraudTest.csv` in this folder, so the path is `data/fraudTest.csv`.

Via the Kaggle CLI:

```bash
pip install kaggle
kaggle datasets download -d kartik2112/fraud-detection -f fraudTest.csv -p data/
unzip data/fraudTest.csv.zip -d data/
```

## Schema

555,719 rows, 23 columns. Columns used in this analysis:

| Column | Description |
| --- | --- |
| `trans_date_trans_time` | Transaction timestamp, source of quarter, month, and hour features |
| `amt` | Transaction amount, the basis of all value-weighted fraud metrics |
| `category` | Merchant category, for example `shopping_net`, `misc_net`, `shopping_pos` |
| `gender` | Cardholder gender |
| `dob` | Cardholder date of birth, used to derive age and age group |
| `city`, `state` | Cardholder location, used for geographic concentration |
| `is_fraud` | Target variable, 0 = legitimate, 1 = fraudulent |

The dataset is simulated. It contains no real cardholder data.
