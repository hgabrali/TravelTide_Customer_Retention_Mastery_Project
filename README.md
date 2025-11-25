
## 🏆 The Mastery Project: End-to-End Data Challenge
 
### 🎯 The Mastery Project: Strategic Execution Summary

This **Mastery Project** served as the culmination of my analytical expertise. I successfully tackled a complex, real-world data challenge engineered to simulate the high-stakes decision-making required in a professional data environment.

My execution involved a comprehensive, end-to-end workflow: I rigorously conducted **Data Exploration (EDA) 🔎**, implemented sophisticated **Feature Engineering ⚙️**, delivered **ML-based Customer Segmentation 🤖**, and developed **Actionable Strategic Recommendations 📈**. This effort profoundly reinforced my technical prowess and problem-solving agility.

### Course Structure Overview
* **Week 1: Project Exploration:** Familiarize yourself with the dataset and business context.
* **Week 2: Feature Engineering & Customer Segmentation:** Execute Feature Engineering to improve data quality. Create meaningful data metrics to analyze user behavior.
* **Week 3: Developing Insights & Strategy:** Segment customers into distinct groups based on data patterns. Refine your data-driven recommendations based on customer insights.
* **Week 4: Presenting Your Findings:** Structure your final project submission with clear insights and justifications. Deliver a data-backed strategy in a professional format.

### What This Course Offers
* **Real-World Data Problem:** Work with a business dataset and develop a complete analytical solution.
* **End-to-End Project Experience:** Move through every stage of a data project, from exploration to strategic recommendations.
* **Portfolio-Ready Work:** Develop a structured project that showcases your analytical and storytelling skills.

## 🎯 Project Goals and Objectives

The primary objective of this project is to develop a data-driven customer retention strategy for TravelTide by leveraging advanced analytical and Machine Learning techniques.

### I. Data Validation and Foundation
1.  **Establish Data Integrity:** Successfully connect to the PostgreSQL database, fetch raw data, and perform comprehensive cleaning (e.g., date conversion, imputing missing discount values with 0) to ensure high data quality.
2.  **Generate Core Metrics (Feature Engineering):** Engineer meaningful user-level attributes (e.g., tenure, booking frequency, discount sensitivity) from session-level data to enable robust behavioral analysis.

### II. Customer Segmentation and Insight Generation
3.  **Validate Business Hypothesis:** Use clustering methods (e.g., KMeans) to objectively test Elena Tarrant's hypothesis regarding the existence of distinct customer segments sensitive to specific rewards/perks.
4.  **Identify Meaningful Segments:** Divide the entire customer base into distinct, interpretable groups based on behavioral patterns and discount sensitivity (ML-Based Segmentation).

### III. Strategic Recommendation and Delivery
5.  **Assign Optimal Perks:** For each identified customer segment, assign the corresponding "favorite perk" from the proposed list (e.g., Free Cancellation, Free Hotel Meal).
6.  **Develop Actionable Strategy:** Deliver a clear, data-backed strategic recommendation to the Head of Marketing (Elena Tarrant) on how to personalize rewards invitations to maximize customer sign-ups and improve long-term retention.



## ✈️ [Company: The TravelTide Company](https://www.travel-tide.com/)

### Context: Poor Customer Retention
E-booking startup TravelTide has maintained a hyper-focus on building the biggest travel inventory and making it easily searchable. Because of this narrow focus, certain aspects of the TravelTide customer experience are underdeveloped, resulting in **poor customer retention**. CEO Kevin Talanick is motivated to retain and add value to existing customers with a Marketing strategy built on a solid understanding of customer behavior.

### Role of the Head of Marketing (Elena Tarrant)
Elena Tarrant, the new Head of Marketing, is an expert in **customer retention strategies**, specifically rewards programs. Elena believes that to grab customers’ attention and maximize the likelihood they will sign up, we must **emphasize the perk we think they are most interested in** when we ask them to sign up.

