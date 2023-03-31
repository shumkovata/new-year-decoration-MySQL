Домашнее задание
Индексы


Описание/Пошаговая инструкция выполнения домашнего задания:
Пересматриваем индексы на своем проекте. По необходимости меняем.
Задача - сделать полнотекстовый индекс, который ищет по свойствам, названию товара и описанию. Если нет аналогичной задачи в проекте - имитируем.
Итог: анализируем свой проект - добавляем или обновляем индексы
в README пропишите какие индексы были изменены или добавлены
explain и результаты выборки без индекса и с индексом.
Реализация полнотекстового индекса.


-------Индекс для поиска по наименованию в таблице "Ели".


Создание индекса (поиск по наименованию)


explain select * from spruce where name_spruce = 'Pine1';


Результат без индекса:


+----+-------------+--------+------------+------+---------------+------+---------+------+------+----------+-------------+
| id | select_type | table  | partitions | type | possible_keys | key  | key_len | ref  | rows | filtered | Extra       |
+----+-------------+--------+------------+------+---------------+------+---------+------+------+----------+-------------+
|  1 | SIMPLE      | spruce | NULL       | ALL  | NULL          | NULL | NULL    | NULL |   69 |    10.00 | Using where |
+----+-------------+--------+------------+------+---------------+------+---------+------+------+----------+-------------+
1 row in set, 1 warning (0.00 sec)


create index ind_spruce_name on spruce(name_spruce);

ANALYZE TABLE spruce;


explain select * from spruce where name_spruce = 'Pine1';


Результат с индексом:


+----+-------------+--------+------------+------+-----------------+-----------------+---------+-------+------+----------+-------+
| id | select_type | table  | partitions | type | possible_keys   | key             | key_len | ref   | rows | filtered | Extra |
+----+-------------+--------+------------+------+-----------------+-----------------+---------+-------+------+----------+-------+
|  1 | SIMPLE      | spruce | NULL       | ref  | ind_spruce_name | ind_spruce_name | 402     | const |    1 |   100.00 | NULL  |
+----+-------------+--------+------------+------+-----------------+-----------------+---------+-------+------+----------+-------+
1 row in set, 1 warning (0.00 sec)



--------Индекс по поиску наименования и цены.


Результат без индекса


explain select * from spruce where name_spruce = 'spruce' and price = 3578;


+----+-------------+--------+------------+------+-----------------+-----------------+---------+-------+------+----------+-------------+
| id | select_type | table  | partitions | type | possible_keys   | key             | key_len | ref   | rows | filtered | Extra       |
+----+-------------+--------+------------+------+-----------------+-----------------+---------+-------+------+----------+-------------+
|  1 | SIMPLE      | spruce | NULL       | ref  | ind_spruce_name | ind_spruce_name | 402     | const |   32 |    10.00 | Using where |
+----+-------------+--------+------------+------+-----------------+-----------------+---------+-------+------+----------+-------------+
1 row in set, 1 warning (0.01 sec)


create index ind_spruce_name_price on spruce(name_spruce, price);

ANALYZE TABLE spruce;


explain select * from spruce where name_spruce = 'spruce' and price = 3578;


Результат с индексом:


+----+-------------+--------+------------+------+---------------------------------------+-----------------------+---------+-------------+------+----------+-------+
| id | select_type | table  | partitions | type | possible_keys                         | key                   | key_len | ref         | rows | filtered | Extra |
+----+-------------+--------+------------+------+---------------------------------------+-----------------------+---------+-------------+------+----------+-------+
|  1 | SIMPLE      | spruce | NULL       | ref  | ind_spruce_name,ind_spruce_name_price | ind_spruce_name_price | 408     | const,const |    1 |   100.00 | NULL  |
+----+-------------+--------+------------+------+---------------------------------------+-----------------------+---------+-------------+------+----------+-------+
1 row in set, 1 warning (0.01 sec)



--------- Полнотекстовый поиск выполняется с помощью функции MATCH()


EXPLAIN SELECT * FROM manufactures WHERE name_manufactures LIKE 'IP%';


Результат без индекса:


+----+-------------+--------------+------------+------+---------------+------+---------+------+------+----------+-------------+
| id | select_type | table        | partitions | type | possible_keys | key  | key_len | ref  | rows | filtered | Extra       |
+----+-------------+--------------+------------+------+---------------+------+---------+------+------+----------+-------------+
|  1 | SIMPLE      | manufactures | NULL       | ALL  | NULL          | NULL | NULL    | NULL |    8 |    12.50 | Using where |
+----+-------------+--------------+------------+------+---------------+------+---------+------+------+----------+-------------+
1 row in set, 1 warning (0.00 sec)


CREATE FULLTEXT INDEX full_name_address on manufactures(name_manufactures, address);

ANALYZE TABLE manufactures;


EXPLAIN SELECT * FROM manufactures WHERE MATCH (name_manufactures, address)
AGAINST ('+IP +Moscow');


Результат с индексом:


+----+-------------+--------------+------------+----------+-------------------+-------------------+---------+-------+------+----------+-------------------------------+
| id | select_type | table        | partitions | type     | possible_keys     | key               | key_len | ref   | rows | filtered | Extra
          |
+----+-------------+--------------+------------+----------+-------------------+-------------------+---------+-------+------+----------+-------------------------------+
|  1 | SIMPLE      | manufactures | NULL       | fulltext | full_name_address | full_name_address | 0       | const |    1 |   100.00 | Using where; Ft_hints: sorted |
+----+-------------+--------------+------------+----------+-------------------+-------------------+---------+-------+------+----------+-------------------------------+
1 row in set, 1 warning (0.00 sec)


SELECT * FROM manufactures WHERE MATCH (name_manufactures, address)
AGAINST ('+IP +Moscow');


+-----------------+-------------------+---------+------------+
| id_manufactures | name_manufactures | address | id_country |
+-----------------+-------------------+---------+------------+
|               1 | IP Sedov S.S.     | Moscow  |          3 |
|               2 | OOO SimaGlobal    | Moscow  |          3 |
|               6 | IP Toporpov A.A.  | Moscow  |          2 |
|               8 | IP Palgov E.A.    | Moscow  |          2 |
+-----------------+-------------------+---------+------------+
4 rows in set (0.00 sec)

