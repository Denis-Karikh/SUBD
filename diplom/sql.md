```sql
CREATE database Shop;
USE Shop;

CREATE TABLE Customers (
 Customers_id int NOT NULL PRIMARY KEY,
 Customers_name varchar(100),  
 Phone_number varchar(50) UNIQUE);

CREATE TABLE Stock (
 Stock_id INT NOT NULL PRIMARY KEY,
 Product_id INT(11) NOT NULL,
 COUNT INT not null); 


CREATE TABLE Orders (
 ORDERS_ID INT NOT NULL PRIMARY KEY,
 Product_id INT(11) NOT NULL,
 Customers_id INT NOT NULL, 
 Buy_date DATE DEFAULT (sysdate())
);

CREATE TABLE Price (
 Price_id int NOT NULL PRIMARY KEY,

 Product_id INT(11) NOT NULL, 
 Price FLOAT NOT NULL CHECK(PRICE >= 0)
);

CREATE TABLE discount_dayweek (
	id TINYINT PRIMARY KEY AUTO_INCREMENT, 
	dayweek VARCHAR(50),
	Category_id int(11),
	discount decimal DEFAULT 0 CHECK(discount between 0 and 90)
);

-- PRODUCT_CATEGORY

CREATE TABLE Product_Category (
  Category_id int(11) NOT NULL AUTO_INCREMENT,
  Name varchar(200) NOT NULL,
  Discount tinyint(11) DEFAULT '0',
  Bsic_Discount tinyint(11) DEFAULT '0',
  EDiscount tinyint(11) DEFAULT '0',
  PRIMARY KEY (Category_id)
);


CREATE TABLE Provider (
  Provider_id int(11) NOT NULL AUTO_INCREMENT,
  Provider_name varchar(200) NOT NULL,
  Phone_number varchar(50) UNIQUE,
  Email varchar(200) UNIQUE,
  PRIMARY KEY (Provider_id)
);


-- PRODUCT
CREATE TABLE Product (
  Product_id int(11) NOT NULL AUTO_INCREMENT,
  Product_name varchar(200) NOT NULL,
  Category_id int(11) NOT NULL,
  Discount int(11) DEFAULT '0',
  Manufacturer_id int(11) NOT NULL,
  basic_dicount decimal(10,0) DEFAULT '0',
  Provider_id int(11) NOT NULL,
  PRIMARY KEY (Product_id)
);

-- Manufacturer
CREATE TABLE Manufacturer (
  Manufacturer_id int(11) NOT NULL AUTO_INCREMENT,
  Name varchar(200) DEFAULT NULL,
  Provider_id int(11) NOT NULL,
  PRIMARY KEY (Manufacturer_id)
);


CREATE INDEX idx_Product_name  ON Product(Product_name);
CREATE INDEX idx_Category_name  ON Product_Category(Name); 
CREATE INDEX idx_Customers_Phone  ON Customers(Phone_number); 
CREATE INDEX idx_Manufacturer_Name  ON Manufacturer(Name); 
CREATE INDEX idx_Product_id  ON Product(Product_id); 
CREATE INDEX idx_Price_Product_id  ON Price(Product_id);
CREATE INDEX idx_Orders_Customers_Product  ON Orders(Customers_id,Product_id); 
CREATE INDEX idx_Product_Category_Product_name  ON Product(Category_id,Product_name);
CREATE FULLTEXT INDEX idx_FULLTEXT_Product ON Product (Product_name,Description,Property);

CREATE DEFINER=root`@% PROCEDURE `Shop.Disco()
Begin
  DECLARE WEEK_DAY VARCHAR(125);
    SET WEEK_DAY = DAYNAME(NOW());
    UPDATE Product_Category pc set EDiscount = 1;
    UPDATE Product p set Discount = basic_dicount;
  UPDATE Product p
   inner join discount_dayweek dd on p.Category_id = dd.Product_Category_id 
   SET p.Discount = p.basic_dicount + dd.discount
   WHERE  WEEK_DAY = dd.dayweek ;
   UPDATE Product_Category pc
   inner join discount_dayweek dd on dd.Product_Category_id = pc.Category_id
   SET pc.EDiscount = 0 where WEEK_DAY =dd.dayweek;
 END;

```
