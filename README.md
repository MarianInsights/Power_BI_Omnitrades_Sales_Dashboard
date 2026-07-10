# 📊 Omnitrades Sales Dashboard

A Power BI dashboard built on a Northwind style sales dataset, covering orders, products, categories and customers for a global trading business. I built this to practice turning a raw relational dataset into a report that a sales or operations team could actually use day to day.

![Omnitrades Sales Dashboard, page 1](Omnitrades%20Power%20BI%20shot%201.png)
*Page 1: sales overview*

## 📌 About the project

The goal was to take five related tables and build something that answers real business questions at a glance, rather than just dumping numbers on a page. I focused on three things: a clean data model, a small set of measures that stay consistent everywhere they appear, and a layout that leads the eye from the headline numbers down into detail.

## ❔Questions the dashboard answers

* What does the business look like overall: total sales, quantity sold, and how many customers, countries and cities are we reaching?
* Which product categories bring in the most revenue, and how has that mix shifted year on year?
* Which countries are the biggest markets, and which cities generate the most orders?
* Who are the top customers by revenue, and how much of total sales sits with each one?
* How has monthly sales performance trended, and where does that trend break down?

## 🧮 Data source and model

Data sourced from the "Telco Customer Churn" dataset on Kaggle, originally adapted from IBM's Sample Data Sets. https://www.kaggle.com/datasets/blastchar/telco-customer-churn

The dataset is structured as a classic sales database: orders, order line items, products, categories and customers, spread across multiple countries. I modeled it as a star schema with five tables:

* **new_OrderDetails**: the fact table, holding sales, quantity and order line detail
* **new_SalesOrder**: order level metadata including the order date, used to build a date hierarchy
* **new_ProductDetails**: product level information
* **new_CategoryDetails**: product category information
* **new_CustomerDetails**: customer and company details including country and city

new_OrderDetails and new_SalesOrder connect to new_CustomerDetails, new_ProductDetails and new_CategoryDetails, which is what lets every visual on the report filter consistently by country, city, category or customer without any extra joins in the visuals themselves.

Most of the KPIs and charts run on implicit aggregations (sum of sales, sum of quantity, count of customers, count of country, count of city), which keeps the model light. Order counts on page two run through a purpose built measure rather than a simple count, since a plain count of rows would have overstated orders that contain more than one line item.

## 📄 Report pages

### Page 1: overview

![Page 1, overview](Omnitrades%20Power%20BI%20shot%201.png)

Five KPI cards sit across the top for an instant read on scale:

| Total Sales | Quantity | #Customers | #Countries | #Cities |
|---|---|---|---|---|
| 1.27M | 51K | 91 | 21 | 69 |

Below that:

* **Sales Trend by Product Category**, a clustered column chart, grouped by year (2006 to 2008) and split by category, so you can see both overall growth and which categories are driving it in a given year
* **Product Category Performance**, a donut chart showing each category's share of total sales, with Beverages out in front at roughly a fifth of all sales, ahead of Dairy Products and Confections
* **Top 5 Countries**, a map sized by sales, with the USA as the clear leader, followed by Germany, Austria, France and Brazil

### Page 2: customers and trends

![Page 2, customers and trends](Omnitrades%20Power%20BI%20shot%202.png)

* **Top 10 Customers**, a treemap sized by sales, with the two largest accounts each bringing in over 100K on their own, well ahead of the rest of the list
* **Sales Trends Over Time**, a monthly line chart from mid 2006 through early 2008, trending upward overall with a fair amount of month to month swing, peaking in early 2008 before a sharp fall at the very end of the period, most likely a partial final month rather than an actual slowdown
* **Top 5 Cities by Orders**, a bar chart led by London, ahead of Rio de Janeiro, Boise, Sao Paulo and Graz
* **A decomposition tree** on sum of sales, letting you drill from country down into individual companies interactively. I added this specifically to give the report a bit of self serve analysis, rather than locking every breakdown into a fixed chart. As an example, drilling into the USA shows it alone accounts for close to a fifth of total company wide sales, split across a handful of major accounts.

## 💡Key insight

The USA leads on every major cut of this data: total sales, order volume in the decomposition tree, and it holds two of the largest individual accounts. That concentration is worth flagging to a sales team, since it means a meaningful share of revenue sits with a small number of relationships.

On the trend side, growth from mid 2006 into early 2008 looks healthy, but the steep drop right at the end of the dataset is the kind of thing I'd want to sanity check against the raw order dates before presenting it as a real slowdown.

## 🛠️ Skills demonstrated

* Building a star schema from normalized source tables and getting the relationships right so cross filtering works everywhere
  
* Choosing between implicit aggregations and a dedicated DAX measure based on what the underlying grain of the data actually requires
  
* Using a date hierarchy to support both yearly comparisons and a continuous monthly trend from the same table
  
* Designing a two page layout that moves from summary to detail, rather than trying to fit everything onto one screen
  
* Using a decomposition tree to give report viewers an actual drill path instead of just static charts
  
* Reading a dataset critically enough to flag an anomaly (the end of period drop) rather than presenting every trend line at face value

## 🧰 Tools used

* Power BI Desktop
* DAX for the order count measure and any calculated logic beyond simple aggregation
* Implicit aggregations for sales, quantity, customer, country and city totals
* A date hierarchy built off the order date field
* Map, column, line, bar, donut, treemap and decomposition tree visuals, plus KPI cards

## 📁 Repository contents

* Omnitrades_Sales_Dashboard.pbix: the full Power BI file, including the data model and both report pages
* images/: dashboard screenshots

## 🔍 How to view it

Download the .pbix file and open it in Power BI Desktop to explore the model and interact with the decomposition tree and cross filtering. If you do not have Power BI Desktop, the screenshots above show both pages in their default state.
