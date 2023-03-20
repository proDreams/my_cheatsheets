---  
share: true  
---  
## Создание таблиц  
  
### Запрос для создания базы данных:  
```mysql  
CREATE DATABASE <database_name>;  
```  
### Отобразить список баз данных:  
```mysql  
SHOW DATABASES;  
```  
### Подключиться к базе данных:  
```mysql  
USE <database_name>;  
```  
  
### Создание таблицы.  
Синтаксис:  
```mysql  
CREATE TABLE <table_name>  
(  
	column_name_1 type_and_parameters  
	...  
	column_name_n type_and_parameters  
);  
```  
Пример:  
```mysql  
CREATE TABLE students  
(  
	id INT NOT NULL AUTO_INCREMENT PRIMARY KEY,  
	first_name VARCHAR(30) NOT NULL  
);  
```  
  
### Основные типы данных  
#### Числовые  
- INT - целочисленные значения.  
- DECIMAL - хранит числа с заданной точностью. Например DECIMAL(5, 2): числа от -999.99 до 999.99.  
- BOOL - 0 или 1, True или False.  
#### Символьные  
- VARCHAR(N) - набор символов, где N максимальная длинна строки.  
- TEXT - тип данных для большого объёма текста, макс размер 65KB.  
#### Дата и время  
- DATE - Только дата, Год-Месяц-День  
- TIME - Только время, Часы-Минуты-Секунды - "hh:mm:ss". Память хранения - 3 байта.  
- DATETIME - Объединяет два предыдущих пункта. Занимает 8 байт в памяти.  
- TIMESTAMP - Хранит дату и время начиная с 1970г в секундах.  
#### Бинарные  
- BLOB - До 65кб бинарных данных  
- LARGEBLOB - До 4GB.  
  
### Первичный ключ - PRIMARY KEY (pk)  
Синтаксис по документации:  
```mysql  
CREATE TABLE <table_name>  
(  
	column_name_1 type_and_parameters  
	...  
	column_name_n type_and_parameters  
	CONSTRAINT [constraint_name]  
	PRIMARY KEY [ USING BTREE | HASH ] (column_name, ...)  
);  
```  
Синтаксис чаще всего встречающийся в запросах:  
```mysql  
CREATE TABLE <table_name>  
(  
	column_name_1 INT NOT NULL AUTO_INCREMENT PRIMARY KEY,   
	...  
);  
```  
- INT - Первичный ключ чаще всего это некая переменная id, но может иметь и другой тип данных  
- NOT NULL - Обозначает, что значение не может быть пустым, очень важно для первичного ключа  
- AUTO_INCREMENT - параметр позваляющий при добавлении значений в таблицу, автоматически увеличивать значение ключа на единицу. Так же это избавляет от необходимости явно указывать значение ключа при внесении данных.  
- PRIMARY KEY - Само обозначение, что поле является ключом.  
  
### Внешний ключ - FOREIGN KEY (fk)  
Синтаксис по документации:  
```mysql  
CREATE TABLE <table_name>  
(  
	column_name_1 type_and_parameters  
	...  
	CONSTRAINT [constraint_name]  
	FOREIGN KEY (column_name, ...)  
	REFERENCES parrent_table (parent_table_column)  
	[ON DELETE something]  
	[ON UPDATE something]  
);  
```  
  
### Пример таблицы с первичными и внешними ключами  
```mysql  
CREATE TABLE Customers  
(  
	id INT PRIMARY KEY AUTO_INCREMENT,  
	Age INT,  
	FirstName VARCHAR(20) NOT NULL,  
	Phone VARCHAR(20) NOT NULL UNIQUE  
);  
  
CREATE TABLE Orders  
(  
	id INT PRIMARY KEY AUTO_INCREMENT,  
	CustomerId INT,  
	CreatedAt DATE,  
	FOREIGN KEY (CustomerId) REFERENCES Customers (id)  
);  
```  
  
### Комментарии  
```mysql  
-- Однострочный комментарий  
# это тоже комментарий  
/*  
Многострочный  
комментарий  
*/  
```  
  
## Запросы  
### Получить все данные из таблицы  
Синтаксис:  
```mysql  
SELECT * FROM <table_name>;  
SELECT <columns> FROM <table_name>;  
```  
Пример:  
```mysql  
SELECT * FROM Customers;  
SELECT id, FirstName FROM Customers;  
```  
- SELECT - обозначает начало запроса на выбор данных  
- * - обозначает, что будут выбраны все данные  
- id и FirstName - обозначает, что будут выданы данные только по этим двум столбцам  
- FROM - указывает из какой таблицы брать данные  
  
### Арифметические операции  
Сложение, вычитание, умножение и деление:  
```mysql  
SELECT 3+5;  
SELECT 3-5;  
SELECT 3*5;  
-- Нужно быть внимательным, что бы размер не вышел за границу 64бит  
SELECT 3/5;  
SELECT 102/0;  
-- При делении на ноль, будет возвращено NULL  
```  
  
### Логические операторы  
- AND - Логическое И. Синтаксис: some1 AND some2  
- OR - Логическое ИЛИ. Синтаксис: some1 OR some2  
- NOT - Логическое НЕ. Синтаксис: NOT some1  
  
