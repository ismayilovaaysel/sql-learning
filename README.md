CHECKPOINT 1
Task 1
select Products.ProductID,Products.ProductName,Products.UnitPrice
from Products

Task 2
select CompanyName,Customers.Country
from Customers
where country='Brazil'

Task 3
select *
from Employees
order by LastName

Task 4
select *
from Orders
limit 10

Task 5
select Products.ProductName,Products.UnitPrice
from Products
where UnitPrice>20
order by UnitPrice desc
limit 5

CHECKPOINT 2
Task 1
select Customers.CompanyName,Orders.OrderID,Orders.OrderDate 
from Customers 
inner join Orders 
on Customers.CustomerID = Orders.CustomerID

Task 2
select Customers.CompanyName,Orders.OrderID,Orders.OrderDate,Employees.EmployeeID,Employees.FirstName,Employees.LastName
from Customers
inner join Orders
on Customers.CustomerID = Orders.CustomerID
inner join Employees
on Orders.EmployeeID = Employees.EmployeeID

Task 3
select Customers.CompanyName,Orders.OrderID, Products.ProductName,"Order Details".Quantity,Products.UnitPrice
from Customers
inner join Orders
on Customers.CustomerID = Orders.CustomerID
inner join "Order Details"
on Orders.OrderID = "Order Details".OrderID
inner join Products
on "Order Details".ProductID = Products.ProductID

Task 4
select Customers.CompanyName,Orders.OrderID
from Customers
left join Orders
on Customers.CustomerID = Orders.CustomerID

Task 5
select Orders.OrderID, Products.ProductName, Products.UnitPrice, "Order Details".Quantity
from Orders
inner join "Order Details"
on Orders.OrderID = "Order Details".OrderID
inner join Products
on "Order Details".ProductID = Products.ProductID
where Products.UnitPrice> 50

CHECKPOINT 3
Task 1
SELECT
    Country,
 COUNT(CustomerID) AS CustomerCount
FROM Customers
GROUP BY Country
HAVING COUNT(CustomerID) > 5;

Task 2
SELECT
    CategoryID,
    AVG(UnitPrice) AS AveragePrice
FROM Products
GROUP BY CategoryID
HAVING AVG(UnitPrice) > 30;

CHECKPOINT 4
task 1
SELECT
    ProductName,
    UnitPrice
FROM Products
WHERE UnitPrice >
(
    SELECT AVG(UnitPrice)
    FROM Products);

Task 2
WITH CustomerSales AS
(
    SELECT
        CustomerID,
        SUM(Freight) AS TotalSales
    FROM Orders
    GROUP BY CustomerID
)

SELECT
    CustomerID,
    TotalSales
FROM CustomerSales
WHERE TotalSales > 5000;

CHECKPOINT 5
Task 1
SELECT
    CustomerID,
    COUNT(OrderID) AS OrderCount,
    RANK() OVER(
        ORDER BY COUNT(OrderID) DESC
    ) AS CustomerRank
FROM Orders
GROUP BY CustomerID;

Task 2
SELECT
    OrderID,
    OrderDate,
    Freight,
    SUM(Freight) OVER(
        ORDER BY OrderDate
    ) AS RunningTotal
FROM Orders;

Task 3
SELECT
    CustomerID,
    CompanyName,
    ROW_NUMBER() OVER(
        ORDER BY CompanyName
    ) AS RowNumber
FROM Customers;

CHECKPOINT 6
Task 1
CREATE INDEX idx_customers_country
ON Customers(Country);
SELECT
    CustomerID,
    CompanyName,
    Country
FROM Customers
WHERE Country = 'Germany';

Task 2
SELECT
    o1.OrderID,
    o1.CustomerID,
    o1.Freight
FROM Orders o1
WHERE Freight >
(
    SELECT AVG(o2.Freight)
    FROM Orders o2
    WHERE o2.CustomerID = o1.CustomerID
);

optimallaşdırılmış join.
SELECT
    o.OrderID,
    o.CustomerID,
    o.Freight
FROM Orders o
JOIN
(
    SELECT
        CustomerID,
        AVG(Freight) AS AvgFreight
    FROM Orders
    GROUP BY CustomerID
) avg_orders
ON o.CustomerID = avg_orders.CustomerID

WHERE o.Freight > avg_orders.AvgFreight;
