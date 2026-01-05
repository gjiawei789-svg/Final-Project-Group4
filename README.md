# Final-Project: Zomato Restaurant Data Analysis & Market Potential Identification (New Delhi)

## Overview
This project performs a comprehensive analysis of the Zomato restaurant dataset, specifically focusing on the **New Delhi** market. By combining Exploratory Data Analysis (EDA), Unsupervised Learning (Clustering), and Supervised Learning (Regression), the project aims to identify market structure and detect anomalies in restaurant popularity.

The core objective is to calculate an **"Undervaluation Index"** to categorize restaurants into:
*   **Hidden Gems:** High-potential restaurants with high quality but lower-than-expected traffic.
*   **Viral Hits:** Over-performing restaurants where actual traffic far exceeds the theoretical model prediction.


## Analysis Workflow

### 1. Data Cleaning and Preprocessing
*   **Data Source:** `zomato_data_updated.csv`
*   **Geographic Focus:** Filtered for `Country == 'India'` and specifically `City == 'New Delhi'`.
*   **Cleaning:** 
    *   Removed entries with invalid `Aggregate rating` (0) or invalid `Votes`.
    *   Final dataset size for New Delhi analysis: **8,661** restaurants.

### 2. Exploratory Data Analysis (EDA)
Key visualizations generated to understand the market landscape:
*   **Distribution:** Analysis of restaurants by Country and City.
*   **Service Features:** Pie charts for `Has Online delivery` and `Has Table booking`.
*   **Economics:** Cost distribution and Price Range analysis.
*   **Demand Concentration (Long-tail Analysis):** 
    *   Identified that the **Top 5%** of restaurants account for **~58.47%** of total votes.
    *   The **Top 20%** account for **~86.48%** of total votes.
    *   Visualized via the "Cumulative Distribution of Restaurant Votes".

### 3. Brand Clustering (Feature Engineering)
To better understand restaurant positioning, the code aggregates data by `Restaurant Name` to create "Brand" features:
*   **Derived Features:** `brand_outlet_count`, `brand_avg_rating`, `brand_std_rating`, `delivery_coverage`, `cuisine_prevalence`, etc.
*   **Model Selection:** Tested `KMeans`, `GaussianMixture`, and `AgglomerativeClustering` with various scalers (`StandardScaler`, `MinMaxScaler`, `RobustScaler`).
*   **Best Model:** **KMeans** (with `MinMaxScaler`, k=5).
*   **Result:** A new feature `Cluster_Auto` was added to the dataset to categorize restaurants into strategic groups (e.g., budget chains, premium dining, etc.).

### 4. Regression Modeling (Vote Prediction)
A regression model was trained to predict the "theoretical" popularity (`log_votes`) based on restaurant attributes.

*   **Features (X):** `Aggregate rating`, `Converted Cost (INR)`, `Has Online delivery`, `Has Table booking`, `Cluster_Auto` (One-hot encoded).
*   **Target (y):** `log_votes` (Log-transformed Votes).
*   **Models Evaluated:**
    *   Linear Regression
    *   Gradient Boosting
    *   **Random Forest** (Selected as the **Best Model** with Test R² ≈ 0.73).
*   **Feature Importance:** The model identified `Aggregate rating` and `Converted Cost (INR)` as the most significant predictors of traffic.

### 5. Market Insights: Undervaluation Index
The project calculates the deviation between predicted and actual performance to classify restaurants.

**Classifications (`viz_status`):**
1.  **Hidden Gem:** 
    *   Logic: `Undervaluation_Index > 50` AND `Aggregate rating >= 4.0`.
    *   Meaning: High theoretical demand, high quality, but low actual traffic.
2.  **Viral Hit:** 
    *   Logic: `Undervaluation_Index < -500` AND `Aggregate rating < 3.5`.
    *   Meaning: Actual traffic massively exceeds prediction despite lower ratings (Hype-driven).
3.  **Normal:** Restaurants performing within expected bounds.

## Output & Visualizations
The code saves visualizations to the `PNG/` directory and map files to the root.

| File Name | Description |
| :--- | :--- |
| `Restaurant Distribution by Country.png` | Global distribution bar chart. |
| `Restaurant Distribution by City in India (Top 15).png` | Top cities by restaurant count. |
| `india_restaurant_heatmap.html` | Interactive Folium heatmap of restaurant density. |
| `Online Orders in New Delhi.png` | Pie chart of delivery availability. |
| `Table Booking in New Delhi.png` | Pie chart of booking availability. |
| `Cumulative Distribution of Restaurant Votes...png` | Lorentz curve showing market concentration. |
| `Feature Importance (Random Forest).png` | Bar chart of factors driving popularity. |
| `Distribution of Undervalued...Restaurants.png` | Histograms of the `Undervaluation_Index`. |
| `restaurant_map.html` | Interactive map pinning **Hidden Gems** (Green) and **Viral Hits** (Red). |
| `Long-tail_Distribution_Analysis.png` | Scatter plot showing where Gems and Viral Hits sit on the market curve. |
