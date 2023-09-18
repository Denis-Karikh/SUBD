## 1.Забрал стартовый репозиторий.

![alt text](https://i.ibb.co/Gs44dcv/123.png)

## 2. Прописал свой скрипт на создание БД.
```sql
CREATE database Shop;
USE Shop;

CREATE TABLE Customers (
 Customers_id int NOT NULL PRIMARY KEY,
 Customers_name varchar(100),  
 Phone_number varchar(100) UNIQUE);

CREATE TABLE Stock (
 Stock_id INT NOT NULL PRIMARY KEY,
 Product_id INT NOT NULL,
 COUNT INT not null); 


CREATE TABLE Orders (
 ORDERS_ID INT NOT NULL PRIMARY KEY,
 Product_id INT NOT NULL,
 Customers_id INT NOT NULL, 
 Buy_date DATE DEFAULT (sysdate())
);

CREATE TABLE Price (
 Price_id int NOT NULL PRIMARY KEY,

 Product_id INT NOT NULL, 
 Price FLOAT NOT NULL CHECK(PRICE >= 0)
);

CREATE TABLE Manufacturer (
  Manufacturer_id int PRIMARY KEY NOT NULL,
  Name varchar ( 200 ),
  Provider_id int NOT NULL
);

CREATE TABLE Provider (
  Provider_id int PRIMARY KEY NOT NULL,
  Provider_name varchar ( 200 ) NOT NULL,
  Product_id int NOT NULL
);

CREATE TABLE Product_Category (
  Category_id int PRIMARY KEY NOT NULL, 
  Name varchar ( 200 ) NOT NULL,

  Discount int default 0 CHECK(PRICE >=0)
);

CREATE TABLE Product (
  Product_id int PRIMARY KEY NOT NULL, 
  Product_name varchar ( 200 )  NOT NULL, 
  Category_id int NOT NULL, 
  Discount int default 0 CHECK(PRICE >=0), 
  Manufacturer_id int NOT NULL 
);


CREATE INDEX idx_Product_name  ON Product(Product_name);
CREATE INDEX idx_Category_name  ON Product_Category(Name); 
CREATE INDEX idx_Customers_Phone  ON Customers(Phone_number); 
CREATE INDEX idx_Manufacturer_Name  ON Manufacturer(Name); 
CREATE INDEX idx_Product_id  ON Product(Product_id); 
CREATE INDEX idx_Price_Product_id  ON Price(Product_id);
CREATE INDEX idx_Orders_Customers_Product  ON Orders(Customers_id,Product_id); 
CREATE INDEX idx_Product_Category_Product_name  ON Product(Category_id,Product_name);
```
## 3.Выполнил шаги согласно инструкции

![alt text](https://i.ibb.co/qYK9zNg/1234.png)

![alt text](https://i.ibb.co/Tvkrh7Y/12345.png)
