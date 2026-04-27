
-- 1) Retrieve all books in the "Fiction" genre ?
SELECT * FROM books 
WHERE Genre = 'Fiction';

-- 2) Find books published after the year 1950 ?
SELECT * FROM books
WHERE Published_Year > 1950;

-- 3) List all customers from the Canada ?
SELECT * FROM customers 
WHERE Country = 'Canada';

-- 4) Show orders placed in November 2023 ?
SELECT * FROM orders
WHERE Order_Date BETWEEN '2023-11-01' AND '2023-11-30';

-- 5) Retrieve the total stock of books available ?
SELECT SUM(Stock) AS TOTAL_STOCK FROM books;

-- 6) Find the details of the most expensive book ?
SELECT * FROM Books ORDER BY Price DESC limit 1;

-- 7) Show all customers who ordered more than 1 quantity of a book ?
SELECT * FROM Customers AS C
JOIN Orders AS O ON C.Customer_ID = O.Customer_ID
WHERE O.Quantity > 1;

-- 8) Retrieve all orders where the total amount exceeds $20 ?
SELECT * from Orders
WHERE Total_Amount > 20;

-- 9) List all genres available in the Books table ?
SELECT DISTINCT Genre FROM Books;

-- 10) Find the book with the lowest stock ?
SELECT * FROM books ORDER BY Stock limit 1;

-- 11) Calculate the total revenue generated from all orders ?
SELECT SUM(Total_Amount) AS Total_Revenue FROM Orders;

-- 12) Retrieve the total number of books sold for each genre ?
SELECT b.Genre, SUM(o.Quantity) AS Total_Qty FROM books b 
JOIN Orders o ON b.Book_ID = o.Book_ID
GROUP BY b.Genre;

-- 13) Find the average price of books in the "Fantasy" genre ?
SELECT Genre, AVG(Price) AS Average_Price
FROM books WHERE Genre = 'Fantasy';

-- 14) List customers who have placed at least 2 orders ?
SELECT c.name, COUNT(o.customer_id) AS Orders_Count
FROM customers c JOIN orders o ON
c.customer_id = o.customer_id
GROUP BY c.name
HAVING COUNT(o.customer_id) >= 2;

-- 15) Find the most frequently ordered book ?
SELECT b.title, COUNT(o.book_id) AS Total_Orders
FROM books b JOIN orders o ON
b.book_id = o.book_id
GROUP BY b.title ORDER BY Total_Orders desc;

-- 16) Show the top 3 most expensive books of 'Fantasy' Genre ?
SELECT title, Genre, Price AS Book_Price
FROM books WHERE Genre = 'Fantasy'
group by title, Genre, Price ORDER BY Book_Price desc limit 3;

-- 17) Retrieve the total quantity of books sold by each author ?
SELECT b.author, SUM(o.quantity) AS Total_Qty_Sold
FROM books b JOIN Orders o ON
b.book_id = o.book_id
GROUP BY b.author ORDER BY Total_Qty_Sold desc;

-- 18) List the cities where customers who spent over $30 are located ?
SELECT  c.city, SUM(o.total_amount) AS Total_Amount FROM customers c 
JOIN orders o ON c.customer_id = o.customer_id
GROUP BY c.city HAVING SUM(o.total_amount) > 30;

-- 19) Find the top 10 customer who spent total amount ?
SELECT c.name, SUM(o.total_amount) AS Total_Spent
FROM customers c JOIN orders o ON
c.customer_id = o.customer_id
GROUP BY c.name ORDER BY Total_Spent DESC limit 10;

-- 20)  Calculate the stock remaining after fulfilling all orders ?
SELECT b.book_id, b.title, b.stock,
COALESCE(SUM(o.quantity),0) AS order_Qty,
b.stock - COALESCE(SUM(o.quantity),0) AS Remaining_Qty
FROM books b
LEFT JOIN orders o ON b.book_id = o.book_id
GROUP BY b.book_id, b.title, b.stock;
