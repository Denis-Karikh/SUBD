```sql
CREATE INDEX IF NOT EXISTS idx_category_name
    ON shop_sc.product_category USING btree
    (name COLLATE pg_catalog."default" ASC NULLS LAST)
    TABLESPACE shop_data;
```
Для более быстрого поиска по категориям продукта.

```sql
CREATE INDEX IF NOT EXISTS idx_product_name
    ON shop_sc.product USING btree
    (product_name COLLATE pg_catalog."default" ASC NULLS LAST)
    TABLESPACE shop_data;
```
Для более быстрого поиска по имени продукта.

```sql
CREATE INDEX IF NOT EXISTS idx_price_product_id
    ON shop_sc.price USING btree
    (product_id ASC NULLS LAST)
    TABLESPACE shop_data;
```	
Для быстрой сортировки по цене.	

```sql
CREATE INDEX IF NOT EXISTS idx_manufacturer_name
    ON shop_sc.manufacturer USING btree
    (name COLLATE pg_catalog."default" ASC NULLS LAST)
    TABLESPACE shop_data;
```
Для поиска по производителю товара.

```sql
CREATE INDEX tsearch_index_title ON shop_sc.product USING GIN  (to_tsvector('russian', product_name));
```
Полнотекстовый индекс по имени продукта
```sql
create index idx_discount_part on shop_sc.product(discount) where discount > 10;
```
Индекс для поиска продуктов у которых скидка более 10%.
```sql
CREATE INDEX IF NOT EXISTS idx_orders_customers_product
    ON shop_sc.orders USING btree
    (customers_id ASC NULLS LAST, product_id ASC NULLS LAST)
    TABLESPACE shop_data;
```
Для более быстрой связки таблиц при запросах.

```sql
CREATE INDEX IF NOT EXISTS idx_product_category_product_name
    ON shop_sc.product USING btree
    (category_id ASC NULLS LAST, product_name COLLATE pg_catalog."default" ASC NULLS LAST)
    TABLESPACE shop_data;
```
Для более быстрой связки таблиц при запросах.
