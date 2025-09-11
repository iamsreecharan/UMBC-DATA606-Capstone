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

E-commerce has transformed global supply chains, but late deliveries remain a critical challenge. Customers expect fast and reliable service, and any delay leads to reduced trust, poor reviews, and financial losses for sellers.  

The COVID-19 pandemic further highlighted how fragile supply chains can be when demand spikes or when logistics face disruption. To address this, companies are turning to **predictive analytics** to anticipate and prevent delays before they occur.  

This project will use data from the **Olist Brazilian E-Commerce dataset** to build machine learning models that predict whether an order will be delivered on time or late, and if delayed, by how many days.  

Although the dataset does not include explicit reasons for delay, this project will **infer the main drivers of delays** through feature analysis. Factors such as **shipping distance, product type, freight cost, and seller region** are expected to play a key role.  

**Why this matters:**  
- For companies → reduces operational costs and improves customer satisfaction.  
- For customers → builds trust with accurate delivery expectations.  
- For supply chain managers → provides data-driven insights for better decision-making.  

**Research Questions:**  
1. Can machine learning models predict whether an e-commerce order will be delivered late?  
2. Which features (e.g., shipping distance, freight value, seller location, product type) are the strongest predictors of delivery delays?  
3. How strongly do delivery delays correlate with customer satisfaction scores?  
4. If an order is predicted to be delayed, by how many days will it be late?  
5. What inferred insights can be drawn about the *reasons* for delay, even though they are not explicitly provided in the dataset?  
6. What actionable recommendations can be made to improve logistics performance?  

---

# 3. Data  

### Data Source  
- **Olist Brazilian E-Commerce Dataset (Kaggle)**  
  [Link to dataset](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce)  
  - Public dataset with ~100,000 customer orders.  
  - Contains multiple tables: orders, order items, products, customers, sellers, payments, reviews, and geolocation.  
  - After merging these tables, one enriched dataset will be created where each row represents a single customer order.  

---

###  Data Size & Shape  
- Orders: ~100,000 rows × 8 columns  
- Order items: ~110,000 rows × 9 columns  
- Customers: ~100,000 rows × 5 columns  
- Sellers: ~3,000 rows × 4 columns  
- Reviews: ~100,000 rows × 4 columns  
- Final merged dataset: ~100K rows (each row = one order, enriched with product, seller, customer, and review details).  

---

### Time Period  
- Covers **2016 to 2018** (based on Olist transaction records).  

---

### What Each Row Represents  
- One customer order placed on the Olist platform.  
- Includes order details, delivery dates, seller and customer information, product category, and customer review.  

---

### Data Dictionary (Key Variables for ML)  

| Column | Type | Description | Example |  
|--------|------|-------------|---------|  
| order_id | String | Unique identifier for an order | `df128bcd9` |  
| order_purchase_timestamp | DateTime | Date and time the order was placed | `2017-03-10 13:57:00` |  
| order_estimated_delivery_date | Date | Estimated delivery date given to the customer | `2017-03-12` |  
| order_delivered_customer_date | Date | Actual delivery date | `2017-03-15` |  
| delivery_delay (derived) | Binary (Target 1) | 1 = Late, 0 = On-time | `1` |  
| delay_days (derived) | Numeric (Target 2) | Number of days late (negative if early) | `+3` |  
| product_category | Categorical | Type of product purchased | `electronics` |  
| freight_value | Numeric | Shipping cost | `25.30` |  
| seller_state | Categorical | State of the seller (origin) | `SP` |  
| customer_state | Categorical | State of the customer (destination) | `RJ` |  
| distance_km (derived) | Numeric | Distance between seller and customer (using geolocation) | `280.5` |  
| review_score | Numeric (1–5) | Customer satisfaction rating | `3` |  

---

### Target Variables  
1. **delivery_delay** → Binary classification:  
   - `0 = On-time`  
   - `1 = Delayed`  

2. **delay_days** → Regression:  
   - Numeric prediction of how many days late (positive) or early (negative) an order is delivered.  

---

### Predictor Variables  
- **Order features:** purchase timestamp, weekday, month.  
- **Product features:** category, weight/dimensions (from product table).  
- **Logistics features:** seller state, customer state, freight value, seller–customer distance.  
- **Customer features:** review score (for correlation analysis).  

---

# Two-Step Design  

This project will adopt a **two-step machine learning design**:  
1. **Classification Model** → Predict whether an order will be delivered on time or late (Yes/No).  
2. **Regression Model** → For orders predicted as delayed, estimate the number of days late.  

---
