# Fetch-Brands-Assignment

Hello product and business team,
I'm sending an update on the take-home assignment, specifically my initial review of the user, brand, and receipt data. I've focused on understanding the data's structure and quality, which is crucial for building reliable analytics.
 
### Key Discoveries & Insights So Far
•	Top Brand Performance: I've been able to identify the top 5 brands by receipts scanned. I took the liberty of slicing the data by “rewardsGroup” as well as “brandCode” - 

Top 5 by receipts scanned by rewardsGroup
•	OSCAR MAYER LUNCH MEAT
•	SARGENTO RICOTTA CHEESE
•	ANNIE'S HOMEGROWN MULTI-SERVING MAC & CHEESE     
•	SARGENTO NATURAL SHREDDED CHEESE 6OZ OR LARGER   
•	SARGENTO STRING OR STICK CHEESE 

Top 5 receipt scanned by brandCode
•	HY-VEE 
•	BRAND ( 
•	KRAFT 
•	HIDDEN VALLEY    
•	KNORR         


•	Comparison of last few months of receipts scanned: the latest month and the second latest month only have Duncan Hines Cake mix in common. Aggregation here was a challenge since the latest month (Mar-21) had NULLs in the rewardsGroup column. As a workaround, I used the 2nd and 3rd latest month (labeled latest and 2nd latest for which we have data).

The full list is below – 
Top 5 receipts scanned in latest month for which we have rewards Group data

rewardsGroup

•	ANNIE'S HOMEGROWN MULTI-SERVING MAC & CHEESE     
•	DUNCAN HINES CAKE MIX 
•	HEINZ GRAVY - JAR 
•	SWISS MISS CAFÉ  
•	SARGENTO NATURAL SHREDDED CHEESE 6OZ OR LARGER    

Top 5 receipts scanned in 2nd latest month for which we have rewards Group data

rewardsGroup

•	KNORR BOUILLON                
•	DUNCAN HINES FROSTING 
•	DIET PEPSI 12 OZ 24+ COUNT
•	SIERRA MIST 12 OZ 12 PACK 
•	DUNCAN HINES CAKE MIX   

•	Between the finished/accepted and rejected receipts, finished receipts amount to $1244 and rejected receipts amount to roughly $20. 
 
### Data Quality: What I've Found & Why It Matters
During my work, I encountered several data quality issues. I discovered these by:
•	Schema Exploration: Examining the JSON structure and trying to flatten it into a tabular format, which immediately highlighted nested fields and array structures.
•	Data Type Coercion: Attempting to convert fields to their expected data types (e.g., numbers for totalSpent, dates for dateScanned). Errors during these conversions, like "HIVE_BAD_DATA" messages, signaled underlying inconsistencies.
•	Direct Inspection: Manually reviewing snippets of the data when unexpected values (like "ITEM NOT FOUND" in a price field or NaN barcodes) appeared.

Here's a breakdown of the key issues:
1.	Inconsistent Data Types:
* Numeric Fields as Strings: Columns that should be numbers (like totalSpent, itemPrice, purchasedItemCount) often contained string values, sometimes representing decimals ("5.0") or even non-numeric text ("ITEM NOT FOUND").
* Impact: This prevents direct mathematical calculations and requires careful data cleansing (e.g., converting to numbers and handling errors) before analysis. It can lead to inaccurate aggregations or failed queries.

2.	Nested & Complex Structures:
* JSON within JSON: IDs and dates are often nested ({"$oid": "..."}, {"$date": timestamp}), and rewardsReceiptItemList is a complex array of dictionaries.
* Impact: This isn't strictly an "issue" but requires significant pre-processing (flattening) to make the data consumable by standard analytical tools or SQL, adding complexity to the data pipeline.

3.	Missing or Misaligned Data Points:

* Missing Barcodes/Brand Codes: Many receipt items lack a barcode, and more critically, the barcodes in receipts often don't have a direct match in the brands dataset's barcode column

* rewardsGroup Missing: The rewardsGroup field, which I discussed for aggregation, was missing for several receipt items. This means we can't currently analyze spending by that specific group by using it as a proxy for brand

* Impact: These gaps make it difficult to reliably link all receipt items to brands, or to conduct certain aggregations. It reduces the completeness and accuracy of our insights
 
Resolving Data Quality Issues: What I Need
To effectively resolve these, I need clarity on a few things:
1.	Data Definitions:
* rewardsGroup Location: Can you confirm where rewardsGroup is truly located in the raw receipts.json or within rewardsReceiptItemList? A precise path would allow me to extract it correctly.
* Purpose of brandCode vs. barcode: What's the intended relationship between barcode (on receipts) and brandCode / barcode (on brands)? Are they meant for direct joins, or is there a different linking key?
* Expected Data Types: For fields with mixed types (e.g., totalSpent), what's the definitive intended data type (e.g., always a decimal number)? What should be done with non-numeric entries (e.g., replace with 0, null, or filter out the row)?

2.	Handling Missing Data:
* What's the preferred strategy for NaN or missing values in critical fields like barcode or brandCode? Should we impute, filter, or use a "unknown" category?
 

Optimizing Data Assets: Further Information Needed
To create truly optimized data assets, beyond just fixing current issues, I'd benefit from understanding:
1.	Business Use Cases: What are the primary questions product and business leaders aim to answer with this data? Knowing this helps prioritize data cleaning efforts and structure the data for specific analytical workflows (e.g., "We need to track brand performance by user demographics," or "We want to analyze average basket size for certain categories").

2.	Data Volume & Growth: What's the typical daily/monthly volume of new data for each file, and what's the anticipated growth rate? This directly impacts infrastructure planning.

3.	Latency Requirements: How quickly do insights need to be available? Is near real-time required, or are daily/weekly updates sufficient? This influences processing architecture.

4.	Source System Context: Understanding how this data is generated upstream (e.g., how "ITEM NOT FOUND" ends up in a price field) can help prevent issues at the source rather than just cleaning them downstream.
 
I believe addressing these points will enable us to build a robust and high-performing data asset. Let's schedule a time to discuss these questions and next steps.

Best regards,
Rik
