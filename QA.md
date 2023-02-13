What are your risk areas? Identify and describe them.

- The data on the tables have a lot of "NULL" values.
- The table analytics have millions of duplicated values. I cleaned the duplicated values but there are still some values are not filled for example userid.
- Have to transform all the revenue and prices dividing by one million for the data to make sense. This operation could cause damage to the data integrity.

## QA Process:
Describe your QA process and include the SQL queries used to execute it.

- Make queries readable using comments and indentations.
- Testing tables and data quality.
- Testing pieces of code to guarantee the totals make sense.

- For Example:
- Notice that the visitors are usually around 1000 but on August 2017 are lest than 40. After taking a look on the second query I noticed that the data only goes until August 1st which makes sense with the rest of the dataset.
```SQL
-- Review visits by year and month
SELECT  EXTRACT(YEAR FROM date) AS year,
        EXTRACT(MONTH FROM date) AS month,
        COUNT(visitid)
FROM all_sessions
GROUP BY year,month
ORDER BY year,month;
```
```SQL
--Checked details about August entries
SELECT  EXTRACT(YEAR FROM date) AS year,
        date,
        count(visitid)
FROM all_sessions
WHERE date >= '20170801' AND date < '20170831' 
GROUP BY year,date
ORDER BY year,date;
```

- Validate duplicate values on tables. I found that the only table with full row duplications was the **analytics** table.
```SQL
--Checking duplicates on analytics table 
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
		unit_price,
		count(*)
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
HAVING COUNT(*) > 1;
```
```SQL
--Checking duplicates on all_sessions table 
SELECT 	fullvisitorid, 
		channelgrouping, 
		time, 
		country, 
		city, 
		totaltransactionrevenue, 
		transactions, 
		timeonsite, 
		pageviews, 
		sessionqualitydim, 
		date, visitid, 
		type, 
		productrefundamount, 
		productquantity, 
		productprice, 
		productrevenue, 
		productsku, 
		v2productname, 
		v2productcategory, 
		productvariant, 
		currencycode, 
		itemquantity, 
		itemrevenue, 
		transactionrevenue, 
		transactionid, 
		pagetitle, 
		searchkeyword, 
		pagepathlevel1, 
		ecommerceaction_type, 
		ecommerceaction_step, 
		ecommerceaction_option,
		count(*)
FROM all_sessions
GROUP BY 	fullvisitorid, 
			channelgrouping, 
			time, 
			country, 
			city, 
			totaltransactionrevenue, 
			transactions, 
			timeonsite, 
			pageviews, 
			sessionqualitydim, 
			date, visitid, 
			type, 
			productrefundamount, 
			productquantity, 
			productprice, 
			productrevenue, 
			productsku, 
			v2productname, 
			v2productcategory, 
			productvariant, 
			currencycode, 
			itemquantity, 
			itemrevenue, 
			transactionrevenue, 
			transactionid, 
			pagetitle, 
			searchkeyword, 
			pagepathlevel1, 
			ecommerceaction_type, 
			ecommerceaction_step, 
			ecommerceaction_option
HAVING COUNT(*)>1;	
```

```SQL
--Checking duplicates on products table
SELECT 	productsku, 
		name, 
		orderedquantity, 
		stocklevel, 
		restockingleadtime, 
		sentimentscore, 
		sentimentmagnitude,
		COUNT(*)
FROM products
GROUP BY productsku, 
		 name, 
		 orderedquantity, 
		 stocklevel, 
		 restockingleadtime, 
		 sentimentscore, 
		 sentimentmagnitude
HAVING COUNT(*) >1;
```
```SQL
--Checking duplicates on sales_by_sku table
SELECT 	productsku, 
		total_ordered,
		COUNT(*)
FROM sales_by_sku
GROUP BY productsku, 
		 total_ordered
HAVING COUNT(*) > 1;
```

```SQL
--Checking duplicates on sales_report table
SELECT 	productsku, 
		total_ordered, 
		name, 
		stocklevel, 
		restockingleadtime, 
		sentimentscore, 
		sentimentmagnitude, 
		ratio,
		COUNT(*)
FROM 	sales_report
GROUP BY productsku, 
		 total_ordered, 
		 name, 
		 stocklevel, 
		 restockingleadtime, 
		 sentimentscore, 
		 sentimentmagnitude, 
		 ratio
HAVING COUNT(*)>1;
```
- Found that the table products and sales_by_SKU do have some entries missing to relate the tables. The table sales_by_sku have some SKUs that do not exist on the table products.

```SQL
-- Cheking if all the sales SKUs are on the products table
SELECT  p.productsku, 
        s.productsku
FROM sales_by_sku s
LEFT JOIN products p ON s.productsku = p.productsku
WHERE p.productsku IS NULL;
```