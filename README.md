# DB Capstone Project (Little Lemon)

* [ER Diagram](#erd)
* [Stored Procedures](#stp)
    * [GetMaxQuantity()](#getmax)
    * [CheckBooking()](#checkbooking)
    * [AddBooking()](#addbooking)
    * [UpdateBooking()](#updatebooking)
    * [CancelOrder()](#cancelorder)
    * [CancelBooking()](#cancelBooking)
    * [AddValidBooking()](#addvalid) 
* [Tableau Vizs](#tbviz)
    * [Customers sales](#chart1) 
    * [Profit chart](#chart2) 
    * [Sales Bubble Chart](#chart3) 
    * [Cuisine Sales and Profits](#chart4) 
    * [Dashboard](#chart5) 


## <a name= "erd"></a>ER Diagram
<img width="518" alt="LittleLemonDM" src="https://github.com/user-attachments/assets/3a407eed-b0ae-4798-8782-6274aacdc756">

## <a name= "stp"></a>Stored Procedures

### <a name= "getmax"></a>GetMaxQuantity()
```
CREATE PROCEDURE GetMaxQuantity()
BEGIN
SELECT MAX(Quantity) AS 'Max Quantity In Order' FROM Orders;
```
