# 1 Pizza_Sales_Analysis
Pizza Sales Performance &amp; Customer Behavior Analysis Using SQL
1. Project Title
Pizza Sales Performance & Customer Behavior Analysis Using SQL

# 2. Objective of the Project

The objective of this project is to analyze pizza sales data using SQL to extract meaningful business insights that support decision-making.

Key goals:
Understand overall sales performance
Identify top-performing pizza types and categories
Analyze customer ordering patterns (time, quantity, frequency)
Measure revenue contribution across products
Evaluate business trends over time

This project simulates a real-world restaurant analytics scenario where SQL is used to transform raw transactional data into actionable insights.

# 3. Dataset Overview

The dataset represents a fictional pizza restaurant chain and consists of transactional order data.

It includes:

Order information (date and time)
Order item details (quantity, pizza type)
Pizza metadata (size, category, price, ingredients)

# 4. Table Description
1. orders

Stores each customer order placed.

Column	Description
order_id:	Unique ID for each order
order_date:	Date of order
order_time:	Time of order

2. order_details

Stores item-level details for each order.

Column	Description
order_details_id:	Unique row identifier
order_id:	Links to orders table
pizza_id:	Identifies pizza ordered
quantity:	Number of pizzas ordered

3. pizzas (assumed existing)

Contains pricing and size information.

Column	Description
pizza_id: Unique pizza identifier
pizza_type_id:	Links to pizza type
size:	Small/Medium/Large etc.
price: Price of pizza

4. pizza_types (assumed existing)

Contains pizza categories and names.

Column	Description
pizza_type_id:	Unique type ID
name:	Pizza name
category:	Veg/Non-Veg etc.

# 5. Business Questions 
 ```sql
CREATE DATABASE Pizzahut;

CREATE TABLE ORDERS (
    ORDER_ID INT NOT NULL,
    ORDER_DATE DATE NOT NULL,
    ORDER_TIME TIME NOT NULL,
    PRIMARY KEY (ORDER_ID)
);```

```sql
CREATE TABLE ORDER_DETAILS (
    ORDER_DETAILS_ID INT NOT NULL,
    ORDER_ID INT NOT NULL,
    PIZZA_ID TEXT NOT NULL,
    QUANTITY INT NOT NULL,
    PRIMARY KEY (ORDER_DETAILS_ID)
);```

### Retrieve the total number of orders placed.

```sql
SELECT COUNT(ORDER_ID) FROM orders AS Total_Orders;

-- Calculate the total revenue generated from pizza sales.
SELECT 
    ROUND(SUM(ORDER_DETAILS.QUANTITY * Pizzas.PRICE),
            2) AS TOTAL_REVENUE
FROM
    order_details
        JOIN
    pizzas ON order_details.PIZZA_ID = pizzas.pizza_id;```

### Identify the highest-priced pizza.
```sql
SELECT 
    pizza_types.NAME, pizzas.PRICE
FROM
    pizza_types
        JOIN
    PIZZAS ON pizza_types.pizza_type_id = PIZZA.pizza_type_id
ORDER BY PIZZA.PRICE DESC
LIMIT 1;

-- Identify the most common pizza size ordered.
SELECT 
    PIZZAS.SIZE, COUNT(ORDER_DETAILS.ORDER_DETAILS_ID) AS ORDER_COUNT
FROM
    PIZZAS
        JOIN
    ORDER_DETAILS ON PIZZAS.PIZZA_ID = order_details.PIZZA_ID
GROUP BY PIZZAS.SIZE ORDER BY ORDER_COUNT DESC;```

### List the top 5 most ordered pizza types along with their quantities.
```sql
SELECT 
    pizza_types.name, SUM(order_details.quantity) AS QUANTITY
FROM
    pizza_types
        JOIN
    pizzas ON pizza_types.pizza_type_id = pizzas.pizza_type_id
        JOIN
    ORDER_DETAILS ON ORDER_DETAILS.PIZZA_ID = PIZZAS.PIZZA_ID
GROUP BY pizza_types.NAME
ORDER BY QUANTITY DESC
LIMIT 5;```

