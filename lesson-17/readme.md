## 1. 1. Создать пользователей client, manager.
```sql
CREATE USER 'client'@'%' IDENTIFIED BY 'password';
CREATE USER 'manager'@'%' IDENTIFIED BY 'password';
```
### Проверяем добавились ли пользователи
```sql
SELECT Host, User FROM mysql.user;
```

## 2. Создать процедуру выборки товаров get_products с использованием различных фильтров:
### Категория, цена, производитель, различные дополнительные параметры.
### Также в качестве параметров передавать по какому полю сортировать выборку, и параметры постраничной выдачи
```sql
DROP PROCEDURE IF EXISTS get_products;

DELIMITER $$

CREATE PROCEDURE get_products(
  IN InFilterBy TEXT,
  IN InFilterValue TEXT,
  IN InOrderBy TEXT,
  IN InOffset INT,
  IN InLimit INT
)
BEGIN
  SELECT
    ProductID,
    VendorID,
    StandardPrice
  FROM
    productvendor
  WHERE
    CASE InFilterBy
      WHEN 'ProductID' THEN ProductID = InFilterValue
      WHEN 'VendorID' THEN VendorID = InFilterValue
      WHEN 'StandardPrice' THEN StandardPrice = InFilterValue
      ELSE NULL IS NULL
    END
  ORDER BY
    CASE InOrderBy
      WHEN 'ProductID' THEN ProductID
      WHEN 'VendorID' THEN VendorID
      WHEN 'StandardPrice' THEN StandardPrice
      ELSE 'ProductID, VendorID, StandardPrice'
    END
  LIMIT InLimit OFFSET InOffset;
END$$

DELIMITER ;
```
## 3. Дать права на запуск процедуры get_products пользователю client
```sql
GRANT EXECUTE ON PROCEDURE otus.get_products TO 'client'@'%';
```

## 4. Создать процедуру get_orders
### Которая позволяет просматривать отчет по продажам за определенный период (час, день, неделя) с различными уровнями группировки (по товару, по категории, по производителю)
```sql
DROP PROCEDURE IF EXISTS get_orders;

DELIMITER $$

CREATE PROCEDURE get_orders(
  IN InStartDate TIMESTAMP,
  IN InEndDate TIMESTAMP,
  IN InGroupBy TEXT
)
BEGIN
  CASE InGroupBy
    WHEN 'ProductID' THEN
      SELECT ProductID, SUM(UnitPrice)
      FROM salesorderdetail
      WHERE ModifiedDate BETWEEN InStartDate AND InEndDate
      GROUP BY ProductID;
    WHEN 'CarrierTrackingNumber' THEN
      SELECT CarrierTrackingNumber, SUM(UnitPrice)
      FROM salesorderdetail
      WHERE ModifiedDate BETWEEN InStartDate AND InEndDate
      GROUP BY CarrierTrackingNumber;
    WHEN 'SpecialOfferID' THEN
      SELECT SpecialOfferID, SUM(UnitPrice)
      FROM salesorderdetail
      WHERE ModifiedDate BETWEEN InStartDate AND InEndDate
      GROUP BY SpecialOfferID;
    ELSE
      SELECT ProductID, SUM(UnitPrice)
      FROM salesorderdetail
      WHERE ModifiedDate BETWEEN InStartDate AND InEndDate
      GROUP BY ProductID;
  END CASE;
END$$

DELIMITER ;
```
## 5. Права дать пользователю manager
```sql
GRANT EXECUTE ON PROCEDURE otus.get_orders TO 'manager'@'%';
```
