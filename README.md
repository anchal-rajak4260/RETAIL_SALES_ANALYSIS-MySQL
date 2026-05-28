# RETAIL_SALES-Using-MySQL

## Project Overview
Project Title: Retail Sales Analysis
Level: Beginner
Database: sql_proj_26

This project is designed to demonstrate SQL skills and techniques typically used by data analysts to explore, clean, and analyze retail sales data. The project involves setting up a retail sales database, performing exploratory data analysis (EDA), and answering specific business questions through SQL queries. This project is ideal for those who are starting their journey in data analysis and want to build a solid foundation in SQL.

## Objectives
1. Set up a retail sales database: Create and populate a retail sales database with the provided sales data.
2. Data Cleaning: Identify and remove any records with missing or null values.
3. Exploratory Data Analysis (EDA): Perform basic exploratory data analysis to understand the dataset.
4. Business Analysis: Use SQL to answer specific business questions and derive insights from the sales data.
5. 
## Project Structure

1. Database Setup
a. Database Creation: The project starts by creating a database named sql_proj_26.
b. Table Creation: A table named retail_sales is created to store the sales data. The table structure includes columns for transaction ID, sale date, sale time, customer ID, gender, age, product category, quantity sold, price per unit, cost of goods sold (COGS), and total sale amount.

CREATE DATABASE sql_proj_26;

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

2. Data Exploration & Cleaning
Record Count: Determine the total number of records in the dataset.
Customer Count: Find out how many unique customers are in the dataset.
Category Count: Identify all unique product categories in the dataset.
Null Value Check: Check for any null values in the dataset and delete records with missing data.

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
    
3. Data Analysis & Findings
The following SQL queries were developed to answer specific business questions:

A. Write a SQL query to retrieve all columns for sales made on '2022-11-05:
SELECT *
FROM retail_sales
WHERE sale_date = '2022-11-05';

B. Write a SQL query to retrieve all transactions where the category is 'Clothing' and the quantity sold is more than 4 in the month of Nov-2022:
SELECT 
  *
FROM retail_sales
WHERE 
    category = 'Clothing'
    AND 
    TO_CHAR(sale_date, 'YYYY-MM') = '2022-11'
    AND
    quantity >= 4
    
C. Write a SQL query to calculate the total sales (total_sale) for each category.:
SELECT 
    category,
    SUM(total_sale) as net_sale,
    COUNT(*) as total_orders
FROM retail_sales
GROUP BY 1

D. Write a SQL query to find the average age of customers who purchased items from the 'Beauty' category.:
SELECT
    ROUND(AVG(age), 2) as avg_age
FROM retail_sales
WHERE category = 'Beauty'

E. Write a SQL query to find all transactions where the total_sale is greater than 1000.:
SELECT * FROM retail_sales
WHERE total_sale > 1000

F. Write a SQL query to find the total number of transactions (transaction_id) made by each gender in each category.:
SELECT 
    category,
    gender,
    COUNT(*) as total_trans
FROM retail_sales
GROUP 
    BY 
    category,
    gender
ORDER BY 1

G. Write a SQL query to calculate the average sale for each month. Find out best selling month in each year:
SELECT 
       year,
       month,
    avg_sale
FROM 
(    
SELECT 
    EXTRACT(YEAR FROM sale_date) as year,
    EXTRACT(MONTH FROM sale_date) as month,
    AVG(total_sale) as avg_sale,
    RANK() OVER(PARTITION BY EXTRACT(YEAR FROM sale_date) ORDER BY AVG(total_sale) DESC) as rank
FROM retail_sales
GROUP BY 1, 2
) as t1
WHERE rank = 1
**Write a SQL query to find the top 5 customers based on the highest total sales **:
SELECT 
    customer_id,
    SUM(total_sale) as total_sales
FROM retail_sales
GROUP BY 1
ORDER BY 2 DESC
LIMIT 5
Write a SQL query to find the number of unique customers who purchased items from each category.:
SELECT 
    category,    
    COUNT(DISTINCT customer_id) as cnt_unique_cs
FROM retail_sales
GROUP BY category
Write a SQL query to create each shift and number of orders (Example Morning <12, Afternoon Between 12 & 17, Evening >17):
WITH hourly_sale
AS
(
SELECT *,
    CASE
        WHEN EXTRACT(HOUR FROM sale_time) < 12 THEN 'Morning'
        WHEN EXTRACT(HOUR FROM sale_time) BETWEEN 12 AND 17 THEN 'Afternoon'
        ELSE 'Evening'
    END as shift
FROM retail_sales
)
SELECT 
    shift,
    COUNT(*) as total_orders    
