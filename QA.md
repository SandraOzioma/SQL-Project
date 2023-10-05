What are your risk areas? Identify and describe them.

Inconsistent data, there were duplicate and different naming conventions in the all_sessions tables. 
Tables with high missing values
Redundant columns and different characters missed up in many tables. 


QA Process:
Describe your QA process and include the SQL queries used to execute it.

1, To know if there the count of Sku column and the  count  of name colume in the temp_products macthes, I  performed the below Queries

--Result--

SELECT COUNT(SKU) 
FROM temp_products;

SELECT COUNT(NAME) 
FROM temp_products;


2, To get column data types and nullability for the tem_products table --

SELECT column_name, data_type, is_nullable
FROM information_schema.columns
WHERE table_name = 'temp_products';

--Result--

"column_name"	"data_type"	"is_nullable"
"stocklevel"	"integer"	"YES"
"sentimentscore"	"real"	"YES"
"sentimentmagnitude"	"real"	"YES"
"orderedquantity"	"integer"	"YES"
"restockingleadtime"	"integer"	"YES"
"name"	"text"	"YES"
"sku"	"text"	"YES"

3, To test for validity of the date column in the all_sessions table

SELECT *
FROM all_sessions
WHERE date < '1900-01-01';

4, check for missing values and  tO identify columnes with nulls and then confirmed with a count 

--Result--

Select all_sessions."fullVisitorId"
From all_sessions
where all_sessions."fullVisitorId" is not  null;

Select COUNT(all_sessions."fullVisitorId")
From all_sessions;
