#  Swiggy Data Analysis using SQL

![sw](https://github.com/Priyaah13/swiggy_sql_project/blob/main/swiggy%20img%20logo.png)


##Create database SQLFT;
#1) 1) Details of customers whose name starts with A; and have
#gmail idand have
#gmail id

SELECT * FROM sqlft.`card (1)`;
SELECT * FROM sqlft.`cuisine (1)`;
SELECT * FROM sqlft.`customers (1)`;
SELECT * FROM sqlft.`details (1)`;
SELECT * FROM sqlft.`dish (1)`;
SELECT * FROM sqlft.`orders (1)`;

SELECT * 
FROM sqlft.`customers (1)`
WHERE name LIKE 'A%' 
AND email LIKE '%@gmail.com';

#2) Details of customers containing 3 times 5 in password
SELECT * FROM sqlft.`customers (1)`
WHERE password LIKE '%5%5%5%';

#3) Name &amp; address of North Indian restaurant which is situated
#in 456 Elm St
SELECT r_name, 
       address 
FROM sqlft.`cuisine (1)`
WHERE cuisine = 'North Indian' 
AND address = '456 Elm St';

#4)Names of restaurants which are either Italian or situated at '433 Oak St'
SELECT r_name 
FROM sqlft.`cuisine (1)`
WHERE cuisine = 'Italian' 
OR address = '433 Oak St';

#5) How many orders were palced with amount 650 or more
SELECT COUNT(*) as TotalOrders 
from sqlft.`orders (1)`
WHERE amount >= 650;

#6)6) Find details of those customers who have never ordered
SELECT * 
FROM sqlft.`customers (1)`
WHERE user_id not in (select distinct user_id FROM sqlft.`orders (1)`);

#7)Details of restaurants having sales greater than x (e.g., 1000):
select r_id, SUM(amount) AS TotalSales 
FROM  sqlft.`orders (1)` 
group by r_id 
having SUM(amount) > 1000;

#8) Show all order details for a particular customer ('Vartika')
SELECT o.* 
From sqlft.`orders (1)` o
Join sqlft.`customers (1)` c
ON o.user_id = c.user_id
WHERE c.name = 'Vartika';

#9) What is the average price per dish
SELECT AVG(price) AS AvgPrice 
FROM sqlft.`card (1)`;

#10) Find out number of times each customer ordered food
#from each restaurants
SELECT user_id, r_id, COUNT(*) AS OrderCount 
FROM sqlft.`orders (1)`
GROUP BY user_id, r_id;

#11)Find the top restaurant in terms of the number of orders 
#for a given month 
#iam doing it for all order month)
SELECT r_id, MONTH(date) AS OrderMonth, COUNT(*) AS OrderCount
FROM sqlft.`orders (1)`
WHERE YEAR(date) = 2022
GROUP BY r_id, OrderMonth
ORDER BY OrderMonth, OrderCount DESC;

SELECT * FROM sqlft.`orders (1)`;

#12) Who is most loyal customer of dominos?
selecT user_id, COUNT(*) AS OrderCount 
FROM sqlft.`orders (1)` o
JOIN sqlft.`cuisine (1)` c ON o.r_id = c.r_id
Where c.r_name = 'Dominos'
GROUP BY user_id 
order BY OrderCount DESC ;

#13) What is the favorite food of each customer


select o.user_id, f.f_name, COUNT(*) AS OrderCount
FROM sqlft.`orders (1)` o
Join sqlft.`card (1)` c ON o.r_id = c.r_id 
Join sqlft.`dish (1)` f ON c.f_id = f.f_id 
GROUP BY o.user_id, f.f_name
ORDER BY o.user_id, OrderCount DESC;


SELECT * FROM sqlft.`details (1)`;
SELECT * FROM sqlft.`dish (1)`;
SELECT * FROM sqlft.`orders (1)`;

#14) Favorite food of each customer along with customer details:
SELECT c.user_id, c.name, c.email, c.password, f.f_name, COUNT(*) AS OrderCount
From sqlft.`customers (1)` c
join sqlft.`orders (1)` o ON c.user_id = o.user_id
Join sqlft.`card (1)` card ON o.r_id = card.r_id
JOIN sqlft.`dish (1)` f ON card.f_id = f.f_id
GROUP BY c.user_id, c.name, c.email, c.password, f.f_name
Order BY c.user_id, OrderCount DESC
LIMIT 0, 1000;

#15)For each restaurant, find out the user who has ordered the maximum number of times
WITH MaxOrder AS (
    SELECT r_id, user_id, COUNT(*) AS OrderCount 
    FROM sqlft.`orders (1)`
    GROUP BY r_id, user_id
)
Select r_id, user_id 
from MaxOrder 
Where (r_id, OrderCount) IN (SELECT r_id, MAX(OrderCount) FROM MaxOrder GROUP BY r_id);
