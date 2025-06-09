--for Q1 1. List all employees along with their department names. 
select name
from employees
where salary = (select max(salary) from employees) ;

-- for Q2Find employees who are working in the 'Engineering' department. 

select e.name as employees_name, d.name as department_name
from employees e
join departments d on e.id = d.id;
-- for Q3 3. Get the names of departments that have employees. 
select d.name as departments_name
from departments d
join employees e on e.id=d.id 
where e.department_id is not null;

-- for Q4 4. Count how many employees are in each department.

SELECT d.name AS department_name, COUNT(e.id) AS employee_count
FROM departments d
LEFT JOIN employees e ON d.id = e.department_id
GROUP BY d.name;
-- 7.Get the average salary of employees per department. 
select d.name as departments_name, avg(e.salary) as average_salary
from departments d 
join employees e on e.id = d.id 
group by d.name ;

--6.List all departments and the total salary paid to their employees. 

select d.name, sum(e.salary) as total_salary_paid
from departments d
join employees e on d.id=e.id 
group by d.name ;

--7. Retrieve names of employees and project names if both are in the same department. 
select e.name, p.name as project_name
from employees e
join projects p on e.department_id = p.department_id;

--8. List employees whose salary is above the department average. 
select name, salary
from employees 
where salary > (select avg(e.salary) from employees e join departments d on e.id=d.id) ;
--9. Find employees who work in departments that have ongoing projects. 
select e.name 
from employees e
join projects p on p.department_id=e.department_id ;
--10. Show employees who are not in the 'Marketing' department.
select e.name
from employees e
join departments d on e.id=d.id 
where d.name <> 'marketing' ;


--11. List all employees and their department names (even if they don’t have one).
select e.name as employee_name, d.name as department_name
from employees e
left join departments d on e.id=d.id ;
---12. Get all departments and their employee names (if any). 
select d.name as departmet_name, e.name as employee_name
from employees e
left join departments d on d.id=e.id  ;
--13. Find employees who are not assigned to any department. 

SELECT id, name
FROM employees
WHERE department_id IS NULL;

--14. Show departments without any employees. 
SELECT d.id, d.name AS department_name
FROM departments d
LEFT JOIN employees e ON d.id = e.department_id
WHERE e.id IS NULL;

--15. List all projects and show employee names from the same department (if any). 
SELECT p.name AS project_name,d.name AS department_name,e.name AS employee_name
FROM projects p
JOIN departments d ON p.department_id = d.id
LEFT JOIN employees e ON e.department_id = p.department_id;

--16. Find all departments and count of employees (0 if no employees). 
SELECT d.name AS department_name, COUNT(e.id) AS employee_count
FROM departments d
LEFT JOIN employees e ON d.id = e.department_id
GROUP BY d.id, d.name;

--17. Show each department's name and the highest salary in that department (even if no employees). 
select d.name as department_name, max(e.salary) as hight_salary
from departments d
left join employees e on e.id=d.id
group by d.name;
--18. Retrieve all employees and any project names they might be working on via department match. 
select e.name as employee_name, p.name as project_name
from employees e
left join projects p on e.department_id = p.department_id ;
--19. List all employees with NULL in place of department if not assigned. 
SELECT e.name AS employee_name,d.name AS department_name
FROM employees e
LEFT JOIN departments d ON e.department_id = d.id;

--20. List departments with either NULL or known employee salaries. 
SELECT d.name AS department_name,e.salary
FROM departments d
LEFT JOIN employees e ON d.id = e.department_id;

--21. Retrieve all departments and their employees (including empty departments).
select d.name as department_name, e.name as employee_name
from departments d
right join employees e on e.id = d.id ;
--22
 select d.name as department_name, e.name as employee_name
from departments d
right join employees e on e.id = d.id
where d.name is null;
--23  Show employees whose departments exist (same as INNER JOIN, but using RIGHT JOIN).
SELECT e.name AS employee_name, d.name AS department_name
FROM departments d
RIGHT JOIN employees e ON e.department_id = d.id;

--. 24 Find departments that do not have any projects but still have employees.
SELECT d.id, d.name
FROM departments d
JOIN employees e ON d.id = e.department_id
LEFT JOIN projects p ON d.id = p.department_id
WHERE p.id IS NULL;
 --25 Get all departments with total salaries or show NULL if no employees.
 select d.id, d.name as department_name, sum(e.salary) as total_salary
 from departments d
 left join employees e on e.id = d.id 
 group by d.id, d.name ;
--26. List all employees and departments, whether matched or not.
-- LEFT JOIN part: all employees, matched departments (or NULL)
SELECT 
    e.name AS employee_name,
    d.name AS department_name
FROM 
    employees e
LEFT JOIN 
    departments d ON e.department_id = d.id

UNION

SELECT 
    e.name AS employee_name,
    d.name AS department_name
