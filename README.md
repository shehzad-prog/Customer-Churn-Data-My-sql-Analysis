 Customer-Churn-Data-My-sql-Analysis
 TELCO CUSTOMER CHURN ANALYSIS



table of Contents
1.	Project Overview
2.	Dataset Overview
o	telco_customer_churn_demographics
o	telco_customer_churn_location
o	telco_customer_churn_population
o	telco_customer_churn_services
o	telco_customer_churn_status
3.	Objectives
4.	Query 1: Analyzing High-Spending Churned Customers and Creating Personalized Offers
o	Query Objective
o	SQL Query
o	Query Results
o	Key Insights
o	Tailoring Personalized Offers
	High Monthly Charges with One-Year Contracts
	Month-to-Month Contract with High Charges
	Two-Year Contract with High Charges
o	General Strategy for Improvement
	Age-Based Offers
	Gender-Specific Offers
5.	Query 2: Understanding Feedback and Complaints from Churned Customers
o	Query Objective
o	SQL Query
o	Query Results
o	Key Insights
o	Potential Actions
	Competitive Analysis
	Customer Feedback
	Targeted Offers
o	Addressing Churn Due to Competitors
	Device Offerings
	Faster Download Speeds
	Data Offerings
	Product Dissatisfaction
o	Recommendations for Personalized Offers
	Targeted Device Upgrades
	Customized Data or Speed Plans
	Service Improvement Initiatives
o	Personalized Retention Plan
6.	Query 3: Influence of Payment Method on Churn Behavior
o	Query Objective
o	SQL Query
o	Query Results
o	Key Insights
o	Actionable Recommendations
Project Overview
The IBM Telco Customer Churn initiative aimed to scrutinize customer churn patterns to unveil insights that could assist the telecommunications provider in enhancing customer retention. By leveraging the IBM Telco Customer Churn dataset and MySQL Workbench, the analysis concentrated on identifying factors that influence churn, such as payment methods, customer demographics, types of contracts, and customer feedback.

Dataset Overview
The dataset consists of five primary tables:
1.	Telco Customer Churn Demographics:
o	Includes demographic details such as customer ID, gender, age, marital status, and number of dependents.
2.	Telco Customer Churn Location:
o	Contains location information like country, state, city, and postal code.
3.	Telco Customer Churn Population:
o	Offers population statistics tied to various zip codes.
4.	Telco Customer Churn Services:
o	Details the services utilized by customers, encompassing phone services, internet types, monthly fees, contract types, and tenure.
5.	Telco Customer Churn Status:
o	Provides insights into customer churn status, satisfaction scores, and reasons for churn.
Here’s a rephrased version of your text:
________________________________________
Table of Contents
1.	Project Overview
2.	Dataset Overview
o	Telco Customer Churn Demographics
o	Telco Customer Churn Location
o	Telco Customer Churn Population
o	Telco Customer Churn Services
o	Telco Customer Churn Status
3.	Objectives
4.	Query 1: Examining High-Spending Churned Customers and Developing Tailored Offers
o	Query Objective
o	SQL Query
o	Query Results
o	Key Insights
o	Customizing Personalized Offers
	High Monthly Charges with One-Year Contracts
	Month-to-Month Contracts with Elevated Charges
	Two-Year Contracts with Significant Charges
o	General Improvement Strategies
	Age-Specific Offers
	Gender-Centric Offers
5.	Query 2: Analyzing Feedback and Complaints from Churned Customers
o	Query Objective
o	SQL Query
o	Query Results
o	Key Insights
o	Potential Actions
	Competitor Assessment
	Customer Insights
	Targeted Promotions
o	Addressing Churn Attributed to Competitors
	Device Offerings
	Enhanced Download Speeds
	Data Packages
	Product Discontent
o	Suggestions for Customized Offers
	Targeted Device Upgrades
	Tailored Data or Speed Plans
	Service Improvement Initiatives
