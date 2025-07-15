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
### 📖 Case Scenario I: Sales Insights
#### 1. Highest Sales by Product Category
```
select Product_category, SUM(sales) as [Total Sales]
from [dbo].[KMS Sql Case Study]
group by Product_category
order by SUM(sales) desc
```
**Insight**: Technology had the highest sales.

#### 2. Top 3 and Bottom 3 Sales Regions
```
select top 3 Region, SUM(sales) as [Total Sales]
from [dbo].[KMS Sql Case Study]
group by Region
order by SUM(sales) desc
```
The top 3 regions in terms of sales are: West (3597549.41), Ontario (3063212.60) and Prarie (2837304.60)


```
select top 3 Region, SUM(sales) as [Total Sales]
from [dbo].[KMS Sql Case Study]
group by Region
order by SUM(sales) asc
```
The bottom 3 regions in terms of sales are: Yukon (975867.39), Northwest Territories (800847.35), and Nunavut (116376.47)

#### 3. Total Appliance Sales in Ontario
```
select Region, SUM(sales) as [Total Sales]
from [dbo].[KMS Sql Case Study]
where Region = 'Ontario'
group by Region
```
**Result**: $3,063,212.60

#### 4. Revenue Uplift Plan for Bottom 10 Customers
```
select top 10 Order_ID, customer_name, Region, Order_Priority, Ship_Mode,Shipping_Cost, Profit, customer_segment, SUM(sales) as [Total Sales]
from [dbo].[KMS Sql Case Study]
group by Order_ID, customer_name, Region, Order_Priority,Ship_Mode, Shipping_Cost, Profit, customer_segment
order by SUM(sales) asc
```
**Recommendation**: I will advice the management that they should cross-sell and up-sell complementary or premium products to these customers through targeted email with limited-time discounts and loyalty points attach to the products recommendation. That at a certain loyalty point they will be eligible for free shipping of their products.

#### 5. Highest Shipping Cost by Shipping Method
```
select top 1 Ship_Mode,SUM(Shipping_Cost) as [Total shipping cost]
from [dbo].[KMS Sql Case Study]
group by Ship_Mode
```
**Result**: Delivery Truck

### 📈 Case Scenario II: Customer Insights
#### 6. Most Valuable Customers & Products Purchased
```
select top 30 Order_ID, customer_name, Order_Quantity,Product_Category, Product_Name, Profit, SUM(sales) as [Total Sales]
from [dbo].[KMS Sql Case Study]
group by Order_ID, customer_name, Order_Quantity,Product_Category, Product_Name, Profit
order by Profit desc
```
**Insight**: Most valuable customers purchased Technology & Office Supplies.

#### 7. Highest Sales by Small Business Customer
```
select top 1 Customer_Name, Customer_Segment, SUM(Sales) as [Highest Sale]
from [KMS Sql Case Study]
where Customer_Segment = 'Small business'
group by Customer_Name, Customer_Segment
order by [Highest Sale] desc
```
**Result**: Dennis Kane, $75,967.59

#### 8. Most Orders by a Corporate Customer (2009-2012)
```
select top 1 Customer_Name, Customer_Segment, Order_Date ,sum(Order_Quantity) as [Highest No of orders]
from [KMS Sql Case Study]
where Customer_Segment = 'corporate' and Order_Date between '2009-01-01' and '2012-12-31'
group by Customer_Name, Customer_Segment, Order_Date
order by [Highest No of orders] desc
```
or
```
select top 1 Customer_Name, Customer_Segment, Order_Date ,sum(Order_Quantity) as [Highest No of orders]
from [KMS Sql Case Study]
where Customer_Segment = 'corporate' and year(Order_Date) between 2009 and 2012
group by Customer_Name, Customer_Segment, Order_Date
order by [Highest No of orders] desc
```
**Result**: Laurel Elliston, 148 orders

#### 9. Most Profitable Consumer Customer
```
select top 1 Customer_Name, Customer_Segment,sum(Profit) as [Highest Profits]
from [KMS Sql Case Study]
where Customer_Segment = 'consumer'
group by Customer_Name, Customer_Segment
order by [Highest Profits] desc
``` 
alternatively
```
select top 1 Customer_Name, Customer_Segment,sum(Profit) as [Highest Profits]
from [KMS Sql Case Study]
group by Customer_Name, Customer_Segment
having Customer_Segment = 'consumer'
order by [Highest Profits] desc
```
**Result**: Emily Phan, $34,005.44 profit

#### 10. Customers Who Returned Items & Their Segments
```
[dbo].[KMS Sql Case Study] k
[dbo].[Order_Status] s

select k.Customer_Name,k.Customer_Segment, s.[Status]
from [dbo].[KMS Sql Case Study] k
join [dbo].[Order_Status] s
on k.Order_ID=s.Order_ID
group by Customer_Name, Customer_Segment, [Status]
order by Customer_Segment
```
I got the list of customers that returned items, and the segment they belong by Joining the two (2) tables given; KMS Sql Case Study and Order_Status

#### 11.	If the delivery truck is the most economical but the slowest shipping method and Express Air is the fastest but the most expensive one, do you think the company appropriately spent shipping costs based on the Order Priority? Explain your answer
```
SELECT Order_Priority, Ship_Mode, COUNT(*) AS Order_Count, SUM(Shipping_Cost) AS Total_Shipping_Cost
FROM [dbo].[KMS Sql Case Study]
GROUP BY Order_Priority, Ship_Mode
ORDER BY Order_Priority, Ship_Mode
```
**🚚 Business Insight: The company do not appropriately on shipping cost because the larger percentage of their orders were delivered through regular air regardless of the order priority. The company ought to deliver their orders through express and regular air, only if the order priority is either critical or high.**


### ✅ Summary of Findings

- Technology is the top-selling category.
- West region leads in sales.
- Personalized marketing can uplift bottom-tier customers.
- Delivery Truck is the costliest shipping method.

### 🔧 Tools Used

- Microsoft SQL Server
- SQL queries

###🔒 License
This project is licensed under the MIT License.

###🕷️ Contribution
Contributions are welcome! Please submit a pull request or open an issue.

### Contact me via:
#### Email: oriadechristopheradekunle@gmail.com
#### WhatsApp/Call: (+234) 903 6878 197
