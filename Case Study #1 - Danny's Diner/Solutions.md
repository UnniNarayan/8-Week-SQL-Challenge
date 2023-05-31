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
### 3. What was the first item from the menu purchased by each customer?

````sql
WITH A as (
SELECT customer_id, product_id, order_date,dense_rank() over(partition by customer_id order by order_date) as rnk
FROM dd.sales
)
SELECT a.customer_id, m.product_name
FROM A
JOIN dd.menu m on a.product_id = m.product_id
WHERE rnk =1
ORDER BY a.customer_id
````

#### Steps:
- Create a temp table ```A``` and use **Windows function** with **DENSE_RANK** to create a new column ```rnk``` based on ```order_date```.
- Instead of **ROW_NUMBER** or **RANK**, use **DENSE_RANK** as ```order_date``` is not time-stamped hence, there is no sequence as to which item is ordered first if 2 or more items are ordered on the same day.

#### Answer:
| customer_id | product_name | 
| ----------- | ----------- |
| A           | curry        | 
| A           | sushi        | 
| B           | curry        | 
| C           | ramen        |

- Customer A's first orders are curry and sushi.
- Customer B's first order is curry.
- Customer C's first order is ramen.

***
### 4. What is the most purchased item on the menu and how many times was it purchased by all customers?

````sql
SELECT m.product_name, count(s.product_id) as times_bought
FROM dd.sales s
JOIN dd.menu m on s.product_id = m.product_id
GROUP BY m.product_name 
ORDER BY count(s.product_id) desc
LIMIT 1
````

#### Steps:
- **COUNT** number of ```product_id``` and **ORDER BY** ```times_bought``` by descending order. 
- Then, use **LIMIT 1** to filter highest number of purchased item.

#### Answer:
| most_purchased | product_name | 
| ----------- | ----------- |
| 8       | ramen |


- Most purchased item on the menu is ramen which is 8 times. Yummy!

***
