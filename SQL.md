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
- DECIMAL - хранит числа с заданной точностью. Можно задать два параметра DECIMAL(5, 2), где 5 общее число символов, а 2 количество знаков после запятой. Например DECIMAL(5, 2): числа от -999.99 до 999.99.  
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
  
## Выборка данных  
  
#### Сортировка результатов запроса: оператор ORDER BY  
Синтаксис:  
```mysql  
SELECT поля_выборки  
FROM список_таблиц  
[WHERE условие]  
ORDER BY выражение [ ASC | DESC ];  
```  
- ASC - сортровка по возрастанию (по умолчанию)  
- DESC - сортировка по убыванию  
  
Пример сортировки с помощью ORDER BY:  
```mysql  
SELECT *  
FROM Products  
ORDER BY Price;  
```  
  
Пример использования псевдонима в запросе с ORDER BY:  
```mysql  
SELECT ProductName, ProductCount * Price AS TotalSum  
FROM Products  
ORDER BY TotalSum;  
```  
  
Пример использования сложного выражения в качестве критерия сортировки:  
```mysql  
SELECT ProductName, Price, ProductCount  
FROM Products  
ORDER BY ProductCount * Price;  
```  
  
#### Ограничение выборки в раличных СУБД  
  
Ключевое слово | Система баз данных  
----|----  
TOP | SQL Server, MS Access  
LIMIT | MySQL, PostgreSQL, SQLite  
FETCH FIRST | Oracle  
  
#### Ограничение выборки: LIMIT  
Синтаксис:  
```mysql  
SELECT поля_выборки  
FROM список_таблиц  
LIMIT [количество_пропущенных_записей,] количество_записей_для_вывода;  
```  
  
Пример использования LIMIT:  
- Выберем первые три строки:   
```mysql  
SELECT *  
FROM Products  
LIMIT 3;  
```  
- Выберем три строчки, начиная со 2 позиции:  
```mysql  
SELECT *  
FROM Products  
LIMIT 2, 3;  
```  
  
#### Аналоги: извлечение диапазона строк в MS SQL Server  
В примерах работаем с базой данных "Недвижимость" и её таблицей "Объект" (Object)  
  
Obj_ID | Type | District | Rooms  
----|----|----|----  
1 | flat | Центр | 2  
2 | flat | Центр | 2  
3 | House | Волжский | 4  
4 | flat | Центр | 2  
5 | House | Волжский | 5  
6 | flat | Пашино | 2  
7 | flat | Центр | 3  
8 | House | Сосновка | 3  
  
Пример вывода первых двух строк:  
```sql  
SELECT TOP 2 *  
FROM Object  
```  
При помощи применённого ограничения диапазона будет выведена следующая таблица:  
Obj_ID | Type | District | Rooms  
----|----|----|----  
1 | flat | Центр | 2  
2 | flat | Центр | 2  
  
#### Ограничение выборки: FETCH  
Синтаксис:  
```mysql  
SELECT поля_выборки  
FROM список_таблиц  
ORDER BY название_поля   
OFFSET количество_пропущенных_записей  
FETCH NEXT n ROWS ONLY;  
```  
  
Пример: будут исключены m строк и выбраны следующие p строк - будут выведены строки от (m + 1) до (m + 1 + p)  
```mysql  
SELECT поля_выборки  
FROM список_таблиц  
ORDER BY название_поля   
OFFSET m ROWS  
FETCH NEXT p ROWS ONLY;  
```  
  
#### Уникальные значения - DISTINCT: вывод уникальных производителей  
Пример:  
```mysql  
SELECT DISTINCT Manufacturer  
FROM Products;  
```  
  
#### Уникальные значения - DISTINCT: вывод уникальных производителей по нескольким столбцам  
```mysql  
SELECT DISTINCT Manufacturer, ProductCount  
FROM Products;  
```  
  
#### Группировка — GROUP BY  
Синтаксис:  
```mysql  
SELECT столбцы  
FROM таблица  
[WHERE условие_фильтрации_строк]  
[GROUP BY столбцы_для_группировки]  
[HAVING условие_фильтрации_групп]  
[ORDER BY столбцы_для_сортировки];  
```  
  
Пример:  
```mysql  
SELECT Manufacturer, COUNT(*) AS ModelsCount  
FROM Products  
GROUP BY Manufacturer;  
```  
  
## Агрегатные функции  
  
В MySQL есть следующие агрегатные функции:   
- AVG: вычисляет среднее значение  
- SUM: вычисляет сумму значений  
- MIN: вычисляет наименьшее значение  
- MAX: вычисляет наибольшее значение  
- COUNT: вычисляет количество строк в запросе  
  
#### Агрегатные функции: AVG  
Найдем среднюю цену товаров из базы данных:  
```mysql  
SELECT AVG(Price) AS AveragePrice  
FROM Products;  
```  
  
#### Агрегатные функции: AVG с использованием фильтрации  
На этапе выборки можно применять фильтрацию. Например, найдём среднюю цену для товаров определённого производителя:  
```mysql  
SELECT AVG(Price)  
FROM Products  
WHERE Manufacturer = 'Apple';  
```  
  
#### Агрегатные функции: COUNT  
Пример:  
```mysql  
SELECT COUNT(*)  
FROM Products;  
```  
  
#### Агрегатные функции: MIN и MAX  
Пример:  
```mysql  
SELECT MIN(Price), MAX(Price)  
FROM Products;  
```  
  
#### Использования HAVING  
Синтаксис:  
```mysql  
SELECT expression1, expression2, ... expression_n,  
		aggregate_function(выражение)  
FROM таблица  
[WHERE conditions]  
GROUP BY столбцы_для_группировки  
HAVING condition;  
```  
- aggregate_function - функция, такая как функции SUM, COUNT, MIN, MAX или AVG.  
- expression1, expression2, ... expression_n - выражения, которые не заключены в агрегированную функцию и должны быть включены в предложение GROUP BY.  
- WHERE conditions - необязательный. Это условия для выбора записей.  
- HAVING condition - Это дополнительное условие применяется только к агрегированным результатам для ограничения групп возвращаемых строк.  
  
Например, найдем все группы товаров по производителям, для которых определено более 1 модели:  
```mysql  
SELECT Manufacturer, COUNT(*) AS ModelsCount  
FROM Products  
GROUP BY Manufacturer  
HAVING COUNT(*) > 1;  
```  
  
Пример 2:  
```mysql  
SELECT Manufacturer, COUNT(*) AS ModelsCount  
FROM Products  
WHERE Price * ProductCount > 80000  
GROUP BY Manufacturer  
HAVING COUNT(*) > 1;  
```  
В данном случае сначала фильтруются строки: выбираются те товары, общая стоимость которых больше 80000. Затем выбранные товары группируются по производителям. И далее фильтруются сами группы - выбираются те группы, которые содержат больше 1 модели.  
  
Пример 3:  
```mysql  
SELECT Manufacturer, COUNT(*) AS Models, SUM(ProductCount) AS Units  
FROM Products  
WHERE Price * ProductCount > 80000  
GROUP BY Manufacturer  
HAVING SUM(ProductCount) > 2;  
ORDER BY Units DESC;  
```  
  
#### Приоритет операций  
1. FROM, включая JOINs  
2. WHERE  
3. GROUP BY  
4. HAVING  
5. Функции WINDOW  
6. SELECT  
7. DISTINCT  
8. UNION  
9. ORDER BY  
10. LIMIT и OFFSET  
  
