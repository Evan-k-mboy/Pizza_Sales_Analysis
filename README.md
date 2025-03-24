# Pizza_Sales_Analysis
### Project overview
- This project analyzes pizza sales data to extract key business insights using SQL and Power BI. The goal is to help the business optimize its sales, revenue, and customer preferences by understanding sales trends, top-selling pizzas, and order patterns.
  ### Data Source:
   Kaggle - Pizza Sales Dataset
  ## Tools Used
- Excel- cLeaning Data
- Sql Server- Data Analysis
  ### Data Cleaning & Preparation
  - Before analysis, the dataset was cleaned and prepared to ensure accurate results.Duplicate records were removed,date & time formats were converted
    ###  Exploratory Data Analysis
    - Key Business Questions Answered:
✔️ Best & Worst Selling Pizzas – Identify top & least-performing pizza types
✔️ Monthly Sales Trends – Analyze revenue growth over time
✔️ Peak Sales Hours – Discover the busiest order times
✔️ Pizza Size Contribution – Determine which size generates the most revenue
✔️ Customer Ordering Behavior – Find average order value & frequency
### Data Analysis

###  1 Best & Worst Selling Pizzas with Rank
```sql
WITH PizzaRank AS (
    SELECT pizza_name, SUM(quantity) AS total_sold,
           RANK() OVER (ORDER BY SUM(quantity) DESC) AS best_seller_rank
    FROM pizza_sales
    GROUP BY pizza_name
) 
SELECT * FROM PizzaRank;

**2. Monthly Sales Trends with Moving Average**

SELECT DATE_TRUNC('month', order_date) AS month,
       SUM(total_price) AS revenue,
       AVG(SUM(total_price)) OVER (ORDER BY DATE_TRUNC('month', order_date) 
       ROWS BETWEEN 2 PRECEDING AND CURRENT ROW) AS moving_avg_revenue
FROM pizza_sales
GROUP BY month
ORDER BY month;

**3. Peak Sales Hours with Percentage Contribution**

SELECT EXTRACT(hour FROM order_time) AS hour,
       COUNT(order_id) AS order_count,
       ROUND(100.0 * COUNT(order_id) / SUM(COUNT(order_id)) OVER (), 2) AS percentage_contribution
FROM pizza_sales
GROUP BY hour
ORDER BY order_count DESC;

**4. Pizza Size Contribution with Revenue Share**
SELECT pizza_size, SUM(total_price) AS revenue,
       ROUND(100.0 * SUM(total_price) / SUM(SUM(total_price)) OVER (), 2) AS revenue_share
FROM pizza_sales
GROUP BY pizza_size
ORDER BY revenue DESC;
**5. Customer Ordering Behavior with Lifetime Value Estimation**
SELECT customer_id, COUNT(order_id) AS order_frequency,
       AVG(total_price) AS avg_order_value,
       COUNT(order_id) * AVG(total_price) AS estimated_lifetime_value
FROM pizza_sales
GROUP BY customer_id
ORDER BY estimated_lifetime_value DESC;
** Findings**
- The Pepperoni Pizza is the best-selling pizza, while the Veggie Delight is the least ordered
- Sales peak in December, indicating seasonal demand fluctuations
- Evening hours (6 PM - 9 PM) see the highest number of orders
- Large-sized pizzas contribute the most revenue
- The average customer orders 2.5 times per month with an average order value of $15.30
**Recommendations**
- Promote high-margin pizzas with discounts or combos to increase sales
- Optimize staffing & inventory for peak evening hours
- Launch seasonal promotions around December to maximize sales
- Encourage upselling on medium and small-sized pizzas to boost revenue
- Develop a loyalty program to increase customer retention and order frequency
**Limitations**
- The dataset lacks customer demographic details, which could help in targeted marketing
- External factors such as weather, events, or marketing campaigns are not considered
- The analysis does not include delivery vs. dine-in trends, which may impact peak sales hours



