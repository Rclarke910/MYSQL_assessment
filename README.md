// Add two more movies of your choosing to the table.

mysql> insert into movies (Title,Runtime,Genre,`IMDB Score`,Rating) VALUES ('Starship Troopers',129,'Sci-Fi',7.2,'PG-13'),
    ->  ('Waltz With Bashir',90,'Documentary',8.0,'R'),
    -> ('Spaceballs',96,'Comedy',7.1,'PG'),
    -> ('Monsters Inc',92,'Animation',8.1,'G');                                 Query OK, 4 rows affected (0.01 sec)
Records: 4  Duplicates: 0  Warnings: 0

mysql> select * from movies;
+-------------------+---------+-------------+------------+--------+
| Title             | Runtime | Genre       | IMDB Score | Rating |
+-------------------+---------+-------------+------------+--------+
| Howard the Duck   |     110 | Sci-fi      |        4.6 | PG     |
| Starship Troopers |     129 | Sci-Fi      |        7.2 | PG-13  |
| Waltz With Bashir |      90 | Documentary |        8.0 | R      |
| Spaceballs        |      96 | Comedy      |        7.1 | PG     |
| Monsters Inc      |      92 | Animation   |        8.1 | G      |
+-------------------+---------+-------------+------------+--------+
5 rows in set (0.00 sec)

mysql> insert into movies (Title,Runtime,Genre,`IMDB Score`,Rating) VALUES ('Lavalantula',83,'Horror',4.7,'TV-14');
Query OK, 1 row affected (0.04 sec)

insert into movies (Title,Runtime,Genre,`IMDB Score`,Rating) values ('Finding Nemo',100,'Animation',8.2,'PG');
Query OK, 1 row affected (0.01 sec)

insert into movies (Title,Runtime,Genre,`IMDB Score`,Rating) values ('Scarface',165,'Action',8.3,'R');
Query OK, 1 row affected (0.00 sec)

//Create a query to find all movies in the Sci-Fi genre.

select * from movies where Genre='Sci-fi';
+-------------------+---------+--------+------------+--------+
| Title             | Runtime | Genre  | IMDB Score | Rating |
+-------------------+---------+--------+------------+--------+
| Howard the Duck   |     110 | Sci-fi |        4.6 | PG     |
| Starship Troopers |     129 | Sci-Fi |        7.2 | PG-13  |
+-------------------+---------+--------+------------+--------+
2 rows in set (0.04 sec)

//Create a query to find all films that scored at least a 6.5 on IMDB

select * from movies where `IMDB Score` > 6.4;
+-------------------+---------+-------------+------------+--------+
| Title             | Runtime | Genre       | IMDB Score | Rating |
+-------------------+---------+-------------+------------+--------+
| Starship Troopers |     129 | Sci-Fi      |        7.2 | PG-13  |
| Waltz With Bashir |      90 | Documentary |        8.0 | R      |
| Spaceballs        |      96 | Comedy      |        7.1 | PG     |
| Monsters Inc      |      92 | Animation   |        8.1 | G      |
| Finding Nemo      |     100 | Animation   |        8.2 | PG     |
| Scarface          |     165 | Action      |        8.3 | R      |
+-------------------+---------+-------------+------------+--------+
6 rows in set (0.00 sec)

//For parents who have young kids, but who don't want to sit through long children's movies, create a query to find all of the movies rated G or PG that are less than 100 minutes long.

mysql> select * from movies where Rating = 'G' OR Rating = 'PG' AND Runtime < 100;
+--------------+---------+-----------+------------+--------+
| Title        | Runtime | Genre     | IMDB Score | Rating |
+--------------+---------+-----------+------------+--------+
| Spaceballs   |      96 | Comedy    |        7.1 | PG     |
| Monsters Inc |      92 | Animation |        8.1 | G      |
+--------------+---------+-----------+------------+--------+
2 rows in set (0.00 sec)

//Create a query to show the average runtimes of movies scoring below a 7.5 on imdb, grouped by their respective genres.

select genre, AVG(Runtime) as average_runtime from movies where rating < 7.5 group by genre;
+-------------+-----------------+
| genre       | average_runtime |
+-------------+-----------------+
| Sci-fi      |        119.5000 |
| Documentary |         90.0000 |
| Comedy      |         96.0000 |
| Animation   |         96.0000 |
| Horror      |         83.0000 |
| Action      |        165.0000 |
+-------------+-----------------+
6 rows in set, 8 warnings (0.00 sec)

//There's been a data entry mistake; Starship Troopers is actually rated R, not PG-13. Create a query that finds the movie by its title and changes its rating to R.

update movies set Rating = 'R' where Title = 'Starship Troopers';
Query OK, 1 row affected (0.30 sec)
Rows matched: 1  Changed: 1  Warnings: 0

//This time let's find the average, maximum, and minimum IMDB score for movies of each rating.

select Rating, AVG(`IMDB Score`) as average,
    -> MAX(`IMDB Score`) as maximum,
    -> MIN(`IMDB Score`) as minimum from movies group by Rating;
+--------+---------+---------+---------+
| Rating | average | maximum | minimum |
+--------+---------+---------+---------+
| PG     | 6.63333 |     8.2 |     4.6 |
| R      | 7.83333 |     8.3 |     7.2 |
| G      | 8.10000 |     8.1 |     8.1 |
| TV-14  | 4.70000 |     4.7 |     4.7 |
+--------+---------+---------+---------+
4 rows in set (0.01 sec)

//That last query isn't very informative for ratings that only have 1 entry. Use a HAVING COUNT(*) > 1 clause to only show ratings with multiple movies showing.

select Rating, AVG(`IMDB Score`) as average, MAX(`IMDB Score`) as maximum, MIN(`IMDB Score`) as minimum from movies group by Rating having count(*) > 1;
+--------+---------+---------+---------+
| Rating | average | maximum | minimum |
+--------+---------+---------+---------+
| PG     | 6.63333 |     8.2 |     4.6 |
| R      | 7.83333 |     8.3 |     7.2 |
+--------+---------+---------+---------+
2 rows in set (0.02 sec)

//Make the movie list more child-friendly. Delete all entries that have a rating of R. Remember to record your queries in a README.md file

Delete from movies where Rating = 'R';
Query OK, 3 rows affected (0.08 sec)

//Delete the Movies Table

drop table movies;

//Delete the Database

drop database movies_db;
