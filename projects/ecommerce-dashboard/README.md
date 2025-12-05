# E-Commerce Inventory & Sales Intelligence Dashboard

## Project Overview

An interactive data analytics dashboard for e-commerce sales intelligence, built with Python, SQLite3, and Google Colab. This project demonstrates advanced SQL query capabilities, batch data processing, and professional data visualization techniques.

## Features

### Database Architecture

- **4-table normalized relational schema** with foreign key constraints
- **16,500+ records** across customers, products, orders, and reviews
- **Indexed columns** for optimized query performance
- **Referential integrity** enforcement

### Advanced SQL Capabilities

- **Window Functions**: RANK() for regional category rankings
- **Moving Averages**: 7-day rolling average with ROWS BETWEEN
- **Complex JOINs**: Multi-table queries across 3+ tables
- **Aggregations**: SUM, COUNT, AVG, MIN, MAX
- **Subqueries & CTEs**: WITH clauses for query optimization
- **LEFT JOINs**: Handling optional relationships

### Interactive Visualizations

1. **Regional Sales Heatmap** - Revenue by region × product category
2. **Daily Revenue Trends** - Time series with moving average
3. **Customer Lifetime Value** - Top 20 customers ranked by CLV
4. **Category Performance** - Revenue distribution pie chart
5. **Product Analysis** - Scatter plot of sales vs ratings

### Interactive Features

- Multi-select region and category filters
- Dynamic dashboard refresh
- Real-time data aggregation
- Hover tooltips with detailed information

## Technology Stack

- **Database**: SQLite3
- **Language**: Python 3.10
- **Data Processing**: Pandas, NumPy
- **Data Generation**: Faker
- **Visualization**: Plotly, Matplotlib, Seaborn
- **Interactive UI**: ipywidgets
- **Environment**: Google Colab / Jupyter Notebook

## Installation

### Using Conda (Recommended)

```bash
# Create conda environment
conda create -n ecommerce-db python=3.10 -y

# Activate environment
conda activate ecommerce-db

# Install dependencies
pip install -r requirements.txt
```

### Using pip

```bash
pip install -r requirements.txt
```

## Usage

### Running in Google Colab

1. Upload `ecommerce_dashboard.ipynb` to Google Colab
2. Run all cells in order (Runtime → Run all)
3. Interact with widgets to filter data dynamically

### Running Locally

```bash
# Activate environment
conda activate ecommerce-db

# Launch Jupyter
jupyter notebook ecommerce_dashboard.ipynb
```

## Project Structure

```txt
ecommerce-dashboard/
├── ecommerce_dashboard.ipynb     # Main notebook
├── requirements.txt              # Python dependencies
├── README.md                     # This file
└── ecommerce.db                  # SQLite database (generated on run)
```

## Database Schema

### Tables

1. **customers** (1,000 records)
   - customer_id, name, email, region, registration_date, customer_segment

2. **products** (500 records)
   - product_id, product_name, category, price, stock_quantity, supplier

3. **orders** (10,000 records)
   - order_id, customer_id, product_id, order_date, quantity, total_amount, status

4. **reviews** (5,000 records)
   - review_id, product_id, customer_id, rating, review_text, review_date

### Relationships

- orders.customer_id → customers.customer_id
- orders.product_id → products.product_id
- reviews.customer_id → customers.customer_id
- reviews.product_id → products.product_id

## Key SQL Queries

### Customer Lifetime Value (CLV)

```sql
SELECT 
    c.customer_id,
    c.name,
    SUM(o.total_amount) as customer_lifetime_value
FROM customers c
INNER JOIN orders o ON c.customer_id = o.customer_id
WHERE o.status = 'Completed'
GROUP BY c.customer_id
ORDER BY customer_lifetime_value DESC
```

### Top Category per Region (Window Function)

```sql
WITH regional_sales AS (
    SELECT 
        c.region,
        p.category,
        SUM(o.total_amount) as total_revenue,
        RANK() OVER (
            PARTITION BY c.region 
            ORDER BY SUM(o.total_amount) DESC
        ) as category_rank
    FROM orders o
    INNER JOIN customers c ON o.customer_id = c.customer_id
    INNER JOIN products p ON o.product_id = p.product_id
    GROUP BY c.region, p.category
)
SELECT * FROM regional_sales WHERE category_rank = 1
```

## Author

- **Course**: Database Management  
- **Institution**: Inha University  
- **Student Name**: Baratov Sokhibjon
- **Student ID**: 12225259
- **Semester**: Fall 2025
