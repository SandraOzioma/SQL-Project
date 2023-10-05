Answer the following questions and provide the SQL queries used to find the answer.

    
**Question 1: Which cities and countries have the highest level of transaction revenues on the site?**


SQL Queries:


WITH  Maxrevenue as (
  Select all_sessions."city", all_sessions."country", analytics."unit_price" * temp_products."orderedquantity" as transaction_revenue
from  all_sessions
	join analytics on all_sessions."visitId"  = analytics."visitId"
	join temp_products on all_sessions."productSKU" = temp_products."sku" 
where (analytics."unit_price" * temp_products."orderedquantity") is not null
	)
	Select city, country,  MAX(transaction_revenue) as max_transaction_revenue
from Maxrevenue
	where transaction_revenue = (
	Select MAX(transaction_revenue)
	from Maxrevenue
	)
group by city,country;


Answer:
"Chicago"	"United States"	2214668.30


**Question 2: What is the average number of products ordered from visitors in each city and country?**


SQL Queries:

Select distinct(all_sessions.city),all_sessions."country", cast(avg(temp_products.orderedquantity) as numeric (5,0))as average_orderquantity   
From all_sessions
	Join temp_products on all_sessions."productSKU" = temp_products."sku" 
where temp_products.orderedquantity != 0
Group by all_sessions."city", all_sessions."country" 
Order by all_sessions."city",all_sessions."country" desc;

Answer:
"Adelaide"	"Australia"	54
"Ahmedabad"	"India"	501
"Akron"	"United States"	15
"Amsterdam"	"United States"	8
"Amsterdam"	"Netherlands"	434
"Ann Arbor"	"United States"	314
"Antwerp"	"Belgium"	269
"Appleton"	"United States"	66
"Ashburn"	"United States"	188



**Question 3: Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?**


SQL Queries:


Select distinct all_sessions.city, all_sessions."country", all_sessions."v2ProductCategory",    temp_products."name", max(temp_products.orderedquantity)
Over (partition by all_sessions.city, all_sessions."country", all_sessions."v2ProductCategory" order by all_sessions.country) as Orderedproduct
from all_sessions 
	Join temp_products on all_sessions."productSKU" = temp_products."sku"
Where temp_products.orderedquantity != 0 and all_sessions.city is not null and all_sessions."v2ProductCategory" is not null
Group by all_sessions."city", all_sessions."country", temp_products."name",all_sessions."v2ProductCategory", temp_products.orderedquantity
Order by all_sessions."v2ProductCategory";

Answer:

"Austin"	"United States"	"Apparel"	" Men's 100% Cotton Short Sleeve Hero Tee Black"	90
"Chicago"	"United States"	"Apparel"	" Womens 3/4 Sleeve Baseball Raglan Heather/Black"	23
"Mountain View"	"United States"	"Apparel"	" Men's Bike Short Sleeve Tee Charcoal"	193



**Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?**


SQL Queries:

with Rankedproducts as (
Select all_sessions.city as City, all_sessions."country" as Country , 
  temp_products."name" as top_selling_product, 
	sum(temp_products.orderedquantity)as total_quantity_sold,
	row_number() over (partition by all_sessions.city, all_sessions."country" 
	order by sum(temp_products.orderedquantity) desc) as product_rank
From all_sessions
	Join temp_products on all_sessions."productSKU" = temp_products."sku"
Group by all_sessions."city", all_sessions."country", temp_products."name"
 )

Select city, country, top_selling_product, total_quantity_sold as total_quantity_sold, product_rank
From Rankedproducts
Where product_rank = 1 ;



Answer:

"Adelaide"	"Australia"	" Men's Watershed Full Zip Hoodie Grey"	54	1
"Ahmedabad"	"India"	" Leatherette Notebook Combo"	1148	1
"Akron"	"United States"	" Men's 100% Cotton Short Sleeve Hero Tee Navy"	15	1
"Amsterdam"	"Netherlands"	" Hard Cover Journal"	1330	1
"Amsterdam"	"United States"	" Men's Long Sleeve Raglan Ocean Blue"	8	1



**Question 5: Can we summarize the impact of revenue generated from each city/country?**

SQL Queries:

SELECT
    all_sessions."city",
    all_sessions.country,
    SUM(temp_products."orderedquantity" * analytics."unit_price") AS total_revenue
From analytics 
	Join all_sessions on analytics."visitId" = all_sessions."visitId"
	Join temp_products on all_sessions."productSKU" = temp_products."sku"
GROUP BY
    all_sessions.city, all_sessions."country"
ORDER BY
    all_sessions.city, all_sessions."country" DESC ;

Answer:

"Ahmedabad"	"India"	3737.36
"Amsterdam"	"Netherlands"	11840.52
"Ann Arbor"	"United States"	661651.99
"Antalya"	"Turkey"	0.00





