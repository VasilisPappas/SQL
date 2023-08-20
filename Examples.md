## Examples

 READY?
 
From now on, we have our database that we can manipulate. Below are some examples implemented using this database:

## 1) I want the content of all tables:
 `select *
 from companies;`

`select *
 from artists;`
> Keep in mind that in both the 'albums' and 'songs' tables, exist null values in the 'release_year', 'country', and 'price' fields, respectively.

 `select * 
 from albums;`

> 'price' column, refers to the cost of each song through digital sales.

 `select * 
 from songs;`

 ## 2) Viewing the oldest album (obviously not null instances selected):
<pre>select * 
from albums
where release_year is not null
order by release_year
limit 1;</pre>

## 3) Viewing the name of the albums and the corresponding release year from oldest to newest (obviously not null instances selected):
<pre>select name as 'name of the album', release_year 
from albums
where release_year is not null
order by release_year;</pre>
> 'name of the album' is an alias. Alternatively we can ignore 'as'.

## 4) Calculate the number of the albums per year (obviously not null instances selected). Also rename the result as 'number of albums':

<pre>select release_year, count(*) 'number of albums' 
from albums
group by release_year
having release_year is not null;</pre>

> The use of a 'where' statement is not permissible after a 'group by' clause. Instead, the 'having' clause is utilized for this purpose.

> Additionally, rather than applying 'count(*)', we can use 'count(id)', where 'id' signifies the primary key of the respective table.

## 5) Which artists (alphabetically) have released albums? Get also the corresponding albums, along with year of release:
> Here is the case of using 2 tables via 'join'.
<pre>select art.name Artist, alb.name Album, alb.release_year
from artists art
join albums alb on art.id=alb.artist_id
order by Artist;</pre>

> Joins are used to combine columns from two tables based on matching values in a field shared by both tables.
> Something interesting is that the above query shows 'null' for the 'release_year' for some rows. But, I am comfortable with the existence of null values in the 'release_year' column, as I have allowed their presence due to the structure of the table.
> Therefore, a join between both tables entails fetching corresponding details from columns that were specifically designed to not allow null values.

> Aliases for tables simplify column referencing, improving readability. In the instance provided, using aliases distinguishes 'name' columns from different tables. I find aliases beneficial even when unnecessary (like 'release_year'), as they quickly clarify column origins.

>  Also, 'join' is the same with 'inner join'.

OR, using a  subquery:
<pre>select name
from artists
where id in ( select artist_id
              from albums)
order by name;</pre>
> If I were to include 'select name, albums.name' in my outer query, it would not function as intended, because we are restricted to viewing columns only from the outer table 'artists'.
## 6) Viewing the 5th newest album:
<pre>select name, release_year
from albums 
order by release_year desc
limit 4,1;</pre>
> It is pointless to state that we do want non null values, cause desceding order starts with non null values.
## 7) Viewing the 8th and 9th newest albums:
<pre>select name, release_year
from albums 
order by release_year desc
limit 7,2;</pre>
## 8) How can I see if there are any artists with no released albums?
<pre>
select art.name Artist, albums.name Album
from artists art
left join albums on art.id = albums.artist_id;</pre>
> A 'left join' displays all records from the left table along with corresponding records from the right table. In this scenario, we are considering all the artists and showcasing album names from the right side, even those with no matching album names (which are represented as null values). Alternatively, we can utilize the condition 'where albums.name is null' to exclusively observe artists who haven't released any albums (despite the 'name' column being designated as non-null).

OR,
<pre>select art.name, albums.name
from albums
right join artists art on art.id = albums.artist_id;</pre>

> 'Left outer join' and 'right outer join' are the same with left and right joins.

## 9) Display artists whose names start with the letter 'M':
<pre>
select name
from artists
where name like "M%";
</pre>

## 10) Display the number of songs for each artist, from highest to lowest:
<pre>select  artists.name as Artist,  COUNT(songs.id) as 'Number of Songs'
from artists
join albums on artists.id = albums.artist_id
join songs on albums.id = songs.album_id
group by albums.artist_id
order by count(songs.id) desc;</pre>

> Here, we need to perform a join operation across 3 tables as we require information from 'songs' and 'artists' tables. These two tables need to be connected through a third table, which is the 'albums' table.

*TIP: The correct form of joining 3 tables is as below:*
<pre>SELECT table1.col, table3.col
FROM table1
JOIN table2 ON table1.primarykey = table2.foreignkey
JOIN table3 ON table2.primarykey = table3.foreignkey;</pre>

And NOT by any means, this: 
<pre>SELECT table1.col, table3.col
FROM table1
JOIN table2 ON table1.primarykey = table2.foreignkey
JOIN table3 ON table2.foreignkey = table3.foreignkey;</pre>

