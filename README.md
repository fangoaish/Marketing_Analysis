# Marketing_Analysis

## Project Overview
Investigating Non-UK Conversions in UK Targeted Marketing Campaigns.

## Objectives:
The primary objective of this project is to analyze the dataset and determine the factors contributing to the increasing share of non-UK conversions in UK-targeted campaigns. This will involve comparing the performance of UK and non-UK conversions, understanding patterns across various dimensions such as platforms (iOS vs. Android), publishers, and fleet types, and using Germany as a reference market for benchmarking. The insights gained will help inform whether adjustments are needed in the geo-targeting setup or other aspects of the campaign strategy.

## Data Sources
The provided dataset aggregates data from passengers converted through digital marketing campaigns in both the UK and Germany. The key dimensions include:

- Publisher: Identifying which publisher the campaign belongs to.
- Mobility Type: Type of fleet used (e.g., Private Hire).
- Conversion Country: The country where the conversion occurred.
- Country Code by Phone: The country code of the user based on their phone.
- Asset Country: The country where the campaign was targeted (expected to be the UK).
- Platform: The platform on which the conversion occurred (iOS or Android).
- Duration Install to Activation: The time between app install and activation.
- Conversions: The number of conversions.
- Month: Time period of the campaign.

 

## Exploratory Data Analysis

## Trend Analysis

### _1) Showcase the UK/Non-UK conversion trend from targeted UK campaigns._
   - Why do I want to know?
      - Understanding the conversion trends over time for UK vs. non-UK markets is crucial because the campaign is specifically geo-targeted to users in the UK. Despite this, a growing number of conversions are occurring in non-UK countries, which may point to issues with the targeting settings, platform behaviors, or other factors like cross-border traffic. By comparing these conversion volumes month-over-month (MoM), I can identify when the shift toward non-UK conversions started and assess the severity of the trend. This will help pinpoint whether this is a recent anomaly or a long-standing issue, allowing for better focus on potential root causes.
   - So what?
      - If non-UK conversions are growing disproportionately, it suggests a failure in geo-targeting, which can have significant implications:
           - **Budget Inefficiency:** Marketing spend intended for UK users is being wasted on non-UK conversions, reducing overall ROI.
           - **Targeting Accuracy:** If geo-targeting is malfunctioning or misconfigured, it affects the campaign's ability to reach its intended audience. This could lead to lower conversions from UK-based users.
           - **Cross-Border Opportunities:** If the non-UK conversions reflect actual user interest, this could present an opportunity to expand campaigns beyond the UK, potentially opening new markets. However, it would require deliberate strategy adjustments rather than unintended spillover.

```
SELECT 
    month,
    SUM(CASE WHEN conversion_country = 'UK' THEN conversions ELSE 0 END) AS UK_Conversions,
    SUM(CASE WHEN conversion_country != 'UK' THEN conversions ELSE 0 END) AS Non_UK_Conversions,
    SUM(CASE WHEN conversion_country != 'UK' THEN conversions ELSE 0 END) /
                  SUM(conversions) AS Non_UK_Conversion_Share,
    SUM(CASE WHEN conversion_country = 'UK' THEN conversions ELSE 0 END) /
                  SUM(conversions) AS UK_Conversion_Share	
FROM 'campaigns.csv'
WHERE asset_country = 'UK'
GROUP BY month
ORDER BY month;
```

