<h1>Case Study #2 - Pizza Runner🍕</h1>
<img width="500" height="707" alt="Screenshot 2025-08-04 at 8 20 18 PM" src="https://github.com/user-attachments/assets/c8224022-92f5-4c23-a145-551e70c046d4" />

##

<h1><a name="🧾 introduction">🧾  Introduction</a></h1>
Welcome to Pizza Runner, where the worlds of food delivery, retro vibes, and data intersect! In this project, I explored the operational and customer data behind a startup founded by Danny — a data-savvy entrepreneur on a mission to Uberize pizza delivery.

This case study focuses on cleaning messy operational data, dissecting customer order patterns, and analyzing delivery performance using advanced SQL techniques. From parsing ingredients to calculating delivery time lags and identifying the most popular extras, this analysis aims to optimize logistics and improve customer satisfaction.

<h1><a name="❓ Problem Statement">❓ Problem Statement</a></h1>
Pizza Runner, a food delivery startup, is scaling operations and needs clean, reliable data for decision-making. The business struggles with inconsistencies in raw customer and runner order data.

Danny wants to understand:

- Which pizzas and extras are most popular?

- How efficient each runner is?

- Where delivery operations can be optimized?

The goal is to clean and analyze the data to support better logistics and performance tracking.

<h1><a name = "dateset"> 🧾 Dataset Provided</a></h1>

The case study is centered around six tables:

- runners – Information about each delivery runner and their registration date

- customer_orders – Records of pizza orders placed by customers, including extras and exclusions

- runner_orders – Delivery data with timestamps, pickup/drop info, and cancellations

- pizza_names – Mapping between pizza IDs and their names

- pizza_recipes – Recipe details showing which toppings are used in each pizza

- pizza_toppings – Full list of available toppings and their unique IDs


<h1><a name = "entity diagram"> 🔗 Entity Relation Diagram (ERD)</a></h1>

<img width="500" height="355" alt="Screenshot 2025-08-04 at 8 30 51 PM" src="https://github.com/user-attachments/assets/504e06ac-ad7f-442f-8394-d11efaa32968" />

<h1><a name= "Case study"> Case Study Questions & Answers</a></h1>


<h4><a name="a.pizzametrics"></a>A. Pizza Metrics🍕🍕</h4>

Q1:How many pizzas were ordered?
```sql
SELECT COUNT(*) AS pizzas_ordered
FROM customer_orders;
```
<img width="150" height="68" alt="q1" src="https://github.com/user-attachments/assets/e0f757e9-8eb2-405d-a7ea-ac86484914e5" />

<h6>Answer:</h6>
<ul> <li>This query calculates the <code>total number of pizzas</code> ordered by all customers.</li> 
<li>It does this by using the <code>COUNT(*)</code> function on the <code>customer_orders</code> table.</li>
 <li>Each row in this table represents a single pizza ordered, so counting the rows gives the total.</li> </ul>

Q2: How many unique customer orders were made?
```sql
SELECT COUNT (DISTINCT order_id) as unique_customer_order
FROM customer_orders;
```
<img width="203" height="63" alt="q2" src="https://github.com/user-attachments/assets/2a9775f0-7299-45f3-9b20-587ff2244583" />

<h6>Answer:</h6>
<ul> <li>This query identifies how many <code>unique orders</code> were placed by customers.</li> 
<li>It uses <code>COUNT(DISTINCT order_id)</code> to ensure only unique order numbers are counted.</li> 
<li>This helps differentiate between multiple pizzas in the same order and completely separate orders.</li> </ul>

Q3: How many successful orders were delivered by each runner?
```sql
SELECT runner_id, COUNT(DISTINCT r.order_id) as successful_orders_delivered
FROM runner_orders as r
WHERE pickup_time <> 'null'
GROUP BY runner_id;
```
<img width="315" height="111" alt="q3" src="https://github.com/user-attachments/assets/65d10107-c1fb-441e-9958-b0a660d36687" />