-- Join the necessary tables to find the total quantity of each pizza category ordered.
```sql
SELECT 
    pizza_types.category,
    SUM(order_details.quantity) AS QUANTITY
FROM
    pizza_types
        JOIN
    PIZZAS ON pizza_types.pizza_type_id = pizzas.pizza_type_id
        JOIN
    order_details ON order_details.PIZZA_ID = pizzas.pizza_id
GROUP BY pizza_types.CATEGORY
ORDER BY QUANTITY DESC;```

### Determine the distribution of orders by hour of the day.
```sql
SELECT 
    HOUR(ORDER_TIME) as hour, COUNT(ORDER_ID) As Order_Count
FROM
    ORDERS
GROUP BY HOUR(ORDER_TIME);

-- Join relevant tables to find the category-wise distribution of pizzas.
SELECT 
    CATEGORY, COUNT(NAME)
FROM
    pizza_types
GROUP BY CATEGORY;```

### Group the orders by date and calculate the average number of pizzas ordered per day.
```sql
SELECT 
    ROUND(AVG(QTD), 0) AS AVG_PIZZAS_PER_DAY
FROM
    (SELECT 
        ORDERS.ORDER_DATE, SUM(ORDER_DETAILS.QUANTITY) AS QTD
    FROM
        ORDERS
    JOIN ORDER_DETAILS ON ORDERS.ORDER_ID = ORDER_DETAILS.ORDER_ID
    GROUP BY ORDERS.ORDER_DATE) AS ORDER_QUANTITY;```

###Determine the top 3 most ordered pizza types based on revenue.
```sql
SELECT 
    PIZZA_TYPES.NAME,
    SUM(ORDER_DETAILS.QUANTITY * PIZZAS.PRICE) AS REVENUE
FROM
    PIZZA_TYPES
        JOIN
    PIZZAS ON pizzas.pizza_type_id = pizza_types.pizza_type_id
        JOIN
    order_details ON order_details.PIZZA_ID = pizzas.pizza_id
GROUP BY PIZZA_TYPES.NAME
ORDER BY REVENUE DESC
LIMIT 3;```

###Calculate the percentage contribution of each pizza type to total revenue.Percentage contribution of each pizza category to total revenue
```sql
SELECT 
    pt.category,

    -- % contribution calculation
    ROUND(
        SUM(od.quantity * p.price) / 
        (SELECT SUM(od2.quantity * p2.price)
         FROM order_details od2
         JOIN pizzas p2 
            ON od2.pizza_id = p2.pizza_id
        ) * 100, 2
    ) AS revenue_percentage

FROM order_details od

JOIN pizzas p 
    ON od.pizza_id = p.pizza_id

JOIN pizza_types pt 
    ON p.pizza_type_id = pt.pizza_type_id

GROUP BY pt.category

ORDER BY revenue_percentage DESC;```


### Analyze the cumulative revenue generated over time.
```sql
SELECT ORDER_DATE, SUM(REVENUE) OVER(ORDER BY ORDER_DATE) AS CUM_REVENUE
FROM
(
SELECT ORDERS.ORDER_DATE, 
SUM(order_details.QUANTITY * pizzas.price) AS REVENUE FROM order_details JOIN pizzas
ON order_details.PIZZA_ID = pizzas.pizza_id JOIN ORDERS
ON ORDERS.ORDER_ID = ORDER_DETAILS.ORDER_ID GROUP BY ORDERS.ORDER_DATE) AS SALES
 ;```


### Determine the top 3 most ordered pizza types based on revenue for each pizza category.
```sql
SELECT NAME, REVENUE FROM
(
SELECT category, name , revenue, rank() over( PARTITION BY  CATEGORY order by revenue DESC) AS RN
FROM
 (
SELECT pizza_types.category, pizza_types.name, SUM(order_details.QUANTITY * pizzas.price) AS REVENUE
FROM pizza_types JOIN pizzas ON pizza_types.pizza_type_id = pizzas.pizza_type_id
JOIN order_details ON order_details.PIZZA_ID = pizzas.pizza_id GROUP BY pizza_types.category, pizza_types.name) AS A) AS B
WHERE RN<=3;```







