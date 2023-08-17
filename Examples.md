## Examples

 READY?
 
From now on, we have our database that we can manipulate. Below are some examples implemented using this database:

## 1) I want the content of all tables:
 `select *
 from companies;`

` select *
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
> 'name of the album' is an alias making more specific the name we want to view. Alternatively we can ignore 'as'.

## 4) Calculate the number of the albums per year (obviously not null instances selected). Also rename the result as 'number of albums':

<pre>select release_year, count(*) 'number of albums' 
from albums
group by release_year
having release_year is not null;</pre>

> 'where' statement cannot be applied after 'group by'. Instead, usage of 'having' is provided.

> Also, instead of 'count(*)' we can use 'count(id)', where id is the primary key of the corresponding table.

## 5) Which artists have released albums? Get also the corresponding albums:

<pre>select art.name Artist, alb.name Album
from artists art
join albums alb on art.id=alb.artist_id;</pre>

> Aliases for tables simplify column referencing, improving readability. In the instance provided, using aliases distinguishes 'name' columns from different tables. I find aliases beneficial even when unnecessary, as they quickly clarify column origins.

> Also, 'join' is the same with 'inner join'.

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
<pre> SELECT art.name, albums.name
FROM artists art
left JOIN albums ON art.id = albums.artist_id;</pre>
> A "LEFT JOIN" displays all the records from the left table, along with matching records from the right table. In this context, we are looking at all the artists, and on the right side, we're showing the names of albums, including those with null values. As an alternative approach, we could include the condition "WHERE albums.name IS NULL" to specifically see only those artists who haven't released any albums.

 OR,
 <pre>SELECT art.name, albums.name
FROM albums
right JOIN artists art ON art.id = albums.artist_id;</pre>

> 'Left outer join' and 'right outer join' are the same with Left and right joins.

## 9) Display artists whose names start with the letter 'M':
<pre>
 select name
 from artists
 where name like "M%";
</pre>

