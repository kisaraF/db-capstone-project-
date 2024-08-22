# DB Capstone Project (Little Lemon)

* [ER Diagram](#erd)
* [Optimised Queries](#salesrepo)
    * [GetMaxQuantity()](#getmax)
    * [GetOrderDetail()](#getorder)
    * [CancelOrder()](#cancelorder)
* [Checking Bookings](#bookcheck)
    * [CheckBooking()](#checkbooking)
    * [AddValidBooking()](#addvalid)
* [CRUD Operations](#crud)
    * [AddBooking()](#addbooking)
    * [UpdateBooking()](#updatebooking)    
    * [CancelBooking()](#cancelBooking)
* [Tableau Vizs](#tbviz)
    * [Customers sales](#chart1) 
    * [Profit chart](#chart2) 
    * [Sales Bubble Chart](#chart3) 
    * [Cuisine Sales and Profits](#chart4) 
    * [Dashboard](#chart5)
* [Python DB Client](#pyc)
    * [Connect to MySQL DB](#connect) 
    * [Show Tables](#showt) 
    * [Query with Join](#join1)
* [Justifications](#jus)


## <a name= "erd"></a>ER Diagram
<img width="518" alt="LittleLemonDM" src="https://github.com/user-attachments/assets/3a407eed-b0ae-4798-8782-6274aacdc756">

## <a name= "salesrepo"></a>Optimised Queries

### <a name= "getmax"></a>GetMaxQuantity()
```
CREATE PROCEDURE GetMaxQuantity()
BEGIN
SELECT MAX(Quantity) AS 'Max Quantity In Order' FROM Orders;
```
***Working Proof:***

<img width="478" alt="4" src="https://github.com/user-attachments/assets/5b0e4592-d37a-4fce-ba61-2ad0fd15f3a3">

### <a name= "getorder"></a>GetOrderDetail()
```
PREPARE GetOrderDetail FROM 'SELECT c.CustomerID, c.CustomerName, o.OrderID, o.Quantity, o.TotalCost 
FROM Customer c 
JOIN Bookings b ON c.CustomerID = b.CustomerID 
JOIN Orders o ON o.BookingID = b.BookingID 
WHERE c.CustomerID = ?';
```
***Working Proof:***

<img width="947" alt="5" src="https://github.com/user-attachments/assets/61d722b0-d1f3-4dd2-adc6-e111fd35d96d">

### <a name= "cancelorder"></a>CancelOrder()
```
CREATE PROCEDURE CancelOrder (IN order_id INT)
BEGIN
DELETE FROM Orders WHERE OrderID = order_id;
END//
```
***Working Proof:***

<img width="430" alt="6" src="https://github.com/user-attachments/assets/f15d774a-ab1e-476a-8e35-d84d90d929f2">

## <a name= "bookcheck"></a>Checking Bookings

### <a name= "checkbooking"></a>CheckBooking()
```
CREATE PROCEDURE CheckBooking(IN booking_date DATE, IN table_no INT)
BEGIN
DECLARE table_status VARCHAR(50);
SELECT COUNT(*) INTO @row_count
FROM Bookings
WHERE Date = booking_date AND TableNumber = table_no;
IF (@row_count > 0) THEN SET table_status = CONCAT('Table ', table_no, ' is already booked');
ELSE SET table_status = CONCAT('Table ', table_no, ' is available');
END IF;
SELECT table_status AS 'Booking Status';
END//
```
***Working Proof:***

<img width="942" alt="7" src="https://github.com/user-attachments/assets/a4993866-05f6-47db-af2e-2914bba6b447">

### <a name= "addvalid"></a>AddValidBooking()
```
CREATE PROCEDURE AddValidBooking(IN booking_date DATE, IN table_no INT, IN customer_id VARCHAR(5), IN emp_id VARCHAR(5))
BEGIN
START TRANSACTION;
SELECT COUNT(*) INTO @row_count FROM Bookings WHERE Date = booking_date AND TableNumber = table_no;
IF (@row_count > 0) THEN ROLLBACK; SELECT CONCAT('Table ', table_no, ' is already booked- booking cancelled') AS "Message"; 
ELSE INSERT INTO Bookings (Date, TableNumber, CustomerID, EmployeeID) VALUES (booking_date, table_no, customer_id, emp_id); COMMIT; SELECT CONCAT('Table ', table_no, ' reserved') AS "Message";
END IF;
END //
```
***Working Proof:***

<img width="943" alt="9" src="https://github.com/user-attachments/assets/42783655-6618-4161-af69-df7e79f19622">

## <a name= "crud"></a>CRUD operations

### <a name= "addbooking"></a>AddBooking()
```
CREATE PROCEDURE AddBooking(IN booking_id INT, IN date DATE, IN table_number INT, IN customer_id VARCHAR(5), IN emp_id VARCHAR(5))
BEGIN
INSERT INTO Bookings (BookingID, Date, TableNumber, CustomerID, EmployeeID) VALUES (booking_id, date, table_number, customer_id, emp_id);
SELECT "New Booking Added" AS Confirmation;
END//
```
***Working Proof:***

<img width="943" alt="10" src="https://github.com/user-attachments/assets/033cdf68-05ca-4453-9306-48f81fa586c8">

### <a name= "updatebooking"></a>UpdateBooking()
```
CREATE PROCEDURE UpdateBooking(IN booking_id INT, IN date DATE)
BEGIN
UPDATE Bookings SET Date = date WHERE BookingID = booking_id;
SELECT CONCAT('Booking ', booking_id, ' updated') AS Confirmation;
END//
```
***Working Proof:***

<img width="565" alt="11" src="https://github.com/user-attachments/assets/e402e928-1889-4199-8e33-6f97802a5b44">

### <a name= "cancelBooking"></a>CancelBooking()
```
CREATE PROCEDURE CancelBooking(IN booking_id INT)
BEGIN
DELETE FROM Bookings WHERE BookingID = booking_id;
SELECT CONCAT('Booking ', booking_id, ' cancelled') AS Confirmation;
END//
```
***Working Proof:***

<img width="564" alt="12" src="https://github.com/user-attachments/assets/87b7b4f9-062f-4d9b-92c1-70a0eabd9d2c">

## <a name= "tbviz"></a>Tableau Vizs

### <a name= "chart1"></a>Customers sales
<img width="790" alt="V1" src="https://github.com/user-attachments/assets/09ce7770-fd1e-4089-81d4-a180f44c1252">

### <a name= "chart2"></a>Profit chart
<img width="791" alt="V2" src="https://github.com/user-attachments/assets/496c0e9e-120a-46fb-a9e7-aa2288368698">

### <a name= "chart3"></a>Sales Bubble Chart
<img width="792" alt="V3" src="https://github.com/user-attachments/assets/e2d63c4c-0462-4884-b3dd-b4bee06850bd">

### <a name= "chart4"></a>Cuisine Sales and Profits
<img width="787" alt="V4" src="https://github.com/user-attachments/assets/fd1a6471-50d8-4bbf-8993-dacfc445adfa">

### <a name= "chart5"></a>Dashboard
<img width="772" alt="V5" src="https://github.com/user-attachments/assets/8ecc0ba2-61c3-4dde-a05d-1d65437d68db">

## <a name= "pyc"></a>Python DB Client

### <a name= "connect"></a>Connect to MySQL DB
<img width="509" alt="D1" src="https://github.com/user-attachments/assets/3636bd40-48fd-468d-baf5-a7baf8877b8b">

### <a name= "showt"></a>Show Tables
<img width="236" alt="D2" src="https://github.com/user-attachments/assets/261cab17-1cc9-45aa-826b-114b2001a7ee">

### <a name= "join1"></a>Query with Join
<img width="389" alt="D3" src="https://github.com/user-attachments/assets/bb826d17-9483-445d-84fa-1034ae181255">

## <a name= "jus"></a>Justifications
* Every task assigned by the capstone project throughout each module has been included here
* ERD tables are designed as per course requirements in module 01 exercise
<img width="574" alt="Just 1" src="https://github.com/user-attachments/assets/873d2a89-180c-4736-a654-beeeda487002">

* Function outputs may not give the same output since the data used to populate is different as the course did only gave a dataset for visualizations in module 3 (SQL Script and Data used to populate are included as well )
* Some functions like `ManageBooking()` were mentioned only at module 04. Maybe it's a naming mistake where they mention `CheckBooking()` as `ManageBooking()`. 
* #### Thank you for reviewing