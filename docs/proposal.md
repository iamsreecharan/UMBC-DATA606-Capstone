# 1. Title and Author  

**Project Title:** Predicting Delivery Delays in E-Commerce Supply Chains  

**Prepared for:** UMBC Data Science Master’s Capstone by Dr. Chaojie (Jay) Wang  

**Author:** Sree Charan Reddy Kailasam  

- [GitHub Repository](https://github.com/iamsreecharan)  
- [LinkedIn Profile](https://www.linkedin.com/in/sree-charan-reddy-kailasam-a1943334a)  
- PowerPoint Presentation: *to be added later*  
- YouTube Demo Video: *to be added later*  

---

# 2. Background  

Late deliveries are one of the most significant challenges in modern supply chains. Customers expect accurate and timely delivery, but delays result in lower satisfaction, negative reviews, and increased costs for sellers.  

The COVID-19 pandemic amplified these issues, making predictive analytics for logistics resilience more important than ever.  

**Project Objective:**  
This project aims to build machine learning models that predict whether an e-commerce order will be delivered on time or late, and if delayed, estimate the number of days late.  

**Why it Matters:**  
Accurate delivery predictions help:  
- **E-commerce platforms** improve trust and customer satisfaction.  
- **Logistics providers** allocate resources effectively and reduce risk.  
- **Sellers** optimize shipping methods and inventory buffers.  
- **Customers** receive realistic delivery expectations and timely updates.  

**Research Questions:**  
1. Can we predict whether an order will be delivered late?  
2. How many days late will delayed orders be?  
3. Which factors (distance, freight cost, product type, seller location) are most predictive of delays?  
4. How do delays correlate with customer satisfaction scores?  
5. What inferred insights can be drawn about the *reasons* for delay, even though not explicitly stated in the dataset?  
6. What recommendations can be made to improve supply chain performance?  

---

# 3. Data  

### Data Source  
- **Olist Brazilian E-Commerce Dataset (Kaggle)**  
  [Link to dataset](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce)  
  - Public dataset with ~100,000 customer orders.  
  - Includes multiple tables: orders, order items, customers, sellers, products, and geolocation.  
  - Merged into a single dataset of **112,650 rows × 19 columns** for this project.  

---

### Data Overview  
- **Size:** ~112,650 rows × 19 columns  
- **Scope:** Brazilian e-commerce transactions across multiple categories and states  
- **Time Period:** 2016–2018  
- **Unit of Analysis:** One order = one row  

---

### What Was Done to Prepare the Final Dataset  
1. **Merged key tables**: orders, order items, customers, sellers, products, and geolocation.  
2. **Selected key variables**: delivery dates, freight value, product features, seller & customer locations.  
3. **Engineered targets**:  
   - `delivery_delay` (0 = on time, 1 = late)  
   - `delay_days` (days late/early).  
4. **Removed identifiers** (`order_id`, `customer_id`, `product_id`, `seller_id`) to prevent data leakage.  
5. **Prepared location coordinates** (lat/lng) for feature engineering (distance calculation).  

---

### Column Data Types and Dictionary (Final Dataset)  

| Column | Type | Definition | Example |  
|--------|------|------------|---------|  
| order_purchase_timestamp | DateTime | Date and time the order was placed | 2017-03-10 13:57:00 |  
| order_estimated_delivery_date | Date | Estimated delivery date | 2017-03-12 |  
| order_delivered_customer_date | Date | Actual delivery date (used to derive target) | 2017-03-15 |  
| delivery_delay | Binary | Target 1: 0 = On-time, 1 = Delayed | 1 |  
| delay_days | Integer | Target 2: Days late (+) or early (-) | +3 |  
| product_category_name | Categorical | Type of product | electronics |  
| product_weight_g | Float | Product weight in grams | 500 |  
| product_length_cm | Float | Product length in cm | 19 |  
| product_height_cm | Float | Product height in cm | 8 |  
| product_width_cm | Float | Product width in cm | 13 |  
| freight_value | Float | Shipping cost | 25.30 |  
| seller_state | Categorical | Origin state of seller | SP |  
| customer_state | Categorical | Destination state of customer | RJ |  
| customer_lat, customer_lng | Float | Customer coordinates | -23.57, -46.58 |  
| seller_lat, seller_lng | Float | Seller coordinates | -23.68, -46.44 |  

---

### Target Variables  
1. **delivery_delay** → Binary classification:  
   - 0 = On-time  
   - 1 = Delayed  

2. **delay_days** → Regression:  
   - Numeric prediction of how many days late/early.  

---

### Feature Candidates  
- **Order features:** purchase date (day, month, weekday), estimated lead time.  
- **Product features:** category, weight, dimensions.  
- **Logistics features:** freight value, seller state, customer state.  
- **Geospatial features:** seller–customer distance (to be derived).  

---

# Two-Step Design  

This project adopts a **two-step design**:  
1. **Classification model** → Predict if an order will be late (Yes/No).  
2. **Regression model** → For delayed orders, predict the number of days late.  

This approach provides both:  
- A quick **risk flag** (for immediate alerts).  
- A **numeric estimate** (for operational planning).  

---
