<p>In this repository, our objective is to explore and utilize various options and capabilities of MySQL using synthetic data. 
These data refer to record companies, artists, albums, and songs. To achieve this, we have established a database named 'record_database'
which encompasses the relevant tables (refer to the file named <a href -"https://github.com/VasilisPappas/SQL/blob/main/Database%20and%20tables"> Database and tables</a>).</p>
Next step is to insert data into the tables. This can be done through the code provided in the file <a href -"https://github.com/VasilisPappas/SQL/blob/main/Inserting%20data"> Inserting data</a>.

## Examples

 READY?
 
From now on, we have our database that we can manipulate. Below are some examples implemented using this database:

### 1) I want the content of all tables:
 `select *
 from companies;`

` select *
 from artists;`
> Keep in mind that in both the 'albums' and 'songs' tables, we have null values in the 'release_year', 'country', and 'price' fields, respectively.

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

