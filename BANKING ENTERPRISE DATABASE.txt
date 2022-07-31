CREATE TABLE branch(branch_name char(30), branch_city char(30), assets real, PRIMARY KEY(branch_name));
CREATE TABLE bank_account(accno int, branch_name char(30), balance real, PRIMARY KEY(accno), FOREIGN KEY(branch_name) REFERENCES branch(branch_name));
CREATE TABLE bank_customer(customer_name char(30), customer_street char(30), customer_city char(30));
CREATE TABLE depositor(customer_name char(30), accno int, FOREIGN KEY(accno) REFERENCES bank_account(accno));
CREATE TABLE loan(loan_number int, branch_name char(30), amount real, PRIMARY KEY(loan_number), FOREIGN KEY(branch_name) REFERENCES branch(branch_name));

INSERT INTO branch values('SBI_Basvangudi', 'Bangalore', 50000);
INSERT INTO branch values('BOI_Sakchit', 'Jamshedpur', 2000);
INSERT INTO branch values('SBI_Chamrajpet', 'Bangalore', 900000);
INSERT INTO branch values('HDFC_Tirupathur', 'Vellore', 6000);
INSERT INTO branch values('PNB_Banashankari', 'Bangalore', 10000);
Commit;
SELECT * FROM branch;

INSERT INTO loan values(1,'SBI_Basvangudi', 2000);
INSERT INTO loan values (3,'BOI_Sakchit', 100000);
INSERT INTO loan VALUES (2, 'SBI_Chamrajpet', 300);
INSERT INTO loan VALUES (4, 'HDFC_Tirupathur', 500);
INSERT INTO loan VALUES (5, 'PNB_Banashankari', 40000);
Commit;
SELETCT * FROM loan;

INSERT INTO bank_account VALUES(12, 'BOI_Sakchit', 1230);
INSERT INTO bank_account VALUES(23, 'HDFC_Tirupathur', 4220);
INSERT INTO bank_account VALUES(51, 'PNB_Banashankari', 9820);
INSERT INTO bank_account VALUES(109, 'SBI_Chamrajpet', 14000);
INSERT INTO bank_account VALUES(84, 'SBI_Basvangudi', 121); 
INSERT INTO bank_account VALUES(123, 'SBI_Basvangudi', 13120);
INSERT INTO bank_account VALUES(13, 'SBI_Basvangudi', 2120);
INSERT INTO bank_account VALUES(902, 'SBI_Basvangudi', 13120);
INSERT INTO bank_account VALUES(832, 'SBI_Basvangudi', 13120);
INSERT INTO bank_account VALUES(119,"SBI_Chamrajpet",12412);
INSERT INTO bank_account VALUES(39,"HDFC_Tirupathur",1221);
INSERT INTO bank_account VALUES(190,"BOI_Sakchit",1212412);
Commit;
SELECT * FROM bank_account;



INSERT INTO bank_customer VALUES("Raju", "MG","Bangalore");
INSERT INTO bank_customer VALUES("Shashank","Jehangir","Ranchi");
INSERT INTO bank_customer VALUES("Abishek","Marine Drive","Delhi");
INSERT INTO bank_customer VALUES("Diwakar","14th Milestone","Jamshedpur");
INSERT INTO bank_customer VALUES("Sid","Teram","Vellore");
Commit;
Select * from bank_customer;

INSERT INTO depositor VALUES("Raju",12);
INSERT INTO depositor VALUES("Shashank",23);
INSERT INTO depositor VALUES("Abishek",51);
INSERT INTO depositor VALUES("Diwakar",109);
INSERT INTO depositor VALUES("Sid",84);
INSERT INTO depositor VALUES("Raju",123);
INSERT INTO depositor VALUES("Raju",832);
INSERT INTO depositor VALUES("Abishek",902);
INSERT INTO depositor VALUES("Abishek",39)
INSERT INTO depositor VALUES("Abishek",190)
INSERT INTO depositor VALUES("Abishek",119)


Commit;

Select * from depositor;

Queries: 
3) SELECT customer_name FROM depositor d, bank_account a where d.accno=a.accno AND a.branch_name="SBI_Basvangudi" GROUP BY d.customer_name HAVING COUNT(d.customer_name)>=2;
4)
SELECT d.customer_name FROM bank_account a,branch b,depositor d WHERE b.branch_name=a.branch_name AND  a.accno=d.accno AND b.branch_city='Bangalore' GROUP BY d.customer_name HAVING COUNT(distinct b.branch_name)=(SELECT COUNT(branch_name) FROM branch WHERE branch_city='Bangalore');
5)  DELETE FROM bank_account WHERE branch_name IN(SELECT branch_name FROM branch WHERE branch_city='Jamshedpur');
