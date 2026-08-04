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

# Business Problem

A fraud analyst at many companies faces 500,000 active return-behaviour users with no principled way to prioritise review. Sorting by return count catches only the most obvious fraudsters. Simple rules (return rate > 50%) have 72% precision — blocking nearly 1 in 3 innocent customers.

## 🗃  Dataset
Why Synthetic Data?

Real fraud data from Indian e-commerce companies is confidential and unavailable publicly. Synthetic data built on domain-specific distributions replicates the statistical properties of real fraud patterns.

## Exploratory Data Analysis

#### Univariate Analysis

1-Return Rate Distribution

<img width="1005" height="347" alt="image" src="https://github.com/user-attachments/assets/8f78b502-eddc-44b1-94f4-3a586a95be2a" />

> ### 💡 Business Insight
> Fraudulent users exhibit an average return rate nearly 3.7× higher than legitimate users (66.6% vs. 18.2%). While return rate is a strong predictor of fraud, the overlap between 28%–45% suggests that combining return rate with additional behavioral features will reduce false positives and improve fraud detection accuracy. The percentile analysis further shows that 75% of customers return less than 26.6% of purchases, providing a data-driven baseline for defining risk thresholds and optimizing manual review efforts.


2-Summary distribution of all 12 features

<img width="994" height="408" alt="image" src="https://github.com/user-attachments/assets/1d0d7c47-71fe-42c8-85ff-27fe6ccd8ff6" />
<img width="1011" height="365" alt="image" src="https://github.com/user-attachments/assets/37449a5a-51a0-425a-bf87-6ab2744edf71" />
<img width="986" height="369" alt="image" src="https://github.com/user-attachments/assets/0c7a1a4a-a5b9-4c63-9516-85a7ff3cea10" />
<img width="983" height="365" alt="image" src="https://github.com/user-attachments/assets/ad6e39fe-cfa6-4769-bcc2-751131537536" />

> ### 💡 Business Insight
> The box plots reveal clear behavioral differences between fraudulent and legitimate customers across all 12 features. Fraudulent users demonstrate higher return frequencies, substantially higher damage claim rates, and faster return initiation,they tend to create new accounts, make fewer genuine purchases, ship products to multiple addresses, repeatedly return high-value items, file more complaints and support tickets, frequently change payment methods, place a larger share of weekend orders, and concentrate purchases within specific product categories this overall all 12 features indicates systematic abuse rather than random customer behavior. While isolated high return rates may occur among genuine customers, combining these complementary behavioral signals enables the design of a risk-based fraud detection framework that improves detection accuracy, reduces manual investigation effort, minimizes revenue leakage, and preserves a frictionless experience for legitimate customers. This kind of analysis translates customer behavior into actionable business rules and supports data-driven fraud prevention strategies.

3-Categorical Variables (City, Category)

<img width="1003" height="406" alt="image" src="https://github.com/user-attachments/assets/73227b07-acd1-41e9-b7e4-8895723d1859" />
<img width="1028" height="356" alt="image" src="https://github.com/user-attachments/assets/7484456b-0c7d-46e7-b8a0-70fb7b1b91e0" />

> ### 💡 Business Insight
> The analysis reveals that return fraud is driven more by customer behavior and product characteristics than by geography. Fraudsters consistently target high-value product categories such as Electronics and Fashion, while city-level fraud rates remain relatively stable. These findings support the implementation of a behavior-driven fraud risk scoring framework that prioritizes high-risk transactions, reduces manual review effort, minimizes revenue leakage, and preserves a seamless return experience for legitimate customers.

4-KDE Plot for key features

<img width="991" height="297" alt="image" src="https://github.com/user-attachments/assets/c8a0ab8b-d16d-4edd-ab54-764d31de85cf" />
<img width="993" height="266" alt="image" src="https://github.com/user-attachments/assets/bca46ca6-432b-487f-ad3e-a90a8bd7ffa0" />

> ### 💡 Business Insight
> The KDE analysis demonstrates that fraudulent customers exhibit distinct behavioral patterns across multiple dimensions, including higher return rates, excessive damage claims, shorter account lifecycles, faster return initiation, increased support interactions, and concentrated purchasing behavior. The limited overlap between fraud and legitimate distributions confirms that these engineered features possess strong predictive power, supporting the development of a behavior-driven fraud detection framework that improves detection accuracy, reduces revenue leakage, and minimizes unnecessary investigations of genuine customers.

