# Netflix-User-Behavior-Analysis

Link for the dataset: [Netflix Userbase Dataset](https://www.kaggle.com/datasets/arnavsmayan/netflix-userbase-dataset)

---

## Table of Contents

1. [About Dataset](#about-dataset)
2. [Data Dictionary](#data-dictionary)
3. [Dataset Screenshot](#dataset-screenshot)
4. [SQL Queries](#sql-queries)
   - [Simple Queries](#simple-queries)
   - [Beginner-Level Queries](#beginner-level-queries)
   - [Intermediate-Level Queries](#intermediate-level-queries)
   - [Advanced-Level Queries](#advanced-level-queries)
5. [Power BI Visuals](#power-bi-visuals)

[Back to Top](#netflix-user-behavior-analysis)

---

## About Dataset 

The dataset provides a snapshot of a sample Netflix userbase 3 years data 2021, 2022 & 2023, showcasing various aspects of user subscriptions, revenue, account details, and activity. Each row represents a unique user, identified by their User ID. The dataset includes information such as the user's subscription type (Basic, Standard, or Premium), the monthly revenue generated from their subscription, the date they joined Netflix (Join Date), the date of their last payment (Last Payment Date), and the country in which they are located.

Additional columns have been included to provide insights into user behavior and preferences. These columns include Device Type (e.g., Smart TV, Mobile, Desktop, Tablet) and Account Status (whether the account is active or not). The dataset serves as a synthetic representation and does not reflect actual Netflix user data. It can be used for analysis and modeling to understand user trends, preferences, and revenue generation within a hypothetical Netflix userbase.

[Back to Top](#table-of-contents)

---

## Data Dictionary 

The dataset contains a total of 10 columns and 1,500 rows:

- **User_ID**: Unique identifier for each user.
- **Subscription_Type**: Type of subscription the user has (e.g., Basic, Premium, Standard).
- **Monthly_Revenue**: The revenue generated monthly from the user's subscription.
- **Join_date_new**: The date when the user subscribed to the service.
- **Last_Payment_Date_modify**: The most recent payment date for the user.
- **Country**: The country where the user is located.
- **Age**: Age of the user.
- **Gender**: The gender of the user.
- **Device**: The primary device used by the user (e.g., Smartphone, Laptop).
- **Plan_Duration**: Duration of the subscription plan (e.g., 1 Month, 6 Months).

[Back to Top](#table-of-contents)

---

## Dataset Screenshot 

![image](https://github.com/user-attachments/assets/f273f739-1b07-4c7e-9698-1c1e7a06211b)

[Back to Top](#table-of-contents)

---

## SQL Queries 

### **Simple Queries**

SELECT TOP 10 * 

FROM [Netflix user base];

![image](https://github.com/user-attachments/assets/939c315b-c52e-44e6-83ea-6c187be54f07)

---

### **Beginner-Level Queries**

Query 1: Select Users from a Specific Country

Code: SELECT User_ID, Subscription_Type, Country

FROM [Netflix user base]

WHERE Country = 'Canada';

This query uses basic SELECT, FROM, and WHERE clauses to filter users from Canada.

 ![image](https://github.com/user-attachments/assets/9c9c1af8-1238-4762-aefd-093dc2dff979)

 ---

Query 2: Find Users with a Specific Device

This query filters the users who use a Smartphone as their device. 

Code: 

SELECT User_ID, Device, Country, Age

FROM [Netflix user base]

WHERE Device = 'Smartphone';

![image](https://github.com/user-attachments/assets/e018298a-3567-455b-910b-7c019f3409fd)

---

### **Intermediate-Level Queries**

Query 3: Count Users by Subscription Type

Code: 

SELECT Subscription_Type, COUNT(User_ID) AS UserCount

FROM [Netflix user base]

GROUP BY Subscription_Type;

This query counts the number of users for each subscription type using the GROUP BY clause.
 
![image](https://github.com/user-attachments/assets/fda804b0-bf0b-438e-8e73-00a885495e18)

---

Query 4: Total Revenue by Country

Code:

SELECT Country, SUM(Monthly_Revenue) AS TotalRevenue

FROM [Netflix user base]

GROUP BY Country;

This query sums the total Monthly Revenue for each country, grouped by Country.

![image](https://github.com/user-attachments/assets/8bdcbdb4-f365-4dd9-86ad-12d2d70b9d87)

---

Query 5: Average Age by Subscription Type

Code

SELECT Subscription_Type, AVG(Age) AS AverageAge

FROM [Netflix user base]

GROUP BY Subscription_Type;

This query calculates the average age of users for each subscription type.

 ![image](https://github.com/user-attachments/assets/e40f6de6-faae-4716-8aaf-dd76887d5be1)

 ---

Query 6: Users Who Joined Before a Specific Date

Code

SELECT User_ID, Subscription_Type, Join_date_new

FROM [Netflix user base]

WHERE Join_date_new < (SELECT MAX(Join_date_new) FROM [Netflix user base] WHERE Join_date_new < '2022-01-01');

This query uses a subquery to find users who joined Netflix before a given date.

 ![image](https://github.com/user-attachments/assets/097396b3-20d8-4e13-a2d5-fa6ef33a7067)

 ---

### **Advanced-Level Queries**

Query 7:Find the Most Recent Payment Date for Each User
Code

WITH MostRecentPayment AS (

    SELECT User_ID, MAX(Last_Payment_Date_modify) AS MostRecentPayment
    
    FROM [Netflix user base]
    
    GROUP BY User_ID

)

SELECT *

FROM MostRecentPayment

ORDER BY MostRecentPayment DESC;

This query will use a CTE to find the most recent payment made by each user.

 ![image](https://github.com/user-attachments/assets/f9baaf2c-908f-4762-afc3-c309e7e4fbba)

 ---

Query 8: Cumulative Revenue per Country

Code: 

SELECT Country, User_ID, Monthly_Revenue,

       SUM(Monthly_Revenue) OVER (PARTITION BY Country ORDER BY Join_date_new) AS CumulativeRevenue

FROM [Netflix user base];

This query uses a window function to calculate the cumulative revenue for each country, ordered by the user's join date.

 ![image](https://github.com/user-attachments/assets/1c6a4485-dd9e-4161-8eaa-01c53599dd67)

 ---

Query 9: Moving Average of Monthly Revenue

Code:

SELECT User_ID, Monthly_Revenue,

       AVG(Monthly_Revenue) OVER (ORDER BY Join_date_new ROWS BETWEEN 2 PRECEDING AND CURRENT ROW) AS MovingAverageRevenue

FROM [Netflix user base];

This query calculates a moving average of the monthly revenue over the previous 3 users, ordered by the join date.

 ![image](https://github.com/user-attachments/assets/12e804b8-85c5-4f03-862f-7d11dedfae3d)

 ---

Query 10: Find Users with Longest Duration Between Join and Last Payment

Code

WITH PaymentDuration AS (

  SELECT User_ID, DATEDIFF(day, Join_date_new, Last_Payment_Date_modify) AS Duration
  
  FROM [Netflix user base]

)

SELECT * 

FROM PaymentDuration

ORDER BY Duration DESC;

This query uses a CTE to calculate the duration between the join date and last payment date, and ranks users based on the longest duration.

![image](https://github.com/user-attachments/assets/5055614c-9a6b-40ac-b3c6-c67ecab8b64a)

[Back to Top](#netflix-user-behavior-analysis)

---

## PowerBi Visuals 

![image](https://github.com/user-attachments/assets/18f46e89-4188-4249-8e7a-d472b8533295)

Visual-1

The bar chart shows the number of users for each subscription type. The Basic plan has the highest number of users with 999, followed by the Standard plan with 768 users, and the Premium plan with 733 users. This indicates that more users prefer the Basic plan over the higher-tier options.

Visual-2

The pie chart shows how users access Netflix based on their device type. Smart TVs are the most used device, with 25.44% of users, followed closely by Tablets at 25.32%, and Smartphones at 24.84%. Laptops account for 24.4%, showing that device usage is fairly evenly distributed across all types

Visual-3

The monthly revenue by subscription type graph shows how revenue fluctuates throughout the year, with peaks observed between July and October. The Basic plan consistently generates the most revenue each month, followed by the Premium and Standard plans. This indicates that more users are subscribed to the Basic plan, but the Premium plan also contributes a significant share.
The map complements this by showing the geographical distribution of revenue. The United States, Canada, and Mexico are the top contributors to Netflix's revenue, with the U.S. standing out as the largest market. Additionally, countries in Europe, like the United Kingdom and Spain, also play a significant role, alongside Australia in the southern hemisphere. Together, these visuals provide a clear view of how revenue is distributed across both time and geography

[Back to Top](#netflix-user-behavior-analysis)

---
 
