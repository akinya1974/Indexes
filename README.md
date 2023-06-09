# «SQL. «Индексы» Акиньшин С.Г.

## Задание-1

### 1. `Напишите запрос к учебной базе данных, который вернёт процентное отношение общего размера всех индексов к общему размеру всех таблиц.`
```
SELECT  SUM(tt.DATA_LENGTH) , SUM(tt.INDEX_LENGTH), CONCAT(  ROUND ((SUM(tt.INDEX_LENGTH) / SUM(tt.DATA_LENGTH)) *100), ' %')  AS Отношение
FROM INFORMATION_SCHEMA.TABLES tt
WHERE  tt.TABLE_SCHEMA = 'sakila' ;
```


![Link](https://github.com/akinya1974/Indexes/blob/main/JPG/Задание-1.jpg)


## Задание-2

### 2. `Выполните explain analyze следующего запроса:`
```
select distinct concat(c.last_name, ' ', c.first_name), sum(p.amount) over (partition by c.customer_id, f.title)
from payment p, rental r, customer c, inventory i, film f
where date(p.payment_date) = '2005-07-30' and p.payment_date = r.rental_date and r.customer_id = c.customer_id and i.inventory_id = r.inventory_id
```

![Link](https://github.com/akinya1974/Indexes/blob/main/JPG/Задание-2.jpg)

Перечислите узкие места, оптимизируйте запрос: внесите корректировки по использованию операторов, при необходимости добавьте индексы.

Лишнее это операторы distinct, over (partition by c.customer_id, f.title), также исключить из выборки таблицу film!


```
explain analyze
select  concat(c.last_name, ' ', c.first_name) as name,
sum(p.amount)
from payment p, rental r, customer c, inventory i
where date(p.payment_date) = '2005-07-30' and p.payment_date = r.rental_date and 
r.customer_id = c.customer_id and i.inventory_id = r.inventory_id
group by name;
```

![Link](https://github.com/akinya1974/Indexes/blob/main/JPG/Переделка.jpg)


## Задание-3

### PostgreSQL поддерживает несколько типов индексов, которые могут использоваться для оптимизации запросов и ускорения поиска данных. Вот некоторые из основных типов индексов, которые доступны в PostgreSQL:

1. B-Tree (Balanced Tree): B-Tree индекс является наиболее распространенным типом индекса в PostgreSQL. Он поддерживает эффективный поиск значений, а также диапазонные запросы, сортировку и уникальность. B-Tree индексы могут использоваться для различных типов данных, включая числа, строки и даты.

2. Hash (Хэш-индекс): Hash индекс используется для быстрого поиска точных соответствий значений. Он хэширует значения и создает индекс на основе хэш-кода. Hash индексы особенно полезны для равенства поиска, но не поддерживают диапазонные запросы или сортировку.

3. GIN (Generalized Inverted Index): GIN индекс используется для индексации полнотекстового поиска и данных с переменной длиной, таких как массивы и JSON-объекты. Он предоставляет мощные возможности поиска и поиска совпадений, основанных на термах или фразах.

4. GiST (Generalized Search Tree): GiST индекс обеспечивает возможность создания пользовательских типов данных и индексации их эффективно. Он широко используется для индексации географических данных, полнотекстового поиска, временных интервалов и других сложных типов данных.

5. SP-GiST (Space-Partitioned Generalized Search Tree): SP-GiST индекс аналогичен GiST индексу, но оптимизирован для индексации данных с пространственными свойствами, такими как геометрические данные.

6. BRIN (Block Range INdex): BRIN индекс разбивает данные на блоки и сохраняет информацию о минимальном и максимальном значении в каждом блоке. Он эффективен для индексации больших таблиц с отсортированными данными, такими как временные ряды.

Это лишь некоторые из наиболее распространенных типов индексов в PostgreSQL. Каждый тип индекса имеет свои особенности и подходит для определенных типов данных и запросов. Выбор правильного типа индекса зависит от конкретной задачи и требований к производительности.

### PostgreSQL поддерживает различные типы индексов, некоторые из которых не поддерживаются в MySQL. Вот несколько типов индексов, которые доступны в PostgreSQL, но отсутствуют в MySQL:

1. GIN (Generalized Inverted Index): GIN индекс используется для полнотекстового поиска и для индексации массивов, JSON-объектов и других типов данных с переменной длиной.

2. GiST (Generalized Search Tree): GiST индекс обеспечивает возможность создания пользовательских типов данных и индексации их эффективно. Он может использоваться для различных типов поиска, таких как географические данные, полнотекстовый поиск, временные интервалы и другие.

3. SP-GiST (Space-Partitioned Generalized Search Tree): SP-GiST индекс похож на GiST индекс, но предназначен для индексации данных с пространственными свойствами, такими как геометрические данные.

4. BRIN (Block Range INdex): BRIN индекс разбивает данные на блоки и сохраняет информацию о минимальном и максимальном значении в каждом блоке. Он эффективен для индексации больших таблиц с отсортированными данными, такими как временные ряды.

5. Hash (Хэш-индекс): Hash индекс используется для поиска точных соответствий значений. В отличие от других индексов, он не может использоваться для поиска диапазонов значений.

6. B-Tree (Balanced Tree): B-Tree индекс является стандартным индексом, который поддерживается и в PostgreSQL, и в MySQL. Однако, PostgreSQL предоставляет более широкие возможности настройки и оптимизации B-Tree индексов.

Это не полный список всех типов индексов, поддерживаемых в PostgreSQL, но они являются некоторыми из наиболее популярных и уникальных типов индексов, отсутствующих в MySQL.
