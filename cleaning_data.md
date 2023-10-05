What issues will you address by cleaning the data?

Making the data set useable and provide insight to the business
Converting to the correct data type  
Filling empty columns with the right data set
converting the unit_price to the right units 
Cleaning structural issue 
cleaning missing data


Queries:
Below, provide the SQL queries you used to clean your data.

/* Creating table from sales_report */

create table temp_sales_report(
productsku text,
total_ordered text,
name text,
stocklevel text,
restockingleadtime text,
sentimentscore text,
sentimentmagnitude text,
ratio text
);

create table temp_sales_report1 as
(select productsku,
total_ordered::integer,
name,
stockLevel::int,
restockingLeadTime::int,
sentimentscore::real,
sentimentmagnitude::real,
ratio::real
from temp_sales_report);

/* Creating table from sales_by_sku*/

create table temp_sales_by_sku as
(select "productSKU" as productsku, total_ordered::"numeric" as total_ordered
From sales_by_sku 
);


/* Creating table from products */

CREATE  TABLE products(
sku text, 
name text,
rderedquantity text,
stocklevel text,
restockingleadtime text	,
sentimentscore text,
sentimentmagnitude text
);

create table temp_products as
(select sku, name, products.orderedquantity::int, stocklevel::int, restockingleadtime::int, sentimentscore:: real, sentimentmagnitude:: real
from products);


*/cleaning all_sessions table */

Update all_sessions
set country = null, 
city = null
where country = '(not set)' or city in ('not available in demo dataset', '(not set)');

update all_sessions
set "productPrice" = "productPrice"/1000000;

Update all_sessions
set "productVariant" = null 
where "productVariant" = '(not set)';

alter TABLE all_sessions
alter COLUMN "fullVisitorId"
type "numeric"
USING "fullVisitorId"::"numeric";

alter TABLE all_sessions
alter COLUMN "time"
type "numeric"
USING "time"::"numeric";

alter TABLE all_sessions
alter COLUMN "time"
type "time"
USING '00:00'::time + (time || ' second')::interval;


alter TABLE all_sessions
alter COLUMN "totalTransactionRevenue"
type "numeric"
USING "totalTransactionRevenue"::"numeric";


alter TABLE all_sessions
alter COLUMN "transactions"
type integer
USING "transactions"::integer,


alter TABLE all_sessions
alter COLUMN "timeOnSite"
type "numeric"
USING "timeOnSite"::"numeric";

alter TABLE all_sessions
alter COLUMN "timeOnSite"
type "time"
USING '00:00'::time + ( all_sessions."timeOnSite"|| ' second')::interval;



alter TABLE all_sessions
alter COLUMN "pageviews"
type integer
USING "pageviews"::integer;


alter TABLE all_sessions
alter COLUMN "sessionQualityDim"
type integer
USING "sessionQualityDim"::integer,


alter TABLE all_sessions
alter COLUMN "date"
type date
USING "date"::date;

--convert visitid to int--

alter TABLE all_sessions
alter COLUMN "visitId"
type integer
USING "visitId"::integer;

alter TABLE all_sessions
alter COLUMN "productRefundAmount"
type "numeric"
USING "productRefundAmount"::"numeric";


alter TABLE all_sessions
alter COLUMN "productQuantity"
type integer
USING "productQuantity"::integer;


alter TABLE all_sessions
alter COLUMN "productPrice"
type "numeric"
USING "productPrice"::"numeric";


alter TABLE all_sessions
alter COLUMN "productRevenue"
type "numeric"
USING "productRevenue"::"numeric"


alter TABLE all_sessions
alter COLUMN "transactionRevenue"
type "numeric"
USING "transactionRevenue"::"numeric";


alter TABLE all_sessions
alter COLUMN "eCommerceAction_type"
type integer
USING "eCommerceAction_type"::integer;


alter TABLE all_sessions
alter COLUMN "eCommerceAction_step"
type integer
USING "eCommerceAction_step"::integer;

UPDATE all_sessions
SET "v2ProductCategory" = null
where "v2ProductCategory" = '${escCatTitle}'

*/cleaning analytics table */

update analytics
set unit_price = unit_price/1000000;

update analytics
set  revenue = unit_price * units_sold;

alter TABLE analytics
alter COLUMN "visitNumber"
type integer
USING "visitNumber"::integer

alter TABLE analytics
alter COLUMN "visitId"
type integer
USING "visitId"::integer


alter TABLE analytics
alter COLUMN "visitStartTime"
type numeric
USING "visitStartTime"::"numeric"


alter TABLE analytics
alter COLUMN "visitStartTime"
type "time"
USING '00:00'::time + ("visitStartTime" || ' second')::interval


alter TABLE analytics
alter COLUMN "date"
type date
USING "date"::"date"


alter TABLE analytics
alter COLUMN "fullvisitorId"
type "numeric"
USING "fullvisitorId"::"numeric"

alter TABLE analytics
alter COLUMN "units_sold"
type integer
USING "units_sold"::integer


alter TABLE analytics
alter COLUMN "pageviews"
type integer
USING "pageviews"::integer

alter TABLE analytics
alter COLUMN timeonsite
type numeric
USING "timeonsite"::numeric

alter TABLE analytics
alter COLUMN timeonsite
type "time"
USING '00:00'::time + (timeOnSite || ' second')::interval

alter TABLE analytics
alter COLUMN "bounces"
type integer
USING "bounces"::integer

alter TABLE analytics
alter COLUMN revenue
type numeric
USING "revenue"::numeric


alter TABLE analytics
alter COLUMN unit_price
type numeric
USING "unit_price"::numeric(5,2)
