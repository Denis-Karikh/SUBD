# 1.Схема БД
## 1.1 Диаграмма БД

<a href="https://ibb.co/mNDNnh1"><img src="https://i.ibb.co/MB2BzPW/Untitled-1.png" alt="Схема" border="0"></a>

## 1.2 Таблицы и описание.

### 1.2.1 Описание таблицы Product
Таблица продуктов в которой содержится информация о товарах, и в какой категории находится данный товар, какая скидка на данных товар и кто поставщик.
| column_name | data_type |
| :---    |    ---: |
| Product_id   | int(11) |
| Product_name | varchar(200) |
| Category_id   | int(11) |
| Discount | int(11) |
| Manufacturer_id   | int(11) |
| basic_dicount | decimal(10,0) |
| Provider_id   | int(11) |
### 1.2.2 Описание таблицы Orders
Таблица в которой находится информация по заказам товара, Id товара, id покупателя, дата покупки.
| column_name | data_type |
| :---    |    ---: |
| ORDERS_ID   | int |
| Product_id | int(11) |
| Customers_id   | int |
| Buy_date | DATE |
### 1.2.3 Описание таблицы Customers
Таблица в которой находится данные о покупателях, имя и телефон.
| column_name | data_type |
| :---    |    ---: |
| Customers_id   | int |
| Customers_name | varchar(100) |
| Phone_number   | varchar(50) |
### 1.2.4 Описание таблицы Provider
Таблица в которой хранится информация о поставщиках, имя, телефон и email.
| column_name | data_type |
| :---    |    ---: |
| Customers_id   | int |
| Customers_name | varchar(100) |
| Phone_number   | varchar(50) |
| Email   | varchar(200) |
### 1.2.5 Описание таблицы Product_Category
Таблица в которой хранится информация по категориям продуктов и разнообразные скидки на категории продуктов.(На них завязана процедура расчета скидок ежидневная)
| column_name | data_type |
| :---    |    ---: |
| Category_id   | int(11) |
| Name | varchar(200) |
| Discount   | tinyint(11) |
| Bsic_Discount   | tinyint(11) |
| EDiscount   | tinyint(11) |
### 1.2.6 Описание таблицы Stock
Таблица склад, в которой отображаются продукты и их кол-во на остатке.
| column_name | data_type |
| :---    |    ---: |
| Stock_id   | int |
| Product_id | int(11)) |
| COUNT   | INT |
### 1.2.7 Описание таблицы  Price
| column_name | data_type |
| :---    |    ---: |
| Price_id   | int |
| Product_id | int(11) |
| Price   | Float |
### 1.2.8 Описание таблицы discount_dayweek
Таблица discount_dayweek для формирования уникальных скидок на каждый день для определённых категорий товаров.
| column_name | data_type |
| :---    |    ---: |
| id   | TINYINT |
| dayweek | VARCHAR(50) |
| Category_id   | int(11) |
| discount   | decimal |



