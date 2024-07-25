# Ototdom Analysis || Snowflake -SQL, Python

## Introduction and Objective

1. Introduction

 
 Objective

The primary objective of this analysis is to search for the affordable apartments in Poland country with the help of online rental housing portals, here using otodom.co for the analysis.

## Steps followed 

#### 1. Data scrapping with the help of brightdata.com from otodom portal.
#### 2. Exporting data from brightdata to snowflake
#### 3. Data Flattening from snowflake to snowflake table 
#### 4. Transformation of latitude and longitude to a proper address with the help of python Geocode API and performing the conversion, after conversion again back load to the Snowflake in different table.
#### 5. Transformation of title which is in polish language and conversion it with the help of python Geo Translator API and after conversion load back to the snowflake table.
#### 6. Joining of tables in Snowflake and resulting few analysis.

#### Data Analysis

Key Observations:

Q1. What is the average rental price of 1 room, 2 room, 3 room and 4 room apartments in some of the major cities in Poland? 
Arrange the result such that avg rent for each type fo room is shown in seperate column


       SELECT CITY, 
       ROUND(AVG(CASE WHEN NO_OF_ROOMS=1 THEN PRICE_NEW END),2) AS Average_1_Room,
       ROUND(AVG(CASE WHEN NO_OF_ROOMS=2 THEN PRICE_NEW END),2) AS Average_2_Room,
       ROUND(AVG(CASE WHEN NO_OF_ROOMS=3 THEN PRICE_NEW END),2) AS Average_3_Room,
       ROUND(AVG(CASE WHEN NO_OF_ROOMS=4 THEN PRICE_NEW END),2) AS Average_4_Room
       FROM OTODOM_DATA_TRANSFORMED 
       WHERE APARTMENT_FLAG = 'apartment' AND CITY IN ('Warszawa', 'Wrocław', 'Kraków', 'Gdańsk', 'Katowice', 'Łódź')
       AND IS_FOR_SALE = 'false'
       GROUP BY CITY
       ORDER BY CITY;
![Avg_Rent City wise](https://github.com/user-attachments/assets/89fd293f-2c59-43af-a48f-352d0b2fcd0a)

Q2. I want to buy an apartment which is around 90-100 m2 and within a range of 800,000 to 1M, display the suburbs in warsaw where I can find such apartments. 


    SELECT SUBURB, count(1) as number_of_apartment, max(price_new) as max_price, min(price_new) as min_price
    FROM OTODOM_DATA_TRANSFORMED
    WHERE city in ('Warszawa') and 
    surface_new BETWEEN 90 and 100 
    AND APARTMENT_FLAG ='apartment'
    and price_new between 800000 and 1000000
    group by suburb 
    order by count(1) desc;

  ![desired apartment](https://github.com/user-attachments/assets/1c1527de-5baf-4359-a616-afadbbecd666)

More Analysis question are available in the files.
