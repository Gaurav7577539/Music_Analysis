-- Q1 Who is the senior most employee based on job title?
-- Solution : select first_name, last_name, title from Employee order by levels desc limit 1;

-- Q2 Which country have the most invoices ?
-- Solution : select billing_country, count(total) from invoice group by billing_country order by count(total) desc limit 1;

-- Q3 what are top 3 values of total invoices?
-- Solution : select total from invoice order by total desc limit 3;

-- Q4 Which city has the best customers? We would like to through a promotional Music Festival in city we made the most money, Write a query that returns one city that has the highest sum of invoice totals.
-- Return both the city name and sum of all invoice totals ?
-- Solution : select billing_city, sum(total) from invoice group by billing_city order by sum(total) desc limit 1;

-- Q5 Who is the best customer? The customer who has spent the most money will be declared the best customer, Write a query that returns the person who has spent most money ?
-- Solution : select c.customer_id, c.first_name, sum(invoice.total) as invoice_total from customer as c join invoice on c.customer_id = invoice.customer_id
   group by c.customer_id, c.first_name order by invoice_total desc limit 1;

-- Q6 Write query to return email, first name, last name & Genre of Rock Music listners, Return your list by alphabetically by email starting with A ?
-- Solution : select distinct c.email, c.first_name, c.last_name from customer as c join invoice on c.customer_id = invoice.customer_id join invoice_line on invoice.invoice_id = invoice_line.invoice_id
   where track_id IN ( SELECT track_id from track join genre on track.genre_id = genre.genre_id where genre.name like "Rock" ) order by email;

-- Q7 Lets invite the artists who have written most rock music in our dataset, Write a query that returns Artist name and total track count of the top 10 rock bands ?
-- Solution : select artist.artist_id, artist.name, count(artist.artist_id) as no_of_songs from track join album on album.album_id = track.album_id join artist on album.artist_id = artist.artist_id
   join genre on track.genre_id = genre.genre_id where genre.name like "Rock" group by artist.artist_id, artist.name order by no_of_songs desc limit 10;

-- Q8 Return all the track names that have a song length longer than the average song length, Return the name and Milliseconds for each track, Order by the song lenght with the longest songs listed first ?
-- Solution : select name, milliseconds from track where milliseconds > ( select avg(milliseconds) from track ) order by milliseconds desc;

-- Q9 Find how much amount spend by each customer on artists? Write a query to return customer name, artist name and total spent ?
-- Solution : select c.first_name, c.last_name, artist.name as artist_name, sum(invoice_line.unit_price*invoice_line.quantity) as total_spend from customer as c
   join invoice on c.customer_id = invoice.customer_id join invoice_line on invoice.invoice_id = invoice_line.invoice_id join track on invoice_line.track_id = track.track_id
   join album on album.album_id = track.album_id join artist on album.artist_id = artist.artist_id group by c.first_name, c.last_name, artist.name order by total_spend desc;

-- Q10 We want to find out most popular Music Genre for each country, We determine the most popular genre as the genre with the highest amount of purchase. Write a query that returns each country
       along with the top Genre, For countries where the maximum number of purchases is shared return all genres ?
-- Solution : select invoice.billing_country, genre.name as music_name, count(invoice_line.unit_price) as purchase from invoice join invoice_line on invoice.invoice_id = invoice_line.invoice_id
   join track on invoice_line.track_id = track.track_id join album on track.album_id = album.album_id join artist on album.artist_id = artist.artist_id join genre on track.genre_id = genre.genre_id
   group by invoice.billing_country, music_name order by purchase desc;

-- Q11 Write a query that determines the customer that has spent on music for each country, Write a query that returns a country along with the top customer and how much they spent,
       For countries the top amount is shared, provide all customers who spent this amount ?
-- Solution : select customer.first_name, customer.last_name, invoice.billing_country, sum(invoice_line.unit_price*invoice_line.quantity) as total_spend from customer join invoice on customer.customer_id = 
   invoice.invoice_id join invoice_line on invoice.invoice_id = invoice_line.invoice_id group by customer.first_name, customer.last_name, invoice.billing_country order by total_spend desc;
