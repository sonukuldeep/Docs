---
author: internet
datetime: 2023-04-08
title: Sql
slug: sql
featured: false
draft: false
tags:
  - backend
  - database
  - sql
ogImage: ""
description: Sql
---

## COMMAN COMMANDS

- CREATE DATABASE record_company;
- USE record_company;
- CREATE TABLE test(test_column INT);
- DROP TABLE test;
- CREATE TABLE bands(id INT NOT NULL AUTO_INCREMENT, name VARCHAR(255) NOT NULL,PRIMARY KEY(id));
- CREATE TABLE albums(id INT NOT NULL AUTO_INCREMENT, name VARCHAR(255) NOT NULL, release_year INT,band_id INT NOT NULL, PRIMARY KEY(id), FOREIGN KEY (band_id) REFERENCES bands(id));
- ALTER TABLE test ADD another_column VARCHAR(255);
- INSERT INTO bands(name) VALUES ('PINKU'),('GOGI'),('GOLI');
- SELECT \* FROM bands;
- SELECT id AS 'ID' FROM bands;
- SELECT id AS 'ID', name AS 'Band Name' FROM bands;
- SELECT \* FROM bands LIMIT 2;
- SELECT \* FROM bands ORDER BY name;
- UPDATE albums SET release_year = 1982 WHERE id = 1; # (WHERE is important here!)
- SELECT \* FROM albums WHERE name LIKE '%er%'; (LIKE works with INT too)
- SELECT \* FROM albums WHERE name LIKE '%er%' AND band_id = 1;
- SELECT \* FROM albums WHERE name LIKE '%er%' OR band_id = 1;
- SELECT \* FROM albums WHERE release_year BETWEEN 2000 AND 2018;
- SELECT \* FROM albums WHERE release_year IS NULL;
- DELETE FROM albums WHERE id = 5;

## CRUD ON TABLE HEADER

- CREATE TABLE table_name(id INT NOT NULL AUTO_INCREMENT, name VARCHAR(255) NOT NULL,PRIMARY KEY(id));
- SELECT table_row FROM table_name;
- ALTER TABLE table_name ADD another_column VARCHAR(255);
- ALTER TABLE table_name ALTER COLUMN column_name data_type; # (change column data type)
- ALTER TABLE table_name DROP COLUMN column_name;

## CRUD ON TABLE DATA

- INSERT INTO table_name(1st_column,2nd_column,3rd_column,...) VALUES ('ABC',1985,1,...),('DEF',1984,1,...),('GHI',2018,1,...);
- SELECT \* FROM table_name;
- UPDATE table_name SET column_name = 1982 WHERE id = 1; # (WHERE is very important here!)
- DELETE FROM table_name WHERE id = 5;

## UPDATE ALL DATA WITHIN A COLUMN

```jsx
UPDATE table_name
     SET column_name = CASE id
     WHEN id_1 THEN value1
     WHEN id_2 THEN value2
     WHEN id_3 THEN value3
     WHEN id_4 THEN value4
     END
     WHERE id BETWEEN 1 AND 4;
```

## JOIN

- SELECT \* FROM 1st_table JOIN 2nd_table ON 1st_table.id = 2nd_table.foreign_key; #(only matching tables are shown, JOIN and INNER JOIN are same)
- SELECT \* FROM 1st_table LEFT JOIN 2nd_table ON 1st_table.id = 2nd_table.foreign_key; #(all value from 1st_table are shown)
- SELECT \* FROM 1st_table RIGHT JOIN 2nd_table ON 1st_table.id = 2nd_table.foreign_key; #(all value from 2nd_table are shown)
