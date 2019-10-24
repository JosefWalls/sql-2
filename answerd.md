PRACTICE JOINS
-------------
1) 
SELECT * FROM invoice_line
WHERE unit_price > 0.99
2)
SELECT i.invoice_date, c.first_name, c.last_name, i.total
FROM customer AS c
INNER JOIN invoice AS i ON c.customer_id = i.customer_id
3)
SELECT c.last_name, c.last_name, e.first_name, e.last_name
FROM customer AS C
INNER JOIN employee AS e ON c.support_rep_id = e.employee_id
4)
SELECT a.title, b.name
FROM album AS a
INNER JOIN artist AS b ON a.artist_id = b.artist_id
5)
SELECT pt.track_id
FROM playlist_track pt
JOIN playlist p ON p.playlist_id = pt.playlist_id
WHERE p.name = 'Music'
6)
SELECT t.name
FROM track t
JOIN playlist_track p ON p.track_id = t.track_id
WHERE p.playlist_id = 5;
7)
SELECT t.name
FROM track t
JOIN playlist_track pt ON t.track_id = pt.track_id
JOIN playlist p ON p.playlist_id = pt.playlist_id
8)
SELECT t.name
FROM track t
JOIN album AS a ON a.album_id = t.album_id
JOIN genre AS g ON g.genre_id = t.genre_id
WHERE g.name = 'Alternative & Punk'


BLACK DIAMOND)
Get all tracks on the playlist(s) called Music and show their name, genre name, album name, and artist name. 
At least 5 joins.





PRACTICE NESTED QUERIES
----------------------------
1)
SELECT * FROM invoice
WHERE invoice_id IN (SELECT invoice_id FROM invoice_line WHERE unit_price > 0.99)
2)
SELECT * FROM playlist_track
WHERE playlist_id IN (SELECT playlist_id FROM playlist WHERE name = 'Music')
3)
SELECT name FROM track
WHERE track_id IN (SELECT track_id FROM playlist_track WHERE playlist_track.playlist_id= 5)
4)
SELECT * FROM track
WHERE genre_id IN (SELECT genre_id FROM genre WHERE name='Comedy')
5)
SELECT * FROM track
WHERE album_id IN (SELECT album_id FROM album WHERE title='Fireball')
6)
SELECT * FROM track
WHERE album_id IN (SELECT album_id FROM album WHERE artist_id IN (
SELECT artist_id FROM artist WHERE name = 'Queen'))




PRACTICE UPDATING ROWS
-----------------------
1)
Find all customers with fax numbers and set those numbers to null.

2)
UPDATE customer
SET company = 'Self'
WHERE company IS null
3)
UPDATE customer 
SET last_name = 'Thompson'
WHERE first_name = 'Julia'
4)
UPDATE customer
SET support_rep_id = 4
WHERE email = 'luisrojas@yahoo.cl'
5)
UPDATE track
SET composer = 'The darkness around us'
WHERE genre_id IN (SELECT genre_id FROM genre WHERE name = 'Metal')
AND composer IS null




GROUP BY
----------------------
1)
SELECT COUNT (*), g.name
FROM track t
JOIN genre g ON t.genre_id = g.genre_id
GROUP BY g.name
2)
SELECT COUNT (*), g.name
FROM track t
JOIN genre g ON t.genre_id = g.genre_id
WHERE g.name = 'Pop' OR g.name='Rock'
GROUP BY g.name
3)
SELECT ar.name, COUNT(*)
FROM artist ar
JOIN album ab ON ab.artist_id = ar.artist_id
GROUP BY ar.name


USE DISTINCT
-------------
1)
SELECT DISTINCT composer FROM track
2)
SELECT DISTINCT billing_postal_code FROM invoice
3)
SELECT DISTINCT company FROM customer



DELETE ROWS
----------------

1)
DELETE
FROM practice_delete
WHERE type = 'bronze'
2)
Delete all 'silver' entries from the table.
DELETE FROM practice_delete
WHERE type = 'silver'
3)
Delete all entries whose value is equal to 150.
DELETE FROM practice_delete
WHERE value = 150














eCommerce
---------------
CREATE TABLE users (users_id SERIAL PRIMARY KEY, name VARCHAR(100), email VARCHAR(250))
CREATE TABLE products (products_id SERIAL PRIMARY KEY, name VARCHAR(100), price INTEGER)
CREATE TABLE orders (order_id SERIAL PRIMARY KEY,  products_id INT REFERENCES products(products_id), users_id INT references users(users_id)

------------------------------------------------------------
INSERT INTO users (name, email)
VALUES ('Josef', 'Joedawg@gmaill'),
	   ('Jaden', 'Dorkus420@gmail.com'),
       ('Hailey', 'SecondMom@gmail.com')

INSERT INTO products (name, price)
VALUES ('Toy', 0),
	   ('Crushed Soda Can', 25),
       ('Used sisccors', 15)
------------------------------------------------------------
Get all products for the first order.

SELECT p.name, p.price, o.order_id
FROM products p
JOIN orders as o On p.products_id = o.products_id
WHERE o.order_id = 1

Get all orders.

SELECT * FROM orders

Get the total cost of an order ( sum the price of all products on an order ).

SELECT SUM(p.price)
FROM orders o
JOIN products AS p ON o.products_id = p.products_id
WHERE o.order_id = 2

-----------------------------------------------------------add foreign key--------
ALTER TABLE orders
ADD COLUMN users_id INT REFERENCES users(users_id)

-----------------------------------------------------------------------
Get all orders for a user.


Get how many orders each user has.
