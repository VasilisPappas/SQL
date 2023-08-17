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

