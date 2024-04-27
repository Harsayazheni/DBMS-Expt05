# DBMS-Expt05
## Aim
 To Study & Implementation of  Sub queries Views.

 ## Theory
 ### Subqueries
The query within another is known as a subquery. A statement containing a subquery is called a parent statement. The rows returned by subquery are used by the parent statement or in other words A subquery is a SELECT statement that is embedded in a clause of another SELECT statement 

You can place the subquery in a number of SQL clauses: 
1) WHERE clause  
2) HAVING clause  
3) FROM clause  
4) OPERATORS( IN.ANY,ALL,<,>,>=,<= etc..)

 Types:
1. Sub queries that return several values 
Sub queries can also return more than one value. Such results should be made use along with the operators in and any.


2. Multiple queries 
Here more than one subquery is used. These multiple sub queries are combined by means of ‘and’ & ‘or’ keywords. 


3. Correlated subquery 
A subquery is evaluated once for the entire parent statement whereas a correlated Sub query is evaluated once per row processed by the parent statement.


VIEW: 
In SQL, a view is a virtual table based on the result-set of an SQL statement. A view contains rows and columns, just like a real table. The fields in a view are fields from one or more real tables in the database. You can add SQL functions, WHERE, and JOIN statements to a view and present the data as if the data were coming from one single table. A view is a virtual table, which consists of a set of columns from one or more tables. It is similar to a table but it does not store in the database. View is a query stored as an object. 


