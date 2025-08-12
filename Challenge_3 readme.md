<h1>Case Study #3 - Foodie - Fi 🫕🥑</h1>
<img width="711" height="643" alt="Screenshot 2025-08-12 at 2 19 25 PM 2" src="https://github.com/user-attachments/assets/f4aef5ef-cba9-4632-bb76-a2f595dd1e60" />


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

<img width="669" height="251" alt="Screenshot 2025-08-12 at 2 24 42 PM 2" src="https://github.com/user-attachments/assets/aa698ae7-8254-410b-8ae1-91f83848e748" />

<h1><a name= "Case study"> Case Study Questions & Answers</a></h1>

Q1. Based off the 8 sample customers provided in the sample from the subscriptions table, write a brief description about each customer’s onboarding journey.
SELECT s.customer_id, p.plan_name, s.start_date
FROM plans as p
JOIN subscriptions as s
USING (plan_id)
WHERE s.customer_id IN (1, 2, 11, 13, 15, 16, 18,19)
ORDER BY s.customer_id;









