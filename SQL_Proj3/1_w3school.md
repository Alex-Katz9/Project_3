# Part 1: W3Schools SQL Lab

*Introductory level SQL*

--

This challenge uses the [W3Schools SQL playground](http://www.w3schools.com/sql/trysql.asp?filename=trysql_select_all). Please add solutions to this markdown file and submit.

What is the name of the customer who has the most orders?

SELECT Orders.OrderID, Customers.CustomerName, COUNT(* )
FROM Orders LEFT JOIN Customers
ON Orders.CustomerID = Customers.CustomerID
GROUP BY Customers.CustomerName
ORDER BY COUNT(* ) DESC;

Ernst Handel with 10 orders has the most

Which supplier has the highest average product price?

SELECT Suppliers.* , Products.* , AVG(Products.Price) AS Average_Price FROM
Suppliers LEFT JOIN Products
ON Suppliers.SupplierID = Products.SupplierID
GROUP BY Suppliers.SupplierID
ORDER BY Average_Price DESC;

Supplier 18: Aux joyeux ecclésiastiques has the highest average product price at 140.75

How many different countries are all the customers from? (Hint: consider DISTINCT.)

SELECT COUNT(DISTINCT(Country)) FROM Customers;

21 countries

What category appears in the most orders?

SELECT Products.CategoryID, Count(OrderDetails.OrderID) FROM OrderDetails
LEFT JOIN Products
ON OrderDetails.ProductID = Products.ProductID
LEFT JOIN Categories
ON Products.CategoryID = Categories.CategoryID
GROUP BY Products.CategoryID;

Category 4, Dairy Products, with 100

What was the total cost for each order?

SELECT Orders.OrderID,
SUM(OrderDetails.Quantity * Products.Price) AS Order_Total
FROM OrderDetails
LEFT JOIN Orders
ON OrderDetails.OrderID = Orders.OrderID
LEFT JOIN Products
ON OrderDetails.ProductID = Products.ProductID
GROUP BY Orders.OrderID;

Which employee made the most sales (by total price)?

SELECT Orders.OrderID, Employees.EmployeeID, Employees.FirstName, Employees.LastName,
SUM(OrderDetails.Quantity * Products.Price) AS Order_Total
FROM OrderDetails
LEFT JOIN Orders
ON OrderDetails.OrderID = Orders.OrderID
LEFT JOIN Products
ON OrderDetails.ProductID = Products.ProductID
LEFT JOIN Employees
ON Orders.EmployeeID = Employees.EmployeeID
GROUP BY Employees.EmployeeID
ORDER BY Order_Total DESC;

Margaret Peacock had the most total sales by overall dollars sold.

Which employees have BS degrees? (Hint: look at the LIKE operator.)

SELECT * FROM Employees
WHERE Notes LIKE '%BS%';

Two employees do, Janet Leverling (BS) and Steven Buchanan (BSc).

Which supplier of three or more products has the highest average product price? (Hint: look at the HAVING operator.)

SELECT Suppliers.* , COUNT(DISTINCT(Products.ProductID)) AS 'Number of Products', AVG(Products.Price) AS 'Average Price'
FROM Products LEFT JOIN Suppliers
ON Products.SupplierID = Suppliers.SupplierID
GROUP BY Suppliers.SupplierID
HAVING COUNT(DISTINCT(Products.ProductID))>2;

Tokyo Traders at 46.