5-Outlier Detection - Finding unusual values

<img width="1005" height="377" alt="image" src="https://github.com/user-attachments/assets/735edde1-c7c5-4f76-a6dd-12111d0f40e5" />

> ### 💡 Business Insight
> Outlier analysis revealed that statistical anomalies are highly associated with fraudulent behavior. While only 5–6% of customers were classified as outliers based on Return Rate and Damage Claim Percentage, these groups contained fraud rates of 84.8% and 74.8%, respectively—far exceeding the overall fraud prevalence. In contrast, Account Age produced no statistical outliers, indicating that fraudsters typically operate within the lower end of the normal account age distribution rather than as extreme values. These findings demonstrate that outlier detection is an effective early-warning mechanism for prioritizing fraud investigations while reducing manual review effort.
>The violin plots reveal that fraudsters behave consistently rather than randomly—operating through newer accounts while exhibiting significantly higher return rates and damage claims. The clear separation between fraud and legitimate customer distributions validates these engineered features as high-value predictors for building accurate, behavior-based fraud detection models.


## Bivariate Analysis

1- The Fraud Quadrant Scatter Plot

<img width="939" height="421" alt="image" src="https://github.com/user-attachments/assets/bff79d1d-803e-4bb1-9f31-deb2cc4fe4c4" />

> ### 💡 Business Insight
> Instead of relying on individual thresholds, I combined Return Rate and Damage Claim Percentage to create a Fraud Quadrant.The Fraud Quadrant analysis demonstrates that fraudulent customers consistently combine high return rates with frequent damage claims, forming a distinct high-risk behavioral cluster. While only ~4.5% of customers fall into this quadrant, they account for a fraud rate of 98.5%, making it the highest-value segment for investigation. Conversely, nearly 89% of customers belong to the low-risk quadrant with virtually no fraud, enabling businesses to automate return approvals for genuine customers while concentrating fraud prevention efforts on a small, high-impact population. This behavior-driven segmentation improves fraud detection accuracy, reduces manual review costs, minimizes revenue leakage, and enhances customer experience.

2- Correlation Heatmap -- All Variables at Once

<img width="1105" height="467" alt="image" src="https://github.com/user-attachments/assets/fd393e10-2abd-4b7b-8e06-8c07cf10e7b0" />
<img width="484" height="250" alt="image" src="https://github.com/user-attachments/assets/33c3fc36-cb5d-49bd-a257-bc2627ffb467" />

> ### 💡 Business Insight
> The correlation analysis demonstrates that fraudulent behavior is driven by consistent customer actions rather than isolated transactions. The strongest predictors of fraud include frequent payment method changes (0.78), support ticket activity (0.75), high-value return count (0.71), damage claim percentage (0.70), unique shipping addresses (0.70), and return rate (0.68). In contrast, older account age, longer purchase history, and slower return behavior are negatively associated with fraud, reflecting genuine customer engagement. These findings validate the use of behavioral analytics as the foundation of a fraud risk scoring framework, enabling organizations to identify high-risk customers earlier, prioritize investigations, reduce revenue leakage, and minimize false positives through data-driven decision making.
> One of the key outcomes of the correlation analysis was that fraud was overwhelmingly explained by behavioral signals rather than static customer attributes. Instead of relying on a single indicator, I identified a combination of high-impact behavioral features—payment method changes, support interactions, return behavior, and shipping address diversity—that can be combined into a robust fraud risk scoring model. This approach enables earlier detection, better model performance, and more efficient allocation of fraud investigation resources.

3- Grouped Box Plot -- Numeric Variable Split by Category

<img width="1113" height="459" alt="image" src="https://github.com/user-attachments/assets/feef0370-8992-4ec5-8afb-13cac8d10b56" />

> ### 💡 Business Insight
> One important finding from the grouped box plots was that fraud behavior remained remarkably consistent across both cities and product categories.The minimal overlap between the two groups confirms that excessive return behavior is a universal fraud characteristic rather than a location- or category-specific phenomenon Instead of creating separate fraud rules for each region, the business can rely on behavioral features like Return Rate %, which consistently separates fraudulent and genuine customers. This simplifies fraud operations, improves model generalization, and enables a scalable, behavior-driven fraud detection strategy.


4- Cross-Tabulation -- Category vs Category

