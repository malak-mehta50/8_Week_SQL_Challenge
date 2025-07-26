<h1>Case Study #1 - Danny's Diner👨🏻‍🍳</h1>
<img width="500" alt="Coding" src="https://github.com/malak-mehta50/images/blob/b10e4253a6875d2f44ac709d5e1059f845ee6750/new%20ss.png"
  
## 

<h1><a name="🧾 introduction">🧾  Introduction</a></h1>

In early 2021, Danny pursued his dream of opening Danny’s Diner, a cozy Japanese restaurant serving sushi, curry, and ramen. While the restaurant quickly became popular, Danny lacked the data analytics expertise needed to fully understand his customers' behavior and optimize business operations using the data collected during the first few months.

To make informed business decisions and improve customer engagement, Danny is seeking help analyzing the available data and building a foundation for data-driven growth.

<h1><a name="❓ Problem Statement">❓ Problem Statement</a></h1>

Danny wants to leverage his customer data to uncover insights into:

- Customer visit patterns

- Spending behavior

- Popular menu items

His ultimate goal is to design a personalized customer loyalty program and make strategic business decisions based on data. He also wants easily accessible datasets that his non-technical team can review without writing SQL themselves.

Due to privacy concerns, Danny has shared a sample dataset representing real customer activity — and your challenge is to turn this into actionable insights using SQL.

<h1><a name = "dateset"> 🧾 Dataset Provided</a></h1>

The case study is centered around three tables:

sales – Records of customer orders

menu – Menu items and their prices

members – Customers who joined the loyalty program

<h1><a name = "entity diagram"> 🔗 Entity Relation Diagram (ERD)</a></h1>

<img width="500" alt="Coding" src="https://github.com/malak-mehta50/images/blob/427f7ff220d7c15a7ebcc7400652d59284503c2e/erd.png">

<h1><a name= "Case study"> Case Study Questions & Answers</a></h1>

1. What is the total amount each customer spent at the restaurant?

```sql
SELECT customer_id, SUM(price) as Total_Spend
FROM sales as s
JOIN menu as m
ON s.product_id = m.product_id
GROUP BY customer_id
ORDER BY customer_id;
```
<img width="500" alt="Coding" scr="https://github.com/malak-mehta50/images/blob/2afcd60a1284b536458d391bcc988821e3a625a0/q2.png">

<h6>Answer:</h6>