Таблица для примера логических операторов:  
```mysql  
CREATE TABLE Products  
(  
	id INT PRIMARY KEY AUTO_INCREMENT,  
	ProductName VARCHAR(30) NOT NULL,  
	Manufacturer VARCHAR(20) NOT NULL,  
	ProductCount INT DEFAULT 0,  
	Price DECIMAL  
);  
```  
- DEFAULT - определяет значение по умолчанию, в случае если будет добавляться новая запись не содержащая данное поле.  
  
Запросы:  
```mysql  
USE productsdb;  
  
SELECT * FROM Products;  
-- Запрос всех значений  
  
SELECT * FROM Products  
WHERE Manufacturer = 'Samsung' AND Price > 50000;  
-- Запрос всех значений у которых производитель указан самсунг И цена больше 50000  
  
SELECT * FROM Products  
WHERE Manufacturer = 'Samsung' OR Price > 50000;  
-- Запрос всех значений у которых производитель указан самсунг ИЛИ цена больше 50000  
  
SELECT * FROM Products  
WHERE NOT Manufacturer = 'Samsung';  
-- Запрос всех значений, у которых производитель НЕ самсунг  
```  
- WHERE - параметр определяющий выборку из таблицы. В нём передаём необходимые для фильтрации значения.  
  
### Приоритет операций  
Пример 1:  
```mysql  
SELECT * FROM Products  
WHERE Manufacturer = 'Samsung' OR NOT Price > 30000 AND ProductCount > 2;  
```  
Порядок выполнения:  
1. `NOT Price > 30000`  
2. `AND ProductCount > 2`  
3. `Manufacturer = 'Samsung' OR`  
  
Пример 2:  
```mysql  
SELECT * FROM Products  
WHERE Manufacturer = 'Samsung' OR NOT (Price > 30000 AND ProductCount > 2);  
```  
Порядок выполнения:  
1. `(Price > 30000 AND ProductCount > 2)`  
2. `NOT`  
3. `Manufacturer = 'Samsung' OR`  
  
### Оператор CASE  
Синтаксис:  
```mysql  
CASE  
	WHEN Условие1 THEN Действие1  
	WHEN Условие2 THEN Действие2  
	...  
	[ELSE Действие3]  
END  
```  
- WHEN - условие  
- THEN - действие  
  
Пример:  
```mysql  
SELECT ProductName, ProductCount,  
CASE  
	WHEN ProductCount = 1  
		THEN 'Товар заканчивается'  
	WHEN ProductCount = 2  
		THEN 'Мало товара'  
	WHEN ProductCount = 3  
		THEN 'Есть в наличии'  
	ELSE 'Много товара'  
END AS Category  
FROM Products;  
```  
- AS <column_name> - добавляет в вывод новый столбец.  
  
### Оператор IF  
Синтаксис:  
```mysql  
IF(Условие, Значение1, Значение2)  
-- Если условие, передаваемое в качестве первого параметра, верно, то возвращается первое значение, иначе возвращается второе.  
```  
  
Пример:  
```mysql  
SELECT ProductName, Manufacturer,  
	IF(ProductCount>3, 'Много товара', 'Мало товара')  
FROM Products;  
```  
  
### Запросы изменения данных  
- INSERT - вставка новых данных в таблицу  
- UPDATE - обновление данных из таблицы  
- DELETE - удаление данных  
  
#### Запросы изменения данных: INSERT  
Данный оператор имеет 2 основные формы:  
1. Вставка в таблицу новой строки, значения полей которой формируются из перечисленных значений:  
```mysql  
INSERT INTO таблица(перечень_полей)  
VALUES(перечень_значений)  
```  
2. Вставка в таблицу новых строк, значения которых формируются из значений строк возвращённых запросом:  
```mysql  
INSERT INTO таблица(перечень_полей)  
SELECT(перечень_значений)  
FROM ...  
```  
Пример:  
```mysql  
CREATE TABLE Products  
(  
	Id INT AUTO_INCREMENT PRIMARY KEY,  
	ProductName VARCHAR(30) NOT NULL,  
	Manufacturer VARCHAR(20) NOT NULL,  
	ProductCount INT DEFAULT 0,  
	Price DECIMAL  
);  
  
INSERT INTO Products (ProductName, Manufacturer, ProductCount, Price)  
VALUES  
('iPhone X', 'Apple', 3, 76000),  
('iPhone 8', 'Apple', 2, 51000),  
('Galaxy S9', 'Samsung', 2, 56000),  
('Galaxy S8', 'Samsung', 1, 41000),  
('P20 Pro', 'Huawei', 5, 36000);  
```  
  
#### Запросы изменения данных: UPDATE  
Синтаксис:  
```mysql  
UPDATE имя_таблицы  
SET столбец1 = значение1, столбец2 = значение2, ...  
[WHERE условие_обновления]  
```  
Пример увеличения цены всех товаров на 3000:  
```mysql  
UPDATE Products  
SET Price = Price + 3000;  
```  
#### Запросы изменения данных: DELETE  
Синтаксис:  
```mysql  
DELETE FROM имя_таблицы  
[WHERE условие_удаления]  
```  
Пример удаления строк, у которых производитель - Huawei:  
```mysql  
DELETE FROM Products  
WHERE Manufacturer = 'Huawei';  
```  
#### Запросы изменения данных: IN  
Синтаксис:  
```mysql  
WHERE выражение [NOT] IN (выражение)  
```  
Пример:  
```mysql  
SELECT *   
FROM Products  
WHERE NOT Manufacturer IN ('Apple', 'Samsung')  
```  
  
  
  
  
  
  
  
  