<img width="1087" height="445" alt="image" src="https://github.com/user-attachments/assets/12af9fd3-d66c-4b16-a26a-2bbcf4e1f786" />

> ### 💡 Business Insight
> One of the most important findings from the cross-tabulation analysis was that geography and product category had relatively little impact once behavioral risk was considered. Customers classified as Critical Risk showed nearly 100% fraud rates regardless of city or category, demonstrating that behavioral risk scoring is far more effective than static business rules. This supports a scalable, enterprise-wide fraud strategy where low-risk customers are processed automatically while investigative resources are focused on the small subset of high-risk users.

5- 

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

 ### How many total users do we have, and how many are confirmed fraudsters?
```sql
SELECT
    COUNT(*)           AS total_rows,
    SUM(is_fraud)      AS fraud_users,
    COUNT(*) - SUM(is_fraud) AS honest_users,
    ROUND(SUM(is_fraud)/COUNT(*)*100, 1) AS fraud_rate_pct
FROM user_behaviour;
```

### Business use: Operations team sees which cities need most fraud attention
```sql
select 
count(*) as total_users,
city,
sum(is_fraud) as fraud_users,
count(*) - sum(is_fraud) as honest_users,
round(sum(is_fraud)/count(*)*100,1) as return_rate_pct
from user_behaviour
group by city
order by fraud_rate_pct desc;
```

### every user whose return rate is higher than 60% -- these are high-risk accounts.
```sql
select 
user_id,city,
return_rate_pct,
damage_claim_pct,
account_age_days,
is_fraud
from user_behaviour
where return_rate_pct > 60
order by return_rate_pct desc
Limit 20;
```

### The average return rate in each city? Which city has the highest average return rate?
```sql
select city,count(*),
avg(return_rate_pct) as average_return_rate,
sum(is_fraud),
round(sum(is_fraud)/count(*)*100,2) as fraud_rate_pct
from user_behaviour
group by city
order by average_return_rate desc;
```
### Classify every user into a risk tier: CRITICAL, HIGH, MEDIUM, or LOW based on their behaviour.
```sql
select user_id,
return_rate_pct,
case when return_rate_pct >= 60 and damage_claim_pct >60 then 'critical'
when return_rate_pct >=40 and damage_claim_pct >50 then 'High'
when return_rate_pct >=20 or support_tickets_filed >5 then  'Medium'
else 'Low'
end as risk_tier,
is_fraud
from user_behaviour
limit 100;
```

### Top 10 Cities by Fraud Count
```sql
select city,
count(*) as total_users,
sum(is_fraud) as fraud_users,
round(sum(is_fraud)/count(*)*100,2) as fraud_rate_pct
from user_behaviour
group by city
order by fraud_users desc
limit 10 ;
```

### How many unique cities are in our dataset? And how many unique return categories?
```sql
select 
count(distinct(city)) as unique_city,
count(distinct(user_id)) as unique_user,
count(distinct(top_return_category)) as unique_return_category
from user_behaviour;
```
```sql
select
distinct(city)
from user_behaviour
order by city asc;
```

### Find users in Mumbai or Delhi who have return rate above 40% AND are confirmed fraud.
```sql
select user_id,return_rate_pct,
top_return_category
from user_behaviour
where(city = 'Mumbai' Or city = 'Delhi')
and return_rate_pct > 40 
and is_fraud = 1
order by return_rate_pct desc;
```

### cities where the average return rate is greater than 10%. 
```sql
select city, count(*) as total_users,
avg(return_rate_pct ) as average_return_rate_pct,
sum(is_fraud) as fraud_users
from user_behaviour
group by city
having avg(return_rate_pct) > 10
order by average_return_rate_pct desc;
```


### users in cities containing 'a' in the name, and create a display string.
```sql
select user_id, upper(city),
concat(user_id,'|',city) as user_city,
concat('Fraud:', if(is_fraud = 1, 'Yes','No')) as fraud_status
from user_behaviour
where city like '%a%'
limit 15; 
```

### users whose accounts were created within the last 6 months (account_age < 180 days).
```sql
select user_id, city, account_age_days,is_fraud,
case when account_age_days < 30 then 'New'
when account_age_days < 60 then 'Growing'
when account_age_days < 180 then 'Established'
else 'Old'
end as account_stage
from user_behaviour
where account_age_days <  180
order by account_age_days desc;
```

