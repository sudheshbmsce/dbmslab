CREATE TABLE PERSON(driver_id char(10), Name char(20), address char(40), PRIMARY KEY(driver_id));
CREATE TABLE CAR (Regno char(10), model char(10), year int, PRIMARY KEY(Regno));
CREATE TABLE ACCIDENT(report_number int, datex date, location char(40), PRIMARY KEY(report_number));
CREATE TABLE OWNS(driver_id char(10), Regno char(10), PRIMARY KEY(driver_id, Regno), FOREIGN KEY(driver_id) REFERENCES PERSON(driver_id), FOREIGN KEY(Regno) REFERENCES CAR(Regno));
CREATE TABLE PARTICIPATED (driver_id char(10), Regno char(10), report_number int, damage_amount int, PRIMARY KEY(driver_id, Regno, report_number), FOREIGN KEY(driver_id) REFERENCES PERSON(driver_id), FOREIGN KEY(Regno) REFERENCES CAR(Regno), FOREIGN KEY(report_number) REFERENCES ACCIDENT(report_number));

INSERT INTO person VALUES('A10','James',’Renukanagar’);
INSERT INTO person VALUES('A14','Rishabh Pant',’Srinagar’);
INSERT INTO person VALUES('B33', ‘David',’Mumbai’);
INSERT INTO person VALUES(‘B56',’Tom ',’Gopal Nagar’);
INSERT INTO person VALUES(‘C14',’Ronith ',’Toy Town’);
commit;
SELECT*FROM person;


INSERT INTO car VALUES('KA690','Nano',2006);
INSERT INTO car VALUES('KA466','Indica',2000);
INSERT INTO car VALUES('BR720','Sumo',2010);
INSERT INTO car VALUES('CK144','Alto',2014);
INSERT INTO car VALUES('RL221','Zing',2015);
commit;
SELECT*FROM car;

INSERT INTO accident VALUES(123,'2001-01-04','Delhi');
INSERT INTO accident VALUES(456,'2008-06-08','Kolkata');
INSERT INTO accident VALUES(789,'2004-04-10','Bangalore');
INSERT INTO accident VALUES(480,'2012-12-15','Jaipur');
INSERT INTO accident VALUES(921,'2003-08-20','Mumbai');
commit;
select*from accident;

INSERT INTO owns VALUES('A01','BR720');
INSERT INTO owns VALUES('A14','CK144');
INSERT INTO owns VALUES('B33','KA466');
INSERT INTO owns VALUES('B56','KA690');
INSERT INTO owns VALUES('C14','RL221');
commit;
SELECT*FROM owns;

INSERT INTO participated VALUES('A01','BR720',123,400);
INSERT INTO participated VALUES('A14','CK144',456,1000);
INSERT INTO participated VALUES('B33','KA466',480,20);
INSERT INTO participated VALUES('B56','KA690',789,90000);
INSERT INTO participated VALUES('C14','RL221',921,1500);
commit;
SELECT*from participated;

Queries for demonstration:
1)	UPDATE participated SET damage_amount=25000 WHERE report_number=123 AND Regno=’BR720’;
2)	INSERT INTO accident values(810, “2008-04-10”, “Chandigardh”);
3)	SELECT COUNT(driver_id) FROM participated x,accident y WHERE x.report_number=y.report_number and year(y.datex)=2008;
4)	SELECT COUNT(driver_id) FROM owns x, car y WHERE x.Regno=y.Regno and y.model='Nano';

commit;
