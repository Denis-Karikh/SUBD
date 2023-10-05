```sql
Выполняем поиск по самым востребованным товарам у бренда
Select m.Name ,p.Product_name ,count(o.Product_id) from Product p
inner join Provider p2 on p.Product_id = p2.Product_id 
JOIN Manufacturer m  on p2.Provider_id  = m.Provider_id 
LEFT JOIN Orders o on o.Product_id =p.Product_id 
group BY m.Name , p.Product_name 



Выбираем все товары в опредленном диапозоне
SELECT * from Product p 
join Price p2 on p.Product_id = p2.Product_id 
where price BETWEEN 500 and 2000

Ищем все товары содержащие в наименование Чай
Select * from Product p 
where Product_name  like 'Чай%'

Ищем товар Чайник
SELECT * from Product p 
Where Product_name = 'Чайник'

Ищем номера поставщиков у которых у нас не заполнено имя, для актуализации данных.
SELECT phone_number from Customers c 
where Customers_name is NULL 

Ищем всё по поставщику 'Алмаз' или 'Рога и копаты'
 Select * from Product p 
 join Provider p2 on p.Product_id = p2.Product_id 
 where p2.Provider_name = 'Алмаз' OR  p2.Provider_name = 'Рога и копыта'
```
