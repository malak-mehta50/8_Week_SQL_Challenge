<h1>Case Study #1 - Danny's Diner👨🏻‍🍳</h1>
<img width="500" height="706" alt="DD" src="https://github.com/user-attachments/assets/2a434473-3645-4d90-9754-cd368b15ab80" />


  
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

<img width="500" height="367" alt="erd" src="https://github.com/user-attachments/assets/8a5dd890-ca49-4d2f-a8db-e5766bae852c" />


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
<img width="200" height="118" alt="q1" src="https://github.com/user-attachments/assets/10434423-3185-4a01-a4fd-5fded66c18f9" />


<h6>Answer:</h6>

  <li>This query calculates the <code>total amount</code> spent by each customer at Danny's Diner.</li>

  <li> It performs an inner join between the <code>sales</code> and <code>menu</code> tables using the <code> product_id</code> to combine purchase records with their corresponding prices.

  <li>The <code>SUM(M.price)</code> function totals up the amount each customer spent based on all their purchases.</li>

  <li>The results are grouped by <code>customer_id</code>, allowing a per-customer summary.</li>

  <li>Lastly, the output is sorted alphabetically by the <code>customer's ID</code> for clarity.</li>

2. How many days has each customer visited the restaurant?

```sql
SELECT customer_id, COUNT (DISTINCT order_date) as Visits
FROM sales
GROUP BY customer_id
ORDER BY customer_id; 
```
<img width="200" height="119" alt="q2" src="https://github.com/user-attachments/assets/136598cd-7184-4a27-8608-36f1f4956521" />



<h6>Answer:</h6>

<ul> <li>This query determines how many <code>distinct days</code> each customer visited the restaurant.</li> 

<li>It uses the <code>COUNT(DISTINCT order_date)</code> function to count the number of unique days tied to each <code>customer_id</code>.</li> 

<li>The data comes directly from the <code>sales</code> table, which records each purchase and its associated <code>order_date</code>.</li> 

<li>The results are grouped by <code>customer_id</code> so that each customer's visits are counted individually.</li>

 <li>The final output is sorted by <code>customer_id</code> in ascending order.</li> </ul>

3. What was the first item from the menu purchased by each customer?

```sql
WITH CTE AS (
SELECT customer_id, order_date, product_name,
RANK() OVER (PARTITION BY customer_id ORDER BY order_date) AS rank
FROM sales as s
JOIN menu as m
ON s.product_id = m.product_id)

SELECT customer_id , product_name
FROM CTE
WHERE rank = 1;
;
```
<img width="200" height="164" alt="q3" src="https://github.com/user-attachments/assets/d6c013d6-275a-47bf-bb38-2c60a24c5a17" />


<ul> <li>This query identifies the <code>first item</code> each customer purchased based on the <code>order_date</code>.</li>

<li>A <code>Common Table Expression (CTE)</code> is used to calculate the <code>rank</code> of menu items ordered per customer by date.</li> 

<li>The <code>RANK()</code> window function is applied to assign a ranking of 1 to the earliest order for each <code>customer_id</code>.</li> 

<li>The final query filters only the rows with <code>rank = 1</code>, returning each customer's first item.</li> <li>The <code>JOIN</code> ensures product names are matched with their respective IDs from the menu.</li> </ul>

4. What is the most purchased item on the menu and how many times was it purchased by all customers?

```sql
SELECT product_name , COUNT(order_date) as orders
FROM sales as s
JOIN menu as m
on s.product_id = m.product_id
GROUP BY product_name
ORDER BY COUNT(order_date) DESC
LIMIT 1;
```

<img width="200" height="69" alt="q4" src="https://github.com/user-attachments/assets/d9f52ef4-b8e0-4a92-94a5-026a77aa25e1" />

<ul> <li>This query determines the <code>most frequently purchased</code> item across all customers.</li>

 <li>It performs a <code>JOIN</code> between the <code>sales</code> and <code>menu</code> tables to link <code>product_id</code> with <code>product_name</code>.</li> 

<li>The <code>COUNT(order_date)</code> function calculates how many times each item appears in the sales data.</li> 

<li>The results are <code>GROUPED</code> by <code>product_name</code> and sorted in <code>descending order</code> of count.</li> <li><code>LIMIT 1</code> ensures that only the top-selling item is returned.</li> </ul>

