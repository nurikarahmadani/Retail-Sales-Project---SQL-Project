# Retail Sales Project - Data Analyst
Projek ini merupakan practice project yang bersumber dari youtube Zero Analyst. Projek ini dilakukan dengan menggunakan Tools SQL Server Management Studio. Struktur pengerjaan projek ini yaitu Database Setup, Data Exploration & Cleaning, dan Data Analysis & Findings

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
## Data Exploration and Cleaning
* Record Count: Menentukan jumlah record dalam dataset.
* Customer Count: Mengetahui jumlah customer unik dalam dataset
* Category Count: Identifikasi semua produk uni dalam dataset
* Null Value Check: Mengecek null dalam dataset dan menghapus data null

```SQL
SELECT COUNT(*) FROM retail_sales;
SELECT COUNT(DISTINCT customer_id) FROM retail_sales;
SELECT DISTINCT category FROM retail_sales;

SELECT * FROM retail_sales
WHERE 
    sale_date IS NULL OR sale_time IS NULL OR customer_id IS NULL OR 
    gender IS NULL OR age IS NULL OR category IS NULL OR 
    quantity IS NULL OR price_per_unit IS NULL OR cogs IS NULL;

DELETE FROM retail_sales
WHERE 
    sale_date IS NULL OR sale_time IS NULL OR customer_id IS NULL OR 
    gender IS NULL OR age IS NULL OR category IS NULL OR 
    quantity IS NULL OR price_per_unit IS NULL OR cogs IS NULL;
```

## Data Analysis and Finding
Kueri SQL berikut dikembangkan untuk menjawab pertanyaan bisnis tertentu:
### 1. Memperoleh data penjualan pada tanggal 2022-11-05.
```SQL
SELECT * FROM retail_sales_tb
WHERE sale_date = '2022-11-05'

```
### 2. Memperoleh semua tranksaksi kategori ‘clothing’ dan jumlah yang terjual atau quantity sold lebih dari 4 pada bulan november 2022
```SQL

```
### 3.	Menghitung total penjualan untuk tiap kategori
```SQL
SELECT * FROM retail_sales_tb
WHERE
    category = 'clothing' AND
    quantiy >= 4 AND
    FORMAT(sale_date, 'yyyy-MM') = '2022-11' ;

```
### 4.	Menghitung rerata usia kustomer kategori ‘beauty’
```SQL
SELECT
    category,
    COUNT(*) as total_pesanan,
    SUM(total_sale) as total_penjualan
FROM retail_sales_tb
GROUP BY category;
```
### 5.	Menampilkan ntranksaksi dengan total sales diatas 1000 usd
```SQL
SELECT * FROM retail_sales_tb
WHERE total_sale > 1000

```
### 6.	Menampilkan total tranksaksi untuk tiap gender tiap kategori
```SQL
SELECT
    category,
    gender,
    COUNT(*) as total_tranksaksi
FROM retail_sales_tb
GROUP BY category, gender
ORDER BY category


```
### 7.	Menghitung rata rata penjualan tiap bulan dan mencari bulan dengan pendapatan tertinggi tiap tahunnya
```SQL
WITH avg_sales_month AS(
    SELECT
        FORMAT(sale_date, 'yyyy') AS tahun,
        FORMAT(sale_date, 'MM') AS bulan,
        AVG(total_sale) as rerata_perbulan
    FROM retail_sales_tb
    GROUP BY
        FORMAT(sale_date, 'yyyy'),
        FORMAT(sale_date, 'MM')
)

SELECT
    tahun,
    bulan,
    rerata_perbulan
FROM
(
    SELECT
        tahun,
        bulan,
        rerata_perbulan,
        RANK() OVER (PARTITION BY tahun ORDER BY rerata_perbulan DESC) AS sales_rank
    FROM avg_sales_month
)AS t1
WHERE sales_rank = 1

```
### 8.	Mencari top 5 kustomer berdasarkan total sales kustomer tersebut
```SQL
SELECT TOP 5
    customer_id,
    SUM(total_sale) as total_sale
FROM retail_sales_tb
GROUP BY customer_id
ORDER BY total_sale DESC

```
### 9.	Mencari jumlah unique customer dari tiap kategori
```SQL
SELECT
    category,
    COUNT(DISTINCT customer_id) as jumlah_cust
FROM retail_sales_tb
GROUP BY category

```
### 10. Tulis query SQL untuk membuat setiap shift dan jumlah pesanan (Contoh Pagi <12, Siang Antara Jam 12 & 17, Malam >17)
```SQL
WITH shift_tb AS
(
    SELECT *,
        CASE
            WHEN DATEPART(HOUR, sale_time) <  12 THEN 'morning'
            WHEN DATEPART(HOUR, sale_time) BETWEEN 12 AND 17 THEN 'afternoon'
            ELSE 'evening'
            END AS shift
    FROM retail_sales_tb
)

SELECT
    shift,
    COUNT(*) as total_orders
FROM shift_tb
GROUP BY shift

```


