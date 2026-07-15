# customer_behavior_analysis
Data analytics project showcasing customer behavior analysis using python, sql and power BI.

---

## ✍️ Author
* **Syed Asger Mehdi** — [GitHub Profile](https://github.com/SyedAsgerMehdi)

---

## 📋 Overview
This project demonstrates a complete Data Analytics workflow, from data preprocessing to interactive dashboard creation. The project involves loading a dataset using Python, performing Exploratory Data Analysis (EDA), cleaning and preparing the data, analyzing it with SQL in PostgreSQL Server, and visualizing key insights using Power BI.

The goal is to turn raw transactional data into **actionable business strategies** to increase customer retention, optimize marketing discounts, and maximize average purchase values.

---

## 📊 Dataset
The dataset contains structured data used to identify trends, patterns, and business insights. It was cleaned and transformed before analysis to ensure accurate results.

---

## 🛠️ Tools & Technologies
* **Python** – Data loading, cleaning, and EDA
* **Pandas & NumPy** – Data manipulation
* **Matplotlib & Seaborn** – Data visualization
* **PostgreSQL** – Database management and SQL analysis
* **SQL** – Querying and extracting insights
* **Power BI** – Interactive dashboard creation

### Technology Stack Mapping:
| Technology | Category | Purpose |
| :--- | :--- | :--- |
| **Python 3.11** | ETL & Preprocessing | Data ingestion, handling missing values, feature engineering |
| **Pandas & NumPy** | Data Wrangling | Handling dataframe structures, binning, mapping categorical variables |
| **PostgreSQL** | Relational Database | Storing clean data and querying complex segments |
| **SQL** | Analytical Querying | Writing subqueries, window functions, and CTEs |
| **Power BI** | Business Intelligence | Interactive dashboard design, data modeling, KPI tracking |

---

## ⚙️ Project Steps
1. Loaded the dataset using Python.
2. Performed Exploratory Data Analysis (EDA).
3. Cleaned and preprocessed the data.
4. Imported the cleaned dataset into PostgreSQL Server.
5. Executed SQL queries to analyze the data.
6. Connected Power BI to PostgreSQL.
7. Built an interactive dashboard to visualize key insights.

### Python Data Preprocessing & Pipeline Detail:
Using Python and **Pandas**, I developed a robust preprocessing pipeline to transform the raw dataset into a clean, relational-friendly schema.
* **Handling Missing Values**: Applied median imputation grouped by product categories to handle any null values in `Review Rating` dynamically.
* **Schema Standardization**: Converted all column names to lowercase and replaced spaces with underscores for SQL compatibility.
* **Feature Engineering**:
  - Categorized continuous `age` data into four distinct life stages using quantile binning (`pd.qcut`): `Young Adult`, `Adult`, `Middle-aged`, and `Senior`.
  - Mapped categorical purchase frequencies (e.g., *Weekly*, *Fortnightly*, *Annually*) into numerical columns (`purchase_frequency_days`) to facilitate predictive metrics.
* **Redundancy Reduction**: Checked collinearity between `discount_applied` and `promo_code_used` (found to be 100% correlated) and dropped the redundant promo code column.

```python
# Feature Engineering: Quartile Binning for Age Groups
labels = ['Young Adult', 'Adult', 'Middle-aged', 'Senior']
df['age_group'] = pd.qcut(df['age'], q=4, labels=labels)

# Frequency Mapping to Numerical Values
frequency_mapping = {
    'Fortnightly': 14, 'Weekly': 7, 'Monthly': 30,
    'Quarterly': 90, 'Bi-Weekly': 14, 'Annually': 365, 'Every 3 Months': 90
}
df['purchase_frequency_days'] = df['frequency_of_purchases'].map(frequency_mapping)
```

---

## 🗄️ Relational Database Analysis (PostgreSQL Queries)
After importing the cleaned data into PostgreSQL, I wrote a suite of analytical SQL queries to extract deep customer intelligence.

### Featured Queries:
#### 1. Dynamic Customer Segmentation (CTE & CASE WHEN)
*Segments customers based on previous transactions to categorize customer health:*
```sql
WITH customer_type AS (
    SELECT customer_id, previous_purchases,
    CASE 
        WHEN previous_purchases = 1 THEN 'New'
        WHEN previous_purchases BETWEEN 2 AND 10 THEN 'Returning'
        ELSE 'Loyal'
    END AS customer_segment
    FROM customer
)
SELECT customer_segment, COUNT(*) AS "Number of Customers"
FROM customer_type 
GROUP BY customer_segment;
```

#### 2. Top 3 Selling Products Per Category (Window Functions)
*Identifies the most popular items across different clothing categories for inventory optimization:*
```sql
WITH item_counts AS (
    SELECT category, item_purchased, COUNT(customer_id) as total_orders,
    ROW_NUMBER() OVER (PARTITION BY category ORDER BY COUNT(customer_id) DESC) as item_rank
    FROM customer
    GROUP BY category, item_purchased
)
SELECT item_rank, category, item_purchased, total_orders
FROM item_counts
WHERE item_rank <= 3;
```

---

## 📊 Dashboard
The Power BI dashboard provides:
* Key Performance Indicators (KPIs)
* Trend Analysis
* Category-wise Comparisons
* Interactive Filters and Slicers
* Charts and Visualizations for better decision-making

### 🖼️ Dashboard Preview
![Customer Behavior Dashboard](screenshots/dashboard.png)

---

## 💡 Results & Executive Insights
* Improved data quality through preprocessing.
* Extracted meaningful insights using SQL queries.
* Created an interactive Power BI dashboard for data visualization.
* Demonstrated an end-to-end data analytics workflow suitable for business reporting.

### Strategic Recommendations:
* **💡 Subscription Opportunity**: Subscribed customers account for **27%** of the total customer base, but have consistent order sizes. **Recommendation**: Implement a loyalty program with exclusive discounts for non-subscribers (the 73% majority) to convert them to recurring subscribers, securing a more predictable revenue stream.
* **💡 Shipping Options**: Average purchase amounts between Standard and Express shipping are identical. **Recommendation**: Incentivize higher purchase sizes by offering free Express shipping for orders above a certain threshold (e.g., $75), maximizing the Average Order Value (AOV).
* **💡 Demographics Optimization**: Adults and Middle-Aged customer segments drive the largest share of overall revenue. **Recommendation**: Tailor targeted marketing campaigns and product recommendations specifically to these segments via social and email channels.

---

## 📁 Project Structure
```
Data-Analytics-Project/
│── dataset/
│── python/
│   ├── data_loading.py
│   ├── eda.py
│   └── data_cleaning.py
│── sql/
│   └── queries.sql
│── powerbi/
│   └── dashboard.pbix
│── screenshots/
│   └── dashboard.png
│── README.md
```

---

## 🛠️ How to Run
1. Clone this repository.
2. Install the required Python libraries.
3. Run the Python scripts to load, clean, and analyze the dataset.
4. Import the processed data into PostgreSQL Server.
5. Execute the provided SQL queries.
6. Open the Power BI file (`.pbix`) and refresh the data connection to view the dashboard.

---

## 🎓 Skills Demonstrated
* Data Cleaning
* Exploratory Data Analysis (EDA)
* SQL Query Writing
* PostgreSQL Database Management
* Data Visualization
* Power BI Dashboard Development
* Data Analytics Workflow
* ETL Pipeline Design & Schema Standardization
* Business Intelligence & Strategic Recommendations
