# Написание запроса к базе данных
1. Написать SQL-запрос, который возвращает объем продаж в количественном выражении в разрезе сотрудников за период с 01.10.2013 по 07.10.2013:  
● Фамилия и имя сотрудника;  
● Объем продаж сотрудника.  
Данные отсортировать по фамилии и имени сотрудника.  

SELECT  
&emsp; CONCAT(Sellers.Surname, ' ', Sellers.Name) AS "Фамилия и имя сотрудника",  
&emsp; SUM(Sales.Quantity) AS "Объем продаж"  
FROM  
&emsp; Sellers  
JOIN  
&emsp; Sales ON Sellers.ID = Sales.IDSel  
WHERE  
&emsp; Sales.Date BETWEEN '20131001' AND '20131007'  
GROUP BY  
&emsp; Sellers.Surname, Sellers.Name  
ORDER BY  
&emsp; "Фамилия и имя сотрудника";  
  
2. На основании созданного в первом задании запроса написать SQL-запрос, который возвращает процент объема продаж в разрезе сотрудников и продукции за период с 01.10.2013 по 07.10.2013:  
● Наименование продукции;  
● Фамилия и имя сотрудника;  
● Процент продаж сотрудником данного вида продукции (продажи сотрудника данной продукции/общее число продаж данной продукции).  
В выборку должна попадать продукция, поступившая за период с 07.09.2013 по 07.10.2013.  
Данные отсортировать по наименованию продукции, фамилии и имени сотрудника.  
  
SELECT  
&emsp; MainProducts.Name AS "Наименование продукции",  
&emsp; CONCAT(Sellers.Surname, ' ', Sellers.Name) AS "Фамилия и имя сотрудника",  
&emsp; (SUM(MainSales.Quantity) * 100.0 / (  
&emsp;&emsp; SELECT  
&emsp;&emsp;&emsp; SUM(TempSales.Quantity)  
&emsp;&emsp; FROM  
&emsp;&emsp;&emsp; Sales TempSales  
&emsp;&emsp; JOIN  
&emsp;&emsp;&emsp; Products TempProducts ON TempSales.IDProd = TempProducts.ID  
&emsp;&emsp; WHERE  
&emsp;&emsp;&emsp; TempProducts.Name = MainProducts.Name  
&emsp;&emsp;&emsp; AND TempSales.Date BETWEEN '20131001' AND '20131007'  
&emsp; )) AS "Процент продаж"  
FROM  
&emsp; Sellers  
JOIN  
&emsp; Sales MainSales ON Sellers.ID = MainSales.IDSel 
JOIN  
&emsp; Products MainProducts ON MainSales.IDProd = MainProducts.ID  
JOIN  
&emsp; Arrivals ON MainProducts.ID = Arrivals.IDProd  
WHERE  
&emsp; MainSales.Date BETWEEN '20131001' AND '20131007'  
&emsp; AND Arrivals.Date BETWEEN '20130907' AND '20131007'  
GROUP BY  
&emsp; MainProducts.Name, Sellers.Surname, Sellers.Name  
ORDER BY  
&emsp; "Наименование продукции", "Фамилия и имя сотрудника";  
