mysql> select * from employees;
+-------+--------+------------+----------------+-----------+------+--------+-------------+
| empno | ename  | dob        | address        | mobile    | dno  | salary | designation |
+-------+--------+------------+----------------+-----------+------+--------+-------------+
| 12345 | akhil  | 1980-05-12 | kakkadad house | 984562137 |  212 |  15000 | clerk       |
| 12362 | nikhil | 1985-06-12 | pala house     | 984562235 |  222 |  15000 | accountant  |
| 14562 | vishnu | 1985-06-12 | palayi house   | 984562235 |  252 |  15000 | accountant  |
| 12377 | cere   | 1980-05-22 | kakkad house   | 984562537 |  212 |  15000 | clerk       |
| 12360 | elzha  | 1980-05-10 | kakkadu house  | 984562537 |  252 |  15000 | po          |
+-------+--------+------------+----------------+-----------+------+--------+-------------+
5 rows in set (0.03 sec)

mysql> select * from department;
+---------+------------+-------------+
| dept_no | dept_name  | location    |
+---------+------------+-------------+
|     212 | mechincal  | firstfloor  |
|     222 | electrical | groundfloor |
|     232 | accounting | groundfloor |
|     252 | techincal  | firstfloor  |
|     262 | management | thirdfloor  |
+---------+------------+-------------+
5 rows in set (0.04 sec)

mysql> create view empdet as select empno,ename,dept_no,location from employees,department where dno=dept_no;
Query OK, 0 rows affected (0.12 sec)

mysql> select * from empdet;
+-------+--------+---------+-------------+
| empno | ename  | dept_no | location    |
+-------+--------+---------+-------------+
| 12345 | akhil  |     212 | firstfloor  |
| 12362 | nikhil |     222 | groundfloor |
| 14562 | vishnu |     252 | firstfloor  |
| 12377 | cere   |     212 | firstfloor  |
| 12360 | elzha  |     252 | firstfloor  |
+-------+--------+---------+-------------+
5 rows in set (0.04 sec)

mysql> create view det as select empno,ename from employees where dno=212;
Query OK, 0 rows affected (0.68 sec)

mysql> select * from det;
+-------+-------+
| empno | ename |
+-------+-------+
| 12345 | akhil |
| 12377 | cere  |
+-------+-------+
2 rows in set (0.04 sec)

mysql> drop view det;
Query OK, 0 rows affected (0.13 sec)

mysql> update empdet set ename='anoop' where empno=12360;
Query OK, 1 row affected (0.13 sec)
Rows matched: 1  Changed: 1  Warnings: 0

mysql> select * from empdet;
+-------+--------+---------+-------------+
| empno | ename  | dept_no | location    |
+-------+--------+---------+-------------+
| 12345 | akhil  |     212 | firstfloor  |
| 12362 | nikhil |     222 | groundfloor |
| 14562 | vishnu |     252 | firstfloor  |
| 12377 | cere   |     212 | firstfloor  |
| 12360 | anoop  |     252 | firstfloor  |
+-------+--------+---------+-------------+
5 rows in set (0.08 sec)

mysql> create table empp as select empno,dno,salary,0 as total from employees;
Query OK, 5 rows affected (1.61 sec)
Records: 5  Duplicates: 0  Warnings: 0

mysql> select * from empp;
+-------+--------+--------+-------+
| empno | dno    | salary | total |
+-------+--------+--------+-------+
| 12345 | 212    |  15000 |     0 |
| 12362 | 222    |  15000 |     0 |
| 14562 | 252    |  15000 |     0 |
| 12377 | 212    |  15000 |     0 |
| 12360 | 252    |  15000 |     0 |
+-------+--------+--------+-------+
5 rows in set (0.00 sec)

mysql> delimiter //
mysql> create procedure uSalary( IN paraml int)
    -> begin
    -> update empp
    -> set total=(select sum(salary) from employees where dno=paraml)
    -> where dno=paraml;
    -> end; //
Query OK, 0 rows affected (0.13 sec)

mysql> delimiter ;
mysql> call uSalary(222);
Query OK, 1 row affected (0.23 sec)

mysql> call uSalary(212);
Query OK, 2 rows affected (0.07 sec)

mysql> select * from empp;
+-------+------+--------+-------+
| empno | dno  | salary | total |
+-------+------+--------+-------+
| 12345 |  212 |  15000 | 30000 |
| 12362 |  222 |  15000 | 15000 |
| 14562 |  252 |  15000 |     0 |
| 12377 |  212 |  15000 | 30000 |
| 12360 |  252 |  15000 |     0 |
+-------+------+--------+-------+
5 rows in set (0.00 sec)

mysql> delimiter &&
mysql> create procedure display()
    -> begin
    -> select * from empp where dno=222;
    -> end &&
Query OK, 0 rows affected (0.17 sec)

mysql> delimiter ;
mysql> call display();
+-------+------+--------+-------+
| empno | dno  | salary | total |
+-------+------+--------+-------+
| 12362 |  222 |  15000 | 15000 |
+-------+------+--------+-------+
1 row in set (0.10 sec)

Query OK, 0 rows affected (0.12 sec)