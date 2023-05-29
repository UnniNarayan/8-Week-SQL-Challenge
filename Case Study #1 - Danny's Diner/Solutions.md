# 🍜 Case Study #1: Danny's Diner

## Solution



***

### 1. What is the total amount each customer spent at the restaurant?

````sql
SELECT s.customer_id, sum(m.price) as total_spent
FROM dd.sales s
JOIN dd.menu m ON s.product_id = m.product_id
GROUP BY s.customer_id
ORDER BY s.customer_id
````

#### Steps:
- Use **SUM** and **GROUP BY** to find out ```total_spent```  by each customer.
- Use **JOIN** to merge ```sales``` and ```menu``` tables as ```customer_id``` and ```price``` are from both tables.


#### Answer:
| customer_id | total_spent |
| ----------- | ----------- |
| A           | 76          |
| B           | 74          |
| C           | 36          |

- Customer A spent $76.
- Customer B spent $74.
- Customer C spent $36.

***
### 2. How many days has each customer visited the restaurant?

````sql
SELECT customer_id, count(distinct order_date) as visits
FROM dd.sales
GROUP BY customer_id
````

#### Steps:
- Use **DISTINCT** and wrap with **COUNT** to find out the ```visits``` for each customer.

#### Answer:
| customer_id | visits |
| ----------- | ----------- |
| A           | 4          |
| B           | 6          |
| C           | 2          |

- Customer A visited 4 times.
- Customer B visited 6 times.
- Customer C visited 2 times.

***