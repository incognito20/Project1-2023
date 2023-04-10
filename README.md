# Obtained Source Data
* **Kaggle:** Uber NYC for-hire vehicles trip data (2021)
NYC for-hire vehicle trip data, taxi zones zip code lookup, NYC weather 2021
https://www.kaggle.com/datasets/shuhengmo/uber-nyc-forhire-vehicles-trip-data-2021
<br><br>

# Determined Questions for Analysis
* How does weather affect ride volume?
* How does weather affect rider fare?
* What trends can be observed over time?
* Do temperature or trip length influence tips?
<br><br>

# Imported and Cleaned the Data
### [Project1_DataPrep.ipynb](https://github.com/incognito20/Project1-2023/blob/main/Project1_DataPrep.ipynb)
<br>

1. Imported necessary dependencies:
    * pandas
    * pyarrow.parquet
    * datetime
    * holidays
<br><br>

2. Defined variables:
    * Manually created a data dictionary with a number identifier and text label for each day of the week
    * Import US Holiday info from holidays module
    * Defined sample size of 0.001
<br><br>

3. Imported the 12 individual parquet files (one for each month in 2021) and created dataframes for each, selecting a random sample of 0.1% of the population for each dataset
    * This still resulted in a total population of ~175K rows of data!
<br><br>

4. Combined the 12 monthly dataframes into one and renamed the columns
<br><br>

5. Created new columns to include calculated fields and prepare for merging with the weather and taxi zone data:
    * **PU_TIME:**  created new column to convert ‘PICKUP_TIME’ string to datetime format
    * **DOW:**  day of week number identifier (defined above)
    * **PICKUP_DAY:**  day of week text label (defined above)
    * **DATE:**  created second column converting ‘PICKUP_TIME’ string to datetime format
    * **MYDATE:**  created new column to convert ‘PICKUP_TIME’ string to date format (used to merge with weather dataframe)
    * **PICKUP_DATE:**  created second column converting ‘PICKUP_TIME’ string to date format
    * **TOTAL:**  sum of all fare elements (base fare, fees, taxes, etc.)
    * **TIP%:**  percent of tip paid (tip amount / total fare * 100)
<br><br>

6. Imported weather CSV file as a new dataframe, removed unnecessary columns, and created new columns:
    * **TEMP_F**:  created new column to convert 'temp' from Celsius to Fahrenheit
    * **CAL_DATE:**  created new column to convert ‘datetime’ string to datetime format
    * **MYDATE:**  created new column to convert ‘datetime’ string to date format (used to merge with ride dataframe)
    * **HOLIDAY:**  created bullion to identify holidays
    * **PRECIP_INCHES:**  created to convert ‘precip’ column from mm to inches
    * **WEATHER:**  created bullion to identify ‘DRY’ or ‘WET’ days (If ‘PRECIP_INCHES’ > .04, ‘WEATHER’ = ‘WET’)
<br><br>

7. Imported taxi zone data from CSV as a new dataframe and dropped ‘service_zone’ column
<br><br>

8. Merged ride dataframe with taxi zone dataframe by pickup location
<br><br>
    *(Note:  We initially added the taxi zone data to complete the API bonus but ended up not using this data in the final analysis)*
<br><br>

9. Merged the resulting dataframe with the weather dataframe on ‘MYDATE’
<br><br>

10. After the final combined dataframe was created, we determined that additional columns could be dropped. Code was created to drop these columns, and also to rename ‘precip’ to ‘PRECIP.’
<br><br>

11. The final dataframe was written to a CSV file. Note that the CSV file could not be uploaded to git hub due to the large file size and was instead retained locally on each group member’s desktop to perform the data analysis (VFH.csv). The VFH.csv file was added to the .gitignore file to ensure ease of access to the data for coding while avoiding errors in git hub related to the large file size. 
<br><br>

# Data Analysis and Visualization
### [data_viz.ipynb](https://github.com/incognito20/Project1-2023/blob/main/data_viz.ipynb)
<br>

1. Imported dependencies:
    * matplotlib.pyplot
    * pandas
    * nympy
    * datetime
    * scipy.stats linregress
<br><br>

2. Imported VFH.csv file as a dataframe
<br><br>

