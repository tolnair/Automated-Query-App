# Automated-Query-App

Automatically Installed PSQL Query Webpage With Database

If you download the repository then you can launch two docker containers, one has the chinook-database, the other has a tomcat based web server.

The install.sh file has been created, it will automatically install docker and docker-compose on the machine, 
then start the containers and make the website available on localhosts' port 8080.

For run the install.sh, use these commands:

- cd Automated-Query-App
- ./install.sh

*If you don't start the install.sh in the Automated-Query-App directory, you'll get an error.

You can use these queries for listing ...

1) Which product is made by whom:

SELECT products.product_name AS "Product", suppliers.company_name AS "Company"
FROM products
JOIN suppliers on products.supplier_id = suppliers.supplier_id
ORDER BY product_name, company_name;

2) To reveal how many products are in each category:

SELECT categories.category_name AS "Category",COUNT(products.category_id) AS "Number of Products" FROM products
LEFT JOIN categories ON categories.category_id = products.category_id
GROUP BY category_name
ORDER BY COUNT(products.category_id) DESC, category_name;

3) Which products are the 10 worst performing ones:

SELECT products.product_name AS "Product Name", cast (sum (order_details.quantity*order_details.unit_price*(1-order_details.discount)) as int)  AS "Amount of Price"
FROM order_details
LEFT JOIN products on products.product_id = order_details.product_id
GROUP BY products.product_name
ORDER BY sum (order_details.quantity*order_details.unit_price*(1-order_details.discount))
LIMIT 10;

4) To know the list of countries where there are more than 5 customers:

SELECT country, count (company_name) as "Number of customers"
FROM customers
GROUP BY country
HAVING COUNT (company_name) > 5
ORDER BY count (company_name) DESC

5) End-of-year presentation of 1997:

SELECT TO_CHAR(order_date, 'YYYY') AS "Year", TO_CHAR(order_date, 'MM') AS "Month", COUNT(order_date) as "Amount of Order",
cast (sum (order_details.quantity*order_details.unit_price*(1-order_details.discount)) as int)  AS "revenue"
FROM orders
JOIN order_details on order_details.order_id = orders.order_id
GROUP BY "Year", "Month"
HAVING TO_CHAR(order_date, 'YYYY') = '1997'
ORDER BY "Year", "Month"

6) A sheet with all US customers who have less than 5 orders:

SELECT company_name, COUNT(orders.order_id) AS "Orders", STRING_AGG(cast ((orders.order_id) as varchar), ',') AS "Order ID's"
FROM customers
LEFT JOIN orders on customers.customer_id = orders.customer_id
WHERE customers.country = 'USA'
GROUP BY customers.company_name
HAVING COUNT(orders.order_id) < 5
ORDER BY COUNT(orders.order_id), company_name
