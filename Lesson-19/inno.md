# 1. Задача: реализовать модель данных БД, определить сущности, построить связи, выполнить декомпозицию и нормализацию
### До:
```sql
create table if not exists customers
(
    title                   varchar(255),
    first_name              varchar(255),
    last_name               varchar(255),
    correspondence_language varchar(255),
    birth_date              varchar(255),
    gender                  varchar(255),
    marital_status          varchar(255),
    country                 varchar(255),
    postal_code             varchar(255),
    region                  varchar(255),
    city                    varchar(255),
    street                  varchar(255),
    building_number         varchar(255)
);
```

### После:

```sql
create table if not exists country
(
    id   int unsigned not null auto_increment primary key,
    name varchar(128) not null
);
create unique index countries_unique_name_idx on country (name); -- Страны уникальны

create table if not exists region
(
    id         int unsigned not null auto_increment primary key,
    country_id int unsigned not null,
    name       varchar(255) not null,
    foreign key (country_id) references country (id)
);
create unique index region_country_unique_name_idx on region (country_id, name); -- Регионы одной страны уникальны

create table if not exists city
(
    id        int unsigned not null auto_increment primary key,
    region_id int unsigned not null,
    name      varchar(255) not null,
    foreign key (region_id) references region (id)
);

create table if not exists street
(
    id      int unsigned not null auto_increment primary key,
    city_id int unsigned not null,
    name    varchar(255) not null,
    foreign key (city_id) references city (id)
);

create table if not exists address
(
    id              int unsigned not null auto_increment primary key,
    street_id       int unsigned not null,
    building_number varchar(255),
    postal_code     varchar(50),
    foreign key (street_id) references street (id)
);

create table if not exists customer
(
    id                      int unsigned not null auto_increment primary key,
    address_id              int unsigned,
    title                   varchar(50),
    first_name              varchar(50),
    last_name               varchar(50),
    correspondence_language varchar(255),
    birth_date              date,
    gender                  enum ('Male','Female', 'Unknown'),
    marital_status          varchar(255),
    foreign key (address_id) references address (id)
);
```