### 1.Write a SQL query that retrieves the all the columns from the Table Grades, where the grade is equal to the minimum grade achieved in each subject.
![Screenshot 2024-04-24 200934](https://github.com/Harsayazheni/DBMS-Expt05/assets/118708467/05c03055-fba2-4633-9b05-cfad1ca27fa6)

#### Query
```
SELECT g.*
FROM Grades g
JOIN (
  SELECT subject, MIN(grade) AS min_grade
  FROM Grades
  GROUP BY subject
) min_grades
ON g.subject = min_grades.subject AND g.grade = min_grades.min_grade
```
#### Output
![Screenshot 2024-04-24 200948](https://github.com/Harsayazheni/DBMS-Expt05/assets/118708467/1584aa0b-9be8-4c0f-bd11-d23d39020375)

### 2.Write a SQL query to Find employees who have an age less than the average age of employees with incomes over 1 million
![Screenshot 2024-04-24 200958](https://github.com/Harsayazheni/DBMS-Expt05/assets/118708467/e8c00378-9b4e-4cfb-ad29-48fd828370e5)

#### Query
```
WITH income_over_1m AS (
    SELECT AVG(age) as avg_age
    FROM Employee
    WHERE income > 1000000
)
SELECT e.id, e.name, e.age, e.city, e.income
FROM Employee e
JOIN income_over_1m io1m ON 1=1
WHERE e.age < io1m.avg_age;
```
#### Output
![Screenshot 2024-04-24 201006](https://github.com/Harsayazheni/DBMS-Expt05/assets/118708467/9c8979e8-6f5f-49cb-b17e-d4ae832cac1e)

### 3.From the following tables write a SQL query to find the order values greater than the average order value of 10th October 2012. Return ord_no, purch_amt, ord_date, customer_id, salesman_id.
![Screenshot 2024-04-24 201014](https://github.com/Harsayazheni/DBMS-Expt05/assets/118708467/6d7ed010-4ec7-4da1-912b-e931acca1524)

#### Query
```
SELECT ord_no, purch_amt, ord_date, customer_id, salesman_id
FROM Orders
WHERE purch_amt > (
    SELECT AVG(purch_amt)
    FROM Orders
    WHERE DATE(ord_date) = '2012-10-10'
);
```
#### Output
![Screenshot 2024-04-24 201022](https://github.com/Harsayazheni/DBMS-Expt05/assets/118708467/bfff135c-e970-4b68-a9cb-6488640ec319)

### 4.Write a SQL query to Identify customers whose city is different from the city of the customer with the highest ID
![Screenshot 2024-04-24 201030](https://github.com/Harsayazheni/DBMS-Expt05/assets/118708467/a131bf4f-80c9-4ac3-ac32-20ceac06ede2)

#### Query
```
WITH max_id_customer AS (
    SELECT city AS max_city
    FROM customer
    WHERE id = (SELECT MAX(id) FROM customer)
)
SELECT c.id, c.name, c.city, c.email, c.phone
FROM customer c
WHERE c.city != (SELECT max_city FROM max_id_customer);
```
#### Output
![Screenshot 2024-04-24 201035](https://github.com/Harsayazheni/DBMS-Expt05/assets/118708467/3fbc7cca-dc05-4075-aba0-c7647048ec67)

### 5.From the following tables, write a SQL query to find all the orders generated in New York city. Return ord_no, purch_amt, ord_date, customer_id and salesman_id.
![Screenshot 2024-04-24 201044](https://github.com/Harsayazheni/DBMS-Expt05/assets/118708467/ced52b9b-3d66-4697-afd6-964e04df2f45)

#### Query
```
SELECT  Orders.ord_no, Orders.purch_amt, Orders.ord_date, Orders.customer_id, Orders.salesman_id
FROM Orders
JOIN Salesman ON Orders.salesman_id = Salesman.salesman_id
WHERE Salesman.city = 'New York';
```
#### Output
![Screenshot 2024-04-24 201052](https://github.com/Harsayazheni/DBMS-Expt05/assets/118708467/d2897193-7cf4-476d-8308-40bfb9e84f91)

### 6.Write a SQL query to Retrieve the medications with dosages equal to the highest dosage
![Uploading Screenshot 2024-04-24 201058.png…]()

#### Query
```
SELECT medication_id, medication_name, dosage
FROM Medications
WHERE dosage = (SELECT MAX(dosage) FROM Medications);
```
#### Output
![Screenshot 2024-04-24 201105](https://github.com/Harsayazheni/DBMS-Expt05/assets/118708467/d3efac2f-a740-4b69-8483-ddb6e199a7cc)

### 7.Write a SQL query to Retrieve the names of customers who have a phone number that is not shared with any other customer.
![Screenshot 2024-04-24 201113](https://github.com/Harsayazheni/DBMS-Expt05/assets/118708467/5bbb0344-6382-4fbc-94ab-15f3c55784cd)

#### Query
```
SELECT name 
FROM customer c1 
WHERE phone NOT IN (SELECT phone FROM customer c2 WHERE c2.phone = c1.phone AND c2.id != c1.id)
```
#### Output
![Screenshot 2024-04-24 201121](https://github.com/Harsayazheni/DBMS-Expt05/assets/118708467/ab9f27fc-06f0-4383-a54a-b70714aec534)

### 8.Write a SQL query to retrieve all columns from the CUSTOMERS table for customers whose Address as Delhi
![Screenshot 2024-04-24 201129](https://github.com/Harsayazheni/DBMS-Expt05/assets/118708467/1c2e6480-21b9-4807-9b78-7babaabc121f)

#### Query
```
SELECT * FROM CUSTOMERS WHERE ADDRESS = 'Delhi';
```
#### Output
![Screenshot 2024-04-24 201140](https://github.com/Harsayazheni/DBMS-Expt05/assets/118708467/08b9f615-cde2-446f-a9e8-ebd3ca18a0f0)

### 9.From the following tables, write a SQL query to find all orders generated by the salespeople who may work for customers whose id is 3007. Return ord_no, purch_amt, ord_date, customer_id, salesman_id.
![Screenshot 2024-04-24 201147](https://github.com/Harsayazheni/DBMS-Expt05/assets/118708467/d9587f22-ead5-4428-ac34-a386d8f5589a)

#### Query
```
SELECT ord_no, purch_amt, ord_date, customer_id, salesman_id
FROM Orders
WHERE salesman_id IN (
    SELECT salesman_id
    FROM Orders
    WHERE customer_id = 3007
)
```
#### Output
![Screenshot 2024-04-24 201156](https://github.com/Harsayazheni/DBMS-Expt05/assets/118708467/8361216a-ec0d-4ade-bcf6-29c60c741f4c)

### 10.From the following tables write a SQL query to find all orders generated by New York-based salespeople. Return ord_no, purch_amt, ord_date, customer_id, salesman_id.
![Screenshot 2024-04-24 201312](https://github.com/Harsayazheni/DBMS-Expt05/assets/118708467/1f4f74c4-6e3c-4ad6-8ce3-5ab68291fb89)

#### Query
```
SELECT 
    Orders.ord_no, 
    Orders.purch_amt, 
    Orders.ord_date, 
    Orders.customer_id, 
    Orders.salesman_id
FROM 
    Orders
INNER JOIN 
    Salesman ON Orders.salesman_id = Salesman.salesman_id
WHERE 
    Salesman.city = 'New York';
```
#### Output
![Screenshot 2024-04-24 201319](https://github.com/Harsayazheni/DBMS-Expt05/assets/118708467/e7f11f68-655b-4c8e-a8d7-98e3d8d80b76)


## Result
Thus , To Study & Implementation of  Sub queries Views is successfully done.