<h6>Answer:</h6>
<ul> <li>This query counts how many <code>successful deliveries</code> each runner made.</li>
 <li>The condition <code>pickup_time <> 'null'</code> filters out undelivered or cancelled orders.</li> 
<li><code>COUNT(DISTINCT order_id)</code> ensures that duplicate entries aren't counted multiple times.</li> 
<li>The results are grouped by each <code>runner_id</code> to break it down per runner.</li> </ul>

4: How many of each type of pizza was delivered?
```sql
SELECT  p.pizza_name, COUNT(c.pizza_id) as pizzas_delivered
FROM runner_orders as r
INNER JOIN customer_orders as c USING (order_id)
INNER JOIN pizza_names as p USING (pizza_id)
WHERE pickup_time <> 'null'
GROUP BY pizza_name;
```

<img width="262" height="95" alt="q4" src="https://github.com/user-attachments/assets/c30f0aef-747a-4735-851f-e4a8c87ce740" />

<h6>Answer:</h6>
<ul> <li>This query calculates the <code>total deliveries</code> for each type of pizza.</li> 
<li>It joins <code>runner_orders</code>, <code>customer_orders</code>, and <code>pizza_names</code> to access order and pizza details.</li> 
<li>The <code>WHERE pickup_time <> 'null'</code> filter ensures only completed deliveries are counted.</li> 
<li>The pizzas are grouped by <code>pizza_name</code> and counted to see how many were delivered.</li> </ul>

Q5: How many Vegetarian and Meatlovers were ordered by each customer?
```sql
SELECT c.customer_id, pizza_name, COUNT (c.pizza_id) AS pizzas_ordered
FROM runner_orders as r
INNER JOIN customer_orders as c USING (order_id)
INNER JOIN pizza_names as p USING (pizza_id)
GROUP BY pizza_name, customer_id;
```
<img width="354" height="242" alt="q5" src="https://github.com/user-attachments/assets/4c41a50f-2c82-48c2-ab2a-d397e429b2c1" />

<h6>Answer:</h6>
<ul> <li>This query shows the number of <code>Vegetarian</code> and <code>Meatlovers</code> pizzas ordered by each customer.</li> 
<li>It joins three tables to access runner, customer, and pizza info.</li>
 <li>The data is grouped by <code>customer_id</code> and <code>pizza_name</code> for detailed breakdown.</li>
 <li><code>COUNT(pizza_id)</code> gives the quantity of each pizza type per customer.</li> </ul>

Q6: What was the maximum number of pizzas delivered in a single order?
```sql
SELECT c.order_id, COUNT(pizza_id)
FROM customer_orders as c
INNER JOIN runner_orders as r
USING (order_id)
WHERE pickup_time <> 'null'
GROUP BY order_id
ORDER BY COUNT (c.pizza_id) DESC
LIMIT 1;
```
<img width="184" height="65" alt="q6" src="https://github.com/user-attachments/assets/8ba0542d-12c4-4f3a-8a80-f3429261244a" />

<h6>Answer:</h6>
<ul> <li>This query finds the <code>largest number of pizzas</code> delivered in a single order.</li>
 <li>It filters out undelivered orders using <code>pickup_time <> 'null'</code>.</li> 
<li>It then groups by <code>order_id</code> and counts the pizzas for each order.</li>
 <li>The <code>LIMIT 1</code> and <code>ORDER BY</code> get the highest count of pizzas delivered in a single order.</li> </ul>

 Q7:For each customer, how many delivered pizzas had at least 1 change and how many had no changes?
