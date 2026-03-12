# Olist E-Commerce SQL Analysis

## Project Overview
This project analyzes the Brazilian E-Commerce dataset from Olist, containing approximately 100,000 orders from 2016-2018. Using SQL, I explored sales performance, customer behavior, delivery operations, and payment patterns to extract actionable business insights.

## Dataset
**Source:** [Kaggle - Brazilian E-Commerce Public Dataset by Olist](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce)

The dataset contains 8 tables:
- **orders** - 99,441 orders with timestamps and delivery information
- **order_items** - 112,650 items with pricing and seller details
- **customers** - 99,441 customers with location data
- **payments** - 103,886 payment records
- **products** - 32,951 products with category information
- **sellers** - 3,095 sellers with location data
- **category_translation** - 71 category name translations (Portuguese to English)

## Key Business Questions Answered

### 1. Revenue Analysis
- **Top Product Categories:** Health & Beauty leads with $1.44M in revenue, followed by Watches & Gifts ($1.31M) and Bed, Bath & Table ($1.24M)
- **Geographic Performance:** São Paulo (SP) dominates with $5.92M in revenue (41,375 orders), followed by Rio de Janeiro (RJ) at $2.13M

### 2. Customer Behavior
- **Payment Preferences:** Credit cards account for 76,505 orders (76%) with an average of 3.5 installments per purchase
- **Boleto (bank slip):** Second most popular with 19,784 orders

### 3. Delivery Performance
- **On-Time Delivery Rate:** 91.89% of orders delivered on time
- **Late Deliveries:** 8.11% of orders delivered after estimated date
- **Longest Delivery Times:** Roraima (RR) averages 29.3 days, likely due to remote location

### 4. Seller Insights
- **Top Seller Revenue:** $249,640 from a seller in Guariba, SP
- **Highest Average Price:** Seller from Lauro de Freitas, BA with $543 average product price

## Technical Skills Demonstrated
- Complex SQL JOINs across multiple tables
- Aggregate functions (SUM, COUNT, AVG)
- Date manipulation and calculations
- CASE statements for conditional logic
- Window functions for percentage calculations
- Data filtering and NULL handling

## Files
- `olist_analysis.sql` - All SQL queries with business context and comments

## Tools Used
- MySQL 8.0
- MySQL Workbench

## Author
**Anirudh Rajanna**
- [LinkedIn](https://linkedin.com/in/anirudh-rajanna98)
- Master of Business Analytics, Iowa State University (May 2026)

## Related Projects
- [Olist E-Commerce Tableau Dashboard](https://public.tableau.com/app/profile/anirudh.rajanna2985/viz/OlistE-CommercePerformanceAnalysis/OlistE-CommerceAnalysis) - Interactive visualization of the same dataset
