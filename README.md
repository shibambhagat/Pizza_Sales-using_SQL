

**Q.1 Retrieve the total number of orders placed**.

'''sql
SELECT 
    *
FROM
    orders;
SELECT 
    COUNT(order_id) AS total_orders
FROM
    orders; 
   '''
    
    
    **Q.2 Calculate the total revenue generated from pizza sales**.
    '''sql
SELECT 
    ROUND(SUM(order_details.quantity * pizzas.price),
            2) AS total_sales
   FROM
    order_details
        JOIN
    pizzas ON pizzas.pizza_id = order_details.pizza_id
   '''
    
    
     **Q.3 Identify the highest-priced pizza**.
    '''sql
SELECT 
    pizza_types.name, pizzas.price
FROM
    pizza_types
        JOIN
    pizzas ON pizza_types.pizza_type_id = pizzas.pizza_type_id
ORDER BY pizzas.price DESC
LIMIT 1;
   '''

 **Q.4 Identify the most common pizza size ordered**.
'''sql
SELECT 
    pizzas.size,
    COUNT(order_details.order_details_id) AS order_count
FROM
    pizzas
        JOIN
    order_details ON pizzas.pizza_id = order_details.pizza_id
GROUP BY pizzas.size
ORDER BY order_count DESC;
   '''


**Q.5 List the top 5 most ordered pizza types along with their quantities**.
'''sql
SELECT 
    pizza_types.name, SUM(order_details.quantity) AS quantity
FROM
    pizza_types
        JOIN
    pizzas ON pizza_types.pizza_type_id = pizzas.pizza_type_id
        JOIN
    order_details ON order_details.pizza_id = pizzas.pizza_id
GROUP BY pizza_types.name
ORDER BY quantity DESC
LIMIT 5;
   '''

**Q.6 Join the necessary tables to find the  total quantity of each pizza category ordered**.
'''sql
SELECT 
    pizza_types.category,
    SUM(order_details.quantity) AS quantity
FROM
    pizza_types
        JOIN
    pizzas ON pizza_types.pizza_type_id = pizzas.pizza_type_id
        JOIN
    order_details ON order_details.pizza_id = pizzas.pizza_id
GROUP BY pizza_types.category
ORDER BY quantity DESC;
   '''


**Q.7 Determine the distribution of orders by hour of the day**. 
   '''sql
SELECT 
    *
FROM
    orders;
SELECT 
    HOUR(order_time) AS hour, COUNT(order_id) AS order_count
FROM
    orders
GROUP BY HOUR(order_time);
   '''


**Q.8 Join relevant tables to find the category-wise distribution of pizzas**.
'''sql
Select category , count(name) from pizza_types
group by category
'''

**Q.9 Group the orders by date and calculate the average number of pizzas ordered per day**.
'''sql
SELECT 
    ROUND(AVG(quantity), 0) AS avg_pizza_ordered_per_day
FROM
    (SELECT 
        orders.order_date, SUM(order_details.quantity) AS quantity
    FROM
        orders
    JOIN order_details ON orders.order_id = order_details.order_id
    GROUP BY orders.order_date) AS order_quantity;
   '''
    
    
    **Q.10 Determine the top 3 most ordered pizza types based on revenue**.
    '''sql
SELECT 
    pizza_types.name,
    SUM(order_details.quantity * pizzas.price) AS revenue
FROM
    pizza_types
        JOIN
    pizzas ON pizzas.pizza_type_id = pizza_types.pizza_type_id
        JOIN
    order_details ON order_details.pizza_id = pizzas.pizza_id
GROUP BY pizza_types.name
ORDER BY revenue DESC
LIMIT 3;
   '''


**Q.11 Calculate the percentage contribution of each pizza type to total revenue**.
'''sql
SELECT 
    pizza_types.category,
    ROUND(SUM(order_details.quantity * pizzas.price) / (SELECT 
                    ROUND(SUM(order_details.quantity * pizzas.price),
                                2) AS total_sales
                FROM
                    order_details
                        JOIN
                    pizzas ON pizzas.pizza_id = order_details.pizza_id) * 100,
            2) AS revenue
FROM
    pizza_types
        JOIN
    pizzas ON pizza_types.pizza_type_id = pizzas.pizza_type_id
        JOIN
    order_details ON order_details.pizza_id = pizzas.pizza_id
GROUP BY pizza_types.category
ORDER BY revenue DESC;
   '''


**Q.12 Analyze the cumulative revenue generated over time**.
'''sql
select order_date,
sum(revenue) over (order by order_date) as cum_revenue
from
(select orders.order_date,
sum(order_details.quantity * pizzas.price) as revenue
from order_details join pizzas
on order_details.pizza_id = pizzas.pizza_id
join orders
on orders.order_id = order_details.order_id
group by orders.order_date) as sales;
   '''


 **Q.13 Determine the top 3 most ordered pizza types based on revenue for each pizza category**.
'''sql
select name, revenue from 
(select category, name, revenue,
rank() over(partition by category order by revenue desc) as rn
from
(select pizza_types.category, pizza_types.name,
sum((order_details.quantity) * pizzas.price) as revenue
from pizza_types join pizzas
on pizza_types.pizza_type_id = pizzas.pizza_type_id
join order_details
on order_details.pizza_id = pizzas.pizza_id
group by pizza_types.category, pizza_types.name) as a) as b
where rn <=3;
   ''' give me the  readme.md format from this sql query to upload my reposatry 
ChatGPT said:
ChatGPT
Sure! Here's a sample README.md format that you can use to upload your SQL queries to your repository:

Pizza Sales Analysis SQL Queries
This repository contains a collection of SQL queries designed for analyzing pizza sales and order data. The queries are written for a database system that includes tables for orders, pizzas, order details, and pizza categories.