o	Personalized Retention Strategies
6.	Query 3: The Impact of Payment Methods on Churn Behavior
o	Query Objective
o	SQL Query
o	Query Results
o	Key Insights
o	Actionable Recommendations
________________________________________
Project Overview
The IBM Telco Customer Churn initiative aimed to scrutinize customer churn patterns to unveil insights that could assist the telecommunications provider in enhancing customer retention. By leveraging the IBM Telco Customer Churn dataset and MySQL Workbench, the analysis concentrated on identifying factors that influence churn, such as payment methods, customer demographics, types of contracts, and customer feedback.
________________________________________
Dataset Overview
The dataset consists of five primary tables:
1.	Telco Customer Churn Demographics:
o	Includes demographic details such as customer ID, gender, age, marital status, and number of dependents.
2.	Telco Customer Churn Location:
o	Contains location information like country, state, city, and postal code.
3.	Telco Customer Churn Population:
o	Offers population statistics tied to various zip codes.
4.	Telco Customer Churn Services:
o	Details the services utilized by customers, encompassing phone services, internet types, monthly fees, contract types, and tenure.
5.	Telco Customer Churn Status:
o	Provides insights into customer churn status, satisfaction scores, and reasons for churn.
________________________________________
Goals:
The primary aim was to tackle three essential queries using MySQL, derive actionable insights, and propose strategies to mitigate customer churn:
Query 1: How can we design personalized offers based on age, gender, and contract type for the top five groups with the highest average monthly charges among churned customers to potentially enhance customer retention rates?

Query 2: What feedback or complaints have been reported by churned customers?

Query 3: How does the payment method influence churn behavior?
Query 1: Examining High-Spending Churned Customers and Developing Tailored Offers
Query 1 Objective
1.	Identify the top five groups of churned customers with the highest average monthly charges and determine how personalized offers can be crafted based on age, gender, and contract type to enhance retention rates.
SQL Query
-- Identify duplicate customer IDs
SELECT `Customer ID`, COUNT(`Customer ID`)
FROM telco_customer_churn_services
GROUP BY `Customer ID`
HAVING COUNT(`Customer ID`) > 1;

WITH telco_services AS (
    SELECT a.`Customer ID`, a.`Contract` AS contract_type, 
           AVG(`Monthly Charge`) OVER (PARTITION BY `Customer ID` ORDER BY `Monthly Charge` DESC) AS average_monthly_charges, 
           b.`Customer Status`
    FROM telco_customer_churn_services a
    JOIN telco_customer_churn_status b ON a.`Customer ID` = b.`Customer ID`
    WHERE b.`Customer Status` = 'churned'
)
SELECT a.*, b.`Age`, b.`Gender`
FROM telco_services a
JOIN telco_customer_churn_demographics b ON a.`Customer ID` = b.`Customer ID`
ORDER BY a.average_monthly_charges DESC
LIMIT 5;
Query Results

Customer ID	Contract Type	Avg Monthly Charges	Customer Status	Age	Gender
8199-ZLLSA	One Year	118.35	Churned	53	Male
2889-FPWRM	One Year	117.80	Churned	31	Male
2302-ANTDP	Month-to-Month	117.45	Churned	76	Female
9053-JZFKV	Two Year	116.20	Churned	25	Male
1444-VVSGW	One Year	115.65	Churned	51	Male
________________________________________
Key Insights
•	High Monthly Charges with One-Year Contracts:
o	Customers aged 53, 31, and 51 (all male) have elevated average monthly charges on one-year contracts.
•	Month-to-Month Contract with High Charges:
o	A 76-year-old female customer has a month-to-month contract with substantial charges.
•	Two-Year Contract with High Charges:
o	A 25-year-old male customer has a two-year contract with high monthly charges.
Customizing Personalized Offers
1.	High Monthly Charges with One-Year Contracts
o	Tailored Offer: Discounts or incentives for transitioning to a two-year plan, highlighting long-term savings and potential bundled services.
2.	Month-to-Month Contract with High Charges
o	Tailored Offer: Propose discounted long-term contracts to reduce monthly rates, focusing on senior-specific plans that emphasize simplicity.
3.	Two-Year Contract with High Charges
o	Tailored Offer: Offer tech-focused add-ons, such as streaming services or discounts on additional services, emphasizing the benefits of retaining their current contract.
General Improvement Strategies
•	Age-Specific Offers:
o	Younger Customers: Likely to appreciate tech upgrades, streaming, or gaming packages; discounts or bundled services may enhance retention.
o	Older Customers: Prefer simpler service plans; enhanced customer support could improve satisfaction.
•	Gender-Centric Offers:
o	While significant differences were not noted, personalized marketing can be adjusted according to gender preferences






Query 2: Understanding Feedback and Complaints from Churned Customers
Query Objective
Analyze the feedback or complaints from churned customers to identify common reasons for leaving and develop strategies to address them.
SQL Query

