 # UNIVARIATE ANALYSIS (Single-Variable Analysis)
- Using: **df_sessions_clean**


This analysis is conducted to understand how each variable in the dataset behaves individually.

| Variable Type | Action to be Taken 🛠️ | Example Columns 💡 |
| :--- | :--- | :--- |
| **Numeric (Sayısal)** | **Statistical Summary:** Mean, median, standard deviation, minimum, maximum, quartiles (`df.describe()`). | `page_clicks`, `seats`, `hotel_price_per_room_night_usd`, `flight_discount_amount` |
| | **Visualization:** Histograms to see the shape of the distribution, and Box Plots to detect outliers. | |
| **Categorical (Kategorik)** | **Frequency Analysis:** Count how often each category occurs (`value_counts()`). | `origin_airport`, `destination_airport` (if there are a small number of distinct values), transaction statuses. |
| | **Visualization:** Bar Plots to show the frequency of occurrences. | |

<img width="1047" height="370" alt="image" src="https://github.com/user-attachments/assets/4eb0ba94-bff6-4ac4-b7fa-978d4530d624" />


<img width="1085" height="710" alt="image" src="https://github.com/user-attachments/assets/f23a357b-911f-4126-a1b7-4e77af8cc98d" />

## 📊 Univariate Analysis: Visualization Insights 🖼️

This section summarizes the findings from the Histograms and Box Plots for the key numeric variables, identifying immediate needs for Feature Engineering (FE).

### 1. `page_clicks`

| Analysis Tool | Finding | Action Requirement 🚨 |
| :--- | :--- | :--- |
| **Histogram** | The distribution is **extremely right-skewed**. The vast majority of data points are concentrated between 0 and 50 clicks, while the mean (17.59) sits just to the right of the mode. | **Logarithmic Transformation (Log(1 + x)) is mandatory** to normalize the data for machine learning algorithms. |
| **Box Plot** | Clearly indicates **severe outliers**, including values exceeding 500 clicks. | |

### 2. `hotel_price_per_room_night_usd`

| Analysis Tool | Finding | Action Requirement ⚙️ |
| :--- | :--- | :--- |
| **Histogram** | The distribution is **right-skewed**, though less severely than `page_clicks`. Most prices are concentrated between 50 and 250 USD. | **Log Transformation is recommended** during the FE phase to improve model performance due to skewness. |
| **Box Plot** | Shows a significant number of outliers starting around 400 USD and extending up to 1200 USD+. | |

### 3. `nights`

| Analysis Tool | Finding | Action Requirement ✅ |
| :--- | :--- | :--- |
| **Histogram** | The distribution is **right-skewed**. Most bookings are for 1 to 5 nights, with the mean (3.71) pulled to the left of the main concentration. | **Log Transformation is the best approach** during the FE phase. |
| **Box Plot** | Outliers are present for bookings exceeding 10 nights (up to max 43). These are likely signals of long-term stays rather than data errors. | **Do not drop outliers.** Treat them as valuable signals of long-term reservations. |


# 📊 Univariate Analysis: Categorical Insights 

<img width="760" height="680" alt="image" src="https://github.com/user-attachments/assets/d0a50521-bec5-4f2f-b8e3-2f8133967eac" />

## ✈️ Univariate Analysis: Categorical Insights Summary

This summary outlines the key findings and implications for Feature Engineering (FE) based on the frequency analysis of categorical variables.

### 1. 🛫 Origin Airport Top 10 Analysis

| Finding 📝 | Impact on Action/FE ⚙️ |
| :--- | :--- |
| **Observation:** Travel activity in the dataset is heavily concentrated in major metropolitan hubs, specifically New York (LGA, JFK) and Los Angeles (LAX). The frequency counts for the top 3 airports are approximately three times or more than those of the subsequent airports. | **FE Implication:** This strongly confirms a geographical concentration of the user base around these major hubs. The geographical analysis must focus on these centers, and the distance to these key hubs should be engineered as a potent feature for the model. |

### 2. 🚫 Cancellation Status Analysis

| Finding 📝 | Impact on Action/FE ⚙️ |
| :--- | :--- |
| **Observation:** Cancellation events are notably rare. Compared to the total session count (49,211), the overall cancellation rate is approximately: $\frac{610}{48601 + 610} \approx 0.0124 \approx 1.24\%$. This signifies that the dataset predominantly consists of successful, completed, or non-cancelled sessions. | **FE/Modeling Implication:** If the `cancellation` column is designated as the target variable for a model, it represents a **highly imbalanced class distribution**. This severe imbalance necessitates specialized techniques during the modeling phase, such as **oversampling, undersampling, or class weighting**, to ensure adequate predictive performance for the minority class. |