### Users who have return rates higher than the AVERAGE return rate of fraud users. 
```sql
select user_id, return_rate_pct from user_behaviour
where return_rate_pct > (select avg(return_rate_pct) from user_behaviour where is_fraud = 1)
order by return_rate_pct desc;
```

### Complete statistical summary of return rates for fraud vs honest users.
```sql
select if(is_fraud = 1 ,'Fraud','Honest') as is_fraud,
max(return_rate_pct) as maximum_return_rate_pct,
min(return_rate_pct) as minimum_return_rate_pct,
avg(return_rate_pct) as average_return_rate_pct,
sum(return_rate_pct) as summation_ofreturn_rate_pct,
round(stddev(return_rate_pct),2) as standard_deviation_of_return_rate_pct
from user_behaviour
group by is_fraud;
```

### City fraud summary
```sql
select city,
count(*) as total_users,
sum(is_fraud) as fraud_users,
count(*)-sum(is_fraud) as honest_users,
avg(return_rate_pct) as avg_return_pct
from user_behaviour
group by city;
```

### What is the fraud rate for each city AND each return category combination?
```sql
select city,top_return_category,
sum(is_fraud) as fraud_users,
count(*) as total_users,
round(sum(is_fraud)/count(*)*100,2) as fraud_rate_pct
from user_behaviour
group by city , top_return_category
order by fraud_rate_pct desc;
```

### Users whose return rate is above the average return rate OF THEIR OWN CITY.
```sql
with city_avg as(
select city,
avg(return_rate_pct) as average_return_rate_pct
from user_behaviour
group by city)
select ub.user_id,
ub.city,
ub.return_rate_pct,
ub.is_fraud,
ca.average_return_rate_pct
from user_behaviour as ub
join city_avg as ca
on ub.city=ca.city
where ub.return_rate_pct > ca.average_return_rate_pct 
order by ub.return_rate_pct;
```

### For each city, show the city's fraud count AND the overall total fraud count side by side.
```sql
with city_stats as (
select city , sum(is_fraud) as fraud_count
from user_behaviour
group by city 
),
overall as  (
select sum(is_fraud) as total_fraud
from user_behaviour
)
Select cs.city, cs.fraud_count,o.total_fraud
from city_stats  cs cross join overall o;
```
    
### Cities having more than the average number of fraud users.
```sql
with fraud_count as(
select city, count(*) as fraud_users
from user_behaviour
where is_fraud = 1
group by city),
avg_fraud as (
select avg(fraud_users) as avg_count
from fraud_count
)
select fc.city,fc.fraud_users, af.avg_count
from fraud_count fc
cross join avg_fraud af
where fc.fraud_users > af.avg_count; 
```

### Find cities where the fraud rate is above the overall average fraud rate.
```sql
with fraud_rate as(
select city , sum(is_fraud)/count(*)*100 as fraud_return_rate
from user_behaviour
group by city),
average_rate as(
select round(sum(is_fraud)/count(*)*100,2)as average_fraud_rate
from user_behaviour)
select fr.city,fr.fraud_return_rate, ar.average_fraud_rate from fraud_rate as fr
cross join average_rate as ar
where fr.fraud_return_rate > ar.average_fraud_rate;
```

### Compare fraud detection performance of 4 different rule-based approaches in one output
```sql
select 'Rule 1: Return>50%' as rule_name,count(*) as users_flagged,
sum(is_fraud) as fraud_caught,round(sum(is_fraud)/count(*)*100,1) as precision_pct
from user_behaviour 
where return_rate_pct > 50
union all
select 'Rule 2: Damage>60%', count(*),
sum(is_fraud) ,round(sum(is_fraud)/count(*)*100,1) 
from user_behaviour where damage_claim_pct > 60
union all
select 'Rule 3: Return>40% AND Damage>40%',
count(*),sum(is_fraud) ,round(sum(is_fraud)/count(*)*100,1)    
from user_behaviour where return_rate_pct>40 and damage_claim_pct>40
union all
select 'Rule 4: Three signals',count(*),sum(is_fraud) ,round(sum(is_fraud)/count(*)*100,1) 
from user_behaviour
where return_rate_pct>40 and damage_claim_pct>40 and account_age_days<365 
order by  precision_pct desc;
```

