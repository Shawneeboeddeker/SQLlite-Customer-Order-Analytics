# SQLlite-Customer-Order-Analytics

/*In this SQL, I'm querying a database with multiple tables in it to quantify statistics about customer an order data.*/

/*#1 How many orders were placed in January?*/

SELECT COUNT(orderid)
FROM BIT_DB.JanSales
WHERE length(orderid) = 6 
AND orderid <> 'Order ID';


/*#2 How many of those orders wee for an iPhone?*/

SELECT *
  FROM BIT_DB.JanSales;
       
SELECT COUNT(orderid)
FROM BIT_DB.JanSales
WHERE Product='iPhone'
AND length(orderid) = 6 
AND orderid <> 'Order ID';

/*#3 Select the customer account numbers for all the orders that were placed in February.*/

SELECT * 
FROM BIT_DB.FebSales;

SELECT distinct acctnum 
FROM BIT_DB.customers cust
INNER JOIN BIT_DB.FebSales Feb
ON cust.order_id = Feb.orderid
WHERE length (orderid) = 6
AND orderid <>'Order ID';


/*#4 WHich prodect was the cheapest one sold in January, and what was the price?*/

SELECT price, Product
FROM BIT_DB.JanSales
WHERE price <> 'Price Each'
AND price <> ' '
AND Product <> ' '
ORDER BY price asc;

SELECT distinct Product, price
FROM BIT_DB.JanSales
WHERE  price in (SELECT min(price) FROM BIT_DB.JanSales);

/*#5 What is the total revenue for each produect sold in January?*/

SELECT * FROM JanSales;

SELECT sum(Quantity)*price as revenue, product
FROM BIT_DB.JanSales
GROUP BY product;

/*#6 WHich prodects were sold in February at 548 Lincoln St. Seattle, WA 98101, how many of each were seld, and what was the total revenue?*/

SELECT * FROM FebSales;

SELECT SUM(Quantity), SUM(quantity)*price as revenue, Product
FROM BIT_DB.FebSales
WHERE location = '548 Lincoln St, Seattle, WA 98101'
GROUP BY Product;

/*#7 How many customers ordered more than 2 products at a time, and what was the average amount spent for those customer?*/

SELECT count(distinct cust.acctnum), AVG(quantity*price)
FROM BIT_DB.FebSales Feb
LEFT JOIN BIT_DB.customers cust
ON Feb.orderID=cust.order_id
WHERE Feb.Quantity > 2
AND length(orderid) = 6
AND orderid <> 'order ID';