![Conversion Rates of Sessions to Orders - Gsearch](https://github.com/fangoaish/SQL__eCommerce-Analysis-for-Maven-Fuzzy-Factory/assets/51399519/db04d55d-6428-45dc-a378-db5751d7aaab)


Overall, the data indicates a positive trend of increasing website traffic and orders over time, with a consistent conversion rate, suggesting that the website is effectively converting sessions into orders regardless of fluctuations in traffic volume. 

### _2) Evaluate the performance of nonbrand and brand campaigns separately to determine if brand campaigns are gaining traction._

<img width="797" alt="eCommerce Q2" src="https://github.com/fangoaish/SQL__eCommerce-Analysis-for-Maven-Fuzzy-Factory/assets/51399519/34f65380-0a94-47b2-ad55-df5b8f03f861">

![Conversion Rates Between Brand and Nonbrand Campaigns](https://github.com/fangoaish/SQL__eCommerce-Analysis-for-Maven-Fuzzy-Factory/assets/51399519/5610e6f0-6739-4507-a2bf-73b88ff055f9)



#### **Brand Campaigns**:
There has been a noticeable increase in brand sessions and orders over the months, indicating a growing interest in brand-specific marketing efforts.
The brand campaign conversion rate fluctuates, peaking at 9.23% in April but stabilizing around 4-6% in subsequent months. This suggests that while there may be variations, the overall effectiveness of brand campaigns in converting sessions to orders remains relatively consistent.


- #### **Nonbrand Campaigns**:
Nonbrand sessions and orders also show an upward trend over the months, indicating increasing traffic and interest from users who are not specifically searching for the brand.
The nonbrand campaign conversion rate remains relatively stable around 3-4%, suggesting a consistent level of effectiveness in converting nonbrand sessions into orders.     


### _3) Compare monthly trends of Gsearch traffic with other channels to understand their respective impacts on overall traffic._
   - Why do I want to know?
      - to bid efficiently and use data to maximize the effectiveness of the marketing budget
   - So what?
      - understand which marketing channels are driving the most sessions and orders through the website
      - understand differences in user characteristics and conversion performance across marketing channels
      - Optimize bids and allocate marketing spend across a multi-channel portfolio to achieve maximum performance
   - Measured by?
      - To identify traffic coming from multiple marketing channels, we will use utm parameters stored in our sessions table
      - We will LEFT JOIN to our orders table to understand which of the sessions converted to placing an order and generating revenue

First, we'll check the different UTM sources and referrers to identify the incoming traffic.

<img width="339" alt="eCommerce Q4 -1" src="https://github.com/fangoaish/SQL__eCommerce-Analysis-for-Maven-Fuzzy-Factory/assets/51399519/883cc548-6d36-485c-a00a-a0f8964d936d">


we have four channels:
   1. gsearch
   2. bsearch
   3. organic
   4. direct


- If utm_source and utm_campaign IS NULL and http_referer IS NOT NULL -> _organic search_
- If utm_source and utm_campaign IS NULL and http_referer IS NULL -> _direct_

<img width="1266" alt="eCommerce Q4 -2" src="https://github.com/fangoaish/SQL__eCommerce-Analysis-for-Maven-Fuzzy-Factory/assets/51399519/8967abae-3590-4e50-9092-d20c6af90cf9">

![Average Conversion Rates By Channels](https://github.com/fangoaish/SQL__eCommerce-Analysis-for-Maven-Fuzzy-Factory/assets/51399519/198cd5b7-8143-42c3-b6a1-eb29e7f5deb1)

- **Gsearch Performance**: Gsearch consistently maintains a significant share of overall traffic throughout the months, with steady growth in sessions and orders. The conversion rate for Gsearch remains relatively stable, averaging 3.73%.

- **Other Channels**: While channels like branded search (Bsearch), direct, and organic traffic contribute to overall traffic. Among these, Bsearch stands out with the highest conversion rate at 4.82%, followed by direct at 4.98%, and organic traffic at 4.38%.

- **Conclusion**: Gsearch emerges as a consistent and impactful driver of overall traffic, with a relatively stable conversion rate. However, it's essential to note the varying performance of other channels, particularly Bsearch, which exhibits a higher conversion rate. Understanding these trends allows for strategic decisions in resource allocation and optimization efforts across different channels to maximize overall traffic and conversions.


## Traffic Source Analysis
### _4) Demonstrate a comprehensive understanding of traffic sources by analyzing nonbrand Gsearch sessions and orders split by device type._
   - Why do I want to know?
      - Understanding nonbrand Gsearch sessions by device type is essential for optimizing user experience and targeting marketing efforts effectively.
   - So what?
      - Analyzing this data enables strategic decision-making, leading to improved user engagement and conversion rates through device-specific optimization and targeted marketing strategies.
   - Measured by?
        - website_session // device_type // order_id

<img width="754" alt="eCommerce Q3" src="https://github.com/fangoaish/SQL__eCommerce-Analysis-for-Maven-Fuzzy-Factory/assets/51399519/4ca43b81-2686-4061-a46b-b80125b1163c">

![Sessions by Device Types](https://github.com/fangoaish/SQL__eCommerce-Analysis-for-Maven-Fuzzy-Factory/assets/51399519/ff4882ea-9719-423c-9f1c-ac03760a6c77)

![Orders by Device Types](https://github.com/fangoaish/SQL__eCommerce-Analysis-for-Maven-Fuzzy-Factory/assets/51399519/9628b4bc-3051-42c7-9234-cff0a7d33bcd)

![Conversion Rates Between Desktop and Mobile](https://github.com/fangoaish/SQL__eCommerce-Analysis-for-Maven-Fuzzy-Factory/assets/51399519/0bca869d-e4f7-4aa8-ba6c-246207f393b7)


- **Desktop vs. Mobile Sessions**: There is a clear trend of increasing sessions on both desktop and mobile devices over the months, with desktop sessions consistently higher than mobile sessions.

- **Desktop vs. Mobile Orders**: Similarly, desktop orders outpace mobile orders, indicating that desktop users are more likely to make purchases compared to mobile users.

- **Conversion Rates**: While desktop conversion rates remain relatively stable at around 4-5%, mobile conversion rates fluctuate, with some months showing significantly lower rates.

- **Conclusion**: Desktop devices dominate both sessions and orders, suggesting that desktop users are the primary drivers of traffic and conversions. However, it's essential to address the lower conversion rates on mobile devices to capitalize fully on this traffic source. Strategies aimed at optimizing the mobile user experience and improving mobile conversion rates should be prioritized to ensure a comprehensive understanding and effective utilization of traffic sources across different devices


## Website Performance Monitoring Analysis
### _5) Monitor and improve website performance by tracking session-to-order conversion rates monthly over the first 8 months._
   - Why do I want to know?
      - to understand and optimize each step of your user's experience on their journey toward purchasing your products
      - look at each step in the conversion flow to see how many customers drop off and how many continue on at each step
   - So What?
      - identify the most common paths customers take before purchasing your products
      - identify how many of your users continue on to each next step in your conversion flow, and how many users abandon at each step
      - optimize critical pain points where users are abandoning, so that you can convert more users and sell more products
   - Measured by?
      - we will create temporary tables using pageview data in order to build our multi-step funnels
      - We will first identify the sessions we care about, then bring in the relevant pageviews, then flag each session as having made it to certain funnel steps, and finally perform a summary analysis

<img width="510" alt="eCommerce Q5" src="https://github.com/fangoaish/SQL__eCommerce-Analysis-for-Maven-Fuzzy-Factory/assets/51399519/5d473d87-c48e-44e6-bf7d-a98ab0f5ee59">

![Conversion Rates of Sessions to Orders](https://github.com/fangoaish/SQL__eCommerce-Analysis-for-Maven-Fuzzy-Factory/assets/51399519/a434b3f2-3f07-4336-9689-da989d99914e)


## Conversion Funnel Analysis
### _6) Monitor and improve website performance by tracking session-to-order conversion rates monthly over the first 8 months._
Before diving into the data directly, let's list out the analysis process
- **STEP 0**: Have an overview of the conversion funnel path
   - Conversion funnel path:
      - /home ; /lander-1
      - /products
      - /the-original-mr-fuzzy
      - /cart
      - /shipping
      - /billing
      - /thank-you-for-your-order
<img width="173" alt="eCommerce Q7" src="https://github.com/fangoaish/SQL__eCommerce-Analysis-for-Maven-Fuzzy-Factory/assets/51399519/1b54b1a5-3f32-476e-9b24-60057c90bdd1">

- **STEP 1**: Identify each relevant pageview for relevant sessions as the specific funnel step
- **STEP 2**: Create the session-level conversion funnel view
- **STEP 3**: Aggregate the data to assess funnel performance

   
   - Session Funnels
<img width="695" alt="eCommerce Q7-1" src="https://github.com/fangoaish/SQL__eCommerce-Analysis-for-Maven-Fuzzy-Factory/assets/51399519/9ba4cf9c-4a5f-4fc1-821d-89b8fec6c5ea">

   - Click Through Rate Funnels
<img width="745" alt="eCommerce Q7-2" src="https://github.com/fangoaish/SQL__eCommerce-Analysis-for-Maven-Fuzzy-Factory/assets/51399519/f6073b36-3191-4eb8-aa90-9ba675f1f020">

The custom lander page has better click-through rates than the original homepage.


## Revenue Analysis
### _7) Estimate the revenue generated from the search lander test by analyzing the increase in conversion rates and subsequent revenue._
   - Why do I want to know?
      - to understand the performance of the key landing pages and then test to improve the results
   - So what?
      - Identifying the top opportunities for landing pages - high volume pages with higher than expected bounce rates or low conversion rates
      - Setting up A/B experiments on your live traffic to see if we can improve our bounce rates and conversion rates
      - Analyzing test results and making recommendations on which version of landing pages we should use forward
   - Measure by?
      - To analyze landing page performance and compare multiple pages, we will again use temporary tables and write a multi-step 'data program'
      - We will find the first pageview for relevant sessions, associate that pageview with the URL seen, and then analyze whether that session had additional pageviews

Before diving into the data directly, let's list out the analysis process
- **STEP 0**: Find out when the new page /lander launched
    - The first_test_pageview ID is **23504** and was created at 2012-06-19 00:35:54
<img width="237" alt="eCommerce Q6" src="https://github.com/fangoaish/SQL__eCommerce-Analysis-for-Maven-Fuzzy-Factory/assets/51399519/6d1248a6-5ec0-41f8-a2ba-5ea4da4e2d29">

- **STEP 1**: Find the first first_seen_landing_page_id for relevant sessions

- **STEP 2**: Identify the landing page of these relevant sessions
  
- **STEP 3**: Make a table to bring in the orders
  
- **STEP 4**: Find the difference between conversion rates
    - The conversion rate for **_/lander-1_** was higher at 4.06% compared to 3.18% for **_/home_**.
    - Therefore, **_/lander-1_** appears to be the more effective landing page in terms of driving conversions.
<img width="391" alt="eCommerce Q6 -1" src="https://github.com/fangoaish/SQL__eCommerce-Analysis-for-Maven-Fuzzy-Factory/assets/51399519/8f504175-3481-4a6d-8c21-6cf685fd5269">

- **STEP 5**: Find out the last time when the traffic was sent to **_/home_**
    - To estimate the revenue generated by the new test lander page, we first identify the most recent appearance of the **_/home_** page. Subsequently, we calculate the total number of sessions since that occurrence.
    - The latest website_session_id is **17145**
    - After this session, the /home landing page is discontinued, and all landing pages are replaced with **_/lander-1_**.
<img width="259" alt="eCommerce Q6 -2" src="https://github.com/fangoaish/SQL__eCommerce-Analysis-for-Maven-Fuzzy-Factory/assets/51399519/dca8d754-6f6c-4b5a-99a0-0d73c235b0f1">


- **STEP 6**: Count the total sessions after the last time **_/home_** page to estimate the revenue
<img width="143" alt="eCommerce Q6 -3" src="https://github.com/fangoaish/SQL__eCommerce-Analysis-for-Maven-Fuzzy-Factory/assets/51399519/bafabab1-258a-49b8-ae1e-c075903c8632">


- **STEP 7**: Calculate Average Revenue
    - 22,972 total website sessions for **_/lander-1_**
    - Conversion rate difference: 0.88%
    - 22,972 * 0.88% incremental conversion = 202 -> incremental orders since July 29th (roughly 4 months)
    - 202/4 = roughly 50 extra orders per month, not bad!
 
![eCommerce Q6-4](https://github.com/fangoaish/SQL__eCommerce-Analysis-for-Maven-Fuzzy-Factory/assets/51399519/75a9a286-6a2b-4273-ad8c-3ef02a9104cc)

![eCommerce Q6-5](https://github.com/fangoaish/SQL__eCommerce-Analysis-for-Maven-Fuzzy-Factory/assets/51399519/35e88389-280c-4731-a990-322c1f4e0759)

### _8) Quantify the impact of the billing test by analyzing revenue per billing page session and monthly billing page sessions._
- **STEP 0**: Check the billing page version
<img width="162" alt="eCommerce Q8" src="https://github.com/fangoaish/SQL__eCommerce-Analysis-for-Maven-Fuzzy-Factory/assets/51399519/eeccca64-3e6e-4714-811e-2be62b6ccb96">

- **STEP 1**: Connect needed metrics altogether
- **STEP 2**: Aggregate the data to assess billing-order performance
   - **_/billing_** page generates 657 sessions, with an average of $22,83 revenue per session
   - **_/billing-2_** page generates 654 sessions, with an average of $31,34 revenue per session
   - INCREASE: **USD 8.51 per session**
<img width="433" alt="eCommerce Q8-1" src="https://github.com/fangoaish/SQL__eCommerce-Analysis-for-Maven-Fuzzy-Factory/assets/51399519/46974763-9266-4fc6-82eb-e9930ccf42af">


- **STEP 3**: The Past month's billing page session
<img width="204" alt="eCommerce Q8-2" src="https://github.com/fangoaish/SQL__eCommerce-Analysis-for-Maven-Fuzzy-Factory/assets/51399519/3448ec00-3b4a-4bcf-b73b-a5ce725d8cad">

The billing test has a noteworthy result, with a $8.51 lift per billing session. With 1,193 billing sessions in the past month, the test has generated $10,160 in additional revenue over this period.


![eCommerce Q8 -3](https://github.com/fangoaish/SQL__eCommerce-Analysis-for-Maven-Fuzzy-Factory/assets/51399519/d65ad669-49a7-4fbc-adb1-11505d19ae9e)

![eCommerce Q8-4](https://github.com/fangoaish/SQL__eCommerce-Analysis-for-Maven-Fuzzy-Factory/assets/51399519/b1d06285-cf23-4e87-83f4-ac4551f755a1)


## Product Analysis
### _9) Evaluate the impact of introducing new products by tracking monthly sessions to the /products page and changes in click-through rates, along with improvements in conversion rates from /products to placing orders._
   - Why do I want to know?
      - to learn how customers interact with each of our products, and how well each product converts customers
   - So what?
      - Understand which of our products generate the most interest on multi-product showcase pages
      - Analyzing the impact on website conversion rates when we add a new product
      - Build product-specific conversion funnels to understand whether certain products convert better than others
   - Measured by?
      - We'll use website_pageviews data to identify users who viewed the /product page and see which products they clicked next
      - From specific product pages, we will look at view-to-order conversion rates and create multi-step conversion funnels

<img width="853" alt="eCommerce Q9" src="https://github.com/fangoaish/SQL__eCommerce-Analysis-for-Maven-Fuzzy-Factory/assets/51399519/7118db23-6031-4f8b-bd7c-0987c9e17353">

![_products Page Performance Between 2012 and 2014](https://github.com/fangoaish/SQL__eCommerce-Analysis-for-Maven-Fuzzy-Factory/assets/51399519/1b9e19de-b9f3-4129-bf56-ed2b68eb493b)


- The introduction of new products has positively impacted user engagement and conversion rates on the /products page. 
- Over the years, we've seen significant increases in monthly sessions and click-through rates, indicating heightened user interest. Additionally, the conversion rates from /products to orders have consistently improved, reflecting the positive influence of new products on driving sales and revenue growth. 


### _10) Elevate the fourth product from a cross-sell item to a primary offering._
Could you please pull sales data since then, and show how well each product cross-sells from one another?_
   - Why do I want to know?
      - to understand which products users are most likely to purchase together, and offer smart product recommendations
      - Using this data, we can develop a deeper understanding of our customer purchase behaviors
   - So what?
      - understand which products are often purchased together
      - Test and optimize the way we cross-sell products on our website
      - understand the conversion rate impact and the overall revenue impact of trying to cross-sell additional products
   - Measured by?
      - We can analyze orders and order_items data to understand which products cross-sell and analyze the impact on revenue
      - We'll also use website_pageviews data to understand if cross-selling hurts overall conversion rates

<img width="937" alt="eCommerce Q10" src="https://github.com/fangoaish/SQL__eCommerce-Analysis-for-Maven-Fuzzy-Factory/assets/51399519/3a58ee22-f250-493d-9d9f-6a59a932894e">

For Product 4, which shows potential cross-selling opportunities with other products but has the lowest total orders among the primary products, the following actions could be considered:
   - **Promotional Bundles**: Offer bundled deals combining Products 4 with other products to incentivize customers to purchase all items together, thereby increasing the likelihood of cross-sales.
   - **Enhanced Product Placement**: Strategically place Product 4 alongside other products on the website or in-store displays to encourage customers to explore and consider purchasing all items as a set.

## Conclusion:
The eCommerce analysis for Maven Fuzzy Factory unveils crucial insights into marketing channel performance, website conversion rates, and product impacts. 


By leveraging MySQL databases and comprehensive data analytics, we've gained valuable knowledge to inform strategic decisions and optimize business outcomes. 


Key findings include the dominance of Gsearch as a primary driver of traffic, the effectiveness of brand campaigns in driving conversions, and the significance of desktop users in generating sales. 


Additionally, the introduction of new products has positively impacted user engagement and conversion rates, while the billing test has shown promising results in revenue generation per session. 


Moving forward, strategic actions such as optimizing landing pages, improving mobile conversion rates, and implementing cross-selling strategies will be crucial for maximizing revenue and enhancing overall business performance.
