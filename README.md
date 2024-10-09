# SQL queries for pizza sales analysis

## Join relevant tables to find the category wise distribution of pizza
 
 ```
select category , count(name) from pizza_types
 group by category;
 ```

## reterive the total number of orders placed

```
select count(order_id) as total_orders from orders;
```
