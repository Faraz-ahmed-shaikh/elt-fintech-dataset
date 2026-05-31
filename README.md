# UPI Fintech Synthetic Dataset

A realistic synthetic dataset simulating two years of digital payment transactions on a UPI-based fintech platform (similar to PhonePe / Google Pay / Paytm). Generated for use in a PySpark + Databricks ELT project following Medallion Architecture (Bronze → Silver → Gold).

---

## Files

| File | Rows | Size |
|---|---|---|
| `transactions.json.gz` | 301,500 | ~10 MB compressed |
| `merchant_metadata.json` | 2,211 | ~326 KB |

---

## Schema

**transactions.json.gz**

| Column | Type | Notes |
|---|---|---|
| transaction_id | string | Unique per transaction |
| customer_id | string | 18,000 unique customers |
| merchant_id | string | FK to merchant_metadata |
| transaction_timestamp | string | Mixed formats intentional |
| amount | float | INR, skewed distribution |
| payment_method | string | UPI, Debit Card, Credit Card, Wallet, Net Banking |
| transaction_status | string | Success / Failed / Pending / Refunded / Reversed |
| city | string | 18 Indian cities |
| device_type | string | Android, iOS, Web, USSD |
| currency | string | Mostly INR, some USD/AED/EUR |

**merchant_metadata.json**

| Column | Type | Notes |
|---|---|---|
| merchant_id | string | Primary key |
| merchant_name | string | |
| merchant_category | string | 14 categories |
| city | string | |
| merchant_tier | string | Small Business / Mid Market / Enterprise |

---

## Intentional Data Quality Issues

The dataset contains realistic operational messiness designed for Bronze layer cleaning:

- ~0.5% duplicate transaction rows
- ~0.7% null merchant_id, ~0.5% null city, ~1.2% null device_type
- Mixed timestamp formats (`YYYY-MM-DD HH:MM:SS` and `DD-MM-YYYY HH:MM`)
- Payment method casing variants (`UPI`, `upi`, `Upi`, `CC`, `credit_card`)
- Currency inconsistencies (`INR`, `inr`, `INR ₹`, plus foreign currencies)
- Duplicate and null-category rows in merchant metadata

---

## Usage in Databricks

```python
import requests

url = "https://raw.githubusercontent.com/<your-username>/<your-repo>/main/transactions.json.gz"
r = requests.get(url)
with open("/tmp/transactions.json.gz", "wb") as f:
    f.write(r.content)

df = spark.read.json("/tmp/transactions.json.gz")
```

---

## Context

This dataset was synthetically generated using Python (NumPy, Pandas) with realistic behavioral patterns — power-law customer activity, time-based spending patterns, city-weighted distributions, and category-specific amount distributions. It is not real financial data.

Date range: May 2024 – May 2026
