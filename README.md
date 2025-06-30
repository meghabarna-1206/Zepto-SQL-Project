# Zepto-SQL-Project
![Zepto logo](2.jpeg)

## 📌 Project Overview

The goal is to simulate how actual data analysts in the e-commerce or retail industries work behind the scenes to use SQL to:

✅ Set up a messy, real-world e-commerce inventory **database**

✅ Perform **Exploratory Data Analysis (EDA)** to explore product categories, availability, and pricing inconsistencies

✅ Implement **Data Cleaning** to handle null values, remove invalid entries, and convert pricing from paise to rupees

✅ Write **business-driven SQL queries** to derive insights around **pricing, inventory, stock availability, revenue** and more

## 📁 Dataset Overview
The dataset was sourced from [Kaggle](https://www.kaggle.com/datasets/palvinder2006/zepto-inventory-dataset/data?select=zepto_v2.csv) and was originally scraped from Zepto’s official product listings. It mimics what you’d typically encounter in a real-world e-commerce inventory system.

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
  sku_id SERIAL PRIMARY KEY,
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
- OR
- Right-click the Zepto table → Table Data Import Wizard.
- In the wizard:
- Choose your file: C:\Users\Megha\OneDrive\Desktop\zepto_v2.csv

- If prompted, choose to import into existing table → Select Zepto.
- Click Next. Ultimately, click Finish.

  
### 3. 🔍 Data Exploration
- Counted the total number of records in the dataset

- Viewed a sample of the dataset to understand structure and content

- Checked for null values across all columns

- Identified distinct product categories available in the dataset

- Compared in-stock vs out-of-stock product counts

- Detected products present multiple times, representing different SKUs

### 4. 🧹 Data Cleaning
- Identified and removed rows where MRP or discounted selling price was zero

- Converted mrp and discountedSellingPrice from paise to rupees for consistency and readability
  
### 5. 📊 Business Insights
- Found top 10 best-value products based on discount percentage

- Identified high-MRP products that are currently out of stock

- Estimated potential revenue for each product category

- Filtered expensive products (MRP > ₹500) with minimal discount

- Ranked top 5 categories offering highest average discounts

- Calculated price per gram to identify value-for-money products

- Grouped products based on weight into Low, Medium, and Bulk categories

- Measured total inventory weight per product category


## 🛠️ How to Use This Project

1. **Clone the repository**
   ```bash
   git clone https://github.com/amlanmohanty/zepto-SQL-data-analysis-project.git
   cd zepto-SQL-data-analysis-project
   ```
2. **Open zepto_SQL_data_analysis.sql**

    This file contains:

      - Table creation

      - Data exploration

      - Data cleaning

      - SQL Business analysis
  
3. **Load the dataset into pgAdmin or any other PostgreSQL client**

      - Create a database and run the SQL file

      - Import the dataset (convert to UTF-8 if necessary)

4. **Follow along with the YouTube video for full walkthrough. 👨‍💼**

## 📜 License

MIT — feel free to fork, star, and use in your portfolio.

## 👨‍💻 About the Author
Hey, I’m Amlan Mohanty — a Data Analyst & Content Creator.
I break down complex data topics into simple, practical content that actually helps you land a job.

 ### 🚀 Stay Connected & Join the Data Drool Community
If you enjoyed this project and want to keep learning and growing as a data analyst, let’s stay in touch! I regularly share content around SQL, data analytics, portfolio projects, job tips, and more.

🎥 YouTube: [Data Drool](https://www.youtube.com/@datadrool)
- Beginner-friendly tutorials, real-world projects, job and career advice

📺 Instagram: [data.drool](https://www.instagram.com/data.drool/)
- Quick SQL tips, data memes, and behind-the-scenes content

💼 LinkedIn: [Amlan Mohanty](https://www.linkedin.com/in/amlanmohanty1/)
- Let’s connect professionally and grow your data career


## 💡 Thanks for checking out the project! Your support means a lot — feel free to star ⭐ this repo or share it with someone learning SQL.🚀
