# Automated-Query-App

Automatically Installed PSQL Query Application

If you download the repository then you can launch two docker containers, one has the chinook-database, the other has a tomcat based web server.

The install.sh file has been created which, when launched, will automatically install docker and docker-compose on the machine, 
then start the containers and make the website available on localhosts' port 8080.

For run the install.sh, use these commands:

cd Automated-Query-App
./install.sh

*If you don't start the install.sh in the Automated-Query-App directory, you'll get an error.

You can use these queries for listing ...

1) Which product is made by whom:

SELECT products.product_name AS "Product", suppliers.company_name AS "Company"
FROM products
JOIN suppliers on products.supplier_id = suppliers.supplier_id
ORDER BY product_name, company_name;

2) To reveal how many products are in each category:

SELECT categories.category_name AS category,COUNT(products.category_id) AS "number_of_products" FROM products
LEFT JOIN categories ON categories.category_id = products.category_id
GROUP BY category_name
ORDER BY COUNT(products.category_id) DESC, category_name;

3) Which products are the 10 worst performing ones:

SELECT products.product_name, cast (sum (order_details.quantity*order_details.unit_price*(1-order_details.discount)) as int)  AS SUM
FROM order_details
left join products on products.product_id = order_details.product_id
group by products.product_name
order by sum (order_details.quantity*order_details.unit_price*(1-order_details.discount))
limit 10;

4) To know the list of countries where there are more than 5 customers:

select country, count (company_name) as number_of_customers
from customers
group by country
having count (company_name) > 5
order by count (company_name) DESC

5) End-of-year presentation of 1997:

select TO_CHAR(order_date, 'YYYY') AS "year", TO_CHAR(order_date, 'MM') AS "month", COUNT(order_date) as "order_count",
cast (sum (order_details.quantity*order_details.unit_price*(1-order_details.discount)) as int)  AS "revenue"
from orders
join order_details on order_details.order_id = orders.order_id
group by "year", "month"
having TO_CHAR(order_date, 'YYYY') = '1997'
order by "year", "month"

6) A sheet with all US customers who have less than 5 orders:

SELECT company_name, 
count (orders.order_id) as orders, 
STRING_AGG (cast ((orders.order_id) as varchar), ',') as order_ids
FROM customers
left join orders on customers.customer_id = orders.customer_id
where customers.country = 'USA'
GROUP BY customers.company_name
having count (orders.order_id) < 5
order by count (orders.order_id), company_name
