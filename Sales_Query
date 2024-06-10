--Amazon_Sales_Analysis

--Q1. What are the total sales made by each customer?
SELECT 
	customer_id,
	SUM(sale) AS total_sales
FROM 
	orders
GROUP BY 
	customer_id;
	
--Q2. How many orders were placed in each state?
SELECT
	state,
	COUNT(order_id) AS total_orders
FROM
	orders
GROUP BY
	state;
	
--Q3. How many unique products were sold?
SELECT
	COUNT(DISTINCT product_id) AS total_unique_product
FROM
	orders;
	
--Q4. How many returns were made for each product category?
SELECT
    o.category,
    COUNT(r.return_id) AS num_returns
FROM
    orders o
LEFT JOIN
    returns r ON o.order_id = r.order_id
GROUP BY
    o.category;
	
--Q5. How many orders were placed each month(2022)
SELECT 
	EXTRACT(MONTH FROM order_date) AS month, 
	COUNT(order_id) AS total_orders
FROM 
	orders
WHERE 
	EXTRACT(YEAR FROM order_date) = 2022
GROUP BY 
	month;


--Q6.Determine the top 5 products whose revenue has decreased compared to the previous year.
WITH last_rev AS (
    SELECT 
        product_id,
        SUM(sale) AS TOTAL_SALE
    FROM 
        orders
    WHERE 
        EXTRACT(YEAR FROM order_date) = 2022
    GROUP BY 
        product_id
),
current_rev AS (
    SELECT 
        product_id,
        SUM(sale) AS TOTAL_SALE
    FROM 
        orders
    WHERE 
        EXTRACT(YEAR FROM order_date) = 2023
    GROUP BY 
        product_id
)

SELECT 
    l.product_id, 
    l.TOTAL_SALE AS last_rev, 
    c.TOTAL_SALE AS curr_rev,
    ((l.TOTAL_SALE - c.TOTAL_SALE) / l.TOTAL_SALE) * 100 AS rev_decrease
FROM 
    last_rev l
JOIN 
    current_rev c ON l.product_id = c.product_id
WHERE 
    c.TOTAL_SALE < l.TOTAL_SALE
ORDER BY 
    rev_decrease DESC
LIMIT 5;

--Q7. List all orders where the quantity sold is greater than the average quantity sold across all orders.
SELECT 
	*
FROM 
	orders
WHERE 
	quantity > (select AVG(quantity) FROM orders)
ORDER BY 
	quantity;
	
--Q8.  Find out the top 5 customers who made the highest profits.
SELECT 
    o.customer_id, 
    SUM((o.price_per_unit - p.cogs) * o.quantity) AS total_profit
FROM 
    orders AS o
LEFT JOIN 
    products AS p ON o.product_id = p.product_id
GROUP BY 
    o.customer_id
ORDER BY 
    total_profit DESC
LIMIT 5;

--Q9. Find the details of the top 5 products with the highest total sales, where the total sale for each product is greater than the average sale across all products.
SELECT 
    p.product_id, 
    p.product_name, 
    SUM(o.sale) AS total_sales
FROM 
    orders o
JOIN 
    products p ON o.product_id = p.product_id
GROUP BY 
    p.product_id, 
    p.product_name
HAVING 
    SUM(o.sale) > (SELECT SUM(sale) / COUNT(DISTINCT product_id) FROM orders)
ORDER BY 
    total_sales DESC
LIMIT 5;

--Q10. Calculate the profit margin percentage for each sale
SELECT 
    order_id,
    ((SUM(o.price_per_unit - p.cogs) * o.quantity) / SUM(o.sale)) * 100 AS profit_margin_percentage
FROM 
    orders AS o
LEFT JOIN 
    products AS p ON o.product_id = p.product_id
GROUP BY 
    order_id;
