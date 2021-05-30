# SQL LECTURES

![cheatsheet](cheatsheet.png)

## COURSE MODULES
| Module                          | Parts  |                                  |
|---------------------------------|--------|----------------------------------|
| [help with pgadmin](pgadmin.md) | 0      |                                  |

## DEFINITIONS
* `Databases` are systems that allow users to store and organize data
	* They are useful when dealing with large amounts of data

## SPREADSHEETS VS DATABASES
* spreadsheets
	* one-time analysis
	* quickly need to chart something out
	* reasonable data set size
	* ability for untrained people to work with data
* databases
	* data integrity
	* can handle masive amounts of data
	* quickly combine different datasets
	* automate steps for re-use
	* can support data for websites and applications


## SELECT
* `SELECT` is the most common statement used, and it allows us to retrieve information from a table
    * `SELECT column_name FROM table_name`
* we can combine `SELECT` with other statements to perform more complex queries

## SELECT DISTINCT
* get only **distinct** values
	* `SELECT DISTINCT column FROM table`
	* `SELECT DISTINCT(column) FROM table`
* ex: see table

| name    | choice  |
|---------|---------|
| Zach    | green   |
| David   | green   |
| Claire  | yellow  |
| David   | Red     |

* `SELECT DISTINCT name FROM table`
	* Zach, David, Claire
* `SELECT DISTINCT choice FROM table`
	* green, yellow, red

## COUNT
* returns number of inpt rows that match a specific condition
* count needs function
	* `SELECT COUNT(name) FROM table`
	* count: 4
* `SELECT COUNT(DISTINCT name) FROM table`
	* count = 3 *because zack + david + claire = 3*

## SELECT WHERE
* `WHERE` allows us to specify conditions on columns for the rows to be returned
	```SQL
	SELECT column1, column2
	FROM table
	WHERE conditions;
	```
* `WHERE` filters rows
	```SQL
	SELECT name, choice FROM table WHERE name = "David" AND choice = "Red"
	```
## ORDER BY
```SQL
SELECT column_1, column_2
FROM table
ORDER BY column_1 ASC / DESC
```
* `ASC` by default
* sort multiple colums
	```SQL
	SELECT company,name,sales
	FROM table
	ORDER BY company,sales
	```
## LIMIT
* limits amount of rows of table
* goes at the last part of query

## BETWEEN
* value >= low and value <= high
* inclusive
* can be used with dates

## IN
* same as python
	```SQL
	SELECT color FROM table
	WHERE color IN ('red','blue')
	```

## LIKE
* match against pattern with use of wildcard characters
* LIKE = case sensitive
	* ILIKE = not case sensitive
* `%` matches any sequence of charaters
	* `WHERE name LIKE 'A%'` finds all names that begin with 'A'
* `_` matches any single character

## COMMON AGGREGATE FUNCTIONS:
* `AVG()` - returns average value
* `COUNT()` - returns number of vals
* `MAX()` - returns max val
* `MIN()` - returns min val
* `SUM()` - adds all vals together
* only for `SELECT`

## ROUND
```SQL
SELECT round(avg(replacement_cost),2)
from FILM
```

## GROUP BY
* aggregate cols by category
	```SQL
	SELECT category_col, AGG(data_col)
	FROM table
	GROUP BY category_col;
	```
* `GROUP BY` clause must appear right after a `FROM` or `WHERE` statement
	```SQL
	SELECT company,division, SUM(sales)
	FROM finance_table
	GROUP BY company,division
	```
## HAVING
* filter after an aggregate
	```SQL
	SELECT company,division, SUM(sales)
	FROM finance_table
	WHERE company != 'Google'
	GROUP BY company,division
	HAVING SUM(sales) > 1000
	```

## AS
* alias
	```SQL
	SELECT SUM(column) AS new_name
	FROM table
	```
	* `AS` get executed at end of query
	```SQL
	SELECT customer_id, SUM(amount) AS total_spent
	FROM payment
	GROUP BY customer_id
	HAVING SUM(amount) > 100
	```

## INNER JOIN
* combine info from multiple tables
* postgres will treat `join` as `inner join` by default
	```SQL
	SELECT payment_id, payment.customer_id, first_name, email
	FROM payment
	INNER JOIN customer
	ON payment.customer_id = customer.customer_id
	```

## OUTER JOIN
* full outer join
	```SQL
	SELECT * FROM customer
	FULL OUTER JOIN payment
	ON customer.customer_id = payment.customer_id
	```
* are there any payment info that isn't attached to customer and vise versa?
	```SQL
	SELECT * FROM customer
	FULL OUTER JOIN payment
	ON customer.customer_id = payment.customer_id
	WHERE customer.customer_id IS null
	OR payment.payment_id IS null
	```
* left outer join
	* results in the set of records that are in the left table
	* `left join` means `left outer join` in postgres
	```SQL
	SELECT film.film_id, title, inventory_id FROM film
	LEFT JOIN inventory ON
	inventory.film_id = film.film_id
	WHERE inventory.film_id is null
	```
* right outer join
	* swapped filled in venn diagram
* double join
	```sql
	SELECT film.title, actor.first_name, actor.last_name  FROM film
	INNER JOIN film_actor
	ON film.film_id = film_actor.film_id
	INNER JOIN actor
	ON actor.actor_id = film_actor.actor_id
	WHERE actor.first_name = 'Nick' AND actor.last_name = 'Wahlberg'
	```
## UNION
* concatenate 2 select statements

## TIME
* `AGE()` gets how old the timestamp is
	* `AGE(dat_col)`
* `TO_CHAR()`
	* converts data types to text
	*  `TO_CHAR(timestamp, string)`
	* YY, MONTH, mon, MM, etc.
	* `TO_CHAR(timestamp, 'MM-dd-YYYY)`
	* [useful link for formatting](https://www.postgresql.org/docs/9.1/functions-formatting.html)


## EXTRACT
```sql
SELECT EXTRACT(YEAR FROM payment_date) AS year
FROM payment
```
* can get
	* year
	* month
	* day
	* week
	* quarter

## STRING
* [USEFUL LINK](https://www.postgresql.org/docs/9.1/functions-string.html)
```SQL
SELECT LOWER(LEFT(first_name,1) || last_name) || '@gmail.com'
AS customer_email
FROM customer
```

## SUBQUERY
* query a query
* uses 2 select statements
```SQL
SELECT title, rental_rate
FROM film
WHERE rental_rate >
(SELECT AVG(rental_rate) FROM film)
```
```SQL
SELECT film_id, title
FROM film
WHERE film_id IN
(SELECT inventory.film_id FROM rental
INNER JOIN inventory ON inventory.inventory_id = rental.inventory_id
WHERE return_date BETWEEN '2005-05-29' AND '2005-05-30')
ORDER BY film_id
```
```SQL
SELECT first_name, last_name
FROM customer AS c
WHERE EXISTS
(SELECT * FROM payment as p
 WHERE p.customer_id = c.customer_id
 AND amount > 11)
```

## SELF JOIN
* join a query to itself
* 2 copies of same table
* no special keyword
	```SQL
	SELECT tableA.col, tableB.col
	FROM table AS tableA
	JOIN table AS tableB ON
	tableA.some_col = tableB.other_col
	```
	```SQL
	SELECT one.title, two.title, one.length FROM film AS one
	JOIN film AS two
	ON one.film_id != two.film_id
	AND one.length = two.length
	```