```sql
SELECT c.customer_id, 
SUM(CASE
 WHEN ((exclusions IS NOT NULL AND exclusions<>'null' AND LENGTH (exclusions)>0) 
  OR ( extras IS NOT NULL AND extras<>'null' AND LENGTH (extras )>0) ) = TRUE
  THEN 1
  ELSE 0
   END)  AS changes,

SUM(CASE
 WHEN ((exclusions IS NOT NULL AND exclusions<>'null' AND LENGTH (exclusions)>0) 
  OR ( extras IS NOT NULL AND extras<>'null' AND LENGTH (extras )>0) ) = TRUE
  THEN 0
  ELSE 1
   END)  AS no_changes
FROM customer_orders as c
INNER JOIN runner_orders as r
USING (order_id)
WHERE pickup_time <>'null'
GROUP BY c.customer_id
ORDER BY c.customer_id;

```

<img width="319" height="168" alt="q7" src="https://github.com/user-attachments/assets/a4cfff06-19b3-4bda-939b-d8ab498f28da" />

<h6>Answer:</h6>

<ul> <li>This query checks if pizzas had any <code>extras</code> or <code>exclusions</code> added or removed.</li> 
<li>Two <code>CASE</code> statements count orders with changes vs. those with none.</li> 
<li>Null, blank, or "null" string values are excluded from being treated as changes.</li> 
<li>Grouping is done by <code>customer_id</code> for personalized results.</li> </ul>

Q8: How many pizzas were delivered that had both exclusions and extras?
```sql
SELECT COUNT (pizza_id) as pizzas_delivered_with_extras_and_exclusions
FROM customer_orders as c
INNER JOIN runner_orders as r
USING (order_id)
WHERE pickup_time <>'null' AND ((exclusions IS NOT NULL AND exclusions<>'null' AND LENGTH (exclusions)>0) 
  AND ( extras IS NOT NULL AND extras<>'null' AND LENGTH (extras )>0) );
```
<img width="347" height="78" alt="q8" src="https://github.com/user-attachments/assets/9ca2259b-fdc5-4b16-a498-4fcacdb669c8" />

<h6>Answer:</h6>
<ul> <li>This query counts all <code>delivered pizzas</code> with <strong>both</strong> extras and exclusions.</li> 
<li>Conditions in the <code>WHERE</code> clause ensure that both fields are present and non-empty.</li> 
<li>It only includes completed orders using <code>pickup_time <> 'null'</code>.</li> </ul>

Q9: What was the total volume of pizzas ordered for each hour of the day?
```sql
SELECT COUNT(pizza_id) as pizzas_ordered, DATE_PART('hour', order_time) as hour
FROM customer_orders
GROUP BY DATE_PART('hour', order_time) 
ORDER BY DATE_PART('hour', order_time);
```
<img width="275" height="190" alt="q9" src="https://github.com/user-attachments/assets/97cd615a-4a2c-41cf-944a-bbd96eb9ab28" />

<h6>Answer:</h6>
<ul> <li>This query analyzes the <code>hourly pizza ordering pattern</code>.</li> 
<li><code>DATE_PART('hour', order_time)</code> extracts the hour from each order timestamp.</li> 
<li>The number of pizzas ordered in each hour is calculated using <code>COUNT(pizza_id)</code>.</li> </ul>

Q10:What was the volume of orders for each day of the week?
```sql
SELECT COUNT(pizza_id) as pizzas_ordered, DATE_PART('dow', order_time) as day,TO_CHAR(order_time, 'Day') as day_2
FROM customer_orders
GROUP BY DATE_PART('dow', order_time), TO_CHAR(order_time, 'Day') 
ORDER BY DATE_PART('dow', order_time);
```
<img width="343" height="141" alt="q10" src="https://github.com/user-attachments/assets/936baa9e-3b5f-4007-8674-dc7d33389428" />
<h6>Answer:</h6>
<ul> <li>This query shows pizza order volume by <code>day of the week</code>.</li> 
<li><code>DATE_PART('dow', order_time)</code> gives the numeric day (0 = Sunday, 6 = Saturday).</li> 
<li><code>TO_CHAR(..., 'Day')</code> converts it to a readable day name.</li> 
<li>Results are grouped and ordered by the day to give a weekly trend overview.</li> </ul>