5. Which item was the most popular for each customer?

```sql
WITH CTE as(
SELECT customer_id, product_name, count(order_date) as order,
RANK () OVER (PARTITION BY customer_id ORDER BY COUNT(order_date) DESC) as rank
FROM sales as s
JOIN menu as m
on s.product_id = m.product_id
GROUP BY customer_id, product_name
)
SELECT customer_id, product_name
FROM CTE
WHERE rank = 1;
;
```
<img width="200" height="167" alt="q5" src="https://github.com/user-attachments/assets/cca87289-e9c7-4b2b-9f7f-e3c289bf9be5" />


<ul> <li>This query finds each customer's <code>most frequently ordered</code> item.</li>

 <li>A <code>CTE</code> is used to calculate the total number of times each <code>product_name</code> was ordered by each <code>customer_id</code>.</li> 

<li>The <code>RANK()</code> function ranks menu items per customer by <code>order count</code> in descending order.</li> 

<li>Only the top-ranked (<code>rank = 1</code>) item(s) per customer are selected in the final output.</li> 

<li>This reveals the favorite item for each individual customer based on their purchase history.</li> </ul>

6. Which item was purchased first by the customer after they became a member?

```sql
WITH CTE as(
 SELECT s.customer_id, order_date, join_date, product_name,
 ROW_NUMBER () OVER (PARTITION BY s.customer_id ORDER BY order_date) as first_item
 FROM Sales as s
 INNER JOIN menu as m
 ON s.product_id = m.product_id
 INNER JOIN members as mem
 ON s.customer_id= mem.customer_id
 WHERE s.order_date >= join_date 
)
SELECT customer_id, product_name
FROM CTE
WHERE first_item = 1;
```
<img width="200" height="96" alt="q6" src="https://github.com/user-attachments/assets/a8d365d5-c455-4778-8e32-46d1d589bd7a" />



<ul> <li>This query finds the <code>first purchase made after membership</code> activation for each customer.</li> 

<li>It joins <code>sales</code>, <code>menu</code>, and <code>members</code> to align orders with join dates and product names.</li>

 <li>The <code>WHERE s.order_date &gt;= mem.join_date</code> filter ensures we only consider post-membership orders.</li>

 <li><code>ROW_NUMBER()</code> ranks each customer's qualifying orders by <code>order_date</code>.</li> 

<li>Finally, we keep only the rows where <code>first_item = 1</code>, i.e., their very first post-join purchase.</li> </ul>

7. Which item was purchased just before the customer became a member?

```sql
WITH CTE as(
 SELECT s.customer_id, order_date, join_date, product_name,
 RANK () OVER (PARTITION BY s.customer_id ORDER BY order_date DESC) as first_item
 FROM Sales as s
 INNER JOIN menu as m
 ON s.product_id = m.product_id
 INNER JOIN members as mem
 ON s.customer_id= mem.customer_id
 WHERE s.order_date < join_date 
)
SELECT customer_id, product_name
FROM CTE
WHERE first_item = 1;
```
<img width="200" height="117" alt="q7" src="https://github.com/user-attachments/assets/802ebe2a-e5db-40ba-b68e-0603f51719ed" />



<ul> <li>This query looks for the purchase that happened <code>immediately before</code> each customer joined the program.</li> 

<li>Only rows with <code>s.order_date &lt; mem.join_date</code> are considered (pre-membership).</li> 

<li><code>ROW_NUMBER()</code> with <code>ORDER BY s.order_date DESC</code> assigns rank 1 to the latest pre-join purchase.</li> 

<li>The final selection keeps <code>rn = 1</code> to return that “just before joining” item.</li> </ul>

8. What is the total items and amount spent for each member before they became a member?

```sql
SELECT s.customer_id, SUM(price) as total_spent, COUNT(product_name) as total_item
FROM Sales as s
INNER JOIN menu as m
ON s.product_id = m.product_id
INNER JOIN members as mem
ON s.customer_id= mem.customer_id
WHERE order_date < join_date 
GROUP BY s.customer_id;

```
<img width="200" height="91" alt="q8" src="https://github.com/user-attachments/assets/614f541c-31b8-4ddb-bdd2-95df288c653b" />



