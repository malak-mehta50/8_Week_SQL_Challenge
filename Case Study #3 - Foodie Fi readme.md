<h1>Case Study #3 - Foodie - Fi 🫕🥑</h1>
<img width="500" height="643" alt="Screenshot 2025-08-12 at 2 19 25 PM 2" src="https://github.com/user-attachments/assets/f4aef5ef-cba9-4632-bb76-a2f595dd1e60" />


<h1><a name="🧾 introduction">🧾  Introduction</a></h1>
Welcome to Foodie-Fi, a subscription-based streaming service offering exclusive food-related content from around the world. In this project, I analyzed customer subscription data to uncover insights on plan preferences, churn patterns, and upgrade timelines. Using SQL, I explored behavioral trends to help optimize retention strategies and drive business growth.

<h1><a name="❓ Problem Statement">❓ Problem Statement</a></h1>
Foodie-Fi, a subscription-based streaming startup, wants to better understand customer behavior to improve retention and plan offerings. The business needs clarity on how users move between plans, how quickly they upgrade, and why some churn after the free trial.

Danny wants to explore:

What percentage of customers churn and when?

How customers transition between different plans?

How long it takes for customers to upgrade to an annual subscription?

The goal is to analyze subscription trends and generate actionable insights for growth.

<h1><a name = "dateset"> 🧾 Dataset Provided</a></h1>
The case study is centered around two tables:

plans – Details of all subscription plans, including the free trial, monthly, and annual options, with plan_id, plan_name, and price.

subscriptions – Records of each customer’s subscription journey, including customer_id, plan_id, and start_date.

<h1><a name = "entity diagram"> 🔗 Entity Relation Diagram (ERD)</a></h1>

<img width="500" height="251" alt="Screenshot 2025-08-12 at 2 24 42 PM 2" src="https://github.com/user-attachments/assets/aa698ae7-8254-410b-8ae1-91f83848e748" />

<h1><a name= "Case study"> Case Study Questions & Answers</a></h1>


<h4><a name="a.customerjourney"></a>A. Customer Journey 🫕🥑 </h4>

Q1. Based off the 8 sample customers provided in the sample from the subscriptions table, write a brief description about each customer’s onboarding journey.
```sql 
SELECT s.customer_id, p.plan_name, s.start_date
FROM plans as p
JOIN subscriptions as s
USING (plan_id)
WHERE s.customer_id IN (1, 2, 11, 13, 15, 16, 18,19)
ORDER BY s.customer_id;
```
<img width="377" height="500" alt="q1" src="https://github.com/user-attachments/assets/619a0dcd-369a-4a9e-8a70-266481671882" />

<h6>Answer:</h6>

<ul>
<li>This query retrieves the <code>onboarding journey</code> for a sample of 8 customers.</li>
<li>It does this by using a <code>JOIN</code> on the <code>plans</code> and <code>subscriptions</code> tables to link customer IDs to their plan names and start dates.</li>
<li>The <code>WHERE</code> clause filters the results to only include the specific customers in the sample.</li>
</ul>

<h4><a name="b.dataanalysisquestions"></a>B. Data Analysis 📊</h4>

Q1. How many customers has Foodie-Fi ever had?

```sql
SELECT COUNT(DISTINCT customer_id) as total_count_of_customers
FROM subscriptions;
```
<img width="216" height="64" alt="q2" src="https://github.com/user-attachments/assets/fe5daea3-02d9-4811-ba57-6ccaf1487081" />

<h6>Answer:</h6>

<ul>
<li>This query calculates the <code>total number of unique customers</code> that Foodie-Fi has ever had.</li>
<li>It does this by using the <code>COUNT(DISTINCT customer_id)</code> function on the <code>subscriptions</code> table.</li>
<li>Counting the distinct customer IDs ensures that each customer is only counted once, even if they have had multiple subscriptions.</li>
</ul>

Q2. What is the monthly distribution of trial plan start_date values for our dataset - use the start of the month as the group by value.
```sql
SELECT DATE_TRUNC('month',start_date) as month_start, COUNT(*) as trial_starts
FROM subscriptions
JOIN plans
USING (plan_id)
WHERE plan_name = 'trial'
GROUP BY month_start
ORDER BY month_start;
```

<img width="306" height="344" alt="q3" src="https://github.com/user-attachments/assets/eb6e0b21-f2d9-4284-a56c-388365cf551c" />

<h6>Answer:</h6>

<ul>
<li>This query shows the <code>monthly distribution of trial plan</code> start dates.</li>
<li>It does this by using the <code>DATE_TRUNC('month', start_date)</code> function to group the trial plans by the first day of each month.</li>
<li>The query then uses <code>COUNT(*)</code> to count how many trial subscriptions started in each given month.</li>
</ul>

