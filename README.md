# IOWA-LIQUOR-SALE

This repository is a display of some metrices from the top 10 liquor items in the the Iowa liquor sales public database on bigquery for the years 2012 - 2021.


# Introduction

This was a project to throw light on the first of all the top 10 liquor items that was sold and also show their indivisual sales made through each year and also show the sale pattern per month.


# Data Preparatioin

The data used in this project is a public data found in the bigquery databasea unders the iowa_liquor_sale dataset. Having several columns in the table, just a few colimns were of interest in this project. These are;

date: Date of order

sale_dollars: Total cost of liquor order (number of bottles multiplied by the state bottle retail)

item_description: Description of the individual liquor product ordered.


# Data Processing

Dropping nulls wouldnt be necessary since having a null in any column would have no effect of the results expected. 

# Descriptive Analysis

The following query was used to check for the number of item in the data set within the selected years (2012 - 2021) that was sold out of which the top 10 will be selected.

```
select distinct(item_description)
FROM `bigquery-public-data.iowa_liquor_sales.sales`
where extract(year from date) between 2012 and 2021
```

From the results, there happen to be 10096 items sold in the the year range.

Then the next query was used to select the top 10 items based of the total amount each brough in for the year range ordered according to the amounts

```
SELECT sum(sale_dollars) as sales_amount, item_description  
FROM `bigquery-public-data.iowa_liquor_sales.sales` 
where extract(year from date) between 2012 and 2021
group by item_description
order by sales_amount desc
```
From the results, these are the top 10 performing items with their corresponding amounts

![IOWA LIQUOR SALE, TOP 10 TABLE](https://user-images.githubusercontent.com/107520777/177021000-c7c7b27c-a3c6-4bb2-ac5e-115dadac61f4.PNG)

Next was to select the data on each of the top 10 items and how they performed through the years. In the following query, the third (3rd) line of the code was used to filter the data for each of the top 10 items in the first "where" statement as the second where was to ensure that all the data worked on is within the right year range.

```
select sum(sale_dollars) as amount, extract(year from date) as years
FROM `bigquery-public-data.iowa_liquor_sales.sales`
where item_description = 'Black Velvet' and extract(year from date) between 2012 and 2021
group by years
order by years
```
The same query was repeated with the change in the firt where statement for all 10 items, subtituting Black Velvet with each of them in quotes in the third line. Each results is copied from the bigqyery results pane and then pasted onto google sheets and aligned to create a larger table that will hold all these information in a simplified form as shown below.

![IOWA LIQUOR SALE, TOP 10 BY YEARS](https://user-images.githubusercontent.com/107520777/177021273-0807cf54-cada-45b8-bcb4-137955441ed1.PNG)


Data that shows the trend of the item sale according to the months of the year is selected as well with the following query

```
select extract(month from date) as months, sum(sale_dollars) as amount
FROM `bigquery-public-data.iowa_liquor_sales.sales`
where item_description = 'Black Velvet' and extract(year from date) between 2012 and 2021
group by months
order by months
```

Just as in the case of the years, the same query was repeated with the change in the firt where statement for all 10 items, subtituting Black Velvet with each of them in quotes in the third line. Data is copied onto google sheets here too and then editted into one large table as shown below.

![IOWA LIQUOR SALE, TOP 10 BY MONTHS](https://user-images.githubusercontent.com/107520777/177021675-ec7770e7-557f-4765-8b86-7a94b45c59b1.PNG)



# Data Visualisation

***These tables as created in google sheets are then plotted using graph samples in Google sheets.***

This is a piechart visualisation of the total amount of sales by each of the top 10 item, each slice representing an item is labled with its Percentages 

![IOWA LIQUOR SALE, TOP 10 ITEMS SOLD](https://user-images.githubusercontent.com/107520777/177021870-71d4bc3b-5ce2-4cfd-8a6a-dd1b8524a3d3.png)
![IOWA LIQUOR SALE, TOP 10 TABLE](https://user-images.githubusercontent.com/107520777/177021881-48ff4df0-d30a-497c-b4a8-8d0659616aae.PNG)

This is a line graph pf the items total sale through the years

![IOWA LIQUOR SALE, TOP 10 BRANDS](https://user-images.githubusercontent.com/107520777/177021908-3f688d1e-cda9-4b02-8b03-56b0d018c299.png)
![IOWA LIQUOR SALE, TOP 10 BY YEARS](https://user-images.githubusercontent.com/107520777/177021921-a8dd55ed-6716-43c9-8e08-6a7048c97617.PNG)

Finally the graph for the monthly item sale through the years

![IOWA LIQUOR SALE BY MONTH, TOP 10 BRANDS](https://user-images.githubusercontent.com/107520777/177021943-fbad49b3-bf27-43f8-9fd6-235c64390bfb.png)
![IOWA LIQUOR SALE, TOP 10 BY MONTHS](https://user-images.githubusercontent.com/107520777/177021951-7f82d56f-9f3c-4c21-9e9b-f77ddf144501.PNG)



# Insights

1. It is observed that the first two items (Black Velvet, 18.0% and Titos Handmade Vodka, 14.9%) had comparatively larger amounts (123,375,523.3 and 101,647,763.3 respectively) creating quite a gab between then and the next items. Those two alone contributes more than a quarter of the entire sum. Further queries should be built to investigate if the higher values are not due to higher unit prices but instead higher unit sale.
2. The Black Velvet performed best through the years because looking at the trend, it startes off with the highest of all and maintains an almost steady pattern through the years and finishes second and the end to Titos Handmade Vodka.
3. Titos Handmade Vodka started with the least total in 2012 but maintained a stead increase in sale through the years and finishes in 2021 as the highest with more than twice the amount of the second item, Black Velvet. Further analyses will be required to find out if the steady rise in amount is not due to a conresponding rise in unit price over the period.
4. The Captain morgan spiced Rum appears to have been sold more than the Camptain morgan original even though sale of the Spiced flavour discontinued in 2019. We cannot still conclude right away and say that its a prefared choice due to the total amount because the data shows that each item has an unit price and so much querys could be writen to verify the unit price variation. This will bring more clarity and certainty as to wheather the spiced flavour is a prefered choice or it has a higher price.
5. Not only the Captain morgan spiced rum discontinued sale at a point, the Jack Daniels Old #7 Black Lbl and the Crown Royal Canadian Whisky also discontinued sale in the year 2018. Jack Daniels Old #7 Black Lbl though discontinuing sale in 2019 wasnt part of the least performing items according to the sales total.
6. Throught the months, there was a peak in October for all item sales but Titos Handmade Vodka, whiles in november there was a dip in all sales except for Titos Handmade Vodka.
7. Generally, spring season had the least sales amount whiles the fall season had the highest sale amount.



# Recommendation

1. The top 10 items should be made available in stock, further analysis should go into the insights 1 to 4 for a more effective action plan that can bring in more income.
2. Reintroduction of the Jack Daniels Old #7 Black Lbl and the Captain Morgan Spiced Rum is recommened since as it stands, thoUgh both items we no more on sale at a pont, they contributed a good amount in total even more than other items with sales running through all the years.
3. It is mostly expected that during the holiday season, parties and occassions cause a rise in the purchase of beverages but it was winter wasn't an example in this scinario, thus a compaingn and promotional sale on the products is recommended to bring cause more sales in the season.

















