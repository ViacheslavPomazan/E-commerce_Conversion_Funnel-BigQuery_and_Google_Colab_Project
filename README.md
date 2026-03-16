# E-commerce_Conversion_Funnel-BigQuery-Google_Colab_Project 🚀
 
Welcome! This repository showcases an e-commerce data analysis project powered by BigQuery and Google Colab.

## Data Source Description 📂

The data source is the BigQuery GA4 public dataset: 'bigquery-public-data.ga4_obfuscated_sample_ecommerce.events_2021*', which provides data for January 2021. The dataset consists of 1,210,147 rows.

## Gallery and Insights 📊🔎

### General

Total number of sessions 116,514 , total number of purchase 1204, CR = 1.03%.

### 1. Sessions and Purchases Distribution by Country.
![map](image/colab_map1.png)

With over 52,000 sessions, the USA significantly outperforms all other countries. Engagement across the African continent remains minimal. Google serves as the primary traffic source for the majority of countries.

### 2. Sessions Distribution by Source/Medium.
![distr. by source/medium](image/session_distribution_by_source-medium1.png)

Traffic from unpaid search results on Google lead with more than 37000 sessions. Traffic from users who typed your URL directly where the medium is not explicitly defined takes second place.

### 3. CR by Event and Device Category.
![distr. by event and device](image/CR_by_event_and_device1.png)

CR from session_start to add_to_cart( ~3.9%) is below the market average (5%). Conversions across all device categories are at the same level.

### 4. CR by Event and Source.
![distr. by event and source](image/CR_by_event_and_source1.png)

Conversion rate across source (data deleted) is higher than others.

## 5. Dynamic Purchase CR by Source/Medium and Its Trend Line.
![dynamic cr](image/dynamic_cr1.png)

Traffic wihh (data deleted/data deleted) has higher CR than others. Overall trend line shows slight growth by month-end.

## 6. Purchase CR by Number of Landing Page Visits.
![cr from landing page](image/cr_session_start_count1.png)

Traffic wihh (data deleted/data deleted) has higher CR than others. Overall trend line shows slight growth by month-end.

## 7. GA4 Ecommerce Event Correlation Matrix.
![heat map](image/heat_map1.png)

1. Strong positive correlations (red zones): begin_checkout ↔ add_payment_info (0.85), add_payment_info ↔ purchase (0.84), begin_checkout ↔ purchase (0.72). 2. Correlation add_to_cart ↔ begin_checkout (0.48) is significantly lower than the conversions from point 1.  3. view_promotion and select_promotion are barely correlated with purchases (~0.14 and 0.03).
   
