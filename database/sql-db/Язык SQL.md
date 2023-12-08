- декларативный язык программирования, применяемый для создания, модификации и управления данными в реляционной базе данных, управляемой соответствующей системой управления базами данных.
- IBM 1970-e - SEQUEL
	- целью разработки было создание простого непроцедурного языка, которым мог воспользоваться любой пользователь, даже не имеющий навыков программирования

### create
```sql
-- Простое создание базы данных
CREATE DATABASE test;

-- Создание базы данных с указанием кодировки по-умолчанию
CREATE DATABASE test DEFAULT CHARACTER SET utf8;

-- Создать базу данных, если таковая не существует
-- Если существует - не пытаться создавать
CREATE DATABASE IF NOT EXISTS test;
```
### Delete/rename
```sql

-- Удаление базы данных

DROP DATABASE test;
-- Удаление БД. Если БД не существует - не выдаст ошибку.

DROP DATABASE IF EXISTS test;
-- Переименование БД
RENAME DATABASE test TO production;
```

### Create a table
```sql
-- Создание таблицы
CREATE TABLE `department` (
 `id` int(11),
 `name` varchar(255)
);
-- Создание таблицы (более реальный пример)
CREATE TABLE `department` (
 `id` int(11) unsigned NOT NULL AUTO_INCREMENT,
 `name` varchar(255) DEFAULT NULL,
 PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;
```

Delete a table 
```sql
-- Удаление таблицы
DROP TABLE `department`;
-- Удаление таблицы, если таковая существует
-- Если таблицы не существует - не будет выброшена ошибка
DROP TABLE IF EXISTS `department`;
-- Посмотреть список таблиц
SHOW TABLES;
```