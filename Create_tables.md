## Scripts to create tables
The following scripts were used to create the tables

--Create table sales_by_SKU
```SQL
CREATE TABLE sales_by_sku ( 
  productSKU             VARCHAR(20),
  total_ordered          INT
);
```

--Create table products
```SQL
CREATE TABLE products(
  productSKU            VARCHAR(20),
  name                  VARCHAR(255),
  orderedQuantity       INTEGER,  
  stockLevel	          INTEGER,
  restockingLeadTime	  INTEGER,
  sentimentScore	      NUMERIC(3,1),
  sentimentMagnitude    NUMERIC(3,1)
);
```

--Create table sales_report
```SQL
CREATE TABLE sales_report(
  productSKU            VARCHAR(20),
  total_ordered         INT,
  name                  VARCHAR(255), 
  stockLevel	          INTEGER,
  restockingLeadTime	  INTEGER,
  sentimentScore	      NUMERIC(3,1),
  sentimentMagnitude    NUMERIC(3,1),
  ratio                 DECIMAL
);
```

--Create table all_sessions
```SQL
CREATE TABLE all_sessions(
  fullVisitorId			      NUMERIC,
  channelGrouping			    VARCHAR(50),
  time					          INTEGER,
  country					        VARCHAR(50),
  city					          VARCHAR(50),
  totalTransactionRevenue	NUMERIC(14,2),
  transactions			      SMALLINT,
  timeOnSite				      SMALLINT,
  pageviews				        SMALLINT,
  sessionQualityDim		    SMALLINT,
  date					          DATE,
  visitId					        INTEGER,
  type					          VARCHAR(15)	,
  productRefundAmount		  NUMERIC(14,2)	,
  productQuantity			    INTEGER	,
  productPrice			      NUMERIC(14,2),
  productRevenue			    NUMERIC(14,2),
  productSKU				      VARCHAR(20),
  v2ProductName			      VARCHAR(255),
  v2ProductCategory		    VARCHAR(50),
  productVariant			    VARCHAR(20),
  currencyCode			      VARCHAR(3),
  itemQuantity			      INTEGER,
  itemRevenue				      NUMERIC(14,2),
  transactionRevenue		  NUMERIC(14,2),
  transactionId			      VARCHAR(20),
  pageTitle				        TEXT,
  searchKeyword			      VARCHAR(50),
  pagePathLevel1			    VARCHAR(255),
  eCommerceAction_type	  SMALLINT,
  eCommerceAction_step	  SMALLINT,
  eCommerceAction_option	VARCHAR(20)	
);
```
--Create table analytics
```SQL
CREATE TABLE analytics(
  visitNumber				    INTEGER,
  visitId					      INTEGER,
  visitStartTime			  INTEGER,
  date					        DATE,
  fullvisitorId			    NUMERIC,
  userid					      VARCHAR(15),
  channelGrouping			  VARCHAR(20),
  socialEngagementType	VARCHAR(40),
  units_sold				    INTEGER,
  pageviews				      SMALLINT,
  timeonsite				    SMALLINT,
  bounces					      SMALLINT,
  revenue					      NUMERIC(10),
  unit_price				    NUMERIC(14,3)
);
```