### Find pairs of users who share the same delivery address count AND have high return rates.
```sql
select a.user_id, b.user_id,a.city,a.unique_addresses_used, a.return_rate_pct,b.return_rate_pct
from user_behaviour as a
inner join user_behaviour as b
on a.city = b.city and a.unique_addresses_used = b.unique_addresses_used and a.user_id<b.user_id
where a.return_rate_pct >40 and b.return_rate_pct >40 and a.unique_addresses_used >4
order by a.return_rate_pct desc
limit 20;
```

### For each city, show a comma-separated list of unique return categories used by fraud users
```sql
SELECT city, count(*) as fraud_users,
GROUP_CONCAT(distinct top_return_category
order by  top_return_category asc SEPARATOR ' | ') as categories_used,
GROUP_CONCAT(distinct top_return_category order by top_return_category asc SEPARATOR ',') as categories_csv
from user_behaviour
where is_fraud = 1
group by city
order by fraud_users DESC;
```

### Summary report with city totals AND a grand total row at the bottom.
```sql
select coalesce(city , 'grand city total') as city ,
count(*) as total_users, sum(is_fraud) as fraud_users,
ROUND(SUM(is_fraud)/COUNT(*)*100, 1)  as fraud_rate_pct,
ROUND(AVG(return_rate_pct), 1) as avg_return_rate
from user_behaviour
group by  city with rollup;
```

### Users in specific return rate brackets for the fraud operations team's daily report.
```sql
select case when return_rate_pct between 0 and 19 then '0-19% (Normal)'
when return_rate_pct between 0 and 19 then '20-40% watch)'
when return_rate_pct between 41 and 59 then '41-59% (Review)'
when return_rate_pct between 60 and 79 then '60-79% (Suspicious)'
when return_rate_pct between 80 and 100 then '80-100%(Block)'
end  as return_rate_bucket,
count(*) as total_users,							
sum(is_fraud) as fraud_users,
round(sum(is_fraud)/count(*)*100,2) as fraud_rate_pct,
avg(damage_claim_pct)as avg_damage_claim
from user_behaviour
group by return_rate_bucket
order by MIN(return_rate_pct); 
```

### Rank users by return rate within each city. Show which user has the highest return rate per city
```sql
select user_id, city, return_rate_pct , rank() over(partition by city order by return_rate_pct desc) as rank_of_city,
dense_rank() over(partition by city order by return_rate_pct desc) as denserank_of_city
from user_behaviour
order by city,rank_of_city;
```

### Divide users into quartiles based on return rate. Show fraud rate in each quartile.
```sql
with user_quartiles as (
select user_id, return_rate_pct, is_fruad,
ntile(4) over(order by return_rate_pct) as quartile
from user_behaviour)
select 
quartile, count(*) as total_users, sum(is_fraud) as fraud_users, round(sum(is_fraud)*100/count(*),2) as fraud_rate_pct 
from user_quartiles 
group by quartile order by quartile;
```

### Compare each city's fraud rate to the previous city (when sorted by fraud rate). Show trend.
```sql
with fraud_rate as(
select city, sum(is_fraud) as fraud_users,
round(sum(is_fraud)/count(*)*100,2) as fraud_rate_pct
from user_behaviour
group by city)
select city,fraud_rate_pct,
lag(fraud_rate_pct,1) over(order by fraud_rate_pct) as previous_fraud_rate,
lead(fraud_rate_pct,1) over(order by fraud_rate_pct)as next_fraud_rate,
fraud_rate_pct - lag(fraud_rate_pct,1) over(order by fraud_rate_pct) as diff_rate,
case when lag(fraud_rate_pct) over(order by fraud_rate_pct) is null then 'First City'
when fraud_rate_pct > lag(fraud_rate_pct) over(order by fraud_rate_pct) then 'Increase'
when fraud_rate_pct < lag(fraud_rate_pct)  over(order by fraud_rate_pct) then 'Decrease'
else 'No Change'end as Trend
from fraud_rate;
```

### Calculate a running total of fraud users as you go through cities sorted by fraud count.
```sql
with city_fraud as(
select city, sum(is_fraud) as fraud_count,
Round(sum(is_fraud)/COUNT(*)*100, 1) as fraud_rate_pct
from user_behaviour
group by city
)
select city,fraud_count,fraud_rate_pct,sum(fraud_count) over(order by fraud_count desc) as running_total_fraud,
sum(fraud_count) over() as grand_total,
round(sum(fraud_count) over(order by fraud_count desc)/sum(fraud_count) over()*100,1) as cumulative_total
from city_fraud;
```

