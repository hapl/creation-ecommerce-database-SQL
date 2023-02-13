## What issues will you address by cleaning the data?

**Table products**
- Found products name duplicated but unique productSKU. So I did not update any data on this table.

**Table sales by SKU**
- Found 8 items that not exist on the products table.

**Table sales_report**
- Compared entries to check duplicated values but I got no results.

**Table all_sessions**
- Found fullVisitorId are duplicate but not the whole line. I did not consider this as a duplication.
- I divided the value of productprice,productrevenue,totaltransactionrevenue and transactionrevenue  by 1 million according to hint on task. Data did not make sense with the values provided.

**Table analytics**
- I found the table analytics had more that 4 million entries. I checked that it had only 1.7+ millions of unique entries. I created a new table with just the unique entries.
- According to notes from the task the unit price and revenue needed to be divided by 1 million. I adjusted this on the new and old table.
- Found that the table analytics has a lot entries that have the same value in visitid and visitstarttime.
- The value for userid is always null.





## Queries:
Below, provide the SQL queries you used to clean your data.

- Checking duplicated on table products
```SQL
-- Checking duplicates on products table
SELECT name, 
	   count(*) 
FROM products
GROUP BY name
HAVING COUNT(*) > 1;

SELECT productsku, 
	   COUNT(*)
FROM products
GROUP BY productsku
HAVING COUNT(*) > 1;
```

- Cheking items relation for the table sales by SKU with products table

```SQL
-- Cheking if all the sales SKUs are on the products table
SELECT  p.productsku, 
        s.productsku
FROM sales_by_sku s
LEFT JOIN products p ON s.productsku = p.productsku
WHERE p.productsku IS NULL;
```

- Checking duplicates on sales_report

```SQL
--Checking duplicates on sales_report table

SELECT 
    productsku, 
	total_ordered, 
	name, 
	stocklevel, 
	restockingleadtime, 
	sentimentscore, 
	sentimentmagnitude, 
	ratio,
	COUNT(*)
FROM sales_report
GROUP BY 
    productsku, 
	total_ordered, 
	name, 
	stocklevel, 
	restockingleadtime, 
	sentimentscore, 
	sentimentmagnitude, 
	ratio
HAVING COUNT(*) > 1;
```

- Review all_sessions table

```SQL
--Checking duplicates
SELECT DISTINCT  * 
FROM all_sessions;

--Checking fullvisitorid duplication on the all_sessions table
SELECT DISTINCT fullvisitorid 
FROM all_sessions
```
- Recalculate values for all_sessions (productprice)

```SQL
--Update productprice (divide by 1000000)
UPDATE all_sessions
SET productprice = (productprice/1000000);

--Update productrevenue (divide by 1000000)
UPDATE all_sessions
SET productrevenue = (productrevenue/1000000);

--Update totaltransactionrevenue (divide by 1000000)
UPDATE all_sessions
SET totaltransactionrevenue = (totaltransactionrevenue/1000000);

--Update transactionrevenue (divide by 1000000)
UPDATE all_sessions
SET transactionrevenue= (transactionrevenue/1000000);
```

- Checking issues with duplicates on the analytics table.

```SQL
--Checkin that duplicates exists
SELECT 	visitnumber, 
		visitid, 
		visitstarttime, 
		date, 
		fullvisitorid, 
		userid, 
		channelgrouping, 
		socialengagementtype, 
		units_sold, 
		pageviews, 
		timeonsite, 
		bounces, 
		revenue, 
		unit_price
		,COUNT(*)
FROM analytics
GROUP BY 	visitnumber, 
			visitid, 
			visitstarttime, 
			date, 
			fullvisitorid, 
			userid, 
			channelgrouping, 
			socialengagementtype, 
			units_sold, 
			pageviews, 
			timeonsite, 
			bounces, 
			revenue, 
			unit_price
HAVING COUNT(*)>1;
```

```SQL
--Creating a new table with only unique values
CREATE TABLE analytics_new AS
	SELECT DISTINCT *  
    FROM analytics;
```
- Updating unit price and revenue on the analytics tables
``` SQL
--Update unit_price  for table analytics new according to notes
UPDATE  analytics_new 
    SET unit_price=(unit_price/1000000);
--Update unit_price for table analytics new according to notes
UPDATE analytics_new
    SET revenue = (revenue/1000000);
```
- Checking values in visitid and visitstarttime
```SQL
--Checking row that have the same value in visitid and visitstarttime
SELECT COUNT(*)
FROM analytics_new
WHERE visitid = visitstarttime;
```
- Checking values for userid
```SQL
--Checking values for userid
SELECT DISTINCT userid 
FROM analytics_new;
```