<ul> <li>This query measures each member’s activity <code>prior to joining</code> the loyalty program.</li>

 <li>It filters rows with <code>order_date &lt; join_date</code> to capture only pre-membership purchases.</li>

 <li><code>COUNT(product_name)</code> gives the number of items bought, while <code>SUM(price)</code> calculates the total money spent.</li> 

<li>Grouping by <code>customer_id</code> aggregates the metrics per customer.</li> </ul>

9. If each $1 spent equates to 10 points and sushi has a 2x points multiplier - how many points would each customer have?

```sql
SELECT s.customer_id
, SUM(CASE
WHEN product_name = 'sushi' THEN price * 10 * 2
ELSE price * 10 
END) as points
FROM menu as m
JOIN sales as s
ON s.product_id = m.product_id
GROUP BY s.customer_id
ORDER BY customer_id;
```

<img width="200" height="114" alt="q9" src="https://github.com/user-attachments/assets/91c7051a-e2ef-451e-a1bf-62a6da40e6ec" />


<ul> <li>This query converts each customer’s spend into <code>loyalty points</code>.</li> 

<li>Each dollar earns <code>10 points</code>, but <code>sushi</code> earns a <code>2x multiplier</code>.</li> 

<li>A <code>CASE</code> expression applies the multiplier only when <code>product_name = 'sushi'</code>.</li> 

<li>Points are summed per <code>customer_id</code> and ordered for readability.</li> </ul>

10. In the first week after a customer joins the program (including their join date) they earn 2x points on all items, not just sushi - how many points do customer A and B have at the end of January?

```sql
SELECT s.customer_id
, SUM(CASE
WHEN order_date BETWEEN mem.join_date AND mem.join_date + INTERVAL '6 days' THEN price * 10 * 2
WHEN product_name = 'sushi' THEN price * 10 * 2
ELSE price * 10 
END) as points
FROM menu as m
JOIN sales as s
ON s.product_id = m.product_id
INNER JOIN members as mem
ON mem.customer_id = s.customer_id
WHERE DATE_TRUNC('month', order_date) = DATE '2021-01-01'
GROUP BY s.customer_id
ORDER BY s.customer_id;
```
<img width="200" height="91" alt="q10" src="https://github.com/user-attachments/assets/08bc093b-4a47-4ad2-9b93-bd3f904be75e" />



<ul> <li>This query applies a special <code>2x points boost for the first 7 days</code> (join date + next 6 days) after a customer joins.</li>

 <li>Outside that window, only <code>sushi</code> still earns <code>2x</code>; everything else earns the standard <code>10 points per $1</code>.</li> 

<li>The <code>BETWEEN mem.join_date AND mem.join_date + INTERVAL '6 days'</code> condition captures the 7-day promo period.</li> 

<li><code>DATE_TRUNC('month', order_date) = DATE '2021-01-01'</code> limits the calculation to January 2021.</li> 

<li>Totals are aggregated by <code>customer_id</code> to get final points per customer.</li> </ul>

<h1><a name="Key insights">Key Insights</a></h1>

- 💸 Customer spending varies a lot. Some customers are much more valuable than others, which gives Danny a chance to reward loyal spenders or create tiered offers.

- 📅 Visit frequency reveals loyalty. Tracking how often each customer visits helps Danny spot regulars vs. one-time visitors — perfect for tailoring loyalty strategies.

- 🍜 First orders matter. The first item each customer ordered shows which dishes make the best first impression. These could be promoted more or used in welcome deals.

- 🏆 Ramen rules (or maybe sushi!). Finding the most popular menu item helps with inventory planning, pricing decisions, and even marketing the diner’s signature dish.

- 💖 Everyone has a favorite. Identifying each customer’s go-to item opens the door to personalized offers like “10% off your favorite ramen this week!”

- 🕰️ Pre- vs. post-membership behavior tells a story. Comparing purchases before and after customers joined the loyalty program helps measure its impact — and tweak it if needed.

- 🎁 Points make things fun. A simple points system (with bonuses for sushi!) encourages spending and adds a little gamification to the dining experience.

- 🚀 That first week matters. Offering double points during the first 7 days after sign-up creates excitement and boosts early engagement — a smart move to hook new members.

- 📊 Small dataset, big value. Even with basic sales and membership data, we uncovered insights that can help with real-world decisions around customer experience, operations, and growth.
