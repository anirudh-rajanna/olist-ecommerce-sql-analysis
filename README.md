-- ============================================
-- OLIST E-COMMERCE SQL ANALYSIS
-- Author: Anirudh Rajanna
-- Dataset: Brazilian E-Commerce Public Dataset by Olist
-- ============================================

-- ============================================
-- QUERY 1: Top 10 Product Categories by Revenue
-- Business Question: Which product categories generate the most revenue?
-- ============================================

SELECT 
    ct.product_category_name_english AS category,
    ROUND(SUM(oi.price + oi.freight_value), 2) AS total_revenue,
    COUNT(DISTINCT oi.order_id) AS total_orders
FROM order_items oi
JOIN products p ON oi.product_id = p.product_id
JOIN category_translation ct ON p.product_category_name = ct.product_category_name
GROUP BY ct.product_category_name_english
ORDER BY total_revenue DESC
LIMIT 10;

-- ============================================
-- QUERY 2: Monthly Revenue Trend
-- Business Question: How has revenue grown over time?
-- ============================================

SELECT 
    DATE_FORMAT(STR_TO_DATE(order_purchase_timestamp, '%Y-%m-%d %H:%i:%s'), '%Y-%m') AS month,
    ROUND(SUM(oi.price + oi.freight_value), 2) AS total_revenue,
    COUNT(DISTINCT o.order_id) AS total_orders
FROM orders o
JOIN order_items oi ON o.order_id = oi.order_id
WHERE o.order_status = 'delivered'
GROUP BY month
ORDER BY month;

-- ============================================
-- QUERY 3: Top 10 States by Revenue
-- Business Question: Which states contribute most to revenue?
-- ============================================

SELECT 
    c.customer_state AS state,
    ROUND(SUM(oi.price + oi.freight_value), 2) AS total_revenue,
    COUNT(DISTINCT o.order_id) AS total_orders,
    ROUND(AVG(oi.price + oi.freight_value), 2) AS avg_order_value
FROM orders o
JOIN order_items oi ON o.order_id = oi.order_id
JOIN customers c ON o.customer_id = c.customer_id
GROUP BY c.customer_state
ORDER BY total_revenue DESC
LIMIT 10;

-- ============================================
-- QUERY 4: Payment Method Analysis
-- Business Question: How do customers prefer to pay?
-- ============================================

SELECT 
    payment_type,
    COUNT(DISTINCT order_id) AS total_orders,
    ROUND(SUM(payment_value), 2) AS total_value,
    ROUND(AVG(payment_value), 2) AS avg_payment,
    ROUND(AVG(payment_installments), 1) AS avg_installments
FROM payments
GROUP BY payment_type
ORDER BY total_orders DESC;

-- ============================================
-- QUERY 5: Top 10 Sellers by Revenue
-- Business Question: Who are the top performing sellers?
-- ============================================

SELECT 
    s.seller_id,
    s.seller_city,
    s.seller_state,
    COUNT(DISTINCT oi.order_id) AS total_orders,
    ROUND(SUM(oi.price + oi.freight_value), 2) AS total_revenue,
    ROUND(AVG(oi.price), 2) AS avg_product_price
FROM sellers s
JOIN order_items oi ON s.seller_id = oi.seller_id
GROUP BY s.seller_id, s.seller_city, s.seller_state
ORDER BY total_revenue DESC
LIMIT 10;

-- ============================================
-- QUERY 6: Average Delivery Time by State
-- Business Question: Which states have the longest delivery times?
-- ============================================

SELECT 
    c.customer_state AS state,
    COUNT(DISTINCT o.order_id) AS total_orders,
    ROUND(AVG(DATEDIFF(o.order_delivered_customer_date, o.order_purchase_timestamp)), 1) AS avg_delivery_days
FROM orders o
JOIN customers c ON o.customer_id = c.customer_id
WHERE o.order_status = 'delivered'
    AND o.order_delivered_customer_date IS NOT NULL
    AND o.order_delivered_customer_date != ''
GROUP BY c.customer_state
ORDER BY avg_delivery_days DESC;

-- ============================================
-- QUERY 7: Late Delivery Analysis
-- Business Question: What percentage of orders are delivered late?
-- ============================================

SELECT 
    CASE 
        WHEN order_delivered_customer_date <= order_estimated_delivery_date THEN 'On-Time'
        WHEN order_delivered_customer_date > order_estimated_delivery_date THEN 'Late'
        ELSE 'Unknown'
    END AS delivery_status,
    COUNT(*) AS total_orders,
    ROUND(COUNT(*) * 100.0 / SUM(COUNT(*)) OVER(), 2) AS percentage
FROM orders
WHERE order_status = 'delivered'
    AND order_delivered_customer_date IS NOT NULL
    AND order_delivered_customer_date != ''
    AND order_estimated_delivery_date IS NOT NULL
    AND order_estimated_delivery_date != ''
GROUP BY delivery_status
ORDER BY total_orders DESC;

-- ============================================
-- QUERY 8: Product Categories by Order Volume
-- Business Question: Which categories have the most orders?
-- ============================================

SELECT 
    ct.product_category_name_english AS category,
    COUNT(DISTINCT oi.order_id) AS total_orders,
    COUNT(oi.order_item_id) AS total_items_sold,
    ROUND(AVG(oi.price), 2) AS avg_price
FROM order_items oi
JOIN products p ON oi.product_id = p.product_id
JOIN category_translation ct ON p.product_category_name = ct.product_category_name
GROUP BY ct.product_category_name_english
ORDER BY total_orders DESC
LIMIT 10;