Table of Contents
Retrieve the total number of orders placed
Calculate the total revenue generated from pizza sales
Identify the highest-priced pizza
Identify the most common pizza size ordered
List the top 5 most ordered pizza types along with their quantities
Find the total quantity of each pizza category ordered
Determine the distribution of orders by hour of the day
Find the category-wise distribution of pizzas
Calculate the average number of pizzas ordered per day
Determine the top 3 most ordered pizza types based on revenue
Calculate the percentage contribution of each pizza type to total revenue
Analyze the cumulative revenue generated over time
Determine the top 3 most ordered pizza types based on revenue for each pizza category
Q1: Retrieve the total number of orders placed
sql
Copy code
SELECT 
    COUNT(order_id) AS total_orders
FROM
    orders;
Q2: Calculate the total revenue generated from pizza sales
sql
Copy code
SELECT 
    ROUND(SUM(order_details.quantity * pizzas.price), 2) AS total_sales
FROM
    order_details
    JOIN pizzas ON pizzas.pizza_id = order_details.pizza_id;
Q3: Identify the highest-priced pizza
sql
Copy code
SELECT 
    pizza_types.name, pizzas.price
FROM
    pizza_types
    JOIN pizzas ON pizza_types.pizza_type_id = pizzas.pizza_type_id
ORDER BY pizzas.price DESC
LIMIT 1;
Q4: Identify the most common pizza size ordered
sql
Copy code
SELECT 
    pizzas.size,
    COUNT(order_details.order_details_id) AS order_count
FROM
    pizzas
    JOIN order_details ON pizzas.pizza_id = order_details.pizza_id
GROUP BY pizzas.size
ORDER BY order_count DESC;
Q5: List the top 5 most ordered pizza types along with their quantities
sql
Copy code
SELECT 
    pizza_types.name, SUM(order_details.quantity) AS quantity
FROM
    pizza_types
    JOIN pizzas ON pizza_types.pizza_type_id = pizzas.pizza_type_id
    JOIN order_details ON order_details.pizza_id = pizzas.pizza_id
GROUP BY pizza_types.name
ORDER BY quantity DESC
LIMIT 5;
Q6: Find the total quantity of each pizza category ordered
sql
Copy code
SELECT 
    pizza_types.category,
    SUM(order_details.quantity) AS quantity
FROM
    pizza_types
    JOIN pizzas ON pizza_types.pizza_type_id = pizzas.pizza_type_id
    JOIN order_details ON order_details.pizza_id = pizzas.pizza_id
GROUP BY pizza_types.category
ORDER BY quantity DESC;
Q7: Determine the distribution of orders by hour of the day
sql
Copy code
SELECT 
    HOUR(order_time) AS hour, COUNT(order_id) AS order_count
FROM
    orders
GROUP BY HOUR(order_time);
Q8: Find the category-wise distribution of pizzas
sql
Copy code
SELECT category, COUNT(name) 
FROM pizza_types
GROUP BY category;
Q9: Calculate the average number of pizzas ordered per day
sql
Copy code
SELECT 
    ROUND(AVG(quantity), 0) AS avg_pizza_ordered_per_day
FROM
    (SELECT 
        orders.order_date, SUM(order_details.quantity) AS quantity
    FROM
        orders
    JOIN order_details ON orders.order_id = order_details.order_id
    GROUP BY orders.order_date) AS order_quantity;
Q10: Determine the top 3 most ordered pizza types based on revenue
sql
Copy code
SELECT 
    pizza_types.name,
    SUM(order_details.quantity * pizzas.price) AS revenue
FROM
    pizza_types
    JOIN pizzas ON pizzas.pizza_type_id = pizza_types.pizza_type_id
    JOIN order_details ON order_details.pizza_id = pizzas.pizza_id
GROUP BY pizza_types.name
ORDER BY revenue DESC
LIMIT 3;
Q11: Calculate the percentage contribution of each pizza type to total revenue
sql
Copy code
SELECT 
    pizza_types.category,
    ROUND(SUM(order_details.quantity * pizzas.price) / 
        (SELECT ROUND(SUM(order_details.quantity * pizzas.price), 2) AS total_sales
         FROM order_details
         JOIN pizzas ON pizzas.pizza_id = order_details.pizza_id) * 100, 2) AS revenue
FROM
    pizza_types
    JOIN pizzas ON pizza_types.pizza_type_id = pizzas.pizza_type_id
    JOIN order_details ON order_details.pizza_id = pizzas.pizza_id
GROUP BY pizza_types.category
ORDER BY revenue DESC;
Q12: Analyze the cumulative revenue generated over time
sql
Copy code
SELECT order_date,
       SUM(revenue) OVER (ORDER BY order_date) AS cum_revenue
FROM
    (SELECT orders.order_date,
            SUM(order_details.quantity * pizzas.price) AS revenue
     FROM order_details 
     JOIN pizzas ON order_details.pizza_id = pizzas.pizza_id
     JOIN orders ON orders.order_id = order_details.order_id
     GROUP BY orders.order_date) AS sales;
Q13: Determine the top 3 most ordered pizza types based on revenue for each pizza category
sql
Copy code
SELECT name, revenue
FROM 
    (SELECT category, name, revenue, 
            RANK() OVER (PARTITION BY category ORDER BY revenue DESC) AS rn
     FROM
         (SELECT pizza_types.category, pizza_types.name,
                 SUM(order_details.quantity * pizzas.price) AS revenue
          FROM pizza_types 
          JOIN pizzas ON pizza_types.pizza_type_id = pizzas.pizza_type_id
          JOIN order_details ON order_details.pizza_id = pizzas.pizza_id
          GROUP BY pizza_types.category, pizza_types.name) AS a) AS b
WHERE rn <= 3;

