# Restaurant-Orders Using mySQL workbench

![Restaurant Logo](https://github.com/Oriakhi-Osariemen/Restaurant-Orders/blob/main/shutterstock_1678594945-1880x880-1.jpg)


## Objective 1: Explore menu_items_table
1. View the menu_items table
```sql SELECT * FROM menu_items; ```

2. Find the number of items on the menu
```sql
SELECT count(*) FROM menu_items;
```

3. What are the least and most expensive items on the menu 
```sql
SELECT *
FROM menu_items
ORDER BY price;

SELECT * FROM menu_items
ORDER BY price DESC;
```

4. How Many Italian dishes are on the menu
```sql
SELECT COUNT(*) FROM menu_items
WHERE category = 'Italian';
```

```sql
5. What are the least and most expensive Italian dishes on the menu
SELECT * 
FROM menu_items
WHERE category = 'Italian'
ORDER BY price;

SELECT * 
FROM menu_items
WHERE category = 'Italian'
ORDER BY price DESC;
```

6. How many dishes are in each category
```sql
SELECT category, COUNT(menu_item_id) AS num_dishes
FROM menu_items
GROUP BY category;
```


7. What is the average dish price within each category
```sql
SELECT category, ROUND(AVG(price)) AS avg_dishes
FROM menu_items
GROUP BY category;
```



## Objective 2: Explore order_details table

1. View the order_details table
```sql
SELECT * FROM order_details;
```

2. What is the date range of the table
```sql
SELECT MIN(order_date), MAX(order_date)
FROM order_details
ORDER BY order_date;
```

3. How many orders were made within this date range
```sql
SELECT COUNT(DISTINCT order_id)
FROM order_details
ORDER BY order_date;
```

4. How many items were ordered within this date range
```sql
SELECT COUNT(*)
FROM order_details;
```

5. Which orders had the most number of items
```sql
SELECT order_id, COUNT(item_id) AS num_item
  FROM order_details
GROUP BY order_id
ORDER BY num_item DESC;
```

6. How many orders had more than 12 items
```sql
SELECT COUNT(*) AS More_than_12 FROM
	(SELECT order_id, COUNT(item_id) AS num_item
	  FROM order_details
GROUP BY order_id
HAVING num_item > 12) AS num_orders;
```




## Objective 3: Analyze Customer Behaveior;

1. Combine the menu_items and order_details tables into a single table.
```sql SELECT * 
  FROM order_details od 
LEFT JOIN menu_items mi 
ON od.item_id = mi.menu_item_id;
```

2. What were the least and most ordered items? What categories were they in?
```sql
SELECT item_name, category,COUNT(order_details_id) AS num_purchases 
  FROM order_details od 
  LEFT JOIN menu_items mi 
	ON od.item_id = mi.menu_item_id
GROUP BY item_name, category
ORDER BY num_purchases DESC;
```

3. What were the top 5 orders that spent the most money
```sql
SELECT order_id, SUM(price) AS total_spend
FROM order_details od 
  LEFT JOIN menu_items mi 
	ON od.item_id = mi.menu_item_id
GROUP BY order_id
ORDER BY total_spend DESC
LIMIT 5;
```

4. View the details of the highest spend order. What insights can you gather from the results?
```sql
SELECT category, COUNT(item_id) AS num_items
FROM order_details od 
  LEFT JOIN menu_items mi 
	ON od.item_id = mi.menu_item_id
WHERE order_id = 440
GROUP BY category
ORDER BY num_items DESC;
```

5. View the details of the top 5 highest spend orders. What insights can you gather from results?
```sql
SELECT category, COUNT(item_id) AS num_items
FROM order_details od 
  LEFT JOIN menu_items mi 
	ON od.item_id = mi.menu_item_id
WHERE order_id IN (440, 2075, 1957, 330, 2675)
GROUP BY category
ORDER BY num_items DESC;
```