FROM 
    departments d
LEFT JOIN 
    employees e ON e.department_id = d.id ;
---- Employees with no matching department (unmatched employees)
SELECT 
    e.name AS employee_name,
    NULL AS department_name,
    'Unmatched Employee' AS type
FROM 
    employees e
LEFT JOIN 
    departments d ON e.department_id = d.id
WHERE 
    d.id IS NULL

UNION

-- Departments with no employees (unmatched departments)
SELECT 
    NULL AS employee_name,
    d.name AS department_name,
    'Unmatched Department' AS type
FROM 
    departments d
LEFT JOIN 
    employees e ON e.department_id = d.id
WHERE 
    e.id IS NULL;


SELECT 
    e.id AS employee_id,
    e.name AS employee_name,
    d.id AS department_id,
    d.name AS department_name
FROM employees e
FULL OUTER JOIN departments d ON e.department_id = d.id;
 --30

-- Create the customers table
CREATE TABLE customers (
    customer_id INT PRIMARY KEY,
    name VARCHAR(100),
    city VARCHAR(100)
);

-- Create the suppliers table
CREATE TABLE suppliers (
    supplier_id INT PRIMARY KEY,
    name VARCHAR(100),
    city VARCHAR(100)
);

-- Create the orders table
CREATE TABLE orders (
    order_id INT PRIMARY KEY,
    customer_id INT,
    amount DECIMAL(10, 2),
    FOREIGN KEY (customer_id) REFERENCES customers(customer_id)
);


-- Insert data into customers
INSERT INTO customers (customer_id, name, city) VALUES
(1, 'Alice', 'Dhaka'),
(2, 'Bob', 'Chattogram'),
(3, 'Charlie', 'Rajshahi'),
(4, 'David', 'Khulna'),
(5, 'Emma', 'Dhaka');

-- Insert data into suppliers
INSERT INTO suppliers (supplier_id, name, city) VALUES
(101, 'SuperMart', 'Dhaka'),
(102, 'ShopEase', 'Khulna'),
(103, 'MartBD', 'Barisal'),
(104, 'FastBuy', 'Sylhet'),
(105, 'SaveMore', 'Rajshahi');

-- Insert data into orders
INSERT INTO orders (order_id, customer_id, amount) VALUES
(1, 1, 1200),
(2, 2, 850),
(3, 3, 400),
(4, 4, 1500),
(5, 5, 600);

-- UNION & UNION ALL 
--01. List all unique cities from customers and suppliers. 
select city from customers 
union 
select city from suppliers ;
--2. List all cities (including duplicates) from customers and suppliers. 
SELECT city FROM customers
UNION ALL
SELECT city FROM suppliers;

--3. Get all customer and supplier names (no duplicates).
SELECT name FROM customers
UNION
SELECT name FROM suppliers;

--4. Get all customer and supplier names (with duplicates). 
SELECT name FROM customers
UNION ALL
SELECT name FROM suppliers;

--5. Get cities from customers that are also present in the supplier list (simulate INTERSECT). 
SELECT name FROM customers
INTERSECT 
SELECT name FROM suppliers;
--6. Get cities from customers that are not in the supplier list (simulate EXCEPT).
SELECT DISTINCT city
FROM customers
WHERE city NOT IN (SELECT city FROM suppliers);

--7. Combine names of customers and suppliers whose city is "Dhaka". 
SELECT c.name AS customer_name, s.name AS supplier_name
FROM customers c
JOIN suppliers s ON c.city = s.city
WHERE c.city = 'Dhaka';
--

8. Combine all cities from all three tables (customers, suppliers, and orders using 
customer’s city). 
SELECT city FROM customers
UNION
SELECT city FROM suppliers
UNION
SELECT c.city
FROM orders o
JOIN customers c ON o.customer_id = c.customer_id;
--9 
-- Step 1: Get customers from cities that are NOT shared with suppliers
SELECT name, city, 'Customer' AS type
FROM customers
WHERE city NOT IN (SELECT DISTINCT city FROM suppliers)

UNION

-- Step 2: Get suppliers from cities that are NOT shared with customers
SELECT name, city, 'Supplier' AS type
FROM suppliers
WHERE city NOT IN (SELECT DISTINCT city FROM customers);

-- Show names of customers and suppliers where name starts with 'S'. 
select name as customer_name,'customer' as type
from customers c 
where name like'S%'

union

select name as supplier_name, 'supplier' as type
from suppliers 
where name like 'S%' ;
--11. Find all common cities in both customers and suppliers.
 SELECT city FROM customers
INTERSECT
SELECT city FROM suppliers;

--12. Get customer names whose cities exist in the supplier table. 
SELECT name AS customer_name, city
FROM customers
WHERE city IN (SELECT city FROM suppliers);

