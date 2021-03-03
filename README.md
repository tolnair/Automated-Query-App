# Automated-Query-App

Automatically Installed PSQL Query Application

If you download the repository then you can launch two docker containers, one has the chinook-database, the other has a tomcat based web server.

The install.sh file has been created which, when launched, will automatically install docker and docker-compose on the machine, 
then start the containers and make the website available on localhosts' port 8080.

You can use this query for listing all Canadian customers in alphabetric order:

SELECT CONCAT(firstname,' ', lastname) AS "Name", city AS "City"
FROM customer
WHERE country = 'Canada'
ORDER BY "Name";