FROM hourly_sale
GROUP BY shift
Findings
Customer Demographics: The dataset includes customers from various age groups, with sales distributed across different categories such as Clothing and Beauty.
High-Value Transactions: Several transactions had a total sale amount greater than 1000, indicating premium purchases.
Sales Trends: Monthly analysis shows variations in sales, helping identify peak seasons.
Customer Insights: The analysis identifies the top-spending customers and the most popular product categories.
Reports
Sales Summary: A detailed report summarizing total sales, customer demographics, and category performance.
Trend Analysis: Insights into sales trends across different months and shifts.
Customer Insights: Reports on top customers and unique customer counts per category.
Conclusion
This project serves as a comprehensive introduction to SQL for data analysts, covering database setup, data cleaning, exploratory data analysis, and business-driven SQL queries. The findings from this project can help drive business decisions by understanding sales patterns, customer behavior, and product performance.   







<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Retail Sales Analysis — README</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=DM+Mono:ital,wght@0,400;0,500;1,400&family=Fraunces:ital,opsz,wght@0,9..144,300;0,9..144,600;1,9..144,300&display=swap" rel="stylesheet">
<style>
  *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

  :root {
    --bg: #0e0f11;
    --surface: #16181c;
    --surface2: #1d2026;
    --border: #2a2d35;
    --border2: #363a45;
    --text: #e8e6df;
    --muted: #8a8a8a;
    --dim: #454852;
    --accent: #d4a853;
    --accent2: #4e9f7d;
    --accent3: #5b8de8;
    --mono: 'DM Mono', monospace;
    --serif: 'Fraunces', serif;
  }

  html { background: var(--bg); color: var(--text); font-family: var(--mono); font-size: 15px; line-height: 1.7; scroll-behavior: smooth; }
  body { max-width: 980px; margin: 0 auto; padding: 0 2rem 6rem; display: grid; grid-template-columns: 220px 1fr; gap: 3rem; align-items: start; }

  /* ── NAV SIDEBAR ── */
  nav {
    position: sticky; top: 2rem;
    padding: 1.5rem 0;
    border-right: 1px solid var(--border);
    min-height: 100vh;
  }
  .nav-logo { font-family: var(--serif); font-size: 1rem; color: var(--accent); font-style: italic; margin-bottom: 2rem; padding-right: 1.5rem; line-height: 1.3; }
  .nav-section { font-size: 10px; color: var(--dim); letter-spacing: .12em; text-transform: uppercase; margin: 1.2rem 0 .5rem; padding-right: 1.5rem; }
  nav a {
    display: block; font-size: 12px; color: var(--muted);
    text-decoration: none; padding: .3rem 1.5rem .3rem 0;
    border-right: 2px solid transparent;
    transition: color .15s, border-color .15s;
  }
  nav a:hover { color: var(--text); }
  nav a.active { color: var(--accent); border-right-color: var(--accent); }

  /* ── MAIN CONTENT ── */
  main { padding: 3rem 0; min-width: 0; }

  /* ── HEADER ── */
  .header {
    border: 1px solid var(--border2); border-radius: 12px;
    padding: 3rem; margin-bottom: 3rem;
    position: relative; overflow: hidden; background: var(--surface);
  }
  .header::before {
    content: ''; position: absolute; top: 0; right: 0;
    width: 320px; height: 320px;
    background: radial-gradient(circle at top right, rgba(212,168,83,0.08) 0%, transparent 65%);
    pointer-events: none;
  }
  .header::after {
    content: 'SQL'; position: absolute; bottom: -24px; right: 20px;
    font-family: var(--serif); font-size: 150px; font-weight: 600;
    color: rgba(255,255,255,0.022); letter-spacing: -6px;
    pointer-events: none; user-select: none;
  }
  .badges { display: flex; flex-wrap: wrap; gap: 8px; margin-bottom: 1.5rem; }
  .badge { font-size: 11px; padding: 3px 12px; border-radius: 99px; border: 1px solid; letter-spacing: .06em; font-weight: 500; }
  .badge-gold  { color: var(--accent);  border-color: rgba(212,168,83,.35);  background: rgba(212,168,83,.07); }
  .badge-green { color: var(--accent2); border-color: rgba(78,159,125,.35);  background: rgba(78,159,125,.07); }
  .badge-blue  { color: var(--accent3); border-color: rgba(91,141,232,.35);  background: rgba(91,141,232,.07); }
  .badge-dim   { color: var(--muted);   border-color: var(--border2);         background: transparent; }

  h1 { font-family: var(--serif); font-size: 2.8rem; font-weight: 300; line-height: 1.1; color: var(--text); margin-bottom: .5rem; }
  h1 em { font-style: italic; color: var(--accent); }
  .header-sub { font-size: 13px; color: var(--muted); }

  /* ── METRICS ── */
  .metrics { display: grid; grid-template-columns: repeat(4, 1fr); gap: 1px; background: var(--border); border-radius: 10px; overflow: hidden; margin-bottom: 3rem; }
  .metric { background: var(--surface); padding: 1.4rem; text-align: center; }
  .metric .val { font-family: var(--serif); font-size: 2.2rem; font-weight: 600; color: var(--accent); line-height: 1; }
  .metric .lbl { font-size: 11px; color: var(--muted); margin-top: 6px; letter-spacing: .05em; }

  /* ── SECTIONS ── */
  .section { margin-bottom: 3rem; scroll-margin-top: 2rem; }
  .section-label {
    font-size: 10px; letter-spacing: .15em; text-transform: uppercase;
    color: var(--dim); margin-bottom: 1.5rem;
    display: flex; align-items: center; gap: 12px;
  }
  .section-label::after { content: ''; flex: 1; height: 1px; background: var(--border); }
  h2 { font-family: var(--serif); font-size: 1.4rem; font-weight: 300; color: var(--text); margin-bottom: 1rem; }
  p  { font-size: 13px; color: var(--muted); line-height: 1.75; margin-bottom: .75rem; }

  /* ── OBJECTIVES ── */
  .obj-grid { display: grid; grid-template-columns: repeat(2, 1fr); gap: 12px; }
  .obj { background: var(--surface); border: 1px solid var(--border); border-radius: 10px; padding: 1.25rem; transition: border-color .2s; }
  .obj:hover { border-color: var(--border2); }
  .obj-num   { font-size: 11px; color: var(--accent); margin-bottom: .5rem; letter-spacing: .1em; }
  .obj-title { font-size: 13px; font-weight: 500; color: var(--text); margin-bottom: .35rem; }
  .obj-desc  { font-size: 12px; color: var(--muted); line-height: 1.6; }

  /* ── SCHEMA TABLE ── */
  .schema-wrap { overflow-x: auto; border-radius: 10px; border: 1px solid var(--border); }
  table { width: 100%; border-collapse: collapse; font-size: 12px; }
  th { background: var(--surface2); color: var(--muted); font-weight: 500; text-align: left; padding: .65rem 1rem; letter-spacing: .05em; font-size: 11px; border-bottom: 1px solid var(--border); }
  td { padding: .6rem 1rem; border-bottom: 1px solid var(--border); color: var(--text); vertical-align: top; }
  tr:last-child td { border-bottom: none; }
  tr:hover td { background: var(--surface2); }
  .type { color: var(--accent3); }
  .pk   { color: var(--accent); font-size: 10px; letter-spacing: .08em; }
  .desc-col { color: var(--muted); }

  /* ── STEPS ── */
  .steps { display: flex; flex-direction: column; }
  .step { display: flex; gap: 1.5rem; padding: 1.5rem 0; border-bottom: 1px solid var(--border); }
  .step:last-child { border-bottom: none; }
  .step-idx {
    width: 32px; height: 32px; flex-shrink: 0;
    border: 1px solid var(--border2); border-radius: 8px;
    display: flex; align-items: center; justify-content: center;
    font-size: 12px; color: var(--accent); font-weight: 500;
  }
  .step-body h3 { font-size: 14px; font-weight: 500; color: var(--text); margin-bottom: .35rem; }
  .step-body p  { font-size: 12px; margin-bottom: .5rem; }

  /* ── CODE BLOCKS ── */
  .code-wrap { position: relative; margin: .6rem 0; }
  pre {
    background: #111318; border: 1px solid var(--border); border-radius: 8px;
    padding: .9rem 3rem .9rem 1.1rem; overflow-x: auto;
    font-family: var(--mono); font-size: 11.5px; line-height: 1.7; color: #c9c5bc;
  }
  .copy-btn {
    position: absolute; top: 8px; right: 8px;
    background: var(--surface2); border: 1px solid var(--border2);
    color: var(--muted); font-family: var(--mono); font-size: 10px;
    padding: 3px 9px; border-radius: 5px; cursor: pointer;
    transition: background .15s, color .15s; letter-spacing: .04em;
  }
  .copy-btn:hover { background: var(--border2); color: var(--text); }
  .copy-btn.copied { color: var(--accent2); border-color: var(--accent2); }
  .kw  { color: #5b8de8; }
  .fn  { color: #d4a853; }
  .str { color: #4e9f7d; }
  .cmt { color: #4a4d58; font-style: italic; }
  .num { color: #c0604a; }

  /* ── QUERIES ── */
  .query-card { background: var(--surface); border: 1px solid var(--border); border-radius: 10px; margin-bottom: 1rem; overflow: hidden; }
  .query-header { padding: .7rem 1.1rem; border-bottom: 1px solid var(--border); display: flex; align-items: center; gap: 10px; }
  .q-num   { font-size: 10px; color: var(--accent); letter-spacing: .1em; font-weight: 500; }
  .q-title { font-size: 12px; color: var(--muted); }
  .query-card .code-wrap { margin: 0; }
  .query-card pre { border: none; border-radius: 0; background: #0d0e10; padding-right: 3rem; }
  .query-card .copy-btn { top: 6px; right: 6px; background: #16181c; }

  /* ── FINDINGS ── */
  .findings { display: grid; grid-template-columns: repeat(2, 1fr); gap: 12px; }
  .finding {
    background: var(--surface); border: 1px solid var(--border);
    border-left: 3px solid var(--accent); border-radius: 0 10px 10px 0;
    padding: 1.1rem 1.25rem;
  }
  .finding h3 { font-size: 13px; font-weight: 500; color: var(--text); margin-bottom: .4rem; }
  .finding p  { font-size: 12px; color: var(--muted); margin: 0; }

  /* ── REPORTS ── */
  .reports { display: grid; grid-template-columns: repeat(3, 1fr); gap: 12px; }
  .report { background: var(--surface); border: 1px solid var(--border); border-radius: 10px; padding: 1.25rem; text-align: center; }
  .report-icon { font-size: 22px; margin-bottom: .75rem; }
  .report h3 { font-size: 13px; font-weight: 500; color: var(--text); margin-bottom: .4rem; }
  .report p  { font-size: 11px; color: var(--muted); margin: 0; }

  /* ── CONCLUSION ── */
  .conclusion {
    background: var(--surface); border: 1px solid var(--border2);
    border-top: 3px solid var(--accent); border-radius: 0 0 12px 12px;
    padding: 2rem;
  }
  .conclusion h2 { margin-bottom: .75rem; }
  .conclusion p  { margin-bottom: .6rem; }
  .conclusion p:last-child { margin-bottom: 0; }

  /* ── GETTING STARTED ── */
  .start-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 12px; }
  .start-card { background: var(--surface); border: 1px solid var(--border); border-radius: 10px; padding: 1.1rem 1.25rem; }
  .start-card h3 { font-size: 12px; color: var(--muted); margin-bottom: .5rem; letter-spacing: .06em; }

  /* ── FOOTER ── */
  .footer { margin-top: 4rem; padding-top: 1.5rem; border-top: 1px solid var(--border); display: flex; justify-content: space-between; align-items: center; }
  .footer p { font-size: 11px; color: var(--dim); margin: 0; }

  @media (max-width: 720px) {
    body { grid-template-columns: 1fr; }
    nav  { display: none; }
    .metrics { grid-template-columns: repeat(2,1fr); }
    .obj-grid, .findings, .start-grid { grid-template-columns: 1fr; }
    .reports { grid-template-columns: 1fr; }
    h1 { font-size: 2rem; }
  }
</style>
</head>
<body>

<!-- ── SIDEBAR NAV ── -->
<nav id="sidebar">
  <div class="nav-logo">Retail Sales<br>Analysis</div>

  <div class="nav-section">Project</div>
  <a href="#overview">Overview</a>
  <a href="#objectives">Objectives</a>

  <div class="nav-section">Database</div>
  <a href="#schema">Schema</a>
  <a href="#structure">Structure</a>

  <div class="nav-section">Analysis</div>
  <a href="#queries">Queries (10)</a>
  <a href="#findings">Findings</a>
  <a href="#reports">Reports</a>

  <div class="nav-section">Wrap-up</div>
  <a href="#start">Getting started</a>
  <a href="#conclusion">Conclusion</a>
</nav>

<!-- ── MAIN ── -->
<main>

  <!-- HEADER -->
  <header class="header" id="top">
    <div class="badges">
      <span class="badge badge-gold">SQL</span>
      <span class="badge badge-green">PostgreSQL</span>
      <span class="badge badge-blue">Data Analysis</span>
      <span class="badge badge-dim">Beginner</span>
      <span class="badge badge-dim">EDA</span>
    </div>
    <h1>Retail Sales<br><em>Analysis</em></h1>
    <p class="header-sub">Database: p1_retail_db &nbsp;·&nbsp; A beginner SQL data analysis project</p>
  </header>

  <!-- METRICS -->
  <div class="metrics">
    <div class="metric"><div class="val">4</div><div class="lbl">Objectives</div></div>
    <div class="metric"><div class="val">10+</div><div class="lbl">SQL Queries</div></div>
    <div class="metric"><div class="val">11</div><div class="lbl">Table Columns</div></div>
    <div class="metric"><div class="val">3</div><div class="lbl">Report Types</div></div>
  </div>

  <!-- OVERVIEW -->
  <section class="section" id="overview">
    <div class="section-label">Overview</div>
    <h2>About this project</h2>
    <p>This project demonstrates SQL skills typically used by data analysts to explore, clean, and analyze retail sales data. It covers setting up a retail database, performing exploratory data analysis, and answering specific business questions through targeted SQL queries.</p>
    <p>Ideal for anyone starting their journey in data analysis who wants to build a solid foundation in SQL — from schema design all the way to business insight generation.</p>
  </section>

  <!-- OBJECTIVES -->
  <section class="section" id="objectives">
    <div class="section-label">Objectives</div>
    <div class="obj-grid">
      <div class="obj">
        <div class="obj-num">01 — Setup</div>
        <div class="obj-title">Database creation</div>
        <div class="obj-desc">Create and populate a retail sales database with the provided sales data.</div>
      </div>
      <div class="obj">
        <div class="obj-num">02 — Cleaning</div>
        <div class="obj-title">Data cleaning</div>
        <div class="obj-desc">Identify and remove any records with missing or null values.</div>
      </div>
      <div class="obj">
        <div class="obj-num">03 — EDA</div>
        <div class="obj-title">Exploratory analysis</div>
        <div class="obj-desc">Perform basic exploratory data analysis to understand the dataset's shape and contents.</div>
      </div>
      <div class="obj">
        <div class="obj-num">04 — Insights</div>
        <div class="obj-title">Business analysis</div>
        <div class="obj-desc">Use SQL to answer specific business questions and derive actionable insights.</div>
      </div>
    </div>
  </section>

  <!-- SCHEMA -->
  <section class="section" id="schema">
    <div class="section-label">Schema</div>
    <h2>Table: retail_sales</h2>
    <div class="schema-wrap">
      <table>
        <thead>
          <tr><th>Column</th><th>Type</th><th>Key</th><th>Description</th></tr>
        </thead>
        <tbody>
          <tr><td>transactions_id</td><td class="type">INT</td><td><span class="pk">PK</span></td><td class="desc-col">Unique transaction identifier</td></tr>
          <tr><td>sale_date</td><td class="type">DATE</td><td></td><td class="desc-col">Date the sale occurred</td></tr>
          <tr><td>sale_time</td><td class="type">TIME</td><td></td><td class="desc-col">Time the sale was recorded</td></tr>
          <tr><td>customer_id</td><td class="type">INT</td><td></td><td class="desc-col">Customer reference number</td></tr>
          <tr><td>gender</td><td class="type">VARCHAR(10)</td><td></td><td class="desc-col">Customer gender</td></tr>
          <tr><td>age</td><td class="type">INT</td><td></td><td class="desc-col">Customer age in years</td></tr>
          <tr><td>category</td><td class="type">VARCHAR(35)</td><td></td><td class="desc-col">Product category (e.g. Clothing, Beauty)</td></tr>
          <tr><td>quantity</td><td class="type">INT</td><td></td><td class="desc-col">Units sold per transaction</td></tr>
          <tr><td>price_per_unit</td><td class="type">FLOAT</td><td></td><td class="desc-col">Sale price per unit</td></tr>
          <tr><td>cogs</td><td class="type">FLOAT</td><td></td><td class="desc-col">Cost of goods sold</td></tr>
          <tr><td>total_sale</td><td class="type">FLOAT</td><td></td><td class="desc-col">Total sale value for the transaction</td></tr>
        </tbody>
      </table>
    </div>
  </section>

  <!-- STRUCTURE -->
  <section class="section" id="structure">
    <div class="section-label">Project structure</div>
    <div class="steps">

      <div class="step">
        <div class="step-idx">1</div>
        <div class="step-body">
          <h3>Database setup</h3>
          <p>Create the database and define the table schema.</p>
          <div class="code-wrap">
            <button class="copy-btn" onclick="copyCode(this)">copy</button>
            <pre><span class="kw">CREATE DATABASE</span> p1_retail_db;

<span class="kw">CREATE TABLE</span> retail_sales (
  transactions_id  <span class="fn">INT</span> <span class="kw">PRIMARY KEY</span>,
  sale_date        <span class="fn">DATE</span>,
  sale_time        <span class="fn">TIME</span>,
  customer_id      <span class="fn">INT</span>,
  gender           <span class="fn">VARCHAR</span>(<span class="num">10</span>),
  age              <span class="fn">INT</span>,
  category         <span class="fn">VARCHAR</span>(<span class="num">35</span>),
  quantity         <span class="fn">INT</span>,
  price_per_unit   <span class="fn">FLOAT</span>,
  cogs             <span class="fn">FLOAT</span>,
  total_sale       <span class="fn">FLOAT</span>
);</pre>
          </div>
        </div>
      </div>

      <div class="step">
        <div class="step-idx">2</div>
        <div class="step-body">
          <h3>Data exploration &amp; cleaning</h3>
          <p>Audit record counts, unique customers and categories, then remove incomplete rows.</p>
          <div class="code-wrap">
            <button class="copy-btn" onclick="copyCode(this)">copy</button>
            <pre><span class="kw">SELECT</span> <span class="fn">COUNT</span>(*) <span class="kw">FROM</span> retail_sales;
<span class="kw">SELECT</span> <span class="fn">COUNT</span>(<span class="kw">DISTINCT</span> customer_id) <span class="kw">FROM</span> retail_sales;
<span class="kw">SELECT</span> <span class="kw">DISTINCT</span> category <span class="kw">FROM</span> retail_sales;

<span class="cmt">-- Remove null records</span>
<span class="kw">DELETE FROM</span> retail_sales
<span class="kw">WHERE</span>
  sale_date      <span class="kw">IS NULL</span> <span class="kw">OR</span> sale_time      <span class="kw">IS NULL</span>
  <span class="kw">OR</span> customer_id <span class="kw">IS NULL</span> <span class="kw">OR</span> gender        <span class="kw">IS NULL</span>
  <span class="kw">OR</span> age         <span class="kw">IS NULL</span> <span class="kw">OR</span> category      <span class="kw">IS NULL</span>
  <span class="kw">OR</span> quantity    <span class="kw">IS NULL</span> <span class="kw">OR</span> price_per_unit <span class="kw">IS NULL</span>
  <span class="kw">OR</span> cogs        <span class="kw">IS NULL</span>;</pre>
          </div>
        </div>
      </div>

      <div class="step">
        <div class="step-idx">3</div>
        <div class="step-body">
          <h3>Business analysis queries</h3>
          <p>Ten targeted queries that answer real business questions — see the queries section below.</p>
        </div>
      </div>

    </div>
  </section>

  <!-- QUERIES -->
  <section class="section" id="queries">
    <div class="section-label">Analysis queries</div>

    <div class="query-card">
      <div class="query-header"><span class="q-num">Q1</span><span class="q-title">Sales on a specific date</span></div>
      <div class="code-wrap"><button class="copy-btn" onclick="copyCode(this)">copy</button>
      <pre><span class="kw">SELECT</span> * <span class="kw">FROM</span> retail_sales
<span class="kw">WHERE</span> sale_date = <span class="str">'2022-11-05'</span>;</pre></div>
    </div>

    <div class="query-card">
      <div class="query-header"><span class="q-num">Q2</span><span class="q-title">Clothing transactions — Nov 2022, qty ≥ 4</span></div>
      <div class="code-wrap"><button class="copy-btn" onclick="copyCode(this)">copy</button>
      <pre><span class="kw">SELECT</span> * <span class="kw">FROM</span> retail_sales
<span class="kw">WHERE</span> category = <span class="str">'Clothing'</span>
  <span class="kw">AND</span> <span class="fn">TO_CHAR</span>(sale_date, <span class="str">'YYYY-MM'</span>) = <span class="str">'2022-11'</span>
  <span class="kw">AND</span> quantity >= <span class="num">4</span>;</pre></div>
    </div>

    <div class="query-card">
      <div class="query-header"><span class="q-num">Q3</span><span class="q-title">Total sales and order count per category</span></div>
      <div class="code-wrap"><button class="copy-btn" onclick="copyCode(this)">copy</button>
      <pre><span class="kw">SELECT</span>
  category,
  <span class="fn">SUM</span>(total_sale) <span class="kw">AS</span> net_sale,
  <span class="fn">COUNT</span>(*)        <span class="kw">AS</span> total_orders
<span class="kw">FROM</span> retail_sales
<span class="kw">GROUP BY</span> <span class="num">1</span>;</pre></div>
    </div>

    <div class="query-card">
      <div class="query-header"><span class="q-num">Q4</span><span class="q-title">Average customer age in the Beauty category</span></div>
      <div class="code-wrap"><button class="copy-btn" onclick="copyCode(this)">copy</button>
      <pre><span class="kw">SELECT</span> <span class="fn">ROUND</span>(<span class="fn">AVG</span>(age), <span class="num">2</span>) <span class="kw">AS</span> avg_age
<span class="kw">FROM</span> retail_sales
<span class="kw">WHERE</span> category = <span class="str">'Beauty'</span>;</pre></div>
    </div>

    <div class="query-card">
      <div class="query-header"><span class="q-num">Q5</span><span class="q-title">High-value transactions (total_sale &gt; 1000)</span></div>
      <div class="code-wrap"><button class="copy-btn" onclick="copyCode(this)">copy</button>
      <pre><span class="kw">SELECT</span> * <span class="kw">FROM</span> retail_sales
<span class="kw">WHERE</span> total_sale > <span class="num">1000</span>;</pre></div>
    </div>

    <div class="query-card">
      <div class="query-header"><span class="q-num">Q6</span><span class="q-title">Transaction count by gender and category</span></div>
      <div class="code-wrap"><button class="copy-btn" onclick="copyCode(this)">copy</button>
      <pre><span class="kw">SELECT</span> category, gender, <span class="fn">COUNT</span>(*) <span class="kw">AS</span> total_trans
<span class="kw">FROM</span> retail_sales
<span class="kw">GROUP BY</span> category, gender
<span class="kw">ORDER BY</span> <span class="num">1</span>;</pre></div>
    </div>

    <div class="query-card">
      <div class="query-header"><span class="q-num">Q7</span><span class="q-title">Best-selling month per year (window function)</span></div>
      <div class="code-wrap"><button class="copy-btn" onclick="copyCode(this)">copy</button>
      <pre><span class="kw">SELECT</span> year, month, avg_sale
<span class="kw">FROM</span> (
  <span class="kw">SELECT</span>
    <span class="fn">EXTRACT</span>(<span class="kw">YEAR  FROM</span> sale_date) <span class="kw">AS</span> year,
    <span class="fn">EXTRACT</span>(<span class="kw">MONTH FROM</span> sale_date) <span class="kw">AS</span> month,
    <span class="fn">AVG</span>(total_sale)               <span class="kw">AS</span> avg_sale,
    <span class="fn">RANK</span>() <span class="kw">OVER</span>(
      <span class="kw">PARTITION BY</span> <span class="fn">EXTRACT</span>(<span class="kw">YEAR FROM</span> sale_date)
      <span class="kw">ORDER BY</span>     <span class="fn">AVG</span>(total_sale) <span class="kw">DESC</span>
    ) <span class="kw">AS</span> rank
  <span class="kw">FROM</span> retail_sales
  <span class="kw">GROUP BY</span> <span class="num">1</span>, <span class="num">2</span>
) t1
<span class="kw">WHERE</span> rank = <span class="num">1</span>;</pre></div>
    </div>

    <div class="query-card">
      <div class="query-header"><span class="q-num">Q8</span><span class="q-title">Top 5 customers by total sales</span></div>
      <div class="code-wrap"><button class="copy-btn" onclick="copyCode(this)">copy</button>
      <pre><span class="kw">SELECT</span> customer_id, <span class="fn">SUM</span>(total_sale) <span class="kw">AS</span> total_sales
<span class="kw">FROM</span> retail_sales
<span class="kw">GROUP BY</span> <span class="num">1</span>
<span class="kw">ORDER BY</span> <span class="num">2</span> <span class="kw">DESC</span>
<span class="kw">LIMIT</span> <span class="num">5</span>;</pre></div>
    </div>

    <div class="query-card">
      <div class="query-header"><span class="q-num">Q9</span><span class="q-title">Unique customers per category</span></div>
      <div class="code-wrap"><button class="copy-btn" onclick="copyCode(this)">copy</button>
      <pre><span class="kw">SELECT</span> category, <span class="fn">COUNT</span>(<span class="kw">DISTINCT</span> customer_id) <span class="kw">AS</span> unique_customers
<span class="kw">FROM</span> retail_sales
<span class="kw">GROUP BY</span> category;</pre></div>
    </div>

    <div class="query-card">
      <div class="query-header"><span class="q-num">Q10</span><span class="q-title">Orders by time-of-day shift (CTE)</span></div>
      <div class="code-wrap"><button class="copy-btn" onclick="copyCode(this)">copy</button>
      <pre><span class="kw">WITH</span> hourly_sale <span class="kw">AS</span> (
  <span class="kw">SELECT</span> *,
    <span class="kw">CASE</span>
      <span class="kw">WHEN</span> <span class="fn">EXTRACT</span>(<span class="kw">HOUR FROM</span> sale_time) <span class="kw">BETWEEN</span> <span class="num">0</span>  <span class="kw">AND</span> <span class="num">11</span> <span class="kw">THEN</span> <span class="str">'Morning'</span>
      <span class="kw">WHEN</span> <span class="fn">EXTRACT</span>(<span class="kw">HOUR FROM</span> sale_time) <span class="kw">BETWEEN</span> <span class="num">12</span> <span class="kw">AND</span> <span class="num">17</span> <span class="kw">THEN</span> <span class="str">'Afternoon'</span>
      <span class="kw">ELSE</span> <span class="str">'Evening'</span>
    <span class="kw">END</span> <span class="kw">AS</span> shift
  <span class="kw">FROM</span> retail_sales
)
<span class="kw">SELECT</span> shift, <span class="fn">COUNT</span>(*) <span class="kw">AS</span> total_orders
<span class="kw">FROM</span> hourly_sale
<span class="kw">GROUP BY</span> shift;</pre></div>
    </div>

  </section>

  <!-- FINDINGS -->
  <section class="section" id="findings">
    <div class="section-label">Findings</div>
    <div class="findings">
      <div class="finding">
        <h3>Customer demographics</h3>
        <p>Sales span diverse age groups across multiple product categories including Clothing and Beauty.</p>
      </div>
      <div class="finding">
        <h3>High-value transactions</h3>
        <p>Several transactions exceeded 1,000 in total sale value, pointing to premium purchase behaviour.</p>
      </div>
      <div class="finding">
        <h3>Seasonal sales trends</h3>
        <p>Monthly averages reveal clear peak seasons — useful for inventory planning and promotions.</p>
      </div>
      <div class="finding">
        <h3>Customer insights</h3>
        <p>Top 5 spenders and per-category unique buyer counts identified for targeted loyalty programs.</p>
      </div>
    </div>
  </section>

  <!-- REPORTS -->
  <section class="section" id="reports">
    <div class="section-label">Reports</div>
    <div class="reports">
      <div class="report">
        <div class="report-icon">📊</div>
        <h3>Sales summary</h3>
        <p>Total sales, customer demographics, and category performance overview.</p>
      </div>
      <div class="report">
        <div class="report-icon">📈</div>
        <h3>Trend analysis</h3>
        <p>Sales trends across months and time-of-day shifts.</p>
      </div>
      <div class="report">
        <div class="report-icon">👤</div>
        <h3>Customer insights</h3>
        <p>Top spenders and unique customer counts per product category.</p>
      </div>
    </div>
  </section>

  <!-- GETTING STARTED -->
  <section class="section" id="start">
    <div class="section-label">Getting started</div>
    <h2>Run locally</h2>
    <div class="start-grid">
      <div class="start-card">
        <h3>Prerequisites</h3>
        <p style="font-size:12px;margin:0;">PostgreSQL 13+ installed and running locally or via a cloud provider such as Supabase or Railway.</p>
      </div>
      <div class="start-card">
        <h3>Tools</h3>
        <p style="font-size:12px;margin:0;">Any SQL client works — psql CLI, pgAdmin, DBeaver, or TablePlus.</p>
      </div>
    </div>
    <div class="code-wrap" style="margin-top:12px;">
      <button class="copy-btn" onclick="copyCode(this)">copy</button>
      <pre><span class="cmt">-- Step 1: Create the database</span>
<span class="kw">CREATE DATABASE</span> p1_retail_db;

<span class="cmt">-- Step 2: Run schema script</span>
\i schema.sql

<span class="cmt">-- Step 3: Load seed data</span>
\i data.sql

<span class="cmt">-- Step 4: Run analysis queries</span>
\i analysis.sql</pre>
    </div>
  </section>

  <!-- CONCLUSION -->
  <section class="section" id="conclusion">
    <div class="section-label">Conclusion</div>
    <div class="conclusion">
      <h2>What this project covers</h2>
      <p>This project serves as a comprehensive introduction to SQL for data analysts, covering database setup, data cleaning, exploratory data analysis, and business-driven SQL queries.</p>
      <p>The findings from this project can help drive business decisions by uncovering sales patterns, understanding customer behaviour, and evaluating product performance across categories and time periods.</p>
      <p>Key SQL concepts practised: <code style="font-family:var(--mono);font-size:11px;color:var(--accent3);">GROUP BY</code>, <code style="font-family:var(--mono);font-size:11px;color:var(--accent3);">WINDOW FUNCTIONS</code>, <code style="font-family:var(--mono);font-size:11px;color:var(--accent3);">CTEs</code>, <code style="font-family:var(--mono);font-size:11px;color:var(--accent3);">AGGREGATE FUNCTIONS</code>, <code style="font-family:var(--mono);font-size:11px;color:var(--accent3);">DATE EXTRACTION</code>, and <code style="font-family:var(--mono);font-size:11px;color:var(--accent3);">NULL HANDLING</code>.</p>
    </div>
  </section>

  <!-- FOOTER -->
  <footer class="footer">
    <p>Retail Sales Analysis &nbsp;·&nbsp; p1_retail_db &nbsp;·&nbsp; Beginner SQL Project</p>
    <p>Built with PostgreSQL</p>
  </footer>

</main>

<script>
  /* ── Copy to clipboard ── */
  function copyCode(btn) {
    const pre = btn.nextElementSibling;
    navigator.clipboard.writeText(pre.innerText).then(() => {
      btn.textContent = 'copied!';
      btn.classList.add('copied');
      setTimeout(() => { btn.textContent = 'copy'; btn.classList.remove('copied'); }, 1800);
    });
  }

  /* ── Active nav highlight on scroll ── */
  const sections = document.querySelectorAll('section[id], header[id]');
  const navLinks  = document.querySelectorAll('nav a');
  const observer  = new IntersectionObserver(entries => {
    entries.forEach(e => {
      if (e.isIntersecting) {
        navLinks.forEach(a => a.classList.remove('active'));
        const active = document.querySelector('nav a[href="#' + e.target.id + '"]');
        if (active) active.classList.add('active');
      }
    });
  }, { rootMargin: '-30% 0px -60% 0px' });
  sections.forEach(s => observer.observe(s));
</script>
</body>
</html>
