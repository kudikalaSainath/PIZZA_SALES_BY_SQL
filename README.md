# PIZZA_SALES_BY_SQL
🚀 Excited to share my latest project: a comprehensive Pizza Sales Analysis using SQL! 🍕📊 Dive into insights on sales trends, customer preferences, and more. Check it out! #DataAnalytics #SQL #PizzaSales #SaiNath
PIZZA_SALES_SQL.pdf

QUESTIONS
Basic:
Retrieve the total number of orders placed.
Calculate the total revenue generated from pizza sales.
Identify the highest-priced pizza.
Identify the most common pizza size ordered.
List the top 5 most ordered pizza types along with their quantities.

Intermediate:
Join the necessary tables to find the total quantity of each pizza category ordered.
Determine the distribution of orders by hour of the day.
Join relevant tables to find the category-wise distribution of pizzas.
Group the orders by date and calculate the average number of pizzas ordered per day.
Determine the top 3 most ordered pizza types based on revenue.

Advanced:
Calculate the percentage contribution of each pizza type to total revenue.
Analyze the cumulative revenue generated over time.
Determine the top 3 most ordered pizza types based on revenue for each pizza category.
-1)Retrieve the total number of orders placed.

select count(order_id) as total_orders
from
portfolio-project-430108.pizzahut.orders

select
round(sum(od.quantity * p.price),2)
as total_revenue
from
portfolio-project-430108.pizzahut.order_details as od
inner join
portfolio-project-430108.pizzahut.pizzas as p
on
p.pizza_id = od.pizza_id

3)select
pt.name,p.price
from
portfolio-project-430108.pizzahut.pizza_types as pt inner join
portfolio-project- 430108.pizzahut.pizzas as p
on
p.pizza_type_id = pt.pizza_type_id
order by p.price desc
limit 1

4)select
p.size, count(od.order_details_id) as ordered_size
from
portfolio-project-430108.pizzahut.order_details as od inner join
portfolio-project-430108.pizzahut.pizzas as p
on p.pizza_id = od.pizza_id
group by p.size
order by ordered_size desc

5)select
pt.name,sum(od.quantity) as Quantity
from
portfolio-project-430108.pizzahut.pizzas as p inner join portfolio-project-430108.pizzahut.pizza_types as pt
on
pt.pizza_type_id = p.pizza_type_id join
portfolio-project-430108.pizzahut.order_details as od
on od.pizza_id = p.pizza_id
group by pt.name
order by Quantity desc limit 5

6)select
pt.category,sum(od.quantity) as quantity
from
portfolio-project-430108.pizzahut.pizza_types as pt
inner join
portfolio-project-430108.pizzahut.pizzas as p on
p.pizza_type_id = pt.pizza_type_id
join
portfolio-project-430108.pizzahut.order_details as od
on od.pizza_id = p.pizza_id
group by pt.category
order by quantity desc

7)select
extract(hour from o.time) as hour ,count(o.order_id) as orders
from
portfolio-project-430108.pizzahut.orders as o
group by hour

8)select
pt.category, count(pt.name) as pizzas_names
from
 portfolio-project-430108.pizzahut.pizza_types as pt
group by pt.category

9)select
round(avg(quantity),0) as avg_day from
(select
o.date,sum(od.quantity) as quantity
from
portfolio-project-430108.pizzahut.orders as o inner join
portfolio-project-430108.pizzahut.order_details as od
on od.order_id = o.order_id
group by o.date)
as order_quantity

10)select
pt.name,sum(od.quantity*p.price) as revenue
from
portfolio-project-430108.pizzahut.pizza_types as pt inner join
portfolio-project-430108.pizzahut.pizzas as p on
pt.pizza_type_id = p.pizza_type_id
inner join
portfolio-project-430108.pizzahut.order_details as od on
p.pizza_id = od.pizza_id
group by pt.name
order by revenue desc
limit 3

11)select
pt.category, round(sum(od.quantity * p.price)/
(select round(sum(od.quantity *p.price),2)
from portfolio-project-430108.pizzahut.order_details as od inner join
portfolio-project-430108.pizzahut.pizzas as p on
p.pizza_id=od.pizza_id) *100,2 ) as revenue
from
portfolio-project-430108.pizzahut.pizza_types as pt inner join portfolio-project-430108.pizzahut.pizzas as p
on pt.pizza_type_id = p.pizza_type_id
inner join
portfolio-project-430108.pizzahut.order_details as od
on p.pizza_id = od.pizza_id
group by
pt.category
order by revenue desc

select
date,round(sum(revenue) over(order by date),2) as cum_revenue
from(
select
o.date, sum(od.quantity*p.price) as revenue
from
portfolio-project-430108.pizzahut.order_details as od inner join
portfolio-project-430108.pizzahut.pizzas as p
on od.pizza_id=p.pizza_id
inner join portfolio-project-430108.pizzahut.orders as o
on o.order_id = od.order_id
group by o.date) as total_sales

13)select category,name,revenue, rank
from
(select
category,name,revenue,rank() over(partition by category order by revenue desc ) as rank
from
(select pt.category,pt.name, sum(od.quantity * p.price) as revenue
from portfolio-project-430108.pizzahut.pizza_types as pt inner join
portfolio-project-430108.pizzahut.pizzas as p on p.pizza_type_id = pt.pizza_type_id join portfolio-project-430108.pizzahut.order_details as od on od.pizza_id = p.pizza_id
group by pt.category,pt.name))
where rank <= 3

CREDITS TO
@WsCube_Tech
