# SQL queries for pizza sales analysis

## Join relevant tables to find the category wise distribution of pizza
 
 ```
select category , count(name) from pizza_types
 group by category;
 ```

## Reterive the total number of orders placed

```
select count(order_id) as total_orders from orders;
```

## Calculate the total revenue generated  from pizza sales

```
select round(sum(orders_details.quantity * pizzas.price),2)as total_sales 
from orders_details join pizzas
on pizzas.pizza_id= orders_details.pizza_id;
```

##  Identify the highest priced pizza
```
select pizza_types.name , pizzas.price
from pizza_types join pizzas
on pizza_types.pizza_type_id = pizzas.pizza_type_id
order by pizzas.price desc limit 1;
```

## Identify the most common pizza size ordered
```
select pizzas.size, count(orders_details.order_details_id) as orders_count
from pizzas join orders_details
on pizzas.pizza_id = orders_details.pizza_id
group by pizzas.size order by orders_count desc limit 1;
```

## List the top 5 most ordered pizza type along with their quantities
```
select pizza_types.name , 
sum(orders_details.quantity) as quantity 
from pizza_types join pizzas
on pizza_types.pizza_type_id=pizzas.pizza_type_id
join orders_details
on orders_details.pizza_id=pizzas.pizza_id
group by pizza_types.name order by quantity desc limit 5;
```

## Join the necessary tables to find the total quantity of each pizza category ordered
```
select pizza_types.category,
sum(orders_details.quantity) as quantity 
from pizza_types join pizzas
on pizza_types.pizza_type_id = pizzas.pizza_type_id
join orders_details
on orders_details.pizza_id = pizzas.pizza_id
group by pizza_types.category order by quantity desc;
```

## Describe the distribution of orders by hour of the day
```
select hour(order_time) as hour, count(order_id) as order_count from orders
group by hour(order_time);
```
## Group the orders by date and calculate the average number of pizzas order per day
```
select avg(quantity) from
(select orders.order_date, sum(orders_details.quantity) as quantity from
orders join orders_details
on orders.order_id = orders_details.order_id
group by orders.order_date) as order_quantity;
```

##  Determine the top 3 most ordered pizza types based on revenue
```
select pizza_types.name , 
sum(orders_details.quantity * pizzas.price) as revenue
from pizza_types join pizzas
on pizzas.pizza_type_id = pizza_types.pizza_type_id
join orders_details
on orders_details.pizza_id = pizzas.pizza_id
group by pizza_types.name order by revenue desc limit 3 ;
```

##  Determine the top 3 most ordered pizzas types based on revenue for each pizza category
```
select name , revenue from 
(select category, name , revenue ,
rank() over(partition by category order by revenue desc) as rn
from 
(select pizza_types.category,pizza_types.name,
sum((orders_details.quantity) * pizzas.price) as revenue
from pizza_types join pizzas
on pizza_types.pizza_type_id = pizzas.pizza_type_id
join orders_details
on orders_details.pizza_id = pizzas.pizza_id
group by pizza_types.category, pizza_types.name) as a ) as b
where rn<=3;
```



















