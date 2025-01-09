# Pizza Sales Analysis SQL Queries

## Project Overview

### Purpose
This project contains a collection of SQL queries designed to analyze and extract insights from a pizza sales database. By leveraging these queries, businesses can gain a deeper understanding of their sales performance, customer preferences, and overall business trends.

### Key Features
1. **Order Analysis**: 
   - Retrieve the total number of orders.
   - Track order distribution by hour of the day to identify peak times.
   - Calculate the average number of pizzas ordered per day.
   
2. **Revenue Insights**: 
   - Calculate the total revenue generated from pizza sales.
   - Identify the most profitable pizza types based on total revenue.
   - Analyze the percentage contribution of each pizza type to overall sales.

3. **Pizza Preferences**: 
   - Identify the most common pizza size ordered.
   - List the top 5 most ordered pizza types by quantity.
   - Calculate the total quantity of each pizza category ordered.

4. **Sales Trends**: 
   - Analyze cumulative revenue generated over time.
   - Identify trends in sales for specific dates and periods.

5. **Category-Wise Performance**: 
   - Understand the distribution of pizzas across different categories.
   - Rank the top 3 pizza types by revenue for each pizza category.

### Objectives
- **Data-Driven Decision Making**: To provide restaurant owners, managers, and analysts with actionable insights that can help optimize menu offerings, pricing strategies, and customer engagement efforts.
- **Sales Optimization**: By identifying best-selling pizzas, peak sales times, and consumer preferences, businesses can streamline inventory management and resource allocation.
- **Business Intelligence**: To offer detailed, insightful reports that help stakeholders understand customer behavior, predict demand, and improve business operations.

### Intended Users
- **Restaurant Owners & Managers**: Use these queries to monitor overall sales performance, identify top-performing products, and optimize pricing and menu offerings.
- **Data Analysts**: Leverage the queries to generate in-depth insights about sales performance, customer preferences, and seasonal trends.
- **Business Intelligence Teams**: Transform the query results into visual reports and dashboards for data-driven decision-making and strategic planning.

### Technologies Used
- **SQL**: The queries are written in SQL, a powerful language used for data retrieval and analysis in relational databases.
- **Database Management Systems (DBMS)**: These queries are designed to be run on any standard SQL-based database such as MySQL, PostgreSQL, or MariaDB.

### Future Enhancements
- **Data Visualization**: Integration with data visualization tools (e.g., Tableau, Power BI) for better presentation of trends, insights, and KPIs.
- **Real-Time Analytics**: Incorporating real-time data processing for dynamic decision-making.
- **Advanced Analytics**: Expanding the analysis to include customer segmentation, demand forecasting, and marketing campaign effectiveness.

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

Here are the additional sections to be included in your README.md file:

### Findings
- **Total Orders**: The total number of orders placed.
- **Revenue Insights**: The total revenue generated from pizza sales, most profitable pizza types, and their contribution to overall sales.
- **Pizza Preferences**: The most common pizza size ordered and the top 5 most ordered pizza types.
- **Sales Trends**: Cumulative revenue generated over time and sales trends for specific dates and periods.
- **Category-Wise Performance**: Distribution of pizzas across different categories and the top 3 pizza types by revenue for each category.

### Reports
- **Order Analysis Report**: Detailed report on the number of orders, order distribution by hour, and average number of pizzas ordered per day.
- **Revenue Report**: Comprehensive report on total revenue, profitable pizza types, and revenue contribution of each pizza type.
- **Pizza Preferences Report**: Analysis of common pizza sizes, top ordered pizza types, and quantity of each pizza category ordered.
- **Sales Trends Report**: Visualization of cumulative revenue and sales trends over time.
- **Category-Wise Performance Report**: Insights into the distribution of pizzas and top-performing pizza types by category.

### Conclusion
The analysis of the pizza sales database using the provided SQL queries offers valuable insights into sales performance, customer preferences, and business trends. By leveraging these insights, businesses can make data-driven decisions to optimize their menu offerings, pricing strategies, and inventory management. The reports generated from the queries provide actionable information for restaurant owners, managers, data analysts, and business intelligence teams.

### How to Use
1. **Set Up Database**: Ensure you have a SQL-based database (e.g., MySQL, PostgreSQL) with the necessary tables and data for pizza sales.
2. **Run Queries**: Execute the provided SQL queries in your database management system to retrieve insights and generate reports.
3. **Analyze Results**: Review the query results to understand sales performance, customer preferences, and trends.
4. **Generate Reports**: Use the query results to create detailed reports and visualizations for data-driven decision-making.
5. **Implement Changes**: Based on the insights, make informed decisions to optimize menu offerings, pricing, and inventory management.

You can now update your README.md file with these sections.
