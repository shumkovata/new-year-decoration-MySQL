      Домашнее задание   SQL выборка

Цель:
Научиться джойнить таблицы и использовать условия в SQL выборке


1. Напишите запрос по своей базе с inner join


SELECT name_manufactures, address, manufactures.id_country, name_country 
FROM manufactures
INNER JOIN country_manufactures ON country_manufactures.id_country = manufactures.id_country
ORDER BY name_country;


![запрос с inner join](запрос%20с%20inner%20join.png)



2. Напишите запрос по своей базе с left join


SELECT manufactures.id_manufactures, name_garland, name_manufactures, address 
FROM manufactures
LEFT JOIN garland ON manufactures.id_manufactures = garland.id_manufactures
ORDER BY manufactures.id_manufactures;


![запрос с left join](запрос%20с%20left%20join.png)



3. Напишите 5 запросов с WHERE с использованием разных операторов, опишите для чего вам в проекте нужна такая выборка данных


----Выбрать производителей, у которых страна производства Китай


SELECT name_manufactures, manufactures.id_country, name_country 
FROM manufactures
INNER JOIN country_manufactures ON country_manufactures.id_country = manufactures.id_country
WHERE name_country = 'China';


![страна производства Китай](страна%20производства%20Китай.png)



----Выбрать производителей, которые находятся в Москве


SELECT * FROM manufactures
WHERE address = 'Moscow';


![Адрес производства Москва](Адрес%20производства%20Москва.png)



----Посмотреть гирлянды у производителя с id_country = 2


SELECT manufactures.id_manufactures, name_manufactures, id_country, name_garland
FROM manufactures
LEFT JOIN garland ON manufactures.id_manufactures = garland.id_manufactures
WHERE id_country = 2
ORDER BY manufactures.id_manufactures;


![гирлянды у производителя с id_country = 2](гирлянды%20у%20производителя%20с%20id_country%20%3D%202.png)



------Посмотреть гирлянды по цене до 2000 рублей


SELECT * FROM garland
WHERE price < 2000;


![гирлянды до 2000](гирлянды%20до%202000.png)



-----Выбрать гирлянды производителя из Екатеринбурга и Оренбурга


SELECT manufactures.id_manufactures, name_manufactures, address, name_garland, price
FROM manufactures
LEFT JOIN garland ON manufactures.id_manufactures = garland.id_manufactures
WHERE address = 'Ekaterinburg' OR address = 'Orenburg'
ORDER BY manufactures.id_manufactures;


![гирлянды из Екатеринбурга и Оренбурга](гирлянды%20из%20Екатеринбурга%20и%20Оренбурга.png)


