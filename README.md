SQL Queries to Clean and Manipulate the Data: 

select count(*)
from pizza;

select *
from pizza;

Select sum(total_price) as total_revenue 
from pizza;


select sum(total_price)/count(distinct order_id) as average_order_value
from pizza;

select
sum(quantity) as total_pizza_sold
from pizza;

select 
count(distinct order_id) as total_nu_orders
from pizza;

select 
sum(quantity)/count(distinct order_id) as average_pizza_per_order
from pizza;

-- hourly trend for pizzas sold

select
hour(order_time) as order_hour,
sum(quantity) as total_pizza_sold 
from pizza
group by order_hour
order by 1;


-- Weekly trend for pizza order 

select
weekofyear(order_date) as number_week,
year(order_date) as year_date,
count(distinct order_id) as total_order
from pizza 
group by weekofyear(order_date),year(order_date)
order by weekofyear(order_date);

-- percentage of sales by pizza category 

select 
pizza_category, sum(total_price)*100/ (select sum(total_price) from pizza) as total_sales 
from pizza
group by pizza_category;

select 
pizza_category, sum(total_price) as total_sales, sum(total_price)*100/ (select sum(total_price) from pizza where month(order_date) = 3) as total_sales 
from pizza
where month(order_date) = 3
group by pizza_category;


-- percentage of pizza sales according to the size 

select 
pizza_size, cast(sum(total_price) as decimal (10,2)) as total_sales,
CAST(sum(total_price)*100/ (select sum(total_price) from pizza) as decimal (10,2)) as total_per_sales
from pizza
group by pizza_size
order by total_per_sales DESC;


-- top 5 best pizza according to the revenue 

select 
pizza_name, 
sum(total_price) as pizza_revenue
from pizza 
group by pizza_name
order by pizza_revenue desc
limit 5;

-- bottom 5 pizza
select 
pizza_name, 
sum(total_price) as pizza_revenue
from pizza 
group by pizza_name
order by pizza_revenue asc
limit 5;

-- top 5 best pizza according to the quantity

select 
pizza_name, 
sum(quantity) as pizza_quantity
from pizza 
group by pizza_name
order by pizza_quantity desc
limit 5;

-- bottom 5 pizza according to the quantity 

select pizza_name, 
sum(quantity) as pizza_quantity
from pizza 
group by pizza_name
order by pizza_quantity asc
limit 5;
