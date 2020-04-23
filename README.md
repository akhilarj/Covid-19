# COVID-19, A Case Study

## Background

The outbreak of Covid-19 has developed into a major international crisis and is impacting important aspects of daily life with emergencies and lockdowns across countries worldwide.

This case study aims to bring data from various sources to provide a deeper understanding of the pandemic and the impact it has globally.

## Executive Summary

This case study covers the following aspects of the pandemic: 

       1. INSIGHTS - Gain better understanding of the pandemic - case trends, affected demographic groups, mortatility rates
       2. RESEARCH - Bring together multiple sources of data under one visualization tool
       3. IMPACT   - Monitor impact of the pandemic in the areas of global health, government responses & economy
       4. SOCIAL PULSE - Drive meaningful interpretations from social interactions in social media to understand how people are impacted and reacting to this global crisis 
       5. FUTURE  - Forecast new cases in the near future using available case history and trends
       
The above scope is achieved through the below 3-step process: 

        1. EDA & VISUALIZATION - Housing various data sources under a single visualization roof covering the following: 
            a. Global Health Indicators - a look at the trend of daily case numbers and countries affected the most
            b. Demographic Indicators - a look into cases by age range for highly affected countries
            c. Government Response Indicators - association analysis of stringency measures implemented vs. increasing case numbers
            d. Economic Indicators - exploring the trend of daily closing prices for key global stock indices
            https://public.tableau.com/profile/akhila.joseph#!/vizhome/CovidDashboard2_15860650422580/Globalhealthindicators?publish=yes
        2. MONITOR SOCIAL INTERACTIONS - gain an understanding of the social media interactions from twitter regarding Covid-19 through topic modeling of tweets. 
        3. FORECAST - absolute number of confirmed and fatal cases worldwide for the upcoming week using timeseries recurring neural-net model - LSTM


---

## Insights from EDA

        1. The number of confirmed and fatal cases are increasing exponentially Mar 2020. 
        2. The US remains as the top affected country worldwide with ~ 840k confirmed cases to date followed by Spain and Italy who are far behind (~200k) the US in terms of absolute numbers
        3. The US reamins as the top country for fatalities as well wiht over 400k fatal cases. 
        4. Within the US, Newyork is the most affected state followed by New Jersey both supassing Hubei province in China where the virus originated. 
        5. The trend remains the same for fatalities with Newyork and New Jersey in the lead followed by Hubei province. 
        6. Mortality rate has now reached close to 7% worldwide. 
        7. Based on the demographic data obtained from Italy, Spain, South Korea and China, 80-90 year olds are the most affected demographic group facing fatal cases, out of which 64% are male patients.  
        8. Looking at association analysis between stringency index (a derivative of stringency measures taken by governments in response to covid-19 situation), most of the countries with exponential growth has managed to curb the growth and stabilize the number of newly confirmed cases with higher stringency measures. United States is an exception in this case. The country raised its overall stringency index only to 76 thus far and the confirmed cases have been growing. This does suggest a direct corelation between stringency measures and the growth of new cases in the countries. 
        9. Looking at the economic landscape, trading volumes started to increase in Feb/Mar period across all major stock indices resulting in a downward trend of closing prices indicating there are several sell off transactions globally. 
        

---

## Modeling, Evaluation and conclusions

### Topic modeling of tweets 

Close to 54,000 tweets where scraped from twitter with hastags - COVID, COVID19, covid, coronavirus, coronavirusimpact, coronavirusoutbreak, corona. 
Tried Sklearn's Latent Dirichlet Allocation on ~54,000 tweets scraped from twitter. Preliminary EDA was performed on these tweets.

        1. Identified top mentions as well as top hastags (excluding the ones that were used for scraping). 
                - Popular hashtags included #CoronavirusUSA, #CoronaLockdown, #Coronavirustruth, #Stayhome, #China, #trump, #wuhan and more. 
                - Popular mentioned included @realdonaldtrump, @YouTube, @POTUS (Presdent of The US), @WHO, @UKChange, @CNN, @CDCgov and more.
        2. Most number of tweets were seen to be generated on Thursday, Wednesday and Saturdays of the week. 
        
Preliminary cleaning & text processing of the tweets was performed prior to passing it to the model. 

Modeling: 
**Sklearn's Latent Dirichlet Allocation** for single words and bigrams. Not many bigrams were identified. Thus, chose to stick with single words. 
Tried a 2nd model - Non-Negative Matrix Factorization as it deemed to be fit for short documents such as tweets as opposed to LDA. Topics generated with NMF model appeared to be very similar to the one generated with LDA. Thus, stuck to LDA model as final model. 

Outcome of topic modeling follows. Topics interpreted from the top words are available on the left of this sankey diagram and top words identified comes on the right of the diagram. 

<img src = "Topics to words Sankey.png"/>

---

### Forecasting of Confirmed and Fatal Cases

Employed timeseries recurring neural net model **LSTM** to forecast as date was the only feature to be used in the model. 

Modeling involved the following steps: 
1) Split the whole dataset into training (90%) and validation (10%) sets.
2) Scaled confirmed and fatal case numbers with MinMax Scaler to normalize the numbers.
3) Generated batches of temporal data using TimeseriesGenerator with 5 steps i.e 5 data points to predict the 6th data point.
4) Instantiated a Sequential model with 150 neurons passed to LSTM layer, shrink the output to 75 neurons that is fed to a dense layer and another dense layer that further shrinks the final output to 2 (confirmed & fatal case). 
5) Activation function used is relu, optimizer used is Adam and loss function is MSE. 

#### Model Performance

Metrics used included MAPE (Mean Absolute Percentage Error and RMSLE)
        
        - MAPE scored at 9% for the validation set translating to 91% accuracy.
        - RMSLE score 0.1
        - Checked the actual accuracy for dates that were not available in the validation set (latest 4 days from 19-Apr to 22 -Apr) and accuracy scored at 95% and 94% for confirmed and fatal cases respectively. 

### Data Sources

Base dataset: John Hopkins Centre of System Science and Engineering (Covid- case numbers and training data)<br>
Demographic data for selected countries : The Lancet Digital Health<br>
Economic Indicators : Yahoo Finance<br>
Worldwide Government Responses: Oxford University Govt. Response Tracker<br>
Tweets: Scraped using twitter scraper API<br>

