-- A. Pizza Metrics

-- How many pizzas were ordered?

SELECT COUNT(*) FROM pizza_runner.customer_orders;

-- How many unique customer orders were made?

SELECT COUNT(DISTINCT(order_id)) FROM pizza_runner.customer_orders;

-- How many successful orders were delivered by each runner?

SELECT 
	runner_id,
	COUNT(DISTINCT(order_id)) AS delivered 
FROM pizza_runner.runner_orders
WHERE pickup_time<>'null'
GROUP BY runner_id;

-- How many of each type of pizza was delivered?

SELECT 
	pizza_name,
	COUNT(*)
FROM pizza_runner.customer_orders AS c
INNER JOIN pizza_runner.pizza_names AS pn ON c.pizza_id = pn.pizza_id
INNER JOIN pizza_runner.runner_orders AS r ON c.order_id = r.order_id
WHERE pickup_time<>'null'
GROUP BY pizza_name;

-- How many Vegetarian and Meatlovers were ordered by each customer?

SELECT 
	customer_id,
	pizza_name,
	COUNT(pizza_name) AS number_of_za
FROM pizza_runner.customer_orders AS C
INNER JOIN pizza_runner.pizza_names AS pn ON c.pizza_id = pn.pizza_id
GROUP BY customer_id, pizza_name
ORDER BY customer_id;

-- What was the maximum number of pizzas delivered in a single order

SELECT 
	order_id,
	COUNT(order_id) AS number_of_pizzas
FROM pizza_runner.customer_orders
GROUP BY order_id
ORDER BY COUNT(order_id) DESC
LIMIT 1;

-- For each customer, how many delivered pizzas had at least 1 change and how many had no changes?

SELECT 
	customer_id,
	SUM(CASE
	WHEN exclusions<>'null' AND LENGTH(exclusions)>0 OR extras<>'null' AND LENGTH(extras)>0 AND extras IS NOT NULL = true THEN 1
	ELSE 0
	END) AS changes,
	SUM(CASE
	WHEN exclusions<>'null' AND LENGTH(exclusions)>0 OR extras<>'null' AND LENGTH(extras)>0 AND extras IS NOT NULL = true THEN 0
	ELSE 1
	END) AS no_changes
FROM pizza_runner.customer_orders AS c
INNER JOIN pizza_runner.runner_orders AS r ON c.order_id = r.order_id
WHERE pickup_time<>'null'
GROUP BY customer_id
ORDER BY customer_id;

-- How many pizzas were delivered that had both exclusions and extras?

WITH CTE AS(
SELECT 
	CASE
	WHEN exclusions<>'null' AND LENGTH(exclusions)>0 AND extras<>'null' AND LENGTH(extras)>0 AND extras IS NOT NULL = true THEN 1
	ELSE 0
	END AS pizzas_changed
FROM pizza_runner.customer_orders AS c
INNER JOIN pizza_runner.runner_orders AS r ON c.order_id = r.order_id
WHERE pickup_time<>'null')
SELECT 
	COUNT(pizzas_changed) AS exclusions_and_extras
FROM CTE
WHERE pizzas_changed = 1;

-- What was the total volume of pizzas ordered for each hour of the day?

SELECT 
	DATE_PART('Hour',order_time) AS hour,
	COUNT(*) AS pizzas
FROM pizza_runner.customer_orders AS c
GROUP BY hour
ORDER BY hour;

-- What was the volume of orders for each day of the week?

SELECT 
	DATE_PART('Day',order_time) AS day,
	COUNT(*) AS pizzas
FROM pizza_runner.customer_orders AS c
GROUP BY day
ORDER BY day;
