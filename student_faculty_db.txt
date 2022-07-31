CREATE database studentfacultydb;
use studentfacultydb;

CREATE TABLE student(snum int, sname char(30), major char(30), level char(30), age int, PRIMARY KEY(snum));
CREATE TABLE faculty(fid int, fname char(30), deptid int, PRIMARY KEY(fid));
CREATE TABLE enrolled(snum int, name char(30), FOREIGN KEY(snum) REFERENCES student(snum));
CREATE TABLE class(name char(30), meet time, room char(30), fid int, FOREIGN KEY(fid) REFERENCES faculty(fid));

INSERT INTO student (snum,sname,major,level,age) VALUES (1,'jhon','CS','Sr',19), 
(2,'smith','CS','Jr',20), 
(3,'jacob','CV','Sr',20), 
(4,'tom','CS','Jr',20), 
(5,'sid','CS','Jr',20), 
(6,'harry','CS','Sr',21);

INSERT INTO faculty (fid,fname, deptid) VALUES 
(11,'Harshith',1000), 
(12,'Mohan',1000), 
(13,'Kumar',1001), 
(14,'Shobha',1002), 
(15,'Shan',1000);

INSERT INTO class (name,meet,room,fid) VALUES 
('class1','12:00:00','room1',14), 
('class10','05:00:00','room128',14), 
('class2','08:00:00','room2',12), 
('class3','07:00:00','room3',11), 
('class4','18:00:00','room4',14), 
('class5','20:00:00','room3',15), 
('class6','08:00:00','room2',14), 
('class7','19:00:00','room3',14);

INSERT INTO enrolled (snum,name) VALUES (1,'class1'), 
(2,'class1'), 
(3,'class3'), 
(4,'class3'), 
(3,'class3'), 
(5,'class4'), 
(1,'class5'), 
(2,'class5'), 
(3,'class5'), 
(4,'class5'), 
(5,'class5'), 
(6,'class5');



Query 1:  Find the names of all juniors (level=Jr) who are enrolled for class taught by professor Harshith.
 SELECT DISTINCT s.sname FROM student s,class c,faculty f,enrolled e WHERE s.snum=e.snum AND e.name=c.name AND s.level='jr' AND f.fname='Harshith' AND f.fid=c.fid;

Query 2:  Find the names of all classes that either meet in room128 or have 5 or more students enrolled.
SELECT DISTINCT name FROM class WHERE room='room128' OR name IN (SELECT e.name FROM enrolled e GROUP BY e.name HAVING COUNT(*)>=5);

Query3:  Find the names of all students who are enrolled in two classes that meet at same time.
SELECT DISTINCT s.sname FROM student s WHERE s.snum IN (SELECT e1.snum FROM enrolled e1,enrolled e2,class c1,class c2 WHERE e1.snum=e2.snum AND e1.name<>e2.name AND e1.name=c1.name AND e2.name=c2.name AND c1.meet=c2.meet);

Query 4: Find the names of faculty members who teach in every room in which some class is taught.
SELECT f.fname,f.fid FROM faculty f WHERE f.fid in (SELECT fid FROM class GROUP BY fid HAVING COUNT(*)=(SELECT COUNT(DISTINCT room) FROM class));

Query 5: Find the names of the faculty members for whome the combined enrollment of the classes that they teach is less then five.
SELECT DISTINCT f.fname FROM faculty f WHERE f.fid IN ( SELECT c.fid FROM class c, enrolled e WHERE c.name = e.name GROUP BY c.name HAVING COUNT(c.name)< 5 );

Querry 6:
SELECT sname FROM student where snum not in(SELECT snum from enrolled);
Querry 7:
select s.age, s.level from student s group by s.age,s.level having s.level in (select s1.level from student s1 where s1.age = s.age group by s1.level,s1.age having count(*) >= all (select count(*) from student s2 where s1.age = s2.age group by s2.level,s2.age))