with telco_services as (
select a.`Customer ID`, a.`Contract` as contract_type, 
avg(`Monthly Charge`) over (partition by `Customer ID` order by `Monthly Charge` desc) as average_monthly_charges, b.`Customer Status`, b.`Churn Reason` as feedback
from telco_customer_churn_services a
join telco_customer_churn_status b
on a.`Customer ID` = b.`Customer ID`
where b.`Customer Status` = "churned")
select a.*, b.`Age`, b.`Gender`
from telco_services a
join telco_customer_churn_demographics b
on a.`Customer ID` = b.`Customer ID`
order by a.average_monthly_charges desc
limit 5;
SELECT s.`Customer ID`, s.`Contract` AS contract_type, s.`Monthly Charge`, st.`Customer Status`, st.`Churn Reason`
FROM telco_customer_churn_services s
JOIN telco_customer_churn_status st
ON s.`Customer ID` = st.`Customer ID`
WHERE st.`Customer Status` = 'churned'
AND st.`Churn Reason` IS NOT NULL;
Query Results
Customer ID	Contract Type	Avg Monthly Charges	Customer Status	Feedback	Age	Gender
8199-ZLLSA	One Year	118.35	Churned	Competitor had better devices	53	Male
2889-FPWRM	One Year	117.80	Churned	Competitor offered higher download speeds	31	Male
2302-ANTDP	Month-to-Month	117.45	Churned	Competitor offered more data	76	Female
9053-JZFKV	Two Year	116.20	Churned	Don't know	25	Male
1444-VVSGW	One Year	115.65	Churned	Product dissatisfaction	51	Male
Key Insights
•	Competitor-Related Reasons:
o	Common Themes: Better devices, higher download speeds, and more data offered by competitors are frequent reasons for churn.
•	Product Dissatisfaction:
o	Indicates potential issues with service quality or product features.
•	Uncertain Reasons:
o	Some customers are unsure why they left, highlighting a possible disconnect or lack of engagement.
Potential Actions
Competitive Analysis
•	Assess Competitors:
o	Evaluate competitors' strengths in device quality, data packages, and download speeds.
o	Aim to match or surpass their offerings.
Customer Feedback
•	Gather Detailed Feedback:
o	Implement regular surveys and feedback mechanisms to pinpoint dissatisfaction.
o	Use insights to drive improvements in products and services.
Targeted Offers
•	Upgrade or Renewal Options:
o	For customers on one-year or two-year contracts, offer better upgrade or renewal deals.
o	Tailor promotions to address specific reasons for churn.
Addressing Churn Due to Competitors
1. Device Offerings
•	Invest in Device Partnerships:
o	Collaborate with popular manufacturers to offer competitive devices.
o	Provide exclusive devices or upgrades.
•	Offer Device Upgrades or Trade-In Options:
o	Allow mid-contract upgrades or trade-ins to keep customers engaged.
•	Device Comparison Marketing:
o	Transparently compare devices with competitors, highlighting advantages.
2. Faster Download Speeds
•	Speed Testing and Improvement:
o	Conduct assessments to identify areas needing speed enhancements.
•	Tiered Internet Packages:
o	Offer competitive pricing on higher-speed packages.
•	Communicate Improvements:
o	Keep customers informed about network upgrades and enhancements.
3. Data Offerings
•	Competitive Data Packages:
o	Reevaluate data plans to offer more flexibility or larger allowances.
•	Unlimited Data Options:
o	Promote unlimited plans or special deals for data-conscious customers.
•	Usage Monitoring Tools:
o	Provide tools to help customers manage and optimize their data usage.
4. Product Dissatisfaction
•	Service Improvements:
o	Investigate and address common pain points in service quality.
•	Customer Surveys:
o	Follow up with dissatisfied customers to understand and resolve issues.
•	Loyalty Programs:
o	Implement rewards or satisfaction guarantees to retain uncertain customers.
Recommendations for Personalized Offers
1. Targeted Device Upgrades
•	Discounted or Free Upgrades:
o	Offer to customers who cited device issues.
•	Age-Based Personalization:
o	Younger customers: Focus on cutting-edge devices.
o	Older customers: Emphasize simplicity and functionality.
2. Customized Data or Speed Plans
•	Tailored Packages:
o	Address needs for higher speeds or more data.
•	Contract-Based Promotions:
o	Offer special deals on extended contracts with enhanced data and speed.
3. Service Improvement Initiatives
•	Satisfaction Guarantees:
o	Allow customers to cancel without penalty if not satisfied after trying improved services.
•	Dedicated Support:
o	Provide specialized support for customers expressing dissatisfaction.
Personalized Retention Plan
Implementation Steps
1.	Identify Target Customers:
o	Use churn feedback to categorize at-risk customers.
2.	Create Marketing Campaigns:
o	Design targeted promotions highlighting new offers.
3.	Train Customer Service Teams:
o	Ensure staff are informed about offers and can communicate effectively.
4.	Monitor Outcomes:
o	Track responses and retention rates to assess effectiveness.
5.	Continuous Feedback Loop:
o	Adapt offers based on customer feedback and emerging trends.
________________________________________
Query 3: Influence of Payment Method on Churn Behavior
Query Objective
Analyze how different payment methods impact churn rates and provide recommendations for improving customer retention through payment options.
SQL Query

