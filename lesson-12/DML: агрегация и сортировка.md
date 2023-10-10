# 1) Группировки с ипользованием CASE, HAVING, ROLLUP, GROUPING()
```sql
with
	zap as (SELECT 	customer_id,COUNT(customer_id) as tot from orders o 
group by customer_id )
Select email,
CASE 
	when zap.tot <= 2 then 'Средний клиент'  WHEN  zap.tot >= 3 then 'Ок клиент' WHEN  zap.tot is null then 'Не ок' 
END as Prof
from customers c
left join zap on zap.customer_id = c.customer_id 
order by email
```
```sql
Select customer_id,sum(total) from orders o 
group by customer_id 
HAVING sum(total)>1000
```
```sql
select customer_id , sum(total) from orders o 
group by customer_id with ROLLUP 
```
```sql
SELECT
  IF(GROUPING(customer_id), 'ИТОГО', customer_id) AS customer_id,
  SUM(total) AS tot
FROM orders o 
GROUP BY customer_id  WITH ROLLUP;
```
# 2) Для магазина к предыдущему списку продуктов добавить максимальную и минимальную цену и кол-во предложений
```sql
SELECT 
    MAX(p.price),
    MIN(p.price),
    COUNT(1) 
FROM products p
```

# 3) Сделать выборку показывающую самый дорогой и самый дешевый товар в каждой категории
```sql
with 
  cats as (SELECT DISTINCT category, MAX(price) as max, MIN(price) as min FROM products p group by category)
SELECT
  c.category, 
  (SELECT group_concat(p2.title separator'\n') as title FROM products p2 where p2.price = c.max and p2.category = c.category) as 'max_title',
  c.max,
  (SELECT group_concat(p2.title separator'\n') as title FROM products p2 where p2.price = c.min and p2.category = c.category) as 'min_title',
  c.min
FROM cats c
```

# 4) Сделать rollup с количеством товаров по категориям
```sql
SELECT
IF(GROUPING(category), 'ИТОГО', category) AS category,
  COUNT(*) AS count
FROM products
GROUP BY category WITH ROLLUP
```

