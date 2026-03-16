# E-commerce_Conversion_Funnel-BigQuery-Google_Colab_Project 🚀
 
Welcome! This repository showcases an e-commerce data analysis project powered by BigQuery and Google Colab.

## Data Source Description 📂

The data source is the BigQuery GA4 public dataset: 'bigquery-public-data.ga4_obfuscated_sample_ecommerce.events_2021*', which provides data for January 2021. The dataset consists of 1,210,147 rows.

## Gallery and Insights 📊 💡

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

### 5. Dynamic Purchase CR by Source/Medium and Its Trend Line.
![dynamic cr](image/dynamic_cr1.png)

Traffic wihh (data deleted/data deleted) has higher CR than others. Overall trend line shows slight growth by month-end.

### 6. Purchase CR by Number of Landing Page Visits.
![cr from landing page](image/cr_session_start_count1.png)

Traffic wihh (data deleted/data deleted) has higher CR than others. Overall trend line shows slight growth by month-end.

### 7. GA4 Ecommerce Event Correlation Matrix.
![heat map](image/heat_map1.png)
1) Strong positive correlations (red zones): begin_checkout ↔ add_payment_info (0.85), add_payment_info ↔ purchase (0.84), begin_checkout ↔ purchase (0.72).
2) Correlation add_to_cart ↔ begin_checkout (0.48) is significantly lower than the conversions from point 1.
3) Events view_promotion and select_promotion are barely correlated with purchases (~0.14 and 0.03).
   
## SQL Queries. 🔍

<details>
<summary>1. Data Extraction from BigQuery and Initial Table Creation.</summary>

 ```sql
 SELECT timestamp_micros(event_timestamp) AS event_timestamp,
        user_pseudo_id || '+' || (SELECT value.int_value FROM UNNEST(event_params) WHERE key = 'ga_session_id') AS user_session_id,
        event_name,
        geo.country AS country,
        device.category AS device_category,
        traffic_source.source AS source,
        traffic_source.medium AS medium,
        traffic_source.name AS campaign
 FROM `bigquery-public-data.ga4_obfuscated_sample_ecommerce.events_2021*`
 ORDER BY event_timestamp
```

</details>

<details>
<summary>2. Creating Daily Aggregated Event Pivot Table for Funnel Analysis and Conversion Rate Calculation by Acquisition Channels</summary>

 ```sql
WITH init AS (
      SELECT date(timestamp_micros(event_timestamp)) as event_date,
             traffic_source.source AS source,
             traffic_source.medium AS medium,
             traffic_source.name AS campaign,
            user_pseudo_id || (SELECT value.int_value FROM t.event_params  WHERE  key='ga_session_id') AS user_session_id,
            event_name
      FROM  `bigquery-public-data.ga4_obfuscated_sample_ecommerce.events_2021*` t
      )
SELECT event_date,
       source,
       medium,
       campaign,
       COUNT(DISTINCT user_session_id) AS user_sessions_count,
       COUNT(DISTINCT CASE WHEN event_name = 'session_start' THEN user_session_id END) AS session_start,
       COALESCE(COUNT(DISTINCT CASE WHEN event_name = 'add_to_cart' THEN user_session_id END), 0) /
            NULLIF(COUNT(DISTINCT CASE WHEN event_name = 'session_start' THEN user_session_id END), 0) AS add_to_cart,
       COALESCE(COUNT(DISTINCT CASE WHEN event_name = 'begin_checkout' THEN user_session_id END), 0) /
                  NULLIF(COUNT(DISTINCT CASE WHEN event_name = 'session_start' THEN user_session_id END), 0) AS begin_checkout,
       COALESCE(COUNT(DISTINCT CASE WHEN event_name = 'add_shipping_info' THEN user_session_id END), 0) /
                  NULLIF(COUNT(DISTINCT CASE WHEN event_name = 'session_start' THEN user_session_id END), 0) AS add_shipping_info,
       COALESCE(COUNT(DISTINCT CASE WHEN event_name = 'add_payment_info' THEN user_session_id END), 0) /
                  NULLIF(COUNT(DISTINCT CASE WHEN event_name = 'session_start' THEN user_session_id END), 0) AS add_payment_info,
       COALESCE(COUNT(DISTINCT CASE WHEN event_name = 'purchase' THEN user_session_id END), 0) /
                  NULLIF(COUNT(DISTINCT CASE WHEN event_name = 'session_start' THEN user_session_id END), 0) AS purchase
FROM init
GROUP BY event_date, source, medium, campaign
ORDER BY event_date, source, medium
```
</details>
