# E-Commerce-Sales-Customer-Analytics

E-Commerce Sales & Customer Analytics Dashboard

An end-to-end analytics project built on the Superstore retail dataset. It covers revenue trends, profitability, regional performance and the effect of discounting, and ends with data-driven business recommendations. The dashboard is built in Power BI using DAX.


Project Overview
	
Goal	Identify revenue trends, find what is hurting profit, and recommend actions
Data	Sample Superstore: 9,994 order lines, 21 columns, Jan 2014 to Dec 2017
Tools	Power BI Desktop, DAX, Power Query, Python (pandas) for validation
Output	Interactive dashboard, insights and recommendations
Business Questions
How are revenue, profit and orders trending over time?
Which categories, sub-categories, regions and segments perform best and worst?
Which products are unprofitable?
How does discounting affect profit?
Dashboard KPIs
Total Revenue
Total Profit and Profit Margin %
Total Orders (distinct Order IDs)
Average Order Value
Sales Growth % (year over year)

Filters: Year, Region, Category, Segment

Key Findings
Metric	Result
Revenue	$2.30M
Profit	$286K (12.5% margin)
Orders / Customers	5,009 / 793
Average Order Value	about $459
Sales growth	2015: -2.8%, 2016: +29.5%, 2017: +20.4%
Discounts above 20% make orders loss-making. Margin is 29.5% with no discount and 11.6% at 11-20%. Every band above 20% loses money, and the 50%+ band has a margin of about -119%. Orders discounted above 20% lost roughly $135K in total, nearly half of total profit.
Furniture grows sales but not profit. It is 32% of sales with only a 2.5% margin, compared with Technology ($145K profit) and Office Supplies ($122K).
Three sub-categories lose money: Tables (-$17.7K), Bookcases (-$3.5K) and Supplies (-$1.2K).
301 of 1,850 products are unprofitable. The worst include the Cubify CubeX 3D Printers and the Lexmark MX611dhe laser printer.
Geography: West and East produce about 70% of profit. Texas, Ohio and Pennsylvania are the biggest loss-making states.
Recommendations
Cap discounts at 20% and require approval above that level.
Reprice, renegotiate supplier cost or phase out Tables and Bookcases.
Review the worst 20-30 products for delisting or minimum-margin pricing.
Investigate Texas, Ohio and Pennsylvania (discount levels, shipping cost).
Promote high-margin lines such as Copiers, Paper and Accessories.

These findings show association, not proven causation. Cost, product mix and shipping also affect profit. An A/B pricing test is a sensible next step before changing policy.

How It Was Built
1. Data preparation (Power Query)
Loaded the CSV with file origin 1252: Western European (Windows), since the file is not UTF-8.
Set data types, with dates parsed as m/d/yyyy using the English (US) locale.
Checked for nulls and duplicates (none found) and removed the unused Row ID column.
2. Data model
Created a Date table with DAX and marked it as the date table.
Related Date[Date] to Superstore[Order Date] (one-to-many).
Kept all measures in a dedicated _Measures table.
3. Core DAX measures
DAX
Total Revenue    = SUM ( Superstore[Sales] )
Total Profit     = SUM ( Superstore[Profit] )
Profit Margin %  = DIVIDE ( [Total Profit], [Total Revenue] )
Total Orders     = DISTINCTCOUNT ( Superstore[Order ID] )
Avg Order Value  = DIVIDE ( [Total Revenue], [Total Orders] )

Revenue LY       = CALCULATE ( [Total Revenue], SAMEPERIODLASTYEAR ( 'Date'[Date] ) )
Sales Growth %   = DIVIDE ( [Total Revenue] - [Revenue LY], [Revenue LY] )
4. Discount analysis
DAX
Discount Band =
SWITCH (
    TRUE (),
    Superstore[Discount] = 0,   "0%",
    Superstore[Discount] <= 0.1, "1-10%",
    Superstore[Discount] <= 0.2, "11-20%",
    Superstore[Discount] <= 0.3, "21-30%",
    Superstore[Discount] <= 0.4, "31-40%",
    Superstore[Discount] <= 0.5, "41-50%",
    "50%+"
)

Low-profit products are found with a bottom 10 Top N filter on Total Profit in a table visual.

5. Visuals

KPI cards, a monthly sales and profit line chart, sub-category profit bars (negatives in red), profit margin by discount band, region and segment charts, and tables of loss-making products and states.


Sample Superstore dataset (orders, customers, products, regions, sales, discount, profit), 2014-2017.
