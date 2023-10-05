Question 1: 
What is the top selling products based on Sales report?

SQL Queries:
SELECT DISTINCT(PRODUCTSKU), max(total_ordered) top_selling_products
FROM temp_sales_by_sku 
where total_ordered != 0
GROUP BY PRODUCTSKU, total_ordered
ORDER BY top_selling_products desc
limit 2;

Answer: 
"productsku"	"top_selling_products"
"GGOEGOAQ012899"	456
"GGOEGDHC074099"	334


Question 2: 
What is the 5 most used channel on the website?

SQL Queries:
Select count(analytics."channelGrouping") as channelgrouping_count,	max (analytics."channelGrouping") over (partition by analytics."channelGrouping") as Max_channelgrouping 
From analytics
Group by analytics."channelGrouping"
Order by channelgrouping_count desc
Limit 5; 

Answer: 
"channelgrouping_count"	"max_channelgrouping"
515197	"Organic Search"
246832	"Referral"
175384	"Direct"
46045	"Social"
45927	"Paid Search"  



Question 3: 
 What is the total ordered products by month and year

SQL Queries:
With Monthlyorders as (
Select DISTINCT EXTRACT(MONTH FROM all_sessions.date) AS month, EXTRACT(year FROM all_sessions.date) AS year, SUM(temp_products."orderedquantity") AS monthly_total_productordered
From analytics
Join all_sessions on analytics."visitId" = all_sessions."visitId"
Join temp_products on all_sessions."productSKU" = temp_products."sku" 	
GROUP BY  month, year
)
SELECT month, year, monthly_total_productordered
FROM Monthlyorders
WHERE monthly_total_productordered = (SELECT MAX(monthly_total_productordered) FROM Monthlyorders);

Answer:
July had the highest orders for the year 2017. 
"month"	"year"	"monthly_total_productordered"

"month"	"year"	"monthly_total_productordered"
7	2017	7559966