Q3:What plan start_date values occur after the year 2020 for our dataset? Show the breakdown by count of events for each plan_name.

```sql
SELECT p.plan_id, p.plan_name, COUNT(*) as event_count
FROM plans as p
JOIN subscriptions as s
USING (plan_id)
WHERE s.start_date > '2020-12-31'
GROUP BY p.plan_id, p.plan_name
ORDER BY p.plan_id;
```
<img width="360" height="139" alt="q4" src="https://github.com/user-attachments/assets/1f3aea5f-37c8-4e79-bfd5-4186d58d4e26" />

<h6>Answer:</h6>
<ul>
<li>This query breaks down the <code>count of events for each plan</code> that started after the year 2020.</li>
<li>It does this by joining the <code>plans</code> and <code>subscriptions</code> tables and using a <code>WHERE</code> clause to filter for <code>start_date</code> values greater than <code>'2020-12-31'</code>.</li>
<li>The results are then grouped by <code>plan_id</code> and <code>plan_name</code> to get the event count for each plan.</li>
</ul>


Q4:What is the customer count and percentage of customers who have churned rounded to 1 decimal place?
```sql
WITH churned AS (
  SELECT COUNT(DISTINCT s.customer_id) AS churn_count
  FROM subscriptions s
  JOIN plans p USING (plan_id)
  WHERE p.plan_id = 4
)
SELECT 
  c.churn_count,
  COUNT(DISTINCT s.customer_id) AS total_customers,
  ROUND(c.churn_count * 100.0 / COUNT(DISTINCT s.customer_id), 1) AS percentage_churn
FROM subscriptions s
CROSS JOIN churned c
GROUP BY c.churn_count;
```
<img width="392" height="63" alt="q5" src="https://github.com/user-attachments/assets/d8007ed8-c316-45a8-a668-19a0e508dc92" />

<h6>Answer:</h6>
<ul>
<li>This query calculates the <code>total count and percentage of customers who have churned</code>.</li>
<li>It does this by using a <code>WITH</code> clause to first count customers with a <code>plan_id</code> of 4, and then <code>CROSS JOIN</code>ing that result with the total number of customers.</li>
<li>The query then calculates the percentage of churned customers by dividing the <code>churn_count</code> by the total number of customers.</li>
</ul>


Q5:How many customers have churned straight after their initial free trial - what percentage is this rounded to the nearest whole number?
```sql
WITH sequenced as (
SELECT customer_id, start_date, plan_id,
LEAD(plan_id) OVER (PARTITION BY customer_id ORDER BY start_date) as next_plan_id
FROM subscriptions
),
immediate_churners as (
SELECT DISTINCT customer_id
FROM sequenced 
WHERE plan_id = 0 and next_plan_id = 4
),
totals as (
SELECT COUNT(DISTINCT customer_id) as total_customers
FROM subscriptions
)
SELECT (SELECT COUNT(*) FROM immediate_churners) as churn_after_trial_count,
ROUND(
    100.0 * (SELECT COUNT(*) FROM immediate_churners)
          / t.total_customers
  , 0) AS churn_after_trial_pct
FROM totals as t;
```
<img width="346" height="66" alt="q6" src="https://github.com/user-attachments/assets/7cf8d765-bfe6-42cc-aa51-4991ac240712" />

<h6>Answer:</h6>

<ul>
<li>This query finds the <code>number and percentage of customers who churned immediately after their free trial</code>.</li>
<li>It does this by using the <code>LEAD()</code> window function to find the next plan a customer moved to after their current one.</li>
<li>The query then identifies customers who moved from a trial plan (<code>plan_id = 0</code>) to a churn plan (<code>plan_id = 4</code>).</li>
</ul>

Q6:What is the number and percentage of customer plans after their initial free trial?
```sql
WITH sequenced as (
SELECT customer_id, start_date, plan_id,
LEAD(plan_id) OVER (PARTITION BY customer_id ORDER BY start_date) as next_plan_id
FROM subscriptions
),
post_trial_converter as (
SELECT DISTINCT customer_id
FROM sequenced 
WHERE plan_id = 0 AND next_plan_id <> 4 AND next_plan_id IS NOT NULL
),
totals as (
SELECT COUNT(DISTINCT customer_id) as total_customers
FROM subscriptions
)
SELECT (SELECT COUNT(*) FROM post_trial_converter) as converted_after_trial_count,
ROUND(
    100.0 * (SELECT COUNT(*) FROM post_trial_converter)
          / t.total_customers
  , 0) AS converted_after_trial_pct
FROM totals as t;
```

<img width="394" height="64" alt="q7" src="https://github.com/user-attachments/assets/05ae986f-430f-4921-b9e0-174e478876b9" />

