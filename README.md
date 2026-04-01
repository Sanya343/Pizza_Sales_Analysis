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

What is the total number of customer orders placed?

What is the total revenue generated from pizza sales?

Which pizza generates the highest selling price?

What pizza size is ordered most frequently?

What are the top 5 most ordered pizza types by quantity?

Which pizza category has the highest order volume?

How are orders distributed across different hours of the day?

What is the distribution of pizza categories in total orders?

What is the average number of pizzas sold per day?

What are the top 3 revenue-generating pizzas overall?

What is the percentage contribution of each pizza category to total revenue?

How does cumulative revenue grow over time?

What are the top 3 pizzas by revenue within each category?
