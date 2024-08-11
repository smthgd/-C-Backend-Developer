# Написание запроса к базе данных
1. Написать SQL-запрос, который возвращает объем продаж в количественном выражении в разрезе сотрудников за период с 01.10.2013 по 07.10.2013:
● Фамилия и имя сотрудника;
● Объем продаж сотрудника.
Данные отсортировать по фамилии и имени сотрудника.

SELECT 
    Sellers.Surname, 
    Sellers.Name,
    SUM(Sales.Quantity) AS "Объем продаж"
FROM 
    Sellers
JOIN 
    Sales ON Sellers.ID = Sales.IDSel
WHERE 
    Sales.Date BETWEEN '20131001' AND '20131007'
GROUP BY 
    Sellers.Surname, Sellers.Name
ORDER BY 
    Sellers.Surname, Sellers.Name;
