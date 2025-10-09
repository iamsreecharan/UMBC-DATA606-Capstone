# 1. Title and Author  

**Project Title:** Predicting Delivery Delays in E-Commerce Supply Chains  

**Prepared for:** UMBC Data Science Master’s Capstone by Dr. Chaojie (Jay) Wang  

**Author:** Sree Charan Reddy Kailasam  

- [GitHub Repository](https://github.com/iamsreecharan)  
- [LinkedIn Profile](https://www.linkedin.com/in/sree-charan-reddy-kailasam-a1943334a)  
- PowerPoint Presentation: *to be added later*  
- YouTube Demo Video: *to be added later*  

---

# 2. Background and Problem Definition  

## 2.1 Industry Context  

E-commerce has transformed how people shop, yet the promise of “fast, reliable delivery” remains one of the hardest goals to maintain.  
When customers experience delivery delays, it directly impacts satisfaction, seller reputation, and long-term loyalty.  
The COVID-19 pandemic highlighted vulnerabilities in logistics networks, where sudden spikes in demand and infrastructure limitations created unpredictable delivery performance.  

Brazil’s e-commerce market is one of the fastest-growing in Latin America, and the **Olist platform** acts as a major marketplace connecting thousands of sellers and logistics partners.  
Understanding and predicting delivery delays in this context can create measurable improvements in customer experience, cost optimization, and operational efficiency.  

---

## 2.2 Problem Statement  

Although Olist provides estimated delivery dates for all orders, some deliveries still arrive later than expected.  
These delays may be influenced by product characteristics, seller and customer locations, freight values, or seasonal patterns.  
Currently, no predictive mechanism proactively identifies which orders are likely to be delayed.  

**Primary Problem:**  
Can we build a machine-learning model that predicts whether an order will be delivered late and, if so, estimate how many days late?  

**Secondary Problems:**  
1. What patterns or factors are most strongly associated with delays?  
2. How do geography, product type, and freight characteristics interact to influence delay likelihood?  
3. What early-warning signals can help logistics providers act before a delay occurs?  

---

## 2.3 Project Objectives  

The overall objective is to use data-driven methods to **predict delivery outcomes** and **provide actionable insights** for logistics optimization.  

**Specific Objectives:**  
- Develop a binary classification model to predict late vs. on-time orders.  
- Develop a regression model to estimate the number of days late.  
- Identify the most important predictors of delay (e.g., distance, freight cost, product category).  
- Visualize temporal and regional delay trends to guide business decisions.  
- Recommend operational strategies to improve delivery-time accuracy.  

---

## 2.4 Research Significance  

Accurate delay prediction has value for all stakeholders in the supply chain:  
- **Customers:** Receive realistic delivery expectations and proactive delay notifications.  
- **Sellers:** Adjust inventory and manage customer communication more effectively.  
- **Logistics Providers:** Optimize routing and capacity planning.  
- **E-Commerce Platforms:** Reduce refund requests and improve reputation metrics.  

---

# 3. Data Description  

## 3.1 Data Source  

**Dataset:** Olist Brazilian E-Commerce Public Dataset  
**Source:** [Kaggle – Olist Brazilian E-Commerce Dataset](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce)  
**Size:** ~112 650 records × 23 features  
**Time Period:** 2016 – 2018  
**Unit of Analysis:** One row per customer order  

The dataset integrates multiple tables — orders, order items, customers, sellers, products, and geolocation — merged into one unified dataset for analysis.

---

## 3.2 Feature Summary  

| Column | Type | Description | Example |  
|--------|------|-------------|----------|  
| order_purchase_timestamp | DateTime | When the order was placed | 2017-03-10 13:57:00 |  
| order_estimated_delivery_date | Date | Estimated delivery date | 2017-03-12 |  
| order_delivered_customer_date | Date | Actual delivery date | 2017-03-15 |  
| delivery_delay | Binary | 0 = On time, 1 = Delayed | 1 |  
| delay_days | Float | Days late (+) or early (–) | +3 |  
| freight_value | Float | Shipping cost | 25.3 |  
| product_category_name | String | Product category | electronics |  
| product_weight_g | Float | Product weight | 500 |  
| product_length_cm | Float | Product length | 19 |  
| product_height_cm | Float | Product height | 8 |  
| product_width_cm | Float | Product width | 13 |  
| seller_state | Categorical | Seller’s state | SP |  
| customer_state | Categorical | Customer’s state | RJ |  
| customer_lat, customer_lng | Float | Customer coordinates | –23.57, –46.58 |  
| seller_lat, seller_lng | Float | Seller coordinates | –23.68, –46.44 |  

---

## 3.3 Target Variables  

1. **delivery_delay (Classification):**  
   Predict whether the order is delayed (1) or on time (0).  

2. **delay_days (Regression):**  
   Predict how many days the order is late or early.  

---

## 3.4 Feature Categories  

- **Order Features:** Purchase date, estimated lead time, weekday, and month.  
- **Product Features:** Category, weight, and dimensions.  
- **Logistics Features:** Freight cost, seller and customer state.  
- **Geospatial Features:** Seller–customer distance derived from latitude/longitude.  

---

## 3.5 Data Preparation Steps  

1. **Merged Multiple Tables:** Combined orders, items, customers, sellers, products, and geolocation.  
2. **Created Target Columns:**  
   - `delivery_delay` → (actual > estimated = 1, else 0)  
   - `delay_days` → Difference in days (actual – estimated)  
3. **Removed Identifiers:** Dropped IDs to prevent data leakage.  
4. **Converted Dates to Datetime Format:** Enabled time-series analysis.  
5. **Verified Numeric and Categorical Types:** Ensured modeling compatibility.  

---

## 3.6 Analytical Approach  

This project follows a **two-step predictive design**:  

1. **Classification Model** – Determines whether an order will be delayed.  
2. **Regression Model** – Estimates the number of days late for delayed orders.  

This approach provides both an early **risk flag** for delayed orders and a **quantitative delay estimate** for planning and communication.

---

## 3.7 Data Limitations  

- Missing values in delivery and product metrics due to incomplete entries.  
- Geospatial precision limited by ZIP-code centroids.  
- External factors such as weather, traffic, or strikes are not included.  
- Dataset represents 2016-2018 period and may not capture post-pandemic trends.  

Despite these constraints, the dataset remains rich and suitable for predictive modeling, offering a comprehensive view of Brazil’s e-commerce logistics performance.


# 4. Exploratory Data Analysis (EDA)

## 4.1 Overview

This section presents an in-depth exploratory data analysis (EDA) of the **Olist Brazilian E-Commerce dataset** to understand the key factors influencing delivery delays.  
The analysis was performed in **Python using Jupyter Notebook**, with **Plotly Express** for interactive visualizations.  

The main goals of this analysis were to:  
1. Assess data quality and completeness.  
2. Identify and handle missing and duplicate records.  
3. Explore relationships among order, product, and logistics features.  
4. Visualize trends in delivery behavior across geography and time.  
5. Prepare a tidy dataset suitable for machine-learning modeling.

---

## 4.2 Data Quality Assessment

**Initial Dataset:** 112 650 rows × 23 columns  

### Key Findings
- **Duplicates:** 10 225 rows (removed).  
- **Missing Values:** Present in `order_delivered_customer_date`, `delay_days`, and a few product/coordinate fields.  
- **Cleaning Strategy:**  
  - Dropped rows missing target variables (`delay_days`, `order_delivered_customer_date`).  
  - Retained minor missing values in non-critical fields to preserve data diversity.  

**Final Dataset:** 100 195 rows × 23 columns  

This process ensured each row represents one valid, complete delivery record.

---

## 4.3 Data Cleaning and Preparation

### Steps Performed
1. **Removed Duplicates** using `df.drop_duplicates()` → 10 225 rows dropped.  
2. **Handled Missing Data** → 2 230 rows removed where targets were null.  
3. **Converted Data Types** → Converted date columns to `datetime`; verified numeric and categorical types.  
4. **Ensured Tidiness** → One row per order, one column per attribute.  

**Outcome:** A clean, consistent dataset ready for analysis and modeling.

---

## 4.4 Summary Statistics

### Numerical Features
- **Freight Value:** Mean ≈ 20 R$, wide range up to 400+.  
- **Product Weight:** Average 2 000–2 500 g, a few heavy outliers.  
- **Delay Days:** From −40 to +100; many negative (early deliveries).  

### Categorical Features
- **States:** 27 customer + 23 seller states.  
- **Product Categories:** ≈ 73 unique.  
- Strong diversity across customers, sellers, and products.

---

## 4.5 Target Variable Exploration

### Delivery Delay (0 = On Time, 1 = Delayed)
- About **90 percent of orders** were on time or early; **10 percent delayed**.  
- Even a 10 percent delay rate represents thousands of late orders, impacting customer trust.

### Delay Days
- **Left-skewed distribution**: most orders delivered before the estimate.  
- Indicates conservative delivery estimates — opportunity to improve time predictions.

---

## 4.6 Geographic Analysis

- Northern and Northeastern states (**Alagoas, Maranhão, Bahia**) show **higher delay rates**.  
- Southeastern hubs (**São Paulo, Rio de Janeiro**) perform best.  
- Suggests influence of **distance, infrastructure, and carrier coverage**.  

**Visualization:** Plotly bar chart – Average delay rate by customer state.  

---

## 4.7 Temporal Analysis

- Delay frequency fluctuates across months.  
- **Spikes in March and December** coincide with seasonal demand and holiday congestion.  
- **Lower rates mid-year** (June–August).  

**Visualization:** Line chart – Average delay rate by month.  

**Interpretation:** Temporal features such as month, weekday, or quarter are likely predictive.

---

## 4.8 Feature Relationships

### Correlation Analysis
- Product dimensions (weight, length, height, width) → strongly correlated.  
- Freight value ↔ delay → weak correlation.  
- Delivery delay ↔ delay days → expected strong correlation.  

**Visualization:** Interactive correlation heatmap (Plotly).

---

## 4.9 Freight Value vs Delay Analysis

- Scatter plots show **no direct linear relationship** between freight cost and delays.  
- Late deliveries occur at all freight levels, suggesting other logistic factors are more influential.  

---

## 4.10 Outlier Detection

- Few outliers in freight value and delay days.  
- Examples: freight > 300 R$ or delay days > 100.  
- Outliers retained for now; will be evaluated during modeling.  

---

## 4.11 Key EDA Insights

1. **Delay Problem Exists Despite High On-Time Rate**  
   A small delay percentage still represents a major operational and customer-satisfaction issue.  

2. **Regional Differences Matter**  
   Geography strongly influences delivery performance; spatial features are essential.  

3. **Seasonality is Visible**  
   Delays peak in March and December; temporal variables can improve forecasts.  

4. **Freight Value Not a Strong Predictor**  
   Higher shipping costs do not guarantee faster delivery.  

5. **Data Ready for Modeling**  
   The dataset is clean, tidy, and balanced structurally, with manageable missingness.

---

## 4.12 Next Steps

1. **Feature Engineering**  
   - Compute seller–customer distance (Haversine).  
   - Extract month, weekday, and season variables.  
2. **Class Imbalance Handling**  
   - Apply class weights or SMOTE for modeling.  
3. **Outlier Treatment**  
   - Cap or scale extreme freight/delay values.  
4. **Model Preparation**  
   - Standardize preprocessing and pipeline reproducibility.

---

## 4.13 Summary

The exploratory analysis shows that while most Olist orders arrive on time, delayed shipments still pose a measurable business risk.  
Regional and seasonal patterns highlight the value of predictive modeling to forecast and reduce delays.  
The dataset is clean, well-structured, and now ready for machine-learning development.

---

## Deliverables from EDA Phase
- Cleaned dataset (`olist_final_clean.csv`)  
- Jupyter Notebook with markdown explanations and commented code  
- Interactive Plotly visualizations  
- Updated project report including this Section 4 (EDA)


---

