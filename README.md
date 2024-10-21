# Email-Marketing-Campaign-Analysis

## Business Objectives & User Stories

This project focuses on analyzing the performance of email marketing campaigns for a local business. The goal was to develop a comprehensive dashboard in Power BI to help the marketing team and executives monitor campaign effectiveness and user engagement, identify improvement opportunities, and optimize future marketing strategies. Below are the business objectives and user stories that guided the design and development of the dashboard.

### Business Objectives:
1. **Improve Campaign Performance Monitoring**  
   The marketing team needed a tool to track the delivery, open, and click-through rates (CTR) of email campaigns over time, as well as user engagement metrics. By understanding trends in these metrics, they aim to improve the effectiveness of future campaigns.
   
2. **Identify Key Customer Segments**  
   The business wanted to identify which customer segments were the most responsive to email campaigns. This would enable more personalized marketing strategies for different customer groups and lead to increased engagement.
   
3. **Reduce Subscription Cancellations**  
   A focus was placed on understanding why users unsubscribe from marketing emails. This would help the marketing team implement retention strategies and reduce churn rates.

4. **Evaluate Campaign Success against Business KPIs**  
   The sales and marketing managers needed to compare the performance of different email campaigns against predefined key performance indicators (KPIs) such as conversion rates, unsubscribe rates, and customer retention.

### User Stories:

| #  | As a (role)                   | I want (request/demand)                                           | So that I (user value)                                          | Acceptance Criteria                                          |
|----|-------------------------------|-------------------------------------------------------------------|-----------------------------------------------------------------|--------------------------------------------------------------|
| 1  | **Marketing Manager**          | Get an overview of email delivery, open, and click-through rates  | I can quickly identify campaign performance and potential issues | A Power BI dashboard with trend charts and daily data refresh |
| 2  | **Sales Representative**       | See detailed user engagement metrics by customer segment          | I can focus my marketing efforts on segments with the highest engagement | A segmented view with customer behavior over time             |
| 3  | **Marketing Analyst**          | Track unsubscribes by time and segment                            | I can understand why and when users are unsubscribing            | A dashboard showing unsubscribe trends and filter by segment  |
| 4  | **Sales Manager**              | Compare campaign performance with our business KPIs               | I can align marketing efforts with company goals                 | KPI dashboard with comparison against budget and targets      |
| 5  | **Marketing Team Member**      | Drill down on campaign-specific performance metrics               | I can optimize future campaigns based on detailed insights       | A detailed campaign view showing delivery, open, and CTR for each campaign |

## Data Transformation

In this project, I utilized Power BI's DAX (Data Analysis Expressions) to transform and calculate key performance metrics for the email marketing dashboard. These calculations help track important metrics such as delivery rates, open rates, and their changes over time. Below is an example of how I calculated the **Delivery Rate (DR)** and its related measures, including monthly comparisons and variance indicators.

### Example: Delivery Rate (DR) and Related Metrics

```DAX
-- Delivery Rate (DR)
Delivery Rate (DR) = 
DIVIDE(
    SUM('Data'[Доставлено]), 
    SUM(Data[Отправлено]), 
    0
)

-- Current Month Delivery Rate
Current Month DR = 
CALCULATE(
    [Delivery Rate (DR)], 
    DATESMTD (Data[Дата])
)

-- Previous Month Delivery Rate
Previous Month DR = 
CALCULATE(
    [Delivery Rate (DR)], 
    PREVIOUSMONTH(Data[Дата])
)

-- Delivery Rate Change (absolute value)
DR Change = 
[Current Month DR] - [Previous Month DR]

-- Delivery Rate Change (percentage)
DR Change % = 
DIVIDE(
    [Current Month DR] - [Previous Month DR], 
    [Previous Month DR], 
    0
)

-- Delivery Rate Variance with Arrow Indicator
DR Var % Arrow = 
VAR _uparrow = UNICHAR(129129) -- Up arrow character
VAR _downarrow = UNICHAR(129131) -- Down arrow character
VAR _varpercentage = [DR Change %]
VAR _varnumber = [DR Change]
VAR _blank = ISBLANK([Delivery Rate (DR)])
RETURN
    IF(
        _blank, 
        BLANK(), 
        IF(
            _varnumber > 0, 
            ROUND(_varpercentage, 2) * 100 & "% " & _uparrow, 
            ROUND(_varpercentage, 2) * 100 & "% " & _downarrow
        )
    )
