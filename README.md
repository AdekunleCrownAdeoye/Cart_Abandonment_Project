# Cart Abandonment Analysis

## Table of Content

- [Project Overview](#project-overview)
- [Data Source](#data-source)
- [Tools](#tools)
- [Data Cleaning and Preparation](#data-cleaning-and-preparation)
- [Exploratory Data Analysis](#exploratory-data-analysis)
- [Data Analysis](#data-analysis)
- [Result and Findings](#result-and-findings)
- [Recommendations](#recommendations)
- [Limitations](#limitations)
- [References](#references)

### Project Overview

A data analysis project utilizing SQL Server and Power BI revealed that the company's lower abandonment rate compared to industry standards is primarily attributed to its optimized mobile checkout process. The analysis identified mobile optimization as a significant factor in reducing cart abandonment. Additionally, the project highlighted the need for targeted marketing strategies for specific product categories and potential improvements in shipping policies to address abandonment concerns.

![Report](https://github.com/user-attachments/assets/bf5911f2-8d29-42e1-8036-7b83a4a1ff3b)

![Dashboard ](https://github.com/user-attachments/assets/1d0c6814-63c8-4ca5-bf4b-ce00c49e4556)


### Data Source

Cart Abandonment: The primary dataset used for this analysis is the "Abandonment_dataset.csv" file, containing detailed information about each cart order made by Magicmade.

### Tools

- Excel | data cleaning
- SQL Server | data analysis
- Power BI | creating report & dashboard

### Data Cleaning and Preparation

In the initial data preparation phase, I performed the following tasks:
1. Data loading and Inspection.
2. Handling missing values and removing duplicates.
3. Data cleaning and formatting.

### Exploratory Data Analysis

EDA involves exploring the data to answer key questions such as:
1. User Behavior:
- What are the primary reasons for cart abandonment among users?
- How do different devices (mobile, tablet, desktop) impact user behavior and abandonment rates?
- What is the typical user journey leading to cart abandonment?
2. Website Optimization:
- How can the checkout process be optimized for mobile users to reduce abandonment?
- What steps can be taken to simplify the checkout process and address common pain points?
- How can the website provide more transparency in pricing and shipping costs?
3. Marketing Strategies:
- What targeted marketing strategies can be implemented to address abandonment for specific product categories?
- How can social media be leveraged more effectively to retarget abandoned carts?
- What other marketing channels or tactics could be explored to improve conversions?
4. Operational Efficiency:
- Are any underlying operational issues contributing to delivery delays and customer dissatisfaction?
- How can the delivery process be optimized to reduce delays and improve customer experience?
- What changes can be made to the "Expedited Rules" system to ensure more efficient deliveries?

### Data Analysis

```SQL
--View Data
SELECT *
FROM MagicMade.Cart_Records

--Check for duplicates
SELECT User_ID,	User_Location, Gender,	Cart_Contents,	Cart_Value,	Session_Date,	Session_Duration,	Abandonment_Reason,
Purchase_Category,	Referral_Medium,	Device_Type,	Cart_Status
FROM MagicMade.Cart_Records
GROUP BY User_ID,	User_Location, Gender,	Cart_Contents,	Cart_Value,	Session_Date,	Session_Duration,	Abandonment_Reason,
Purchase_Category,	Referral_Medium,	Device_Type,	Cart_Status
HAVING COUNT(*) > 1;   --Filters the duplicate


--Count the number of rows
SELECT COUNT (*)
FROM MagicMade.Cart_Records;

--Sum of cart value
SELECT SUM(Cart_Value) AS Total_Value
FROM MagicMade.Cart_Records;

--Number of rows by cart status
SELECT Cart_Status, COUNT(*) AS Number
FROM MagicMade.Cart_Records
GROUP BY Cart_Status;

--Rows by gender
SELECT Gender, COUNT(*) AS Number
FROM MagicMade.Cart_Records
GROUP BY Gender;

--Gender by Abandoned Cart status
SELECT Gender, Cart_Status, Count (*) AS Number
FROM MagicMade.Cart_Records
WHERE Cart_Status = 'Abandoned'
GROUP BY Gender, Cart_Status;

--Rows by Location
SELECT User_Location, COUNT(*) AS Number
FROM MagicMade.Cart_Records
WHERE Cart_Status = 'Paid'
GROUP BY User_Location
ORDER BY 2 DESC;

--Abandoned Cart status by Location 
SELECT User_Location, COUNT(*) AS Number
FROM MagicMade.Cart_Records
WHERE Cart_Status = 'Abandoned'
GROUP BY User_Location
ORDER BY 2 DESC;

--Paid by Cart Contents
SELECT Cart_Contents, COUNT(*) AS Number
FROM MagicMade.Cart_Records
WHERE Cart_Status = 'Paid'
GROUP BY Cart_Contents
ORDER BY 2 DESC;

--Abandoned Cart status by cart contents 
SELECT Cart_Contents, COUNT(*) AS Number
FROM MagicMade.Cart_Records
WHERE Cart_Status = 'Abandoned'
GROUP BY Cart_Contents
ORDER BY 2 DESC;

--Abandonment reasons
SELECT Abandonment_Reason, COUNT(*) AS Number
FROM MagicMade.Cart_Records
WHERE Cart_Status = 'Abandoned'
GROUP BY Abandonment_Reason
ORDER BY 2 DESC;

--Paid by Purchase Category
SELECT Purchase_Category, COUNT(*) AS Number
FROM MagicMade.Cart_Records
WHERE Cart_Status = 'Paid'
GROUP BY Purchase_Category
ORDER BY 2 DESC;


--Top 10 Abandoned Cart status by Purchase Category
SELECT TOP 10 Purchase_Category, COUNT(*) AS Number
FROM MagicMade.Cart_Records
WHERE Cart_Status = 'Abandoned'
GROUP BY Purchase_Category
ORDER BY 2 DESC;

--Referral Medium by Paid Cart status
SELECT Referral_Medium, COUNT(*) AS Number
FROM MagicMade.Cart_Records
WHERE Cart_Status = 'Paid'
GROUP BY Referral_Medium
ORDER BY 2 DESC;

--Referral Medium by Abandoned Cart status
SELECT Referral_Medium, COUNT(*) AS Number
FROM MagicMade.Cart_Records
WHERE Cart_Status = 'Abandoned'
GROUP BY Referral_Medium
ORDER BY 2 DESC;

--Device Type by Paid Cart status
SELECT Device_Type, COUNT(*) AS Number
FROM MagicMade.Cart_Records
WHERE Cart_Status = 'Paid'
GROUP BY Device_Type
ORDER BY 2 DESC;

--Device Type by Abandoned Cart status
SELECT Device_Type, COUNT(*) AS Number
FROM MagicMade.Cart_Records
WHERE Cart_Status = 'Abandoned'
GROUP BY Device_Type
ORDER BY 2 DESC;

--Create ranges for session duration
SELECT Duration_Range, COUNT(*) AS Number
FROM(SELECT
    CASE
        WHEN Session_Duration >= 5 AND Session_Duration <= 28 THEN '5-28'
        WHEN Session_Duration >= 29 AND Session_Duration <= 52 THEN '29-52'
        WHEN Session_Duration >= 53 AND Session_Duration <= 76 THEN '53-76'
		WHEN Session_Duration >= 77 AND Session_Duration <= 100 THEN '77-100'
        ELSE '101-120'
    END AS Duration_Range
FROM MagicMade.Cart_Records
--WHERE Cart_Status = 'Abandoned'
) AS Ranges
GROUP BY Duration_Range
ORDER BY Number DESC;
```

### Result and Findings

The analysis results are as follows:
1.	The company's abandonment rate is lower than the industry standard rate with a difference of 22.2%.
2.	The majority of abandoned users are on mobile devices (35.67%) followed by tablets and desktops
3.	The company's abandonment rate across devices is lower than the industry standard, especially on mobile
4.	The top reasons for cart abandonment are no guest checkout option, complex checkout, no total order upfront, and shipping costs.
5.	Most abandoned users come from social media (44-42%) and the session range where abandonment is highest is between 77-100 sessions
6.	The cart contents with the highest abandonment are accessories, toys, and footwear
7.	The majority of abandoned users are female (54.35%)
8.	The location date suggests that abandonment is spread across several regions in North America with Virginia being notably higher.
9.	The trend of abandoned users by week shows fluctuation with some peaks suggesting periodic increases in abandonment rates
10.	items such as sunglasses, wristwatches, and tablets are among the top categories for abandoned purchases

### Recommendations
1.	Optimize for Mobile: Since most users abandon carts on mobile, optimizing the checkout process for mobile devices could reduce abandonment rates
2.	Simplify Checkout: Offering a guest checkout option and simplifying the checkout process could address the two top reasons for abandonment.
3.	Transparency in Pricing: Displaying total costs upfront, including shipping, could reduce abandonment due to no total order cost upfront
4.	Leverage Social Media: Since a significant portion of traffic comes from social media, retargeting campaigns on these platforms could help recover abandoned carts.
5.	Tailored Marketing Strategies: Develop targeted marketing strategies for the top abandoned items like sunglasses, wristwatches, and tablets to improve conversions in these categories
6.	Address Shipping Costs: Offering free shipping thresholds or flat-rate shipping could mitigate abandonment due to shipping costs.

### Limitations

I had to create a range for session duration to help me categorize the session duration by Cart_Status.

### References

- [Amdari](https://www.amdari.io)