<h6>Answer:</h6>
<ul>
<li>This query determines the <code>number and percentage of customers who converted</code> to a paid plan after their initial free trial.</li>
<li>It does this by using the <code>LEAD()</code> window function to identify the next plan after a customer's trial plan (<code>plan_id = 0</code>).</li>
<li>The query then counts the customers whose next plan was not a churn plan (<code>next_plan_id <> 4</code>) and calculates their percentage of the total customer base.</li>
</ul>

Q7: What is the customer count and percentage breakdown of all 5 plan_name values at 2020-12-31?
```sql
WITH snap as(
SELECT s.customer_id, p.plan_name,
ROW_NUMBER () OVER (PARTITION BY s.customer_id ORDER BY s.start_date DESC) as RN
FROM subscriptions as s
JOIN plans as p
USING (plan_id)
WHERE start_date <= '2020-12-31'
)
SELECT plan_name, COUNT (*) as customer_count,
ROUND(
    COUNT(*)::numeric / SUM(COUNT(*)) OVER () * 100, 1
  ) AS percentage
FROM snap
where RN = 1
GROUP BY plan_name
ORDER BY plan_name;
```
<img width="408" height="169" alt="q8" src="https://github.com/user-attachments/assets/1cba7591-1735-4a94-bcf6-3532e807bc63" />

<h6>Answer:</h6>

<ul>
<li>This query shows the <code>customer count and percentage breakdown for all 5 plan types</code> as of December 31, 2020.</li>
<li>It does this by using the <code>ROW_NUMBER()</code> window function to find the most recent plan for each customer on or before that date.</li>
<li>The query then groups by <code>plan_name</code> and calculates the percentage of customers for each plan at that specific point in time.</li>
</ul>

Q8: How many customers have upgraded to an annual plan in 2020?
```sql
WITH upgrade as(
SELECT s.customer_id, p.plan_name,
ROW_NUMBER () OVER (PARTITION BY s.customer_id ORDER BY s.start_date DESC) as RN
FROM subscriptions as s
JOIN plans as p
USING (plan_id)
WHERE start_date <= '2020-12-31' AND p.plan_name = 'pro annual'
)
SELECT COUNT (*) as upgraded_customer_count
FROM upgrade
where RN = 1
GROUP BY plan_name
ORDER BY plan_name;
```
<img width="222" height="63" alt="q9" src="https://github.com/user-attachments/assets/2861404f-c1f1-47ca-ace2-ed6d51273635" />

<h6>Answer:</h6>

<ul>
<li>This query counts the <code>number of customers who upgraded to an annual plan</code> during the year 2020.</li>
<li>It does this by using the <code>ROW_NUMBER()</code> window function to find the latest plan for each customer, but only considering those with a <code>plan_name</code> of 'pro annual' and a <code>start_date</code> in 2020.</li>
<li>The final count represents the total number of customers who met this criteria.</li>
</ul>

Q9:How many days on average does it take for a customer to an annual plan from the day they join Foodie-Fi?
```sql
WITH trial AS (
  SELECT customer_id, MIN(start_date) AS trial_date
  FROM subscriptions
  WHERE plan_id = 0
  GROUP BY customer_id
),
annual AS (
  SELECT customer_id, MIN(start_date) AS annual_date
  FROM subscriptions
  WHERE plan_id = 3
  GROUP BY customer_id
),
diffs AS (
  SELECT
    t.customer_id,
    a.annual_date - t.trial_date AS delta   -- this is an INTERVAL
  FROM trial t
  JOIN annual a USING (customer_id)
)
SELECT 
  ROUND(AVG(annual_date - trial_date), 1) AS avg_days_to_annual
FROM trial
JOIN annual USING (customer_id)
WHERE annual_date > trial_date;
```

<img width="185" height="65" alt="q10" src="https://github.com/user-attachments/assets/c4a09f80-c915-4941-b2d1-4cbecd169c4d" />

<h6>Answer:</h6>

<ul>
<li>This query calculates the <code>average number of days</code> it takes for a customer to upgrade to an annual plan from their initial joining date.</li>
<li>It does this by using <code>WITH</code> clauses to find the minimum <code>start_date</code> for both the trial plan and the annual plan for each customer.</li>
<li>The query then calculates the average of the difference between these two dates across all customers who made the upgrade.</li>
</ul>

<h1><a name="Key insights">Key Insights</a></h1>
- The analysis identified the overall churn rate and a specific segment of customers who churned immediately after their free trial. 💔

- A significant number of customers successfully converted from a free trial to a paid plan, indicating the trial's effectiveness. 🎉

- The data reveals monthly patterns in trial plan sign-ups and shows the distribution of all plan types over time. 📈

- An average timeline for customers to upgrade to an annual plan from their initial sign-up was established. 🗓️

- A snapshot of the customer base at the end of 2020 provides a clear breakdown of plan distribution at that time. 📸