--13. Get supplier names whose cities are not in the customer table.
select name as customer_name, city
from customers 
where city not in (select city from suppliers);
---14. Show cities that are unique to customers only. 
select distinct city
from customers  
where city not in (select city from suppliers);
--15. List all customer IDs and supplier IDs where the city is the same. 
select c.customer_id, s.supplier_id
from customers c
join suppliers s on s.city=c.city;
--16. Find customer names who placed orders (based on orders table). 
select c.name as customer_name, o.order_id, o.amount as order_amount
from customers c
join orders o on c.customer_id=o.customer_id ;
--17. Find customers who placed orders and are from a city not listed in suppliers. 
select c.name as customer_name, o.order_id, o.amount as order_amount
from customers c
join orders o on c.customer_id=o.customer_id 
where city not in (select city from suppliers);
--18. Show city names present in all three tables. 
select distinct city
from customers c 
intersect
select distinct city
from suppliers 
INTERSECT 
select c.city
from orders o
join customers c on c.customer_id=o.customer_id ;

--19. Show city names only present in suppliers, not in customers or orders. 
select city
from suppliers 
where city not in (select city from customers suppliers 
union select c.city
from orders o
join customers c on c.customer_id=o.customer_id );
--20. Combine supplier and customer names who live in cities that have orders over 1000
select c.name as customer_name, s.name as supplier_name, o.amount as order_amount
from customers c
join suppliers s on c.city=s.city
join orders o on c.customer_id=o.customer_id
where o.amount > 1000 ;

--31 Get names of customers who are not from any city in the supplier list. 
select 
	name as customer_name,
	city
	from customers
	where city not in (select city from suppliers) ;
SELECT 
    c.name AS customer_name,
    c.city
FROM 
    customers c
LEFT JOIN 
    suppliers s ON c.city = s.city
WHERE 
    s.city IS NULL;

       
--32. Get names of suppliers who are also customers (assume some names match). 

--33. Get cities that appear in at least two of the three tables. 
SELECT city
FROM (
    SELECT city FROM customers
    UNION ALL
    SELECT city FROM suppliers
    UNION ALL
    SELECT c.city 
    FROM orders o
    JOIN customers c ON o.customer_id = c.customer_id
) all_cities
GROUP BY city
HAVING COUNT(*) >= 2;


--34. Get cities that appear in only one table. 
-- Cities only in customers
SELECT DISTINCT city FROM customers
WHERE city NOT IN (
    SELECT city FROM suppliers
    UNION
    SELECT c.city FROM orders o JOIN customers c ON o.customer_id = c.customer_id
)
UNION

-- Cities only in suppliers
SELECT DISTINCT city FROM suppliers
WHERE city NOT IN (
    SELECT city FROM customers
    UNION
    SELECT c.city FROM orders o JOIN customers c ON o.customer_id = c.customer_id
)
UNION

-- Cities only in orders (via customers)
SELECT DISTINCT c.city
FROM orders o
JOIN customers c ON o.customer_id = c.customer_id
WHERE c.city NOT IN (
    SELECT city FROM customers
    UNION
    SELECT city FROM suppliers
);

--35. List all cities and the number of times they appear in all tables. 
SELECT city, COUNT(*) AS number_of_times
FROM (
    SELECT city FROM customers
    UNION ALL
    SELECT city FROM suppliers
    UNION ALL
    SELECT c.city 
    FROM orders o 
    JOIN customers c ON c.customer_id = o.customer_id
) all_city
GROUP BY city;

36. --List customer names who placed orders and are from the same city as a supplier.
 SELECT DISTINCT c.name AS customer_name
FROM customers c
JOIN orders o ON c.customer_id = o.customer_id
JOIN suppliers s ON c.city = s.city;

--37. Show all customer names who didn’t order and aren’t from supplier cityes. 
--37. Show all customer names who didn’t order and aren’t from supplier cityes. 
SELECT name AS customer_name, city
FROM customers
WHERE customer_id NOT IN (SELECT customer_id FROM orders)
  AND city NOT IN (SELECT city FROM suppliers);
--38. Use IN to simulate INTERSECT: get all cities in both customers and suppliers.
select distinct city
from customers c 
where city in (select city from suppliers);
--39. Use NOT IN to simulate EXCEPT: get all customers not in supplier cities. 
select name as customer_name from customers 
where city not in (select city from suppliers s );

--40.  List all customers from cities that suppliers are not located in.
SELECT name AS customer_name, city
FROM customers
WHERE city NOT IN (SELECT city FROM suppliers);
--41.  List all names (customer and supplier) where the city is common
SELECT name FROM customers WHERE city IN (SELECT city FROM suppliers)
UNION
SELECT name FROM suppliers WHERE city IN (SELECT city FROM customers);
--42. Find duplicate cities across customers and suppliers
select distinct city
from customers c 
where city in (select city from suppliers s );
