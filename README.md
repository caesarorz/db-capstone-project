# Meta Capstone Project


<img src="image/db1_booking_littlelemon2.JPG"
     alt="Markdown Monster icon"
     style="float: left; margin-right: 10px;" />



# Inserting data

```
INSERT INTO staff (StaffID, Role, Salary) VALUES
(1,"Manager", 7000),
(2, "Assistant ", 4500),
(3, "Head Chef", 6500.30),
(4, "Assistant  ", 3500.20),
(5, "Assistant", 3500.20),
(6, "Assistant", 3500.20);
```

```
INSERT INTO costumer (CostumerID, Name, ContactNumber) VALUES
(1,"Rose Hugh", 4352021),
(2, "Natiel Ferdison", 4334674),
(3, "Clarck Hudson", 4334674),
(4, "Yaburn Trabar", 4334674),
(5, "Rafael Montolla", 4334645),
(6, "Yessica Thouk", 2334674),
(7, 'Anna Iversen', 1235678),
(8, "Joakim Iversen", 2342389),
(9, 'Vanessa McCarthy', 4567834),
(10, 'Marcos Romero', 4128745),
(11, 'Hiroki Yamane', 2672309),
(12, 'Diana Pinto', 51245674);
```

No order assigned yet
```
INSERT INTO bookings (BookingID, Date, TableNo, CostumerID, StaffID) VALUES
(1, "2-11-23", 2, 4, 2),
(2, "2-11-23", 2, 4, 2),
(3, "2-11-23", 4, 3, 5),
(4, "2-11-23", 6, 4, 6);
```

```
INSERT INTO menuitem (MenuItemID, Name, Type, Price) VALUES
(1, 'Olives','Starters',5),
(2, 'Flatbread','Starters', 5.15),
(3, 'Minestrone', 'Starters', 8.85),
(4, 'Tomato bread','Starters', 8),
(5, 'Falafel', 'Starters', 7),
(6, 'Hummus', 'Starters', 5),
(7, 'Greek salad', 'Main Courses', 15.05),
(8, 'Bean soup', 'Main Courses', 12),
(9, 'Pizza', 'Main Courses', 15),
(10, 'Greek yoghurt','Desserts', 7.75),
(11, 'Ice cream', 'Desserts', 6),
(12, 'Cheesecake', 'Desserts', 4),
(13, 'Athens White wine', 'Drinks', 25),
(14, 'Corfu Red Wine', 'Drinks', 30.55),
(15, 'Turkish Coffee', 'Drinks', 10),
(16, 'Turkish Coffee', 'Drinks', 10.00),
(17, 'Kabasa', 'Main Courses', 17.05);
```

```
INSERT INTO orderdeliverystatus
(OrderDeliveryStatusID, StatusName)
VALUES
(1, "In Progress"),
(2, "Not placed"),
(3, "Not delivered"),
(4, "Delivered");
```


```
insert into orderdelivery (OrderDeliveryID, Date, OrderDeliveryStatusID)
VALUES (3, "2-11-23", 1);
```

```
insert into menu (MenuID, MenuItemID)
VALUES
(1,1),
(2,1),
(3,3);
```

```
INSERT INTO orders (
OrderID,
Date,
Quantity,
Cost,
OrderDeliveryID,
MenuID,
BookingID,
CustomerID
) VALUES
(3, "2-11-23", 3, 95.45, 3, 3, 1, 4),
(2, "2-11-23", 2, 40.05, 1, 2, 1, 2),
(1, "2-11-23", 3, 115.55, 1, 3, 1, 6),
(4, "2-11-23", 4, 95.45, 4, 4, 2, 5)
```

```
insert into orderdelivery
(OrderDeliveryID,Date,DeliveryStatusID)
values
(1, "2-11-23", 1),
(2, "2-11-23", 1),
(4, "2-11-23", 1)
```

# Part 1


## Task 1
In the first task, Little Lemon need you to create a virtual table called OrdersView that focuses on OrderID, Quantity and Cost columns within the Orders table for all orders with a quantity greater than 2.

Here’s some guidance around completing this task:

1. Use a CREATE VIEW statement.
2. Extract the order id, quantity and cost data from the Orders table.
3. Filter data from the orders table based on orders with a quantity greater than 2.

You can query the OrdersView table using the following syntax:



```
CREATE VIEW OrdersView AS select OrderID, Quantity, Cost from orders
```

```
Select * from OrdersView;
```


## Task 2
For your second task, Little Lemon need information from four tables on all customers with orders that cost more than $150. Extract the required information from each of the following tables by using the relevant JOIN clause:

1. Customers table: The customer id and full name.
2. Orders table: The order id and cost.
3. Menus table: The menus name.
4. MenusItems table: course name and starter name.
5. The result set should be sorted by the lowest cost amount.


```
select c.CostumerID, c.Name, o.OrderID, o.Cost, m.Name as MenuName, i.Name as Course
from costumer c
inner join orders o
on c.CostumerID = o.CustomerID
inner join menu m
on o.MenuID = m.MenuID
inner join menu_has_menuitem h
on m.MenuID = h.menu_MenuID
inner join menuitem i
on i.MenuItemID = h.MenuItem_MenuItemID
where i.Type = "Main Courses"
```


## Task 3
For the third and final task, Little Lemon need you to find all menu items for which more than 2 orders have been placed. You can carry out this task by creating a subquery that lists the menu names from the menus table for any order quantity with more than 2.

Here’s some guidance around completing this task:

1. Use the ANY operator in a subquery
2. The outer query should be used to select the menu name from the menus table.
3. The inner query should check if any item quantity in the order table is more than 2.

Part 1 (my way)
```
select m.Name from orders o
inner join menu m
on m.MenuID = o.MenuID
where o.Quantity > 2
```
```
select * from orders where
Quantity > 2;
```


Solution
```
select Name from menu where
MenuID = ANY (select MenuID from orders where
Quantity > 2)
```



# Part 2

## Task 1
In this first task, Little Lemon need you to create a procedure that displays the maximum ordered quantity in the Orders table.

Creating this procedure will allow Little Lemon to reuse the logic implemented in the procedure easily without retyping the same code over again and again to check the maximum quantity.

```
create procedure getMaxQuantity(OUT maxQuantity INT)
select max(Quantity) INTO maxQuantity from orders;
```

```
call getMaxQuantity(@maxQty)
```

```
select @maxQty
```

## Task 2
In the second task, Little Lemon need you to help them to create a prepared statement called GetOrderDetail. This prepared statement will help to reduce the parsing time of queries. It will also help to secure the database from SQL injections.

The prepared statement should accept one input argument, the CustomerID value, from a variable.

The statement should return the order id, the quantity and the order cost from the Orders table. 

Once you create the prepared statement, you can create a variable called id and assign it value of 1.


```
PREPARE GetOrderDetail FROM "SELECT OrderID, Quantity, Cost FROM orders WHERE CustomerID = ?"
```

```
SET @id = 1;
EXECUTE GetOrderDetail USING @id;
```

## Task 3
Your third and final task is to create a stored procedure called CancelOrder. Little Lemon want to use this stored procedure to delete an order record based on the user input of the order id.

Creating this procedure will allow Little Lemon to cancel any order by specifying the order id value in the procedure parameter without typing the entire SQL delete statement.


```
CREATE PROCEDURE CancelOrder (IN OrderNo INT)
DELETE FROM orders WHERE OrderID = OrderNo
```

```
call CancelOrder(3)
```



