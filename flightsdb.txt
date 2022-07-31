CREATE database airlineflightdb;
USE airlineflightdb;

CREATE TABLE flights(flno int, frm char(255), too char(255), distance int, departs time, arrives time, price integer);


CREATE TABLE flights(flno int, frm char(255), distance int, departs time, arrives time, price integer, PRIMARY KEY(flno));
CREATE TABLE aircraft(aid int, anmae char(255), cruisingrange int,PRIMARY KEY(aid));
CREATE TABLE employee(eid int, ename char(255), salary int, PRIMARY KEY(eid));
CREATE TABLE certified(eid int, aid int, FOREIGN KEY (eid) REFERENCES employee(eid), FOREIGN KEY (aid) REFERENCES aircraft(aid));

INSERT INTO employees (eid,ename,salary) VALUES
        (1,'Ajay',30000),
        (2,'Ajith',85000),
        (3,'Arnab',50000),
        (4,'Harry',45000),
        (5,'Ron',90000),
        (6,'Josh',75000),
        (7,'Ram',100000);
(8,”Rishabh”,12000);

INSERT INTO aircraft 
(aid,anmae,cruisingrange) values 
(123,'Airbus',1000), 
(302,'Boeing',5000), 
(306,'Jet01',5000), 
(378,'Airbus380',8000), 
(456,'Aircraft',500), 
(789,'Aircraft02',800), 
(951,'Aircraft03',1000);


INSERT INTO certified 
(eid,aid) VALUES 
(1,123), 
(2,123), 
(1,302), 
(5,302), 
(7,302), 
(1,306), 
(2,306), 
(1,378), 
(2,378), 
(4,378), 
(6,456), 
(3,456), 
(5,789), 
(6,789), 
(3,951), 
(1,951), 
(1,789);


INSERT INTO flights 
(flno,frm,too,distance,departs,arrives,price) VALUES 
(1,'Bangalore','Mangalore',360,'10:45:00','12:00:00',10000), 
(2,'Bangalore','Delhi',5000,'12:15:00','04:30:00',25000), 
(3,'Bangalore','Mumbai',3500,'02:15:00','05:25:00',30000), 
(4,'Delhi','Mumbai',4500,'10:15:00','12:05:00',35000), 
(5,'Delhi','Frankfurt',18000,'07:15:00','05:30:00',90000), 
(6,'Bangalore','Frankfurt',19500,'10:00:00','07:45:00',95000), 
(7,'Bangalore','Frankfurt',17000,'12:00:00','06:30:00',99000);


Query 1:  
Find the names of aircraft such that all pilots certified to operate them have salaries more than Rs.80,000. 
SELECT DISTINCT a.anmae FROM 
aircraft a,certified c,employee e 
WHERE a.aid=c.aid 
AND c.eid=e.eid 
AND NOT EXISTS (
SELECT * FROM employee e1 WHERE e1.eid=e.eid AND e1.salary<80000);

Query 2: 
For each pilot who is certified for more than three aircrafts, find the eid and the maximum cruising range of  the aircraft for which she or he is certified. 

SELECT c.eid,MAX(cruisingrange) FROM certified c,aircraft a WHERE c.aid=a.aid GROUP BY c.eid HAVING COUNT(*)>3;
Query 3: 
 Find the names of pilots whose salary is less than the price of the cheapest route from Bengaluru to  Frankfurt.
SELECT DISTINCT e.ename FROM employee e WHERE e.salary<(SELECT MIN(f.price) FROM flights f WHERE f.frm='Bangalore' AND f.too='Frankfurt');

Query 4: 
 For all aircraft with cruising range over 1000 Kms, find the name of the aircraft and the average salary of  all pilots certified for this aircraft.
SELECT a.aid,a.anmae,AVG(e.salary) FROM aircraft a,certified c,employee e WHERE a.aid=c.aid AND c.eid=e.eid AND a.cruisingrange>1000 GROUP BY a.aid,a.anmae;
Query 5: 
 Find the names of pilots certified for some Boeing aircraft.
SELECT distinct e.ename FROM employee e,aircraft a,certified c WHERE e.eid=c.eid AND c.aid=a.aid AND a.anmae='Boeing';
Query 6: 
 Find the aids of all aircraft that can be used on routes from Bengaluru to New Delhi.
SELECT a.aid FROM aircraft a WHERE a.cruisingrange> (SELECT MIN(f.distance) FROM flights f WHERE f.frm='Bangalore' AND f.too='Delhi');
Query 7:
A customer wants to travel from Madison to New York with no more than two changes of flight. List the  choice of departure times from Madison if the customer wants to arrive in New York by 6 p.m.
SELECT DISTINCT f1.* from flights f1,flights f2 where (f1.frm = "Bangalore" and f1.too = "Frankfurt" AND hour(f1.arrives)<6) or (f1.frm = "Bangalore" and f2.too = "Frankfurt" and f1.too=f2.frm and hour(f2.arrives)<6);
Query 8:
Print the name and salary of every non-pilot whose salary is more than the average salary for pilots.
(SELECT ename FROM employee WHERE salary>(SELECT avg(salary) FROM employee) EXCEPT (SELECT DISTINCT e2.ename from employee e2, certified c WHERE c.eid=e2.eid)) 
