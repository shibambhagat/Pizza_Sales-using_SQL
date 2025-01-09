Here is a suitable format for your README.md file:

```markdown
# Pizza Sales Analysis SQL Queries

This repository contains a collection of SQL queries designed for analyzing pizza sales and order data. The queries are written for a database system that includes tables for orders, pizzas, and order details.

## Table of Contents
- [Retrieve the total number of orders placed](#retrieve-the-total-number-of-orders-placed)
- [Calculate the total revenue generated from pizza sales](#calculate-the-total-revenue-generated-from-pizza-sales)
- [Identify the highest-priced pizza](#identify-the-highest-priced-pizza)
- [Identify the most common pizza size ordered](#identify-the-most-common-pizza-size-ordered)
- [List the top 5 most ordered pizza types along with their quantities](#list-the-top-5-most-ordered-pizza-types-along-with-their-quantities)
- [Find the total quantity of each pizza category ordered](#find-the-total-quantity-of-each-pizza-category-ordered)
- [Determine the distribution of orders by hour of the day](#determine-the-distribution-of-orders-by-hour-of-the-day)
- [Find the category-wise distribution of pizzas](#find-the-category-wise-distribution-of-pizzas)
- [Calculate the average number of pizzas ordered per day](#calculate-the-average-number-of-pizzas-ordered-per-day)
- [Determine the top 3 most ordered pizza types based on revenue](#determine-the-top-3-most-ordered-pizza-types-based-on-revenue)
- [Calculate the percentage contribution of each pizza type to total revenue](#calculate-the-percentage-contribution-of-each-pizza-type-to-total-revenue)
- [Analyze the cumulative revenue generated over time](#analyze-the-cumulative-revenue-generated-over-time)
- [Determine the top 3 most ordered pizza types based on revenue for each pizza category](#determine-the-top-3-most-ordered-pizza-types-based-on-revenue-for-each-pizza-category)

### Retrieve the total number of orders placed
```sql
SELECT COUNT(order_id) AS total_orders
FROM orders;
```

### Calculate the total revenue generated from pizza sales
```sql
SELECT ROUND(SUM(order_details.quantity * pizzas.price), 2) AS total_sales
FROM order_details
JOIN pizzas ON pizzas.pizza_id = order_details.pizza_id;
```

### Identify the highest-priced pizza
```sql
SELECT pizza_types.name, pizzas.price
FROM pizza_types
JOIN pizzas ON pizza_types.pizza_type_id = pizzas.pizza_type_id
ORDER BY pizzas.price DESC
LIMIT 1;
```

### Identify the most common pizza size ordered
```sql
SELECT pizzas.size, COUNT(order_details.order_details_id) AS order_count
FROM pizzas
JOIN order_details ON pizzas.pizza_id = order_details.pizza_id
GROUP BY pizzas.size
ORDER BY order_count DESC;
```

### List the top 5 most ordered pizza types along with their quantities
```sql
SELECT pizza_types.name, SUM(order_details.quantity) AS quantity
FROM pizza_types
JOIN pizzas ON pizza_types.pizza_type_id = pizzas.pizza_type_id
JOIN order_details ON order_details.pizza_id = pizzas.pizza_id
GROUP BY pizza_types.name
ORDER BY quantity DESC
LIMIT 5;
```

### Find the total quantity of each pizza category ordered
```sql
SELECT pizza_types.category, SUM(order_details.quantity) AS quantity
FROM pizza_types
JOIN pizzas ON pizza_types.pizza_type_id = pizzas.pizza_type_id
JOIN order_details ON order_details.pizza_id = pizzas.pizza_id
GROUP BY pizza_types.category
ORDER BY quantity DESC;
```

### Determine the distribution of orders by hour of the day
```sql
SELECT HOUR(order_time) AS hour, COUNT(order_id) AS order_count
FROM orders
GROUP BY HOUR(order_time);
```

### Find the category-wise distribution of pizzas
```sql
SELECT category, COUNT(name) 
FROM pizza_types
GROUP BY category;
```

### Calculate the average number of pizzas ordered per day
```sql
SELECT ROUND(AVG(quantity), 0) AS avg_pizza_ordered_per_day
FROM (
    SELECT orders.order_date, SUM(order_details.quantity) AS quantity
    FROM orders
    JOIN order_details ON orders.order_id = order_details.order_id
    GROUP BY orders.order_date
) AS order_quantity;
```

### Determine the top 3 most ordered pizza types based on revenue
```sql
SELECT pizza_types.name, SUM(order_details.quantity * pizzas.price) AS revenue
FROM pizza_types
JOIN pizzas ON pizza_types.pizza_type_id = pizza_types.pizza_type_id
JOIN order_details ON order_details.pizza_id = pizzas.pizza_id
GROUP BY pizza_types.name
ORDER BY revenue DESC
LIMIT 3;
```

### Calculate the percentage contribution of each pizza type to total revenue
```sql
SELECT pizza_types.category,
    ROUND(SUM(order_details.quantity * pizzas.price) / (
        SELECT ROUND(SUM(order_details.quantity * pizzas.price), 2) AS total_sales
        FROM order_details
        JOIN pizzas ON pizzas.pizza_id = order_details.pizza_id
    ) * 100, 2) AS revenue
FROM pizza_types
JOIN pizzas ON pizza_types.pizza_type_id = pizza_types.pizza_type_id
JOIN order_details ON order_details.pizza_id = pizzas.pizza_id
GROUP BY pizza_types.category
ORDER BY revenue DESC;
```

### Analyze the cumulative revenue generated over time
```sql
SELECT order_date,
    SUM(revenue) OVER (ORDER BY order_date) AS cum_revenue
FROM (
    SELECT orders.order_date, SUM(order_details.quantity * pizzas.price) AS revenue
    FROM order_details
    JOIN pizzas ON order_details.pizza_id = pizzas.pizza_id
    JOIN orders ON orders.order_id = order_details.order_id
    GROUP BY orders.order_date
) AS sales;
```

### Determine the top 3 most ordered pizza types based on revenue for each pizza category
```sql
SELECT name, revenue
FROM (
    SELECT category, name, revenue, RANK() OVER(PARTITION BY category ORDER BY revenue DESC) AS rn
    FROM (
        SELECT pizza_types.category, pizza_types.name, SUM(order_details.quantity * pizzas.price) AS revenue
        FROM pizza_types
        JOIN pizzas ON pizza_types.pizza_type_id = pizzas.pizza_type_id
        JOIN order_details ON order_details.pizza_id = pizzas.pizza_id
        GROUP BY pizza_types.category, pizza_types.name
    ) AS a
) AS b
WHERE rn <= 3;
```

Feel free to copy and paste this into your README.md file.
