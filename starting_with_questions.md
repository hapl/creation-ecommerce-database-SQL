Answer the following questions and provide the SQL queries used to find the answer.

    
**Question 1: Which cities and countries have the highest level of transaction revenues on the site?**


**SQL Queries:**

```SQL
-- First solution with just one calculation
SELECT  city,
		country,
		SUM(COALESCE(transactionrevenue,0)) revenue
FROM all_sessions
GROUP BY city,country
HAVING SUM(COALESCE(transactionrevenue,0)) > 0
ORDER BY country,
         city,
         revenue DESC
```
```SQL
--Calculate revenue adding columns for city and country revenue v1
SELECT DISTINCT rev.city,
				rev.country,
				rev.revenue_city,
				rev.revenue_country
FROM
(
	--Subquery to calculate revenue by city and country
		SELECT  country,
				city,
				SUM(COALESCE(transactionrevenue,0)) OVER(PARTITION BY country
				RANGE BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING) revenue_country,
				SUM(COALESCE(transactionrevenue,0)) OVER(PARTITION BY city
				RANGE BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING) revenue_city
		FROM all_sessions
		WHERE transactionrevenue >0
	) rev
```
```SQL
--Calculating using the analytics table and all_sessions joined for revenue
SELECT DISTINCT rev.city,
				rev.country,
				rev.revenue_city,
				rev.revenue_country
FROM
(
	--Subquery to calculate revenue by city and country
		SELECT  s.country,
				s.city,
				SUM(COALESCE(a.revenue,0)) OVER(PARTITION BY s.country
				RANGE BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING) revenue_country,
				SUM(COALESCE(a.revenue,0)) OVER(PARTITION BY s.city
				RANGE BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING) revenue_city
		FROM all_sessions s
		INNER JOIN analytics_new a on a.visitid=s.visitid
		WHERE a.revenue >0
	) rev
```
**Answer:**

The country with the highest revenue is United States and the city with the highest revenue is the non register city ("not available in demo dataset"). I created two possible solutions:
- The first one is just considering the revenue in general.
- The second one is considering the revenue by city and adding it as columns.
- The third query is considering the revenue from the table analytics_new



**Question 2: What is the average number of products ordered from visitors in each city and country?**


**SQL Queries:**

```SQL
-- AVG products ordered by city and country
SELECT  city,
		country,
		AVG(COALESCE(productquantity,0)) ordered
FROM all_sessions
GROUP BY city,
         country
ORDER BY country,city

-- AVG products ordered by city and country excluding AVG 0
SELECT  city,
		country,
		AVG(COALESCE(productquantity,0)) ordered
FROM all_sessions
GROUP BY city,
         country
HAVING AVG(COALESCE(productquantity,0)) > 0
ORDER BY country,city
```

**Answer:**

I calculated the average for all the existing countries and cities. Some sales does not have the country and city setup properly on the data.I included all the data without excluding the zeroes.  If I exclude the zeroes it gave me twenty-seven countries with values.


**Question 3: Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?**


**SQL Queries:**

```SQL
--Check the countries and cities with product categories
SELECT  country,
        city,
        v2productcategory,
        SUM(productquantity) ordered
FROM all_sessions
WHERE productquantity > 0
GROUP BY  country,
          city,
          v2productcategory 
```

**Answer:**

With the data available I can deduct the following:
- United States is the highest demand country and top two categories with high demand are Bags (50 units) and Notebooks and Journals with 65. The two cities appear as "not available in demo dataset", for this reason is hard to determine which cities are those. The other categories that shows are related to electronics (Nest and electronics in general).
- The order countries in general have demand for bags,apparel. Many of the cities are not available or some categories are empty.





**Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?**


**SQL Queries:**

```SQL
SELECT  country,
        city,
        productsku,
        v2productname,
        SUM(productquantity) ordered
FROM all_sessions
WHERE productquantity > 0
GROUP BY country,
        city,
        productsku,
        v2productname
ORDER BY country,
         city,
         ordered DESC,
         productsku

```

Answer:

The most sold product is "GGOEGOCB017499" "Leatherette Journal" in the United States, the second product on this country are the Reusable Shopping bags. Also, in United States the demand for Nest related products can be noticed.

In countries other than United States, the most sold product is "GGOEWAEA083899"	"Waze Dress Socks" in Spain with 10 units sold. The rest of the countries are falling under items related to technology but the demand is not more than one item of each.




**Question 5: Can we summarize the impact of revenue generated from each city/country?**

**SQL Queries:**

```SQL
SELECT DISTINCT rev.country,
				rev.city,
				rev.revenue_city,
				rev.revenue_country
FROM
(
--Subquery to calculate revenue by city and country

		SELECT  s.country,
				s.city,
				SUM(COALESCE(a.revenue,0)) OVER(PARTITION BY s.country
				RANGE BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING) revenue_country,
				SUM(COALESCE(a.revenue,0)) OVER(PARTITION BY s.city
				RANGE BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING) revenue_city
		FROM all_sessions s
		INNER JOIN analytics_new a on a.visitid=s.visitid
		WHERE a.revenue >0
) rev
ORDER BY    rev.revenue_city DESC,
                rev.country,
                rev.city
```

**Answer:**
The top three cities with most revenue in United States are:
- Non register city (this value is everywhere and I cannot define if this is comming from the same city or multiple).
- New York
- Sunnyvale

United States keeps the first position as most revenue country, followed by Israel and Switzerland.








