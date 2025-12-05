# E-Commerce Interactive Dashboard

## Overview

An advanced interactive dashboard for e-commerce sales analytics built with Python, SQLite3, and Plotly. Features real-time filtering, dynamic visualizations, and comprehensive SQL analytics including window functions and customer lifetime value calculations.

## Features

### Interactive Controls
- **Region Filter**: Select specific regions or view all
- **Category Filter**: Filter by product category
- **Date Range Slider**: Dynamically adjust the time period for analysis

### Key Performance Indicators (KPIs)
- Total Revenue
- Total Orders
- Average Order Value
- Average Customer Lifetime Value

### Visualizations

1. **Regional Sales Heatmap**
   - Revenue distribution across regions and categories
   - Color-coded intensity for quick insights

2. **Revenue Distribution Pie Chart**
   - Category-wise revenue breakdown
   - Percentage contribution to total revenue

3. **Daily Revenue Trends**
   - Time series analysis with 7-day moving average
   - Window function implementation: `AVG() OVER()`

4. **Customer Lifetime Value Analysis**
   - Top 15 customers by total spending
   - Aggregation using `SUM()` and `GROUP BY`

5. **Product Performance Scatter Plot**
   - Revenue vs. customer ratings correlation
   - Bubble size indicates units sold
   - Color-coded by category

6. **Top Categories by Region**
   - Window function analysis using `RANK() OVER(PARTITION BY)`
   - Shows best-performing category per region

## Advanced SQL Features

### Window Functions
```sql
-- Moving average calculation
AVG(daily_total) OVER (
    ORDER BY order_date
    ROWS BETWEEN 6 PRECEDING AND CURRENT ROW
) as moving_avg_7day

-- Regional ranking
RANK() OVER (
    PARTITION BY c.region
    ORDER BY SUM(o.total_amount) DESC
) as category_rank
```

### Complex Joins
```sql
-- Multi-table joins
FROM orders o
INNER JOIN customers c ON o.customer_id = c.customer_id
INNER JOIN products p ON o.product_id = p.product_id
LEFT JOIN reviews r ON p.product_id = r.product_id
```

### Aggregations
- `SUM()` for total revenue calculations
- `COUNT()` for order counting
- `AVG()` for mean calculations
- `COALESCE()` for handling NULL values

## Prerequisites

### Required Software
- Python 3.10 or higher
- Jupyter Notebook or Google Colab
- SQLite3 (included in Python)

### Database Setup
**Important**: You must first run the `database_manipulation.ipynb` notebook in the `../analysis/` folder to generate the `ecommerce.db` database file.

## Installation

### Option 1: Using Conda (Recommended)

```bash
# Create conda environment
conda create -n ecommerce-dashboard python=3.10 -y

# Activate environment
conda activate ecommerce-dashboard

# Install dependencies
pip install -r requirements.txt

# Enable ipywidgets for Jupyter
jupyter nbextension enable --py widgetsnbextension
```

### Option 2: Using pip

```bash
# Install dependencies
pip install -r requirements.txt

# Enable ipywidgets
jupyter nbextension enable --py widgetsnbextension
```

## Usage

### Running in Google Colab

1. Upload both notebooks to Google Colab
2. First run `../analysis/database_manipulation.ipynb` (generates database)
3. Then open and run `interactive_dashboard.ipynb`
4. Use the interactive widgets at the top to filter data
5. All visualizations update automatically

### Running Locally

```bash
# 1. First generate the database
cd ../analysis
jupyter notebook database_manipulation.ipynb
# Run all cells to generate ecommerce.db

# 2. Then run the dashboard
cd ../dashboard
jupyter notebook interactive_dashboard.ipynb
# Run all cells and use the interactive controls
```

## Project Structure

```txt
projects/ecommerce/
├── analysis/
│   ├── database_manipulation.ipynb  # Database generation
│   ├── requirements.txt
│   ├── README.md
│   └── ecommerce.db                 # Generated database
│
└── dashboard/
    ├── interactive_dashboard.ipynb  # This dashboard
    ├── requirements.txt
    └── README.md                    # This file
```

## How the Dashboard Works

1. **Connection**: Connects to `../analysis/ecommerce.db`
2. **Filter Options**: Loads available regions, categories, and date ranges
3. **User Input**: Interactive widgets capture user selections
4. **Dynamic Queries**: SQL queries are built with WHERE clauses based on filters
5. **Visualization Update**: All charts refresh with filtered data
6. **Real-time**: Updates happen automatically when filters change

## SQL Query Examples

### Customer Lifetime Value (CLV)
```sql
SELECT
    c.customer_id,
    c.name,
    c.region,
    COUNT(o.order_id) as total_orders,
    SUM(o.total_amount) as customer_lifetime_value,
    AVG(o.total_amount) as average_order_value
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
    WHERE o.status = 'Completed'
    GROUP BY c.region, p.category
)
SELECT * FROM regional_sales WHERE category_rank = 1
```

## Customization

### Adding New Visualizations

1. Create a new visualization function in Section 4
2. Add the function call in `create_dashboard()` in Section 5
3. Optionally add new SQL queries in `get_filtered_data()`

### Modifying Filters

Edit the widget definitions in Section 5:
```python
region_dropdown = widgets.Dropdown(
    options=filter_options['regions'],
    value='All',
    description='Region:'
)
```

## Troubleshooting

### "Database not found" error
- Make sure you ran `../analysis/database_manipulation.ipynb` first
- Check that `ecommerce.db` exists in the `../analysis/` folder

### Widgets not displaying
```bash
# Install and enable ipywidgets
pip install ipywidgets
jupyter nbextension enable --py widgetsnbextension

# For JupyterLab
jupyter labextension install @jupyter-widgets/jupyterlab-manager
```

### No data appears
- Check your filter selections - try setting all filters to "All"
- Verify the database has data by running a simple query

## Performance Notes

- All queries include indexes on foreign keys and frequently filtered columns
- Date filtering uses string comparison (SQLite date format)
- Large date ranges may take longer to process
- Moving average calculation uses efficient window functions

## Assignment Requirements Met

### Database Design
- 4-table normalized schema (customers, products, orders, reviews)
- Foreign key relationships
- Proper constraints and indexes

### Batch Data Generation
- 1,000 customers
- 500 products
- 10,000+ orders
- 5,000+ reviews

### Advanced SQL
- **Aggregations**: SUM, AVG, COUNT
- **Joins**: Multi-table INNER and LEFT joins
- **Window Functions**: RANK(), AVG() OVER()
- **Subqueries**: CTEs (WITH clauses)

### Interactive Visualization
- ipywidgets for user controls
- Plotly for dynamic, interactive charts
- Real-time filtering and updates
- Multiple visualization types

## Technologies Used

- **Database**: SQLite3
- **Language**: Python 3.10
- **Data Processing**: pandas, NumPy
- **Visualization**: Plotly Express, Plotly Graph Objects
- **Interactivity**: ipywidgets
- **Environment**: Jupyter Notebook / Google Colab

## Author

- **Student Name**: Baratov Sokhibjon
- **Student ID**: 12225259
- **Course**: Database Management
- **Institution**: Inha University
- **Semester**: Fall 2025

## License

This project is for educational purposes as part of the Database Management course.

---

**Note**: This dashboard demonstrates advanced database concepts including window functions, complex joins, and interactive data visualization for business intelligence applications.
