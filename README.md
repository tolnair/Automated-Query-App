# Automated-Query-App

Automatically Installed PSQL Query Application
![automated_query_script](https://user-images.githubusercontent.com/66158707/109824763-671f0b80-7c39-11eb-85b7-25293c53cdd3.png)

If you download the repository then you can launch two docker containers, one has the database, the other has a tomcat based web server.

The install.sh file has been created which, when launched, will automatically install docker and docker-compose on the machine, 
then start the containers and make the website available on localhosts' port 8080.

You can use this query for listing all Canadian customers in alphabetric order:

SELECT CONCAT(firstname,' ', lastname) AS "Name", city AS "City"
FROM customer
WHERE country = 'Canada'
ORDER BY "Name";
