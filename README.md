# Supermarket Sales Analysis using SQL 

The growth of supermarkets in most populated cities are increasing and market competitions are also high. The dataset is one of the historical sales of supermarket company which has recorded in 3 different branches for 3 months data. Predictive data analytics methods are easy to apply with this dataset.

### Attribute information
Invoice id: Computer generated sales slip invoice identification number
Branch: Branch of supercenter (3 branches are available identified by A, B and C).
City: Location of supercenters
Customer type: Type of customers, recorded by Members for customers using member card and Normal for without member card.
Gender: Gender type of customer
Product line: General item categorization groups - Electronic accessories, Fashion accessories, Food and beverages, Health and beauty, Home and lifestyle, Sports and travel
Unit price: Price of each product in $
Quantity: Number of products purchased by customer
Tax: 5% tax fee for customer buying
Total: Total price including tax
Date: Date of purchase (Record available from January 2019 to March 2019)
Time: Purchase time (10am to 9pm)
Payment: Payment used by customer for purchase (3 methods are available – Cash, Credit card and Ewallet)
COGS: Cost of goods sold
Gross margin percentage: Gross margin percentage
Gross income: Gross income
Rating: Customer stratification rating on their overall shopping experience (On a scale of 1 to 10)

### Acknowledgements
Thanks to all who take time and energy to perform Kernels with this dataset and reviewers.

### Purpose
This dataset can be used for predictive data analytics purpose.


## Analysis

### 1.Which product line generates the highest revenue?
'''Sql
SELECT product_line , Sum( total) as TR from Sales
	group by product_line
	ORDER by TR desc
	limit 1;'''
### 2.What is the average total sales per transaction?
'''Sql 
SELECT (sum(total)/ count(invoice_id)) as Avg_Sale_per_trans
from Sales;
'''
### 3.How does revenue vary across different branches and cities?
'''Sql
SELECT city, Sum(total) FROM sales
group by city
order by city;
'''
### 4.What is the trend of total sales over time? (Daily, weekly, or monthly trends)
'''Sql
SELECT to_char(date, 'month') as M , sum(total) from Sales
group by M;
'''
### 5.Which customer type (Member vs. Normal) contributes more to total revenue?
'''Sql
SELECT customer_type , sum(total) from Sales
group by customer_type
ORDER by 1
limit 1;
'''
### 6.Do male and female customers have different spending behaviors?
'''Sql
SELECT gender , sum(total) from Sales
group by gender;
'''
### 7.What is the most common payment method used by customers?
'''Sql
SELECT payment, count(payment) from Sales
group by payment
order by 1 DESC
limit 1; 
'''
### 8.What time of day sees the highest number of sales? (Peak sales hours)
'''Sql
Select extract( hour from time) as pick_HOUR , sum(total) as total_sales from Sales
group by pick_HOUR
order By Total_sales
limit 1;
'''

### 9.How does gross income vary across product lines?
SELECT product_line, Sum(gross_income) from Sales
group by product_line;

-- 10.Which branch has the highest customer satisfaction based on ratings?
SELECT branch, round(avg(rating),1) as highest_rating from Sales
group by branch
order by highest_rating desc
LIMIT 1;

-- 11.Is there any seasonal pattern in sales based on the date column?

SELECT to_char(date, 'month') , sum(total) from Sales
GROUP by to_char;

-- 12.Which product line has the highest profit margins?

SELECT product_line, sum(gross_income) from Sales
group by 1
order by sum
limit 1;

-- 13.How do gross income and gross margin percentage vary across different product lines?

SELECT product_line, round(avg(gross_margin_percentage),2) ,sum(gross_income) from Sales
group by 1
order by sum;

-- 14.Does the tax amount affect total sales in any significant way?

select product_line, sum(tax_5) from Sales
group by 1
order by 2;

-- 15. What is the moving average of total sales over the last 7 days?

SELECT date,
sum(total) as daily_sales,
avg(sum(total)) over(order by date rows BETWEEN 6 PRECEDING and current row ) as moving_average_sales
from Sales
GROUP by date;

-- 16. What is the moving average of total sales over the last 7 days?
SELECT date,
sum(total) as daily_sales,
avg(sum(total)) over(order by date rows BETWEEN 6 PRECEDING and current row ) as moving_average_sales
from Sales
GROUP by date;

-- 17. Which product lines have the highest and lowest revenue growth over time?
-- (Use window functions to calculate month-over-month or week-over-week growth.)"

SELECT product_line,
date_trunc('month' , date) as Sales_month,
sum(total) as Monthly_sales,
lag(sum(total)) OVER(partition by Product_line order by date_trunc('month', date)) as Previous_month_saes,
(sum(total) - lag(sum(total)) OVER (PARTITION by product_line order by date_trunc('month', date))) as Sales_growth
from Sales
group by product_line,sales_month;

-- 18. What is the average transaction value for each customer type?
-- (Group data by customer type and analyze spending behavior.)"

SELECT customer_type, count(invoice_id) as total_transactions,avg(total) as Avg_transaction_value from Sales
group by customer_type;

-- 19. Find the top 5 invoices with highest total purchases.

SELECT invoice_id, total from Sales
order by total desc
limit 5;



