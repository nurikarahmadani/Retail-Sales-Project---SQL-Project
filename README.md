# Retail Sales Project - Data Analyst
Projek ini merupakan practice project yang bersumber dari youtube Zero Analyst.Projek ini dilakukan dengan menggunakan Tools SQL Server Management Studio. Struktur pengerjaan projek ini yaitu Database Setup, Data Exploration & Cleaning, dan Data Analysis & Findings

## Database Setup
```SQL
CREATE DATABASE p1_retail_db;

CREATE TABLE retail_sales
(
    transactions_id INT PRIMARY KEY,
    sale_date DATE,	
    sale_time TIME,
    customer_id INT,	
    gender VARCHAR(10),
    age INT,
    category VARCHAR(35),
    quantity INT,
    price_per_unit FLOAT,	
    cogs FLOAT,
    total_sale FLOAT
);
```

