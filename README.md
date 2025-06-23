# zhvi-TN-housing-trends · v1.0.0

This project builds a lightweight, SQLite-based dataset of housing trends in Tennessee using publicly available CSVs. It includes a reproducible Python pipeline that transforms, cleans, and loads raw data into a structured database—ideal for analysis, visualization, or integration into dashboards.

---

### 👤 About the Author

Eric is a data-driven problem solver with a strong focus on reproducibility, precision, and thoughtful design. He combines technical expertise in Python, SQL, and ETL workflows with a meticulous approach to organization and documentation. This project reflects a commitment to building transparent, scalable tools for data exploration.

---


# 🏠 ZHVI Housing Trends Pipeline

This project analyzes Zillow Home Value Index (ZHVI) monthly data across U.S. by zip codes, dating back to year 2000 through May 2025. It automates the ingestion, transformation, and storage of large housing datasets into an SQLite database, enabling clean analysis and dashboard creation.

- Data Source: https://www.zillow.com/research/data/
    - Section: Home Values
    - Reports: ZHVI [1 - 5+] Bedroom Time Series ($)
    - Geography Used: Zip Code
    - Export Date: 6/19/2025


<pre><code>## 📁 Project Structure
zhmi-housing-trends/
├── data/
│   └── zhvi_data.db             ← Final SQLite database
├── dashboard/
│   └── housing_dashboard.pbix   ← Interactive Power BI dashboard (TBD - optional)
├── notebooks/
│   └── transform_load_db_pipeline.ipynb   ← Data pipeline (ETL)
├── zhvi_raw_files/
│   └── *.csv                    ← Raw Zillow CSV files (not tracked)
├── sql/
│   └── sample_queries.sql       ← Example SQL queries
├── .gitignore
└── README.md
</code></pre>


---

## ⚙️ How It Works

### 1. **Extract**
Raw `.csv` files representing home values by bedroom count are read from the `zhvi_raw_files/` folder.

### 2. **Transform**
- Filters for `RegionType = "zip"`
- Unpivots wide monthly columns into long format
- Extracts bedroom count from file names
- Cleans and converts data types for consistency

### 3. **Load**
The cleaned dataset is saved to `data/zhvi_data.db`, ready for fast SQL querying or dashboard consumption.



---

## 📊 Preview Query: Average ZHVI by Zip (Nashville, 2020+)

## sql
```
SELECT 
    strftime('%Y-%m', Date) AS Month,
    BedroomCount,
    RegionName AS "Zip Code",
    ROUND(AVG(HomeValue), 2) AS AvgHomeValue
FROM zhvi_data
WHERE State = 'TN' 
  AND City = 'Nashville' 
  AND strftime('%Y', Date) >= '2020'
GROUP BY Month, BedroomCount, RegionName
ORDER BY "Zip Code", BedroomCount, Month;
```

---

## 🚀 How to Run

```bash
git clone https://github.com/yourusername/zhvi-TN-housing-trends.git
cd zhvi-TN-housing-trends
pip install -r requirements.txt
python notebooks/transform_load_db_pipeline.py
```
> ⚠️ Note: Place all raw Zillow .csv files into the zhvi_raw_files/ folder before running the pipeline.



---

## 🛠 Tools Used

- Python (`pandas`, `sqlite3`)
- SQLite (via DB Browser or `pd.read_sql_query`)
- Jupyter & VS Code for development and testing


---

## 📌 Next Steps

- Publish dashboard insights for public viewing
- Add automated tests for pipeline integrity
- Explore regional trend forecasting (ARIMA or Prophet)
- Open cleaned dataset for public or team analysis

---

### 🔧 Building the Database Locally

To build the `zhvi_data.db` file:

1. Clone this repository
2. Ensure the `zhvi_raw_files/` folders contain the provided `.csv` files
3. Run:  
   ```bash
   python notebooks/transform_load_db_pipeline.py

4. The database will be generated in `data/zhvi_data.db`







## 📄 License

MIT License. See `LICENSE.md` for details.
