## Question 1: Find all duplicate records

**SQL Queries:**

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
**Answer:**
- The table **analytics** have more than eight-hundred-thousand duplicated entries. I created another table to work on the queries after cleaning duplicates.
- The table **all_sessions** does not have duplicates.
- The table **products** does not have duplicates.
- The table **sales_by_sku** does not have duplicates.
- The table **sales_report** does not have duplicates.




## Question 2: Find the total number of unique visitors (`fullVisitorID`)

**SQL Queries:**
```SQL
SELECT COUNT(DISTINCT fullvisitorid)
FROM 	analytics_new;
```

**Answer:**
There are **120018** on the analytics table.



## Question 3: Find the total number of unique visitors by referring sites

**SQL Queries:**

```SQL
--Get unique visitid list from table all_sessions coming from referral
SELECT COUNT(DISTINCT visitid) 
FROM all_sessions
WHERE channelgrouping = 'Referral';
```
**Answer:**
- There are **2488** unique visitors on the all_sessions table


## Question 4: Find each unique product viewed by each visitor

**SQL Queries:**
```SQL
--Get unique products by visit
SELECT 	unq.visitid,
		unq.productsku
FROM
(
--Subquery to count distinct
		SELECT 	visitid,
				productsku,
				COUNT(DISTINCT productsku)
		FROM all_sessions
		GROUP BY visitid,productsku
)unq
```
**Answer:**

- There are **15129** unique products by each visitor.


## Question 5: Compute the percentage of visitors to the site that actually makes a purchase

**SQL Queries:**
```SQL
SELECT ROUND(((CAST(cal.purchases as DECIMAL(10,2))/CAST(cal.visits AS DECIMAL(10,2)))*100),2) pct_purchases
FROM
(
SELECT COUNT(visitid) purchases, 
--Subquery to get total count
	    (SELECT COUNT(visitid)FROM all_sessions) visits
FROM all_sessions
WHERE totaltransactionrevenue IS NOT NULL
) cal;
```
**Answer:**

- Only the 0.54 percent of the visitors purchase.