SELECT s.`Payment Method`, COUNT(st.`Customer ID`) AS total_customers, SUM(CASE WHEN st.`Churn Label` = 'Yes' THEN 1 ELSE 0 END) AS churned_customers,
ROUND(SUM(CASE WHEN st.`Churn Label` = 'Yes' THEN 1 ELSE 0 END) / COUNT(st.`Customer ID`) * 100, 2) AS churn_rate_percentage
FROM telco_customer_churn_services s
JOIN telco_customer_churn_status st
ON s.`Customer ID` = st.`Customer ID`
GROUP BY s.`Payment Method`
ORDER BY churn_rate_percentage DESC;

Query Results
Payment Method	Total Customers	Churned Customers	Churn Rate (%)
Mailed Check	385	142	36.88
Bank Withdrawal	3,909	1,329	34.00
Credit Card	2,749	398	14.48
Key Insights
1.	Mailed Check:
o	Churn Rate: 36.88%
o	Observation: Highest churn rate, suggesting dissatisfaction or difficulties with the payment process.
2.	Bank Withdrawal:
o	Churn Rate: 34.00%
o	Observation: Significant churn rate indicates potential issues with the withdrawal process or related banking services.
3.	Credit Card:
o	Churn Rate: 14.48%
o	Observation: Lowest churn rate, implying higher satisfaction with the payment experience.
Analysis
•	Higher Churn with Mailed Checks and Bank Withdrawals:
o	Indicates challenges or less engagement with these payment methods.
•	Credit Card Payments:
o	Preferred due to convenience, security, and possibly rewards.
Recommendations
1. Address Issues with Mailed Checks
•	Customer Feedback Analysis:
o	Gather feedback to identify pain points.
•	Incentives to Switch:
o	Offer discounts or bonuses for transitioning to electronic payments.
•	Process Improvements:
o	Streamline mailed check processing and provide clear instructions.
2. Improve Bank Withdrawal Processes
•	Identify Common Complaints:
o	Analyze issues such as failed transactions or delays.
•	Enhanced Communication:
o	Provide updates on payment status to build trust.
•	Alternative Options:
o	Introduce direct debit or automatic payment setups.
3. Promote Credit Card Use
•	Exclusive Rewards Program:
o	Implement rewards for credit card users.
•	Highlight Benefits:
o	Market the convenience and security of credit card payments.
•	Partnerships:
o	Collaborate with credit card companies for exclusive offers.
4. Customer Engagement Strategies
•	Personalized Communication:
o	Target customers using higher-churn methods with tailored messages.
•	Loyalty Programs:
o	Reward customers for continued use and preferred payment methods.
•	Regular Follow-Ups:
o	Check in with customers to address concerns proactively.
Implementation Steps
1.	Month 1:
o	Analyze feedback from customers using higher-churn payment methods.
o	Launch initial campaigns promoting credit card usage.
2.	Month 2:
o	Develop and offer incentives for switching payment methods.
o	Improve communication regarding bank withdrawals.
3.	Month 3:
o	Roll out loyalty programs for credit card users.
o	Collect initial feedback on initiatives.
4.	Ongoing:
o	Monitor churn rates and refine strategies as needed.
________________________________________
Conclusion and Recommendations
Through comprehensive data analysis, the following key strategies have been identified to reduce customer churn:
1.	Personalized Offers Based on Customer Profiles:
o	Tailor promotions considering age, gender, and contract type.
o	Younger customers may value tech enhancements, while older customers might prioritize simplicity.
2.	Addressing Competitor Challenges:
o	Enhance device offerings, data packages, and download speeds.
o	Regularly assess market trends to stay competitive.
3.	Improving Payment Methods Experience:
o	Encourage the use of convenient payment methods like credit cards.
o	Address issues with mailed checks and bank withdrawals through process improvements and customer education.
4.	Engaging with Customer Feedback:
o	Implement continuous feedback mechanisms.
o	Use insights to drive service improvements and customer satisfaction.
5.	Implementing Targeted Retention Strategies:
o	Develop marketing campaigns that address specific reasons for churn.
o	Train customer service teams to effectively communicate new offers.
By focusing on these areas, the company can enhance customer loyalty, reduce churn rates, and foster a more satisfied customer base.
________________________________________
Tools Used
•	MySQL Workbench: For Analyzing the whole day
