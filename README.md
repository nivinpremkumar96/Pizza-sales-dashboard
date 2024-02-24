# Pizza-sales-dashboard
##Pizza sales analysis using ms sqlserver and excel

--  Problem Statement
-- KPI Requirement 


-- 1. What is the total revenue .?

select avg(total_price) as total_revenue
from pizza_sales

-- Result - 817860.05

-- 2. What is the avarage order value .?

select SUM(total_price) / COUNT (Distinct (order_id))  as average_order_value
from pizza_sales

-- Result - 38.30

-- 3. Total pizzas sold .?

select SUM(quantity)  as total_pizza_sold
from pizza_sales

-- Result - 49574

-- 4.Total Order Placed .?

select COUNT(distinct(order_id)) as total_order_placed 
from pizza_sales

-- Result - 21350

-- Average Pizza per order.?

select cast(SUM(quantity) as decimal (10,2)) /cast(COUNT(distinct(order_id)) as decimal(10,2))
from pizza_sales

-- Result - 2.3219672131147

-- Chart Requirement 

-- Daily trend for Total order 

select DATENAME (DW, order_date) as order_day ,COUNT(distinct(order_id)) as total_order
from pizza_sales
group by DATENAME (DW, order_date)

-- Hourly trend for Total order 

select datepart(HOUR,order_time) as order_hours, count(distinct order_id) as total_order
from pizza_sales 
group by datepart(HOUR,order_time)
order by datepart(HOUR,order_time)


 -- Percentage of sales by pizza category 

 select pizza_category,SUM(total_price)as total_sales,(SUM(total_price)*100 / (select SUM(total_price) from pizza_sales)) as pct_sales
 from pizza_sales
 group by pizza_category

 -- Percentage of sales by pizza size

select pizza_size,round(SUM(total_price),2) as total_sales,round(SUM(total_price)*100 / 
(select SUM(total_price) from pizza_sales),2) as pct_sales
 from pizza_sales
 group by pizza_size

 -- Total Pizzas Sold by pizza Category

 select pizza_category, SUM(quantity) as total_pizzas
 from pizza_sales
 group by pizza_category


-----------------------------------------------END-------------------------------------------------------------------------
 -- Top 5 best sellers by pizza name 
 
 select Top 5 pizza_name , SUM(quantity) as total_quantity 
 from pizza_sales
 group by pizza_name
 order by sum(quantity) desc

