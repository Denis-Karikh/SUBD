# Проанализировать типы данных в своем проекте, изменить при необходимости. 
Проанализировав данные, было принятно решение изменить значения в  TABLE Price значение Price с int на DECIMAL(10, 2) , так же в таблице Product_Category значение с int на DECIMAL(2, 2)
У хранение цены и скидки в типе данных decimal есть ряд преимуществ :
```sql
ALTER TABLE Shop.Price MODIFY Price DECIMAL(10,2);
ALTER TABLE Shop.Product ADD COLUMN info JSON;
ALTER TABLE Shop.Product MODIFY Discount DECIMAL(2,2);
```

```
Точность: DECIMAL предоставляет точное представление десятичных чисел. Это важно для цен, так как нам нужно избегать ошибок округления при выполнении математических операций, таких как сложение и вычитание цен. Если бы вы использовали тип данных INT, то при выполнении таких операций могли бы возникнуть проблемы с округлением.

Фиксированная точность: Вы можете указать фиксированную точность и масштаб для полей DECIMAL. Например, DECIMAL(10, 2) означает, что у вас есть 10 знаков в числе, из которых 2 после десятичной точки. Это позволяет контролировать количество знаков до и после десятичной точки и обеспечивает консистентность данных.

Удобочитаемость: Цены чаще всего представляются с дробной частью (например, $19.99), и DECIMAL позволяет хранить и отображать цены в формате, близком к тому, что видят пользователи.

Поддержка валют: Если вам нужно хранить информацию о валюте вместе с ценами, вы можете легко добавить дополнительное поле для валюты, не внося изменений в формат цены.
```

# Добавить тип JSON в структуру. Проанализировать какие данные могли бы там хранится. привести примеры SQL для добавления записей и выборки.
```sql
INSERT INTO Shop.Product_Category (Category_id, Name) VALUES (1, 'Дом');

INSERT INTO Shop.Provider (Provider_id, Provider_name, Product_id) VALUES (1, 'ООО Чайник', 1);
INSERT INTO Shop.Manufacturer (Manufacturer_id, Name, Provider_id) VALUES (1, 'ООО Чайник', 1);

INSERT  INTO Shop.Product (Product_id, Product_name, Category_id, Manufacturer_id, info) 
VALUES (2, 'Чайник', 1, 1 ,'{"Описание": "Чайник для заварки", "Размер":"75х75х120", "Состав":"Стекло"}');

SELECT
Product_name,
Discount,
json_extract(info, '$."Описание"') AS "Описание",
json_extract(info, '$."Размер"') AS "Размер",
json_extract(info, '$."Состав"') AS "Состав"
FROM Shop.Product p
```