### Use a CTE to calculate the average return rate per city, then find users above their city average.
```sql
with city_averages as (
select city, round(avg(return_rate_pct),2) as city_avg_return_rate,count(*) as city_user_count
from  user_behaviour
group by city
)
select ub.user_id,ub.city,ub.return_rate_pct,ca.city_avg_return_rate,
ROUND(ub.return_rate_pct - ca.city_avg_return_rate, 1) as above_city_avg_by,
ub.is_fraud
from user_behaviour as ub
inner join city_averages as ca
on ub.city = ca.city
where ub.return_rate_pct > ca.city_avg_return_rate
order by above_city_avg_by desc
limit 30;
```

### If a user appears multiple times (from multiple data loads), keep only the most recent record.
```sql
with ranked_users as (
select *, row_number() over( partition by user_id order by account_age_days desc) as rn
from user_behaviour)
select user_id, city, return_rate_pct, damage_claim_pct,
account_age_days, is_fraud
from  ranked_users
where rn = 1 
order by user_id;
```

### Build a fraud analytics pipeline: city stats, risk tiers, and final summary.
```sql
with city_stats as(
select city, count(*) as total_users, sum(is_fraud) as fraud_count,
round(sum(is_fraud)/count(*)*100,1) as fraud_rate_pct,
round(avg(return_rate_pct),1) as avg_return_rate
from user_behvaiour
group by city
),
city_risk as (
select *,
case when fraud_rate_pct >=7 then 'High Risk City'
when fraud_rate_pct >= 5  then 'MODERATE RISK'
else 'NORMAL'
end  as city_risk_tier
from city_stats
),
city_ranked as (
select *,
rank() over (order by fraud_rate_pct desc)  as fraud_rank,
rank() over (order by avg_return_rate desc) as return_ran
from city_risk
)
SELECT * FROM city_ranked;
```


### Pivot the data to show fraud count by city across different risk tiers in column format.
```sql
select city,
SUM(case when return_rate_pct >= 60 and damage_claim_pct >= 60
and is_fraud=1 then 1 else 0 end) as critical_fraud,
SUM(case when (return_rate_pct between 40 and 59
or damage_claim_pct between 50 and 59)
and is_fraud=1 then 1 else 0 end)    as high_fraud,
SUM(case when return_rate_pct between  20 and 39
and is_fraud=1 then 1 else 0 end)    as medium_fraud,
SUM(case when return_rate_pct < 20
and is_fraud=1 then 1 else 0 end)    as low_fraud,
SUM(is_fraud) as total_fraud,
COUNT(*) as total_users,
ROUND(SUM(is_fraud)/COUNT(*)*100, 1) as overall_fraud_rate
from user_behaviour
group by city
order by  total_fraud desc;
```

### Query Optimizations
```sql
create index idx_city on  user_behaviour(city);
create index idx_is_fraud on user_behaviour(is_fraud);
create index idx_return_rate on user_behaviour(return_rate_pct);
create index idx_city_fraud on user_behaviour(city, is_fraud);
```

### CHECK how query uses indexes
```sql
Explain select user_id, return_rate_pct, is_fraud
from  user_behaviour
where city = 'Mumbai'
and is_fraud = 1
and return_rate_pct > 50
order by return_rate_pct desc;
```

```sql
select user_id, return_rate_pct, is_fraud
from user_behaviour
where city = 'Mumbai'   
and is_fraud = 1
and return_rate_pct > 50
order by return_rate_pct desc;
```

### Calculate a 3-record moving average of fraud rates across cities sorted by city name.
```sql
with city_fraud_rates AS (
select city,ROUND(SUM(is_fraud)/COUNT(*)*100, 2) AS fraud_rate_pct
from user_behaviour
group by city
order by city
),
city_with_row as (
select *,row_number() over (order by city) as rn
from city_fraud_rates
)
select city,fraud_rate_pct,
ROUND(AVG(fraud_rate_pct) over (order by rn rows between 1 preceding and 1 following), 2)  as moving_avg_3,
ROUND(AVG(fraud_rate_pct) over (order by rn rows between unbounded preceding and current row), 2) as cumulative_avg,
SUM(fraud_rate_pct) over (order by rn rows between 1 preceding and 1 following) as moving_sum_3
from city_with_row
order by rn;
```




