# java-DB-AdvancedSQL-join-excercises

## Join excercises




### 1 Select all product names for suppliers from USA.
```sql
SELECT pr.product_name, pr.supplier_id, su.country  from products pr
JOIN suppliers su ON pr.supplier_id = su.supplier_id
WHERE su.country = 'USA'
```
###  2 Select order id, order date, employee first and last name for employee whose last name starts with 'D' and for order that was shipped to Germany.
```sql
SELECT o.order_id, o.order_date, e.first_name AS employee_first_name, e.last_name AS employee_last_name, o.ship_country
FROM orders o
JOIN employees e ON o.employee_id = e.employee_id
JOIN customers c ON o.customer_id = c.customer_id
WHERE e.last_name LIKE 'D%' AND o.ship_country = 'Germany';

```
### 3 Select all orders shipped by United Package for orders sooner than '1996-08-12'
```sql
SELECT o.order_id, o.order_date, s.company_name 
FROM orders o
JOIN shippers s ON o.Ship_Via = s.shipper_id
WHERE s.company_name = 'United Package'AND o.shipped_date < '1996-08-12';

```
### 4 Select shippers company name that doesn't have any orders assigned.
```sql
SELECT s.company_name
FROM shippers s
LEFT JOIN orders o ON s.shipper_id = o.Ship_Via
WHERE o.Ship_Via IS NULL;

```
### 5 Select order ids that contains product from category 'Beverages' ordered '1996-08-14'.
```sql
SELECT DISTINCT o.order_id, c.category_name, o.order_date
FROM orders o
JOIN order_details od ON o.order_id = od.order_id
JOIN products p ON od.product_id = p.product_id
JOIN categories c ON p.category_id = c.category_id
WHERE c.category_name = 'Beverages'AND o.order_date = '1996-08-14';

```
### 6 Select product names and availability of products supplied by 'Lyngbysild' from category 'Seafood'
```sql
SELECT p.product_name, p.units_in_stock
FROM products p
JOIN suppliers s ON p.supplier_id = s.supplier_id
JOIN categories c ON p.category_id = c.category_id
WHERE s.company_name = 'Lyngbysild' AND c.category_name = 'Seafood';

```
### 7 Select last names of employees assigned to 'Northern' region.
```sql
SELECT e.last_name
FROM employees e
JOIN employee_territories et ON e.employee_id = et.employee_id
JOIN territories t ON et.territory_id = t.territory_id
JOIN region r ON t.region_id = r.region_id
WHERE r.region_description = 'Northern';

```
### 8 Select employee id and his boss last name for those who have orders with date later than '1996-07-18'. if an employee doesn't have a boss don't include him.
```sql
SELECT e.employee_id,   e2.last_name AS boss_last_name
FROM employees e
JOIN orders o ON e.employee_id = o.employee_id
LEFT JOIN employees e2 ON e.reports_to = e2.employee_id
WHERE o.order_date > '1996-07-18' AND e.reports_to IS NOT NULL;
```
### 9 List the employees in the warehouse with orders that are not shipped yet.
```sql
SELECT
    e.employee_id, e.first_name, e.last_name,o.order_id, o.order_date
FROM employees e
JOIN orders o ON e.employee_id = o.employee_id
JOIN shippers s ON o.ship_via = s.shipper_id
WHERE o.shipped_date IS NULL;
```
### 10 Calculate the price for each product with its name in each order after discount is applied1
```sql
SELECT
    o.order_id, p.product_name, od.unit_price, od.quantity, od.discount,
   (od.unit_price * od.quantity * (1 - od.discount)) AS discounted_price
FROM orders o
JOIN order_details od ON o.order_id = od.order_id
JOIN products p ON od.product_id = p.product_id;
```
