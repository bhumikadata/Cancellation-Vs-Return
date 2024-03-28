# E-commerce Orders Analysis

This repository contains SQL queries to analyze e-commerce order data, specifically to calculate cancellation rates and return rates for each month based on the order date, delivery date, and cancel date information.

## Problem Statement

Given an e-commerce dataset with order details including order date, delivery date, and cancel date, we need to calculate the cancellation rate and return rate for each month. If an order is cancelled after it is delivered, it is considered a return order; otherwise, it is considered a cancelled order.

![Day23](https://github.com/bhumikadata/E-Commerce-order-analysis/assets/131578649/6efabb3b-4dd7-4b6a-b840-8d749b9a30ff)


### SQL Solution

I have provided a SQL solution to address this problem. The solution involves:

1. Extracting the year and month from the order date.
2. Calculating the total number of orders placed, cancelled orders, and returned orders for each month.
3. Using these values to compute the cancellation rate and return rate.
4. Rounding the rates to two decimal places.
5. Ordering the output by the increasing order of year and month.

## SQL Query

```sql
WITH all_data AS (
    SELECT 
        STRFTIME('%Y%m', order_date) AS order_year_month,
        COUNT(order_date) AS total_order_placed,
        SUM(CASE WHEN delivery_date < cancel_date THEN 1 ELSE 0 END) AS no_of_returned_orders,
        SUM(CASE WHEN order_date < cancel_date AND delivery_date IS NULL THEN 1 ELSE 0 END) AS no_of_canceled_order
    FROM 
        orders
    GROUP BY 
        STRFTIME('%Y%m', order_date) 
)
SELECT 
    order_year_month,
    ROUND(no_of_canceled_order*1.0/(total_order_placed - no_of_returned_orders)*100,2) AS cancellation_rate,
    ROUND(no_of_returned_orders*1.0/(total_order_placed - no_of_canceled_order)*100,2) AS return_rate
FROM 
    all_data;
```

## Use Case

- Analyzing cancellation and return rates in e-commerce order data offers valuable insights into platform performance and customer behavior. By monitoring trends over time, businesses can identify areas for improvement in product quality, delivery efficiency, and customer service.

- This data also aids in optimizing inventory management and supply chain operations, ensuring adequate stock levels and minimizing losses.
  
- Understanding the reasons behind cancellations and returns helps enhance the overall customer experience, leading to higher satisfaction and retention rates.





