# KMS-Inventory-Analysis
## 🌟 Project Objective
Analyze Kultra Mega Stores (KMS) sales and operations data to uncover actionable insights and solve two real-life business problems using SQL.

## Company Overview
Kultra Mega Stores (KMS), headquartered in Lagos, specialises in office supplies a
furniture. Its customer base includes individual consumers, small businesses (retail), and
large corporate clients (wholesale) across Lagos, Nigeria.
You have been engaged as a Business Intelligence Analyst to support the Abuja division of
KMS. The Business Manager has shared an Excel file containing order data from 2009 
2012 and has requested that you analyze the data and present your key insights and
findings.

## 📂 Data Sources & Preparation
### Data Sources
-	KMS Sql Case Study: Sales and product data
-	Order Status: Returned product status

## 📊 Data Import & Transformation
|Column Name|	Original Type|	New Type|	Reason|
|-----------|-------------|-----------|----------|
|Sales, Discount, Profit, Unit Price, Shipping Cost, Product Base Margin|	Various|	decimal(10,2)|	For numeric accuracy|
|Order_ID|	smallint|	int|	Values exceeded smallint range, not suitable as PK|
|Order Quantity|	tinyint|	int|	To accommodate larger order quantities|
|Row_ID|	-|	Primary Key|	Chosen due to uniqueness|

- No transformations were applied to the Order Status file.

## 🔍 Exploratory Data Analysis
### Case Scenario I 
1. Which product category had the highest sales? 
2. What are the Top 3 and Bottom 3 regions in terms of sales? 
3. What were the total sales of appliances in Ontario? 
4. Advise the management of KMS on what to do to increase the revenue from the bottom 10 customers
5. KMS incurred the most shipping cost using which shipping method? 
### Case Scenario II 
6. Who are the most valuable customers, and what products or services do they typically purchase? 
7. Which small business customer had the highest sales? 
8. Which Corporate Customer placed the most number of orders in 2009 – 2012? 
9. Which consumer customer was the most profitable on 
10. Which customer returned items, and what segment do they belong to? 
11. If the delivery truck is the most economical but the slowest shipping method and Express Air is the fastest but the most expensive one, do you think the company appropriately spent shipping costs based on the Order Priority? Explain your answer

## 📊 Data Analysis
📖 Case Scenario I: Sales Insights
1. Highest Sales by Product Category
```
select Product_category, SUM(sales) as [Total Sales]
from [dbo].[KMS Sql Case Study]
group by Product_category
order by SUM(sales) desc
```
**Insight**: Technology had the highest sales.
