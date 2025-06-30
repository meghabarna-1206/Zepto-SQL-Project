# Zepto-SQL-Project
![Zepto logo](2.jpeg)

## 📌 Project Overview

The goal is to simulate how actual data analysts in the e-commerce or retail industries work behind the scenes to use SQL to:

✅ Set up a messy, real-world e-commerce inventory **database**

In real-world scenarios, analysts first structure messy e-commerce inventory data into SQL by handling inconsistencies like missing values, pricing errors, and vague categories to make it analysis-ready.

✅ Perform **Exploratory Data Analysis (EDA)** to explore product categories, availability, and pricing inconsistencies

Analysts use SQL for exploratory data analysis to uncover patterns, anomalies, and data quality issues like stock gaps and pricing errors before deeper insights can be drawn.

✅ Implement **Data Cleaning** to handle null values, remove invalid entries, and convert pricing from paise to rupees

Analysts clean the data using SQL by correcting invalid entries and standardizing values like prices, ensuring consistency and reliability for accurate business analysis.

✅ Write **business-driven SQL queries** to derive insights around **pricing, inventory, stock availability, revenue** and more

With clean data, analysts write business-driven SQL queries to extract insights on revenue, stock levels, and discounts that guide pricing, restocking, and inventory decisions.

## 📁 Dataset Overview
The dataset was sourced from [Kaggle](https://www.kaggle.com/datasets/palvinder2006/zepto-inventory-dataset/data?select=zepto_v2.csv) and was originally scraped from Zepto’s official product listings. It mimics what is typically encountered in a real-world e-commerce inventory system.

Each row represents a unique SKU (Stock Keeping Unit) for a product. Duplicate product names exist because the same product may appear multiple times in different package sizes, weights, discounts, or categories to improve visibility – exactly how real catalog data looks.

🧾 Columns:
- **sku_id:** Unique identifier for each product entry (Synthetic Primary Key)

- **name:** Product name as it appears on the app

- **category:** Product category like Fruits, Snacks, Beverages, etc.

- **mrp:** Maximum Retail Price (originally in paise, converted to ₹)

- **discountPercent:** Discount applied on MRP

- **discountedSellingPrice:** Final price after discount (also converted to ₹)

- **availableQuantity:** Units available in inventory

- **weightInGms:** Product weight in grams

- **outOfStock:** Boolean flag indicating stock availability

- **quantity:** Number of units per package (mixed with grams for loose produce)

## 🔧 Project Workflow

### 1. Database & Table Creation
We start by creating a SQL table with appropriate data types:

```sql
CREATE TABLE zepto (
  sku_id INT AUTO INCREMENT PRIMARY KEY,
  category VARCHAR(120),
  name VARCHAR(150) NOT NULL,
  mrp NUMERIC(8,2),
  discountPercent NUMERIC(5,2),
  availableQuantity INTEGER,
  discountedSellingPrice NUMERIC(8,2),
  weightInGms INTEGER,
  outOfStock BOOLEAN,
  quantity INTEGER
);
```

### 2. Data Import
- Before Begining:
- Open the CSV file (zepto_v2.csv) in Excel or Notepad.
- Re-save it as:
- CSV UTF-8 (Comma delimited) (*.csv). This fixes any encoding-related issue.

- In the left panel under “Schemas”, right-click zepto_sql_project.
- Click “Set as Default Schema” (this is important).

- In MySQL Workbench, go to:
- Server → Data Import

 OR
 
- Right-click the Zepto table → Table Data Import Wizard.
- In the wizard:
- Choose your file: C:\Users\Megha\OneDrive\Desktop\zepto_v2.csv

- If prompted, choose to import into existing table → Select Zepto.
- Click Next. Ultimately, click Finish.

  
### 3. 🔍 Data Exploration
- Counted the total number of records in the dataset

  ```sql
  select count(*) from zepto;
  ```

- Viewed a sample of the dataset to understand structure and content

  ```sql
  SELECT * FROM zepto
  LIMIT 10;
  ```

- Checked for null values across all columns

  ```sql
  SELECT * FROM zepto
  WHERE name IS NULL
  OR
  category IS NULL
  OR
  mrp IS NULL
  OR
  discountPercent IS NULL
  OR
  discountedSellingPrice IS NULL
  OR
  weightInGms IS NULL
  OR
  availableQuantity IS NULL
  OR
  outOfStock IS NULL
  OR
  quantity IS NULL;
  ```

- Identified distinct product categories available in the dataset

  ```sql
  SELECT DISTINCT category
  FROM zepto
  ORDER BY category;
  ```

- Compared in-stock vs out-of-stock product counts

  ```sql
  SELECT outOfStock, COUNT(sku_id)
  FROM zepto
  GROUP BY outOfStock;
  ```

- Detected products present multiple times, representing different SKUs

  ```sql
  SELECT name, COUNT(sku_id) AS `Number of SKUs`
  FROM zepto
  GROUP BY name
  HAVING count(sku_id) > 1
  ORDER BY count(sku_id) DESC;
  ```  

### 4. 🧹 Data Cleaning
- Identified and removed rows where MRP or discounted selling price was zero

  ```sql
  SELECT * FROM zepto
  WHERE mrp = 0 OR discountedSellingPrice = 0;

  DELETE FROM zepto
  WHERE mrp = 0;
  ```

- Converted mrp and discountedSellingPrice from paise to rupees for consistency and readability

  ```sql
  UPDATE zepto
  SET mrp = mrp / 100.0,
  discountedSellingPrice = discountedSellingPrice / 100.0;

  SELECT mrp, discountedSellingPrice FROM zepto;
  ```
  
### 5. 📊 Business Insights
- Found top 10 best-value products based on discount percentage

  ```sql
  SELECT DISTINCT name, mrp, discountPercent
  FROM zepto
  ORDER BY discountPercent DESC
  LIMIT 10;
  ```

- Identified high-MRP products that are currently out of stock

  ```sql
  SELECT DISTINCT name,mrp
  FROM zepto
  WHERE outOfStock = TRUE and mrp > 300
  ORDER BY mrp DESC;
  ```

- Estimated potential revenue for each product category

  ```sql
  SELECT category,
  SUM(discountedSellingPrice * availableQuantity) AS total_revenue
  FROM zepto
  GROUP BY category
  ORDER BY total_revenue;
  ```

- Filtered expensive products (MRP > ₹500) with minimal discount

  ```sql
  SELECT DISTINCT name, mrp, discountPercent
  FROM zepto
  WHERE mrp > 500 AND discountPercent < 10
  ORDER BY mrp DESC, discountPercent DESC;
  ```

- Ranked top 5 categories offering highest average discounts

  ```sql
  SELECT category,
  ROUND(AVG(discountPercent),2) AS avg_discount
  FROM zepto
  GROUP BY category
  ORDER BY avg_discount DESC
  LIMIT 5;
  ```

- Calculated price per gram to identify value-for-money products

  ```sql
  SELECT DISTINCT name, weightInGms, discountedSellingPrice,
  ROUND(discountedSellingPrice/weightInGms,2) AS price_per_gram
  FROM zepto
  WHERE weightInGms >= 100
  ORDER BY price_per_gram;
  ```

- Grouped products based on weight into Low, Medium, and Bulk categories

  ```sql
  SELECT DISTINCT name, weightInGms,
  CASE WHEN weightInGms < 1000 THEN 'Low'
	  WHEN weightInGms < 5000 THEN 'Medium'
	  ELSE 'Bulk'
	  END AS weight_category
  FROM zepto;
  ```
  
- Measured total inventory weight per product category

  ```sql
  SELECT category,
  SUM(weightInGms * availableQuantity) AS total_weight
  FROM zepto
  GROUP BY category
  ORDER BY total_weight;
  ```