3. Performed data analysis to answer the following questions:
    1. How Does Weather Affect Ride Volume?
        * Does temperature affect number of rides per day?
        * Does precipitation affect number of rides per day?
    2. How Does Weather Affect Rider Fare?
        * Does temperature affect rider fare?
        * Does precipitation affect rider fare?
    3. What Trends Can Be Observed Over Time?
        * Are there trends in ridership that can be identified based on day of the week?
        * Are there trends in ridership that can be identified month-to-month? 
    4. Do Temperature or Trip Length Influence Tips?
        * Does temperature affect riders’ decision to tip?
        * Does trip length affect riders’ decision to tip?
<br><br>

3. Created a scatter plot to show the relationship between the number of rides completed per day and temperature
    * Used groupby, unique, and count functions to determine the temperature and ride count for each day of the year
    * Set both of these values as ‘float’
    * Created a scatter plot with temperature on the x-axis and ride count on the y-axis
    * Plotted the linear regression for the relationship and determined the r-value
    * Printed the plot to the Jupyter notebook and saved the image to the “figures” folder ([Fig1.png](https://github.com/incognito20/Project1-2023/blob/main/figures/Fig1.png)).
<br><br>

4. Created a scatter plot to show the relationship between the number of rides completed per day and precipitation
    * Used groupby, unique, and count functions to determine the amount of precipitation and ride count for each day of the year
    * Set both of these values as ‘float’
    * Created a scatter plot with precipitation on the x-axis and ride count on the y-axis
    * Plotted the linear regression for the relationship and determined the r-value
    * Printed the plot to the Jupyter notebook and saved the image to the “figures” folder ([Fig2.png](https://github.com/incognito20/Project1-2023/blob/main/figures/Fig2.png)).
<br><br>

5. Created a scatter plot to show the relationship between rider fare and temperature
    * Used groupby, unique, and median functions to determine the temperature and median rider fare for each day of the year
    * Set both of these values as ‘float’
    * Created a scatter plot with temperature on the x-axis and median rider fare on the y-axis
    * Plotted the linear regression for the relationship and determined the r-value
    * Printed the plot to the Jupyter notebook and saved the image to the “figures” folder ([Fig3.png](https://github.com/incognito20/Project1-2023/blob/main/figures/Fig3.png)).
<br><br>

6. Created a scatter plot to show the relationship between rider fare and precipitation
    * Used groupby, unique, and median functions to determine the amount of precipitation and median rider fare for each day of the year
    * Set both of these values as ‘float’
    * Created a scatter plot with precipitation on the x-axis and median rider fare on the y-axis
    * Plotted the linear regression for the relationship and determined the r-value
    * Printed the plot to the Jupyter notebook and saved the image to the “figures” folder ([Fig4.png](https://github.com/incognito20/Project1-2023/blob/main/figures/Fig4.png)).
<br><br>

7. Created a bar chart to show the relationship between the ride volume and day of the week<br><br>
    *(Note:  We chose to aggregate the data based on average ride count per day for each day of the week, to account for potential seasonal variables, such as the traditional academic calendar, major holidays, and changes in weather)*
    * Used groupby, unique, and nunique functions to determine the day of the week and daily ride count per day
    * Then, calculated the average daily ride count for each day of the week, based on the number of times each day of the week occurred for the year (in most cases, each day of the week occurred 52 times)
    * Set both of these values as ‘float’
    * Created a bar chart with days of the week on the x-axis and average daily ride count on the y-axis
    * Printed the plot to the Jupyter notebook and saved the image to the “figures” folder ([Fig5.png](https://github.com/incognito20/Project1-2023/blob/main/figures/Fig5.png)).
<br><br>

8. Created a bar chart to show the relationship between the ride volume and month of the year<br><br>
    *(Note:  We chose to aggregate the data based on average ride count per day for each month, to account for day-to-day variation over the month, which could skew the total monthly ride count)*
    * Created a variable to calculate the month for each row of data
    * Used groupby, unique, and nunique functions to determine the total ride count per month and the number of days in each month
    * Then, calculated the average daily ride count for each month by dividing the total monthly ride count by the number of days in the month
    * Set both of these values as ‘float’
    * Created a bar chart with months on the x-axis and average daily ride count on the y-axis
    * Printed the plot to the Jupyter notebook and saved the image to the “figures” folder ([Fig6.png](https://github.com/incognito20/Project1-2023/blob/main/figures/Fig6.png)).
<br><br>

9. Created a scatter plot to show the relationship between temperature and the percentage of rides that included a driver tip
<br><br>
    *(Note:  We chose to aggregate tips as a percentage of the rides each day in which the tip was greater than $0.00. Earlier in our analysis, we calculated the total percentage of rides with tips for the full population and noted that only ~16% of all rides included a tip.)*
    * Used groupby, unique, and count functions to determine the temperature, daily ride count, and number of rides that included a driver tip (‘P_TIP’ > 0) for each day of the year
    * Then, calculated the percentage of rides that included a tip each day (daily ride count / count of rides with tips * 100)
    * Set both of these values as ‘float’
    * Created a scatter plot with temperature on the x-axis and percent of rides with tips on the y-axis
    * Plotted the linear regression for the relationship and determined the r-value
    * Printed the plot to the Jupyter notebook and saved the image to the “figures” folder ([Fig7.png](https://github.com/incognito20/Project1-2023/blob/main/figures/Fig7.png)).
<br><br>

10. Created a scatter plot to show the relationship between trip length and the percentage of rides that included a driver tip<br><br>
    *(Note:  We chose to aggregate trip length as a daily average to account for the sheer volume of rides per day, each with a unique value for trip length)*
    * Used groupby, mean, and count functions to determine the average trip length, daily ride count, and number of rides that included a driver tip (‘P_TIP’ > 0) for each day of the year
    * Then, calculated the percentage of rides that included a tip each day (daily ride count / count of rides with tips * 100)
    * Set both of these values as ‘float’
    * Created a scatter plot with average trip length on the x-axis and percent of rides with tips on the y-axis
    * Plotted the linear regression for the relationship and determined the r-value
    * Printed the plot to the Jupyter notebook and saved the image to the “figures” folder ([Fig8.png](https://github.com/incognito20/Project1-2023/blob/main/figures/Fig8.png)).
    <br><br>

# Analysis of Results

## How does weather affect ride volume?

We identified a positive correlation between ridership and temperature. In other words, daily ride count tended to increase as temperature increased. However, this correlation is weak (r-value:  ~0.218). In contrast, we discovered a negative correlation between ridership and precipitation, with ride count decreasing slightly as precipitation increases. This correlation is very weak (r-value:  ~-0.060). Overall, it appears that temperature may contribute to an increase in ride volume; however, precipitation does not appear to have an impact on ridership.

## How does weather affect rider fare?

We identified a positive correlation between median rider fare and temperature, with median rider fare tending to increase as temperature increased. This correlation was fairly moderate, compared to other trends we observed (r-value:  ~0.625). There was also a positive correlation between median rider fare and precipitation. However, this correlation was weak (r-value:  ~0.170). Overall, it appears that temperature may play a role in rider fare; however, precipitation does not appear to have any impact on rider fare.

## What trends can be observed over time?

We identified time-based trending in the average daily ride count, with trends observed over days of the week, and month-to-month over the year. Weekly, the average daily ride count appears to gradually increase each day beginning on Monday, with the peak ride volume occurring on Saturday. Ride volume drops down dramatically on Sunday, but is still slightly higher than on Monday. This trend is what we expected, based on anecdotal evidence (i.e., our own personal rideshare experiences), as people tend to work on weekdays, and may be more likely to use a rideshare when attending social functions on the weekend.

Month-to-month, the average daily ride count appears to increase slightly beginning in January, with a peak in June. Then, a slight decrease can be observed over July and August, with another spike in ride count, peaking higher in October and November. We speculate that monthly ride volumes could correspond with local weather patterns, as well as other seasonal factors, such as the academic calendar and major holidays. Additionally, it’s possible that the COVID-19 Pandemic could have impacted ridership in the beginning of 2021, as a result of government lockdowns in place in New York City at that time. The peaks in ride volume we observed could have been impacted by the release of the COVID-19 vaccines and subsequent lifting of lockdowns. More research is needed to determine the impacts of the COVID-19 Pandemic on rideshare usage.

## Do temperature or trip length influence tips?

We identified a positive correlation between temperature and the percentage of riders who tip, with the percentage of rides in which the driver received a tip increasing as temperature increased. A similar correlation was observed between trip length (in miles) and the percentage of rides that received a tip. In both cases, the correlation was fairly weak (r-values:  ~0.299 and ~0.344, respectively). This indicates that weather conditions and trip length may have a small impact on tipping; however, as the relationship is not very strong, there are likely other factors that influence a rider’s decision to tip.



