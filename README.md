# Return_fraud_Behavioural_Analytics
Return Fraud Behavioural Analytics System | Python, MySQL, Power BI, Scikit-learn
A complete end-to-end fraud detection system built for Indian e-commerce

# Problem Statement
Indian e-commerce platforms lose an estimated ₹12,000–15,000 Crore annually to return fraud — a specific abuse pattern where users:

1-Order high-value products (electronics, fashion, appliances)
2-Use the product for 3–8 days
3-Return it claiming "arrived damaged" or "wrong item received"
4-Receive a full refund while keeping or reselling the product

Unlike payment fraud (which has mature detection), return fraud is behavioural — it looks identical to legitimate returns from the outside. The only signal is the pattern of behaviour over time: how often a user returns, how quickly, what reason they claim, how new their account is.

## Dataset & schema

- Source: `Synthetic_fraud_Data.csv`, loaded via `LOAD DATA INFILE` into a MySQL table `user_behaviour`
- Grain: one row per user
- Database: `Return_Fraud`

| Column | Type | What it captures |
|---|---|---|
| `user_id` | VARCHAR(20), PK | Unique user identifier |
| `city` | VARCHAR(50) | User's city |
| `account_age_days` | INT | Account tenure |
| `orders_last_90d` | INT | Recent order volume |
| `return_rate_pct` | DECIMAL(5,1) | % of orders returned |
| `damage_claim_pct` | DECIMAL(5,1) | % of returns filed as damage claims |
| `avg_days_to_return` | INT | Average time-to-return |
| `unique_addresses_used` | INT | Distinct delivery addresses used |
| `high_value_return_count` | INT | Count of high-value item returns |
| `negative_reviews_after_return` | INT | Reviews left post-return |
| `support_tickets_filed` | INT | Support contact volume |
| `payment_method_changes` | INT | Payment method churn |
| `weekend_order_pct` | DECIMAL(5,1) | % of orders placed on weekends |
| `category_concentration_score` | DECIMAL(4,3) | How concentrated a user's returns are by category |
| `top_return_category` | VARCHAR(50) | Most-returned product category |
| `is_fraud` | TINYINT | Confirmed fraud label (ground truth) |

