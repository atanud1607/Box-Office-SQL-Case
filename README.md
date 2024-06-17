# Box-Office-SQL-Case
An SQL Case on Box Office Data

My Scripts are given below - 
Create Database boxoffice_db;
Use boxoffice_db;
Show tables;

/*1. Show the list of movies released in 2020.*/
select release_date
from movie_details;

Alter table movie_details
Add column New_Movie_Release Date;

select *
from movie_details;

Set sql_safe_updates=0;
Update movie_details
Set New_Movie_Release = str_to_date(release_date,"%d-%b-%y");

select movie_name, New_Movie_Release
from movie_details
where New_Movie_Release between "2020-01-01" and "2020-12-31";

select *
from movie_commercials;

/*2. List the top 5 movies which grossed the highest collections across all years. */
select movie_name, movie_total_worldwide
from movie_commercials
order by movie_total_worldwide desc
limit 5;

/*3. List the name of the producers who has produced comedy movies in 2019. */

select movie_name, producer, New_Movie_Release
from movie_details
where movie_genre = "Comedy"
and New_Movie_Release between "2019-01-01" and "2019-12-31"; 

/*Can also write the below function instead of the last function on the above Query*/
/*and Year(New_Movie_Release) = 2019;*/

 /*4. Which movie in 2020 had the shortest duration?*/
 SELECT movie_name, runtime 
 FROM movie_details
 where New_Movie_Release between "2020-01-01" and "2020-12-31"
 ORDER BY runtime ASC
 LIMIT 1;
 
 /*5. List the movie with the highest opening weekend. Is this the same movie which had the highest overall collection?*/
 /*PART 1*/
select movie_name, movie_weekend
from movie_commercials
Order by movie_weekend desc
limit 1;
 
  /*PART 2*/
select movie_name, movie_total_worldwide
From movie_commercials
Order by movie_total_worldwide desc
limit 1;

select if (
(select movie_name, movie_weekend
from movie_commercials
Order by movie_weekend desc
limit 1) = (select movie_name, movie_total_worldwide
		   From movie_commercials
		   Order by movie_total_worldwide desc
           limit 1),
"Yes,the movies are same", "No, the movies are not same") as Status;

/*HENCE, BOTH ARE NOT SIMILAR. THE MOVIE WITH THE HIGHEST OPENING WEEKEND IS WAR. AND THE MOVIE WITH THE HIGHEST OVERALL COLLECTION IS PADMAAVAT
On running the above SQL Query, the result takes the second condition as they are not similar. Hence, the data output is "No"*/

/*6. List the movies which had the weekend collection same as the first week collection*/
select movie_name, movie_weekend, movie_firstweek
from movie_commercials
where movie_weekend = movie_firstweek;

/*7. List the top 3 movies with the highest foreign collection.*/
select movie_name, movie_total_worldwide
from movie_commercials
order by 2 desc
limit 3;

/*8. List the movies that were released on a non-weekend day. */

Set sql_safe_updates=0;
Update movie_details
Set New_Movie_Release = str_to_date(release_date,"%d-%b-%y");

select movie_name, New_Movie_Release, weekday(New_Movie_Release) 
from movie_details
where weekday(New_Movie_Release) Not In (5,6);

/*9. List the movies by Reliance Entertainment which were non comedy.*/
select movie_name, movie_genre, banner
from movie_details
where banner = "Reliance Entertainment"
and movie_genre <> "Comedy";

/*10. List the movies produced in the month of October, November, and December that were released on the weekends.*/

Set sql_safe_updates=0;
Update movie_details
Set New_Movie_Release = str_to_date(release_date,"%d-%b-%y");

select* from movie_details;

select movie_name, monthname(New_Movie_Release), weekday(New_Movie_Release)
from movie_details
where weekday(New_Movie_Release) IN (5,6) and monthname(New_Movie_Release) IN ("October", "November", "December");

Select Distinct monthname(New_Movie_Release)
from movie_details;

select movie_name, monthname(New_Movie_Release), weekday(New_Movie_Release)   
from movie_details
where (monthname(New_Movie_Release) IN ("October", "November", "December")) or (weekday(New_Movie_Release) IN (5,6));

