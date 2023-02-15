![image](https://user-images.githubusercontent.com/100429663/219097998-77220f55-1a11-4e27-bd62-f53582273828.png)

# Zillow Time Series Modeling: Forecasting Real Estate Prices of US Ski Resort Towns

By: Tim Rabbitt

## Overview:
    
A small group of clientele is looking to invest in real estate, members within this group happen to be avid skiers and are particularly interested in real estate that is near a ski resort. While finding suitable real estate near a resort is a main focus, they are hoping that their potential investment is both affordable and forward-thinking. For this project, we will conduct a time series analysis of Zillow's historical average home values in the United States. The goal is to provide the top 5 zip codes, based on projected 3-year return on investment, that is near a ski resort for our clients to consider as investment opportunities.

## Business Understanding:
Our clients offered the following criteria to consider for this project:

1. They would like to invest in an area that is near an attractive resort. This means the resort must have more than 2000 acres of skiable terrain and experience an average of more than 300 inches of annual snowfall.

2. They are not naive as to the price of real estate near most premier ski resorts. They hope to find an investment opportunity 'under the radar'.  Our client's budget is $500,000, applied to the most recent housing data in our dataset (2018).

3. Our clients would like to limit risk by using historical, 5yr, and 3 yr ROI, as well as coefficient of variation of historical housing prices in areas of interest.

By following this criterion for our analysis, we can narrow our focus to only modeling zipcodes that fulfill these requirements. Once those zipcodes are identified, we can perform a time series analysis to find which zip codes project to be the best 3-year investment. 

## Data Understanding:

We used two datasets for our analysis. The first dataset consists of nearly 15,000 zip codes within the United States. This dataset has 14,723 rows and 272 columns. Columns mainly consist of the monthly average home values of each zip code between April 1996 and April 2018. The dataset also provides additional information about each zipcode, such as:
* `RegionID` - Unique ID number. 
* `RegionName` - Zipcode
* `City` - The name of the city in which the Zipcode is located.
* `State` - The name of the State in which the City is located. 
* `Metro` - The name of the Metro region. 
* `CountyName` - The name of the County 
* `SizeRank` - Zipcode’s size ranking relative to other records in the dataset.

The second dataset used for this analysis consists of information on 462 ski resorts in North America. This dataset provides metrics of interest such as average annual snowfall, skiable acreage, the nearest city, and the state the resort is located in. Further information on this dataset can be found at [Kaggle.com](https://www.kaggle.com/datasets/aryanjuyal/comparison-of-north-american-ski-resorts)

* `Resort` - The name of the ski resort
* `City` - Name of closest City                       
* `State` - Name of the State in which the Resort is located                       
* `Peak elevation (ft)` - Peak elevation of Resort         
* `Base elevation (ft)` - Base elevation of Resort        
* `Vertical drop (ft)`  - Difference in peak and base elevation        
* `Skiable acreage` - Total skiable acreage of the Resort             
* `Total trails` - Total number of trails on the Resort                
* `Total lifts` - Total number of lifts on the Resort                 
* `Avg annual snowfall (in)` - Average annual snowfall in inches  
* `Lift ticket (USD)` - Lift ticket price           


By utilizing both datasets we were able to accomplish our clients wishes of filtering zipcodes based on historical average home sales values as well as proximity to attractive ski resorts.

## Data Preparation:
 
![image-2.png](attachment:image-2.png)

The time series chart above shows all filtered zipcodes that met the criteria laid out by the client. The red dashed line shows our client budget of $500K. You can clearly see the housing markert bubble of 2008, for this anlysis we decided to use home values from 2010 to 2018 to avoid that time period skewing our data.

![image.png](attachment:image.png) 

As you can see, real estate values were much more stable after 2010. With a consistant slight upward trend. Analysis the time period above provided a much more stable time series to forecast future home value.

## Model Training and Forecasting

Once we had completed our data filtering and preprocessing steps, we trained a SARIMA (Seasonal Autoregressive Integrated Moving Average) model to forecast future home values on zipcodes of interest. SARIMA takes into account the past values and predicts future values based on that, while also taking into account any seasonality patterns. 

The following images are the model diagnostics printed after fitting the data series for zip code 59718. Ideally we want to make sure that our residuals are not correlated and have a normal distribution. Using the diagnostics below you can get an idea of whether or not the model is accurately capturing the time series or not.

![image.png](attachment:image.png)

**Top left:** This plot should show white noise, meaning our residual errors fluctuate around the mean of zero with uniform variance. The mean of our residuals does appear to be zero there is close to uniform variance. Which is great.

**Top Right:** Our density plot mirrors the N(0,1) line which is the notation for standard normal distribution. This suggests that our residuals are normally distributed.

**Bottom Left:** The qq-plot shows that our residuals are following a linear trend, again suggesting they are normally distributed. 

**Bottom Right:** The correlogram shows some correlation between the residuals and their lagged versions. This could be due to exogenous variables that our model is not accounting for. Future analysis could consider including these features in the modeling process, for now that is outside the scope of this project.

These observations lead us to conclude that our model provides a satisfactory fit to help forecast future values.

Once our model is fit to our training data we can use it to forecast values from 2016-2018 (our test data). We can see how accurate our model is at forecasting future values by analyzing how close of a fit it had the actual values during that time.

![image-4.png](attachment:image-4.png)
If we feel our model is satisfactory in forecasting our test data we then use it to predict future values. In the case of this project, we forecasted the monthly average home value of each zip code 3 years into the future (2021).

![image-5.png](attachment:image-5.png)
Once our model has successfully generated future value predictions, we can calculate a potential 3 year ROI on real estate purchased in April 2018.

## Recommendations and Conclusions

Our time series analysis produced the following 5 zip codes that we recommend to the client for investment opportunities. Our model predicted zipcode 84102, located in Salt Lake City and near Alta Ski Resort, as the most fruitful investment. The projected 3-year return on investment for all zipcodes in our top 5 is greater than 20 percent and most (except zipcode 97701) are well above the median 3-year ROI of all other zipcodes we performed modeling on. 

![image.png](attachment:image.png)

Here is a ranking of each zipcode based on highest three year return on investment projections and their associated ski resort metrics of interest.


### #1 Salt Lake City, UT: Zipcode 84102

#### 3 Year ROI - 35.48%

Resort: **Alta Ski Area**

Skiable acerage: **2,200**

Average annual snowfall (in): **560** 

### #2 Spokane, WA: Zipcode 99207

#### 3 Year ROI - 31.39%

Resort: **49 Degrees North**

Skiable acerage: **2,325**

Average annual snowfall (in): **300**


### #3 South Lake Tahoe, CA: Zipcode 96150

#### 3 Year ROI - 30.57%

Resort: **Heavenly Mountain Resort**

Skiable acerage: **4,800**

Average annual snowfall (in): **360**

### #4 Kalispell, MT: Zipcode 59901

#### 3 Year ROI - 29.06%

Resort: **Whitefish Mountain Resort**

Skiable acerage: **3,000**

Average annual snowfall (in): **300** 

### #5 Bend, OR: Zipcode 97701

#### 3 Year ROI - 21.09%

Resort: **Mt. Bachelor**

Skiable acerage: **4,318**

Average annual snowfall (in): **462**

This analysis was meant to provide our clients with options for smart real estate investment opportunities in areas where they can enjoy their favorite pass time, skiing. We believe that the recommended areas put forth through this modeling process can help them to accomplish this goal. While time series modeling is not a flawless method of predictive power, it provides us with more confidence in our decision-making when trying to make good investments. Choosing which zipcode to eventually invest in will be left to the client to decide based on personal preference.

# Limitations and Future Analysis

1. A limitation of this analysis that was encountered was the omission of resorts that were near cities not recorded or recognized in our Zillow dataset. This means that it is certainly possible for there to be additional zipcodes that fit our client's criteria that were not captured during our data filtering procedures. Finding a more robust dataset that contains zip codes and all of their associated cities/towns could help with this issue.


2. Exploring other exogenous features that contribute to the home value volatility of resort towns may help to better understand correlations within the data. These could include work-from-home opportunities, job markets, and weather.


3. The Zillow dataset used for this analysis had its most recent data points in April 2018. Finding more recent data will allow us to more accurately forecast future home value into 2025 and beyond.

# For More Information

Please check out my [Jupyter Notebook](https://github.com/trabbitt90/Zillow-Housing-Time_Series_Analysis/blob/main/Zillow%20Time%20Series%20Analysis.ipynb) and [Presentation](https://github.com/trabbitt90/Zillow-Housing-Time_Series_Analysis/blob/main/presentation.pdf)

# Navigating the Repository
README.md ---> Document for reviewers of the project

Zillow Time Series Analysis.ipynb ---> Modeling and methods explaination in Jupyter Notebook

Presentation.pdf ---> PDF of presentation slides