## 11) Show for each company which albums are released in alphabetical order:

<pre>select albums.name as Album, companies.name as 'Record Company'
from companies
join artists on companies.id=artists.company_id
join albums on artists.id=albums.artist_id
order by albums.name;</pre>

> The structure of the 4 tables is as following: companies -> artists -> albums -> songs. According to the specific query we need to connect 'companies' and 'albums' and this can be done via 'artists'. So, double joining is required!

## 12) Which is the duration of each album?
<pre>select albums.name as Album, sum(songs.length) as Duration
from albums
join songs on albums.id=songs.album_id
group by albums.id;</pre>
> (Obviously) 'group by songs.album_id' gives the same result... The magic power of '=' :)


## 13) Extract the characteristics of the album with the longest duration:
<pre>select albums.name as Album, artists.name Artist, albums.release_year year, sum(songs.length) as 'Duration of Album'
from artists
join albums on artists.id=albums.artist_id
join songs on albums.id=songs.album_id
group by albums.id
order by sum(songs.length) desc
limit 1;</pre>

Instead, If I wanted to print the maximum duration I could use a subquery:
<pre>select max(album_length) as max_album_length
from (
      select sum(s.length) as album_length
      from albums a
      join songs s on a.id = s.album_id
      group by a.id
) as album_length;</pre>
> I have to admit that I prefer double joins or single joins over subqueries. Additionally, joining tends to be faster (most times) and more efficient when dealing with large datasets.
> However, it's important to understand how to utilize subqueries effectively for optimization purposes and for gaining a better understanding of the underlying procedures in SQL.

## 14) Find the song with the longest duration:
<pre>
select  *
from songs
where length = (select max(length)
                from songs);</pre>

OR,
<pre>select *
from songs
order by length desc
limit 1;</pre>

## 15) Remove the existing database and some tables:

<pre>drop database record_database</pre>

<pre>drop table songs</pre>

<pre>drop table albums</pre>

<pre>drop table artists</pre>

<pre>drop table companies</pre>

> Please exercise caution before proceeding with the next queries, as you must FIRST recreate the database and the deleted tables. Additionally, keep in mind that you need to utilize 'USE DATABASE record_database'...

> EXTREMELY IMPORTANT: Structure of tables: companies -> artists -> albums -> songs. In this arrangement, 'companies' is a parent table for 'artists', while 'songs' is a child table for 'albums'. By default the deletion of a parent table is restricted, as associated data in the child tables must be purged beforehand, a safeguard upheld by SQL.
> There is an available option of using 'ON DELETE CASCADE' to automatically delete the data from child when deleting the parent table, but we won't deal with that.

## 16) It's time to delete some data refering to 'songs' table:
<pre>delete from songs
where id=182;</pre>
> The above statement deletes a row. Make sure to reinsert it to the table 'songs' to proceed to the next queries.

## 17) We need to insert a new info about the budget of each company (float number). Here, this can be done by:

<pre>alter table companies
add column budget float;</pre>

You can also modify any of the columns in a table:

<pre>alter table companies
modify column budget int;</pre>

But, I changed my mind... So I'm gonna remove this column:

<pre>alter table companies
drop column budget;</pre>

> In the 'alter table' statement, 'column' is optional.

## 18) If I want to change some of the info of a row:
 <pre>
 update songs
 set price=10.00
 where id=182;</pre>

 And once again I want to undo what I did so:
 <pre>
 update songs
 set price=2.00
 where id=182;</pre>
 
## 19) Let's say we want to display the names of the companies and all album names in the same column, while in another column, we aim to show the respective years (founded or released).

<pre>select name as 'name of album or company', release_year as 'year of release or foundation'
from albums
union
select name, year_founded
from companies;</pre>

> 'Union' combines rows from different tables while 'join' combines columns. 'Union' is handful when we cannot join tables.
> The number of columns in the 'select' statements must be the same, as well as their types.
## 20) Using a FULL JOIN with MySQL:
 Full join is useful when we want to see the hole view of combined tables. I mean the 'non matches (null values)' from the left and right sides at the same time. But in MySQL this is done via 'union':

<pre>select artists.name Artist,albums.name Album
from artists
left join albums on artists.id=albums.artist_id
union
select artists.name Artist,albums.name Album
from artists
right join albums on artists.id=albums.artist_id;</pre>
 
> This displays all artists and all albums that exist, even if there is an album without an associated artist or an artist without a released album. As a result, we obtain null values in both the left and right columns. BUT, In this case there are no albums without corresponding artists (logical I guess), so it resembles a 'left join'(with respect to Artist) since we only observe null values in the right column of the outcome.

## 21) partition over:
