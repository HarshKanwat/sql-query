
Here the question and answer of SQL queries:

-- CH1 - Select Queries 101

-- Find the title of each film
SELECT Title
FROM Movies;

-- Find the director of each film
SELECT Director
FROM Movies;

-- Find the title and director of each film
SELECT Title, Director
FROM Movies;

-- Find the title and year of each film
SELECT Title, Director
FROM Movies;

-- Find all the information about each film
SELECT *
FROM Movies;

-- CH2 - Queries with constraints (Pt. 1)

-- Find the movie with a row id of 6
SELECT *
FROM Movies
WHERE Id = 6;

-- Find the movies released in the years between 2000 and 2010
SELECT *
FROM Movies
WHERE Year BETWEEN 2000 AND 2010;

-- Find the movies not released in the years between 2000 and 2010
SELECT *
FROM Movies
WHERE Year NOT BETWEEN 2000 AND 2010;

-- Find the first 5 Pixar movies and their release  year
SELECT *
FROM Movies
WHERE Id BETWEEN 1 AND 5;

-- CH3 - Queries with constraints (Pt. 2)

-- Find all the Toy Story movies
SELECT *
FROM Movies
WHERE Title LIKE "%Toy Story%";

-- Find all the movies directed by John Lasseter
SELECT *
FROM Movies
WHERE Director = "John Lasseter";

-- Find all the movies (and director) not directed by John Lasseter
SELECT *
FROM Movies
WHERE Director != "John Lasseter";

-- Find all the WALL-* movies
SELECT *
FROM Movies
WHERE Title LIKE "%WALL%";

-- CH4 - Filtering and sorting Query results

-- List all directors of Pixar movies (alphabetically), without duplicates
SELECT DISTINCT Director
FROM Movies
ORDER BY Director;

-- List the last four Pixar movies released (ordered from most recent to least)
SELECT *
FROM Movies
ORDER BY Year DESC
LIMIT 4;

-- List the first five Pixar movies sorted alphabetically
SELECT *
FROM Movies
ORDER BY Title ASC
LIMIT 5;

-- List the next five Pixar movies sorted alphabetically
SELECT *
FROM Movies
ORDER BY Title ASC
LIMIT 5
OFFSET 5;

-- CH5 - Review Simple SELECT Queries

-- List all the Canadian cities and their populations 
SELECT *
FROM North_american_cities
WHERE Country LIKE "Canada";

-- Order all the cities in the United States by their latitude from north to south
SELECT *
FROM North_american_cities
WHERE Country = "United States"
ORDER BY Latitude DESC;

-- List all the cities west of Chicago, ordered from west to east
SELECT *
FROM North_american_cities
WHERE Longitude < -87.69
ORDER BY Longitude ASC;

-- List the two largest cities in Mexico (by population)
SELECT *
FROM North_american_cities
WHERE Country LIKE "Mexico"
ORDER BY Population DESC
LIMIT 2;

-- List the third and fourth largest cities (by population) in the United States and their population
SELECT *
FROM North_american_cities
WHERE Country LIKE "United States"
ORDER BY Population DESC
LIMIT 2
OFFSET 2;

-- CH6 - Multi-table queries with JOINs

-- Find the domestic and international sales for each movie
SELECT Title, International_sales, Domestic_sales
FROM Movies JOIN Boxoffice
ON Id=Movie_id;

-- Show the sales numbers for each movie that did better internationally rather than domestically
SELECT Title, International_sales, Domestic_sales
FROM Movies JOIN Boxoffice
ON Id=Movie_id
WHERE International_sales > Domestic_sales;

-- List all the movies by their ratings in descending order
SELECT Title, Rating
FROM Movies JOIN Boxoffice
ON Id=Movie_id
ORDER BY Rating DESC;

-- CH7 - OUTER JOIN

-- Find the list of all buildings that have employees
SELECT DISTINCT Building
FROM Employees
LEFT JOIN Buildings ON Building=Building_name
WHERE Years_employed NOT NULL;

-- Find the list of all buildings and their capacity
SELECT *
FROM Buildings;

-- List all buildings and the distinct employee roles in each building (including empty buildings)
SELECT DISTINCT Building_name, Role 
FROM Buildings 
LEFT JOIN employees ON building_name = building;

-- CH8 - A short note on NULLs

-- Find the name and role of all employees who have not been assigned to a building
SELECT *
FROM Employees
LEFT JOIN Buildings
ON Building_name = Building
WHERE Building IS NULL;

-- Find the names of the buildings that hold no employees
SELECT *
FROM Buildings
LEFT JOIN Employees
ON Building_name = Building
WHERE Building IS NULL;

-- CH9 - Queries with expressions

-- List all movies and their combined sales in millions of dollars
SELECT Title, (Domestic_sales + International_sales)/1000000 AS Total_Sales_Millions
FROM Movies
LEFT JOIN Boxoffice ON Id=Movie_Id;

-- List all movies and their ratings in percent
SELECT Title, Rating*10 as Percent
FROM Movies
LEFT JOIN Boxoffice ON Id=Movie_Id;

-- List all movies that were released on even number years
SELECT Title, Year
FROM Movies
LEFT JOIN Boxoffice ON Id=Movie_Id
WHERE Year % 2 = 0;

-- CH10 - Queries with aggregates (Pt. 1)

-- Find the longest time that an employee has been at the studio
SELECT MAX(Years_employed)
FROM Employees;

-- For each role, find the average number of years employed by employees in that role
SELECT Role, AVG(Years_Employed) 
FROM Employees
GROUP BY Role;

-- Find the total number of employee years worked in each building
SELECT Building, SUM(Years_Employed) 
FROM Employees
GROUP BY Building;

-- CH11 - Queries with aggregates (Pt. 2)

-- Find the number of Artists in the studio (without a HAVING clause)
SELECT Role, COUNT(*) AS Number_of_Artists
FROM Employees
WHERE Role = "Artist";

-- Find the number of Employees of each role in the studio
SELECT Role, COUNT(*)
FROM Employees
GROUP BY Role;

-- Find the total number of years employed by all Engineers
SELECT Role, SUM(Years_Employed)
FROM Employees
GROUP BY Role
HAVING Role = "Engineer";

-- CH12 - Order of execution of a Query

-- Find the number of movies each director has directed
SELECT *, COUNT(Title)
FROM Movies
GROUP BY Director;

-- Find the total domestic and international sales that can be attributed to each director
SELECT Director, sum(Domestic_sales + International_Sales) as Total_Sales
FROM Movies
LEFT JOIN Boxoffice ON Id = Movie_ID
GROUP BY Director;