### Our Mission as Data Analysts
Our mission is two-fold:
1.  Check if the data supports Elena’s hypothesis about the existence of customers interested in the proposed perks.
2.  For each customer, **assign a likely favorite perk**.

## 🧭 Approaching the Project: Detailed Time Structure

In each of the four weeks, we will get one step further in our goal to help Elena and TravelTide!

### 1️⃣ EDA - Exploring the Data:
* **Goal:** Familiarize yourself with the business context and available data. Clean and prepare the data.
* **Outcome:** A cleaned, filtered table at the session level; a first aggregated table on a user level, ready for customer analysis.

### 2️⃣ Feature Engineering - Devising Metrics:
* **Goal:** Perform **Feature Engineering** to devise metrics or new attributes that are meaningful for segmentation. Think about how customers in the travel business differ from each other.

### 3️⃣ Customer Segmentation - Grouping the Customers:
* **Goal:** Perform customer analysis using the created metrics. Use **clustering methods** (Machine Learning) to segment customers and determine the meaning of the computationally found groups.
* **Outcome:** The assignment of all suitable customers into groups based on segmentation criteria and an assigned perk that will suit them.

### 4️⃣ Presentation:
* **Goal:** Create an executive summary and slides of the customer analysis results. Describe the characteristics (behavior, preferences) of each customer group/cluster.
* **Deliverables:** Include recommendations on how to measure the success of the customer analysis in the upcoming months. Use data storytelling principles and good visualizations (bar plot, scatter plot, radar plot, etc.).

 [Data Science Toolkit Comparison and Project Functionality](https://github.com/hgabrali/TravelTide_Customer_Retention_Mastery_Project/blob/main/Project%20workflow.md)

 # ✈️ Travel Booking Analytics: Behavioral Segmentation & ML

![Python](https://img.shields.io/badge/Python-3.9%2B-blue) ![Library](https://img.shields.io/badge/Library-Pandas%20%7C%20Scikit--Learn-orange) ![Status](https://img.shields.io/badge/Status-Completed-green)



## 📋 Project Overview
This repository hosts an end-to-end data analysis and machine learning pipeline designed to optimize revenue strategies for a digital travel booking platform. By analyzing **49,000+ user sessions**, this project moves beyond simple descriptive statistics to uncover deep behavioral patterns.

The core objective was to answer two critical business questions:
1.  **Temporal Dynamics:** How does user value and reliability shift between Weekdays and Weekends?
2.  **Customer Personas:** Can we use Unsupervised Machine Learning to identify and group high-value users?

## 🛠️ Tech Stack
* **Data Manipulation:** `Pandas`, `NumPy`
* **Machine Learning:** `Scikit-Learn` (K-Means Clustering, StandardScaler)
* **Visualization:** `Seaborn`, `Matplotlib`
* **Statistical Testing:** T-Tests (SciPy)

---

## 💡 Executive Summary & Key Results

Through rigorous EDA and Feature Engineering, we transformed raw transactional data into actionable business intelligence.

### 1. The "Weekend Effect" (Temporal Analysis)
Our analysis revealed that **Weekends** are the prime window for revenue generation, but they come with higher volatility.
* **Spending Power:** Users spend **+30% more** on hotels and **+21% more** on flights during the weekend.
* **Engagement:** Weekend sessions show significantly deeper research behavior (**+43%** higher page clicks).
* **Risk:** However, the cancellation rate spikes to **15%** on weekends (vs. 9% on weekdays), indicating impulsive behavior.

### 2. ML Segmentation (K-Means Clustering)
We successfully deployed a K-Means algorithm to classify the user base into 3 distinct personas:
* 🐋 **The Weekend Whales:** High-spend, leisure-focused users who book premium inventory.
* 🛒 **The Window Shoppers:** High-engagement but high-churn browsers (primary targets for retargeting).
* 💼 **The Budget Commuters:** Low-spend, efficiency-focused users sensitive to discounts.

> **Business Impact:** These insights allow for dynamic pricing strategies, personalized UI experiences, and targeted churn-prevention flows based on the day of the week and user cluster.

---


