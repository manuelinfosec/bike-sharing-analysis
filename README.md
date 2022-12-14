# Bike Sharing Analysis

Bike sharing is a fundamental service, commonly used in the urban mobility sector. it is easily accessible (as no driving license is required to ride a bike), cheaper than normal car sharing service (since bike maintenance and insurance are substantially cheaper than automobile ones), and, finally, is often a fast way to commute within the city. Therefore, understanding the driving factors of bike sharing requests is essential for both companies and users.

From a company's perspective, identifying the expected bike demand in a specific area, within a specific time frame, can significantly increase revenue and customer satisfaction. Moreover, bike relocation can be optimized to further reduce operational costs. From a user's perspective, probable the most important factor is bike availability in the shortest wait time, which is obviously in alignment with the company's interests.

## Project Objective
In this project, I will analyze bike sharing data from Capital Bikeshare Washington, D.C., USA, for the period between January 1, 2011, and December 21, 2012. The data is aggregated on an hourly basis. This means that no initial and final locations of the individual rides are available, but only the total number of rides per hour. Nevertheless, additional meteorological information is available in the data, which could serve as a driving factor for identifying the total number of requests for a specific time frame (bad weather conditions could have a substantial impact on bike sharing demand).

Although the conducted analysis is related to bike sharing, the provided techniques could be easily transferred to other types of sharing business models, such as car or scooter sharing.

## Dataset
The original dataset is available at https://archive.ics.uci.edu/ml/datasets/Bike+Sharing+Dataset#.

According to the description of the original data, provided in the [Readme.txt](data/README.txt), the columns can be split into three main groups:
- **temporal features**: This contains information about the time at which the record was registered. This group contains the **dteday, season, yr, mnth, hr, holiday, weekday** and **workingday** columns.
- **weather related features**: This contains information about the weather conditions. The **weathersit, temp, atemp, hum** and **windspeed** columns are included in this group.
- **record related features**: This contains information about the number of records for the specific hour and date. This group included the **casual, registered** and **cnt** columns.

> Note: **instant** wasn't included in any of the aforementioned groups. The reason for this is that it is an index column and will be exluded from the analysis, as it does not contain any information relevant to this analysis.

### Preprocessing Details
1. `seasons`: each of the four divisions of the year
    - `1`: Winter
    - `2`: Spring
    - `3`: Summer
    - `4`: Fall.

2. `yr`: year
    - `0`: 2011
    - `1`: 2012

3. `weekday`: day of the week
    - `0`: Sunday
    - `1`: Monday
    - `2`: Tuesday
    - `3`: Wednesday
    - `4`: Thursday
    - `5`: Friday
    - `6`: Saturday

4. `hum`: humidity percentage
    - `0` - `100`

5. `windspeed`: registered windspeed
    - `0` - `67`

6. `weathersit`: currnet weather conditions
    - `1`: clear
    - `2`: cloudy
    - `3`: light snow or rain
    - `4`: heavy snow or rain


## Univariate Analysis
### REGISTERED VS. CASUAL USE ANALYSIS

![](figs/rides_distribution.png)
<p align="center"><sub>Distribution of registered versus casual rides per hour</sub</p>

- Registered users perform way more rides than casual ones.
- Both distributions are skewed to the right, meaning for most of the entries in the data, zero or a small number of rides were registered.
- Every entry has quite a large number of rides (that is, higher than 800).


![](figs/rides_daily.png)<p align="center"><sub>Number of rides daily for registered and casual customers</sub</p>

- The number of registered rides is always significantly higher than the number of casual rides per day.
- During the winter, the overal number of rides decreases (as bad weather and low tempreatures have a negative impact on ride sharing services)
- With the lot of variance in the time series above, it's necessary to smooth out the curves by taking the rolling mean and standard deviation of the two time series and plot those instead.

![](figs/rides_aggregated.png)
<p align="center"><sub>Rolling mean and standard deviation of rides</sub</p>

- This way, visualisation is now possible for the average number of rides for a specific time period (also known as a **window**) but also the expected deviation from the mean.
- The values of the rolling mean and standard deviation at certain time instance is only computed from the last *window* entries in the time series (in our case, this is 7), and not from the entries of the whole series.

![](figs/weekday_hour_distributions.png)
<p align="center">Distribution of rides on a daily and hourly basis<sub></sub></p>

Working days:
- The highest number of rides for registered users takes place around 8 AM and 6PM. This is totally expected, as it is likely that most registered users use the bike sharing service for commuting.
- The casual usage of bike sharing services on working days is quite limited, as the plot shows.

Weekends:
- The distribution changes for both casual and registered users.
- The registered rides still outnumber the casual ones.
- Both distributions are almost uniformly distributed betwen the time interval of 11 AM to 6 PM.

It can be claimed that most of the usage of bike sharing services occurs during working days, right before and right after the standard working time (that is, 9 to 5).

### SEASONAL IMPACT ON RIDES
![](figs/season_hour_distributions.png)
<p align="center"><sub>Distribution of rides on a seasonal level (hours of the day)</sub></p>

- Each season has a similar graph shape.
- There are fewer rides (registered and casual) during the winter. This makes sense, as fewer rides are likely to occur when the weather conditions are poor.

![](figs/season_weekday_distributions.png)
<p align="center"><sub>Distribution of rides on a seasonal level (days of the week)</sub></p>

- There is a decreasing number of registered rides over the weekend (compared to the rest of the week), while the number of casual rides increases.

This could enforce an initial **hypothesis**, that is, that registered customers mostly use the bike sharing service for commuting (which could be the reason for the decreasing number of registered rides over the weeekend)

## Hypothetical Testing
This section performs a general conclusion about a population in the dataset based on analysis and measurements performed on a sample. In essence, it tries to address the question, *"Is a certain hypothetical value in line with the value obtained by direct measurements or not?"*

### HYPTOTHESIS TESTING ON REGISTERED RIDES
1. Formulate the null hyptotheis. 
  
    > Is there a significant difference between registered rides during working days and the weekend?

    Therefore, the null hypothesis is that the average number of rides for registered uses during the working days is same as the average number of rides during the weekdend. In other words:

    $ H_0 $: average registered rides over the weekday - average registered rides over weekends = 0.

    $ H_a $: average registered rides over the weekday - average registered rides over weekends $\neq$ 0.

2. After null and alternative hypothesis is established, collect data for the two groups. 

3. Perform the two-sample t-tests.

With a standard critical value of 0.05 (5% significant level), the p-value from the test returns 0.000 which is far below the critical value. As conclusion, the null hyptothesis can be rejected and initial observation (there is a statistically significan difference between the number of rides performed during the working days and weekends) is correct.

4. Plot the distribution of two samples:

![](figs/registered_rides_distribution.png)
<p align="center"><sub>Registered Rides Distribution: workdays vs weekends</sub></p>

5. Perform the same type of hypothesis testing to validate the second assumption from the last section -- that is, casual users perform more rides during the weekend. 
    
    > Is the average number of rides during the working days the sasame as the average number of rides during the weekend, for casual users?

    Therefore, the alternative hypothesis will then result in a statistically significant difference in the average number of rides between the two groups.

After running tests, the p-value returned 0, which is strong evidence against the null hypothesis. Hence, casual customers also behave differently over the weekend (in this case, they bike sharing service more) as seen in the plot.

![](figs/casual_rides_distribution.png)
<p align="center"><sub>Casual Rides Distribution: workdays vs weekends</sub></p>

## Correlation Testing
### ANALYSIS OF WEATHER-RELATED FEATURES

In this section, I'll focus on analysis of the group of features representing the weather conditions. My expectation is to observe a strong dependency of those features on the current number of rides, as bad weather can significantly influence bike sharing services. 

As most of the weather features seem to be continously distributed, the first and most common way to measure the relationship between two different continous variable is to measure their correlation.

Correlation is a statistic that measures the degree to which two variables move in relation to each other. It provides a numerical measure between -1 and 1.

![](figs/correlation_temp.png)
<p align="center"><sub>Correlation between rides and temp</sub></p>


![](figs/correlation_atemp.png)
<p align="center"><sub>Correlation between rides and atemp</sub></p>

Plotting **hum** and **windspeed** seperately:

![](figs/correlation_hum.png)
<p align="center"><sub>Correlation between rides and hum</sub></p>

![](figs/correlation_windspeed.png)
<p align="center"><sub>Correlation between rides and windspeed</sub></p>

It is observable that higher tempreatures have a positive impact on the number of rides. Note  that as the values in the **registered** colum are widely spread with respect to the different values in **temp**, there is a lower correlation compared to the **casual** column. The same pattern is observed in the **hum** visuals where the humidity level has a negative correlation with both types of rides.

This means that with a high level of humidity (mist or rain), customers will tend not to use the bike sharing service. Lastly, there is a minimal correlation between the number of rides and the weind speed (weak positive correlation).

![](figs/correlation_matrix.png)
<p align="center"><sub>Correlation matrix between continous weather features and rides</sub></p>

## Time Series Analysis
### Stationarity
When studying time series, an important concept is the notion of stationarity. A time series is said to be *strongly stationary* if all aspects of its behavior do not change in time. It is possible to have a time a time series that is *weakly stationary*, that is, its mean, standaard deviation, and covariance are stationary with respect to time.

The **Augumented Dickey-Fuller stationarity test** can be used to identify stationarity in a time series. This is a statistical test in which the null hypothesis is that the time series is nonstationary. Hence when performing the test, a small p-value would be strong evidence against the time series being non-stationary.

![](figs/stationarity_test_registered.png)
<p align="center"><sub>Stationarity test results for aggregated daily registered rides</sub></p>

![](figs/stationarity_test_casual.png)
<p align="center"><sub>Stationarity test results for aggregated daily casual rides</sub></p>

From the performed tests, it is obvious that neither the moving average nor standard deviations are stationary. Furthermore, the Dickey-Fuller test returns values of 0.355 and 0.372 for **registered** and **casual** users, respectively. This is strong evidence that the time series is not stationary, and we need to process them in order to obtain a stationary one.

### Detrending a Time Series
A common way to detrend a time series and make it stationary is to subtract either its rolling mean or its last value, or to decompose it into a component that will contain its trend, seasonality, and resudial components.

![](figs/daily_registered_diff.png)
<p align="center"><sub>Stationarity test results for aggregated daily registered rides (after detrend) </sub></p>

![](figs/daily_casual_diff.png)
<p align="center"><sub>Stationarity test results for aggregated daily casual rides (after detrend) </sub></p>

Both techniques (rolling mean and last value shifting) returned a time sereies, which is stationary, according to the Dickey-Fuller test (0.00). Note that an interesting pattern occurs in the casual series: a rolling standard deviation exhibits a clustering effect, that is, periods in which the standard deviation is higher and periods which it is lower.

This effect is also commmon in certain fields (finance, for instance) and is also known as **volatility clustering**.

*A possible interpretation, relative to the data, is that the number of casual rides increses during the summer periods and drops during the winter.*


#### Time Series Decomposition

Another technique for detrending time series is a previously mentioned technique, **time series decomposition**. This involves breaking the time series into seperate components.

- *Trend component*: This component represents a long-term progression of the series. A trend component is present when there is a persistent increase or decrease in the series.

- *Seasonal component*: This component represents seasonality patterns in the data. A seasonal component persists when the data is influenced by certain seasonal factors  (for exxample, monthly, quarterly or yearly factors).

- *Residual component*: This componnet represents an irregular or noisy component. This component describes random fluctuation in the data, which are not captured by the other components. In general, this is the residual of the time series, that is, once the other components have bee removed.

The decomposition is *additive* if the following holds:

<img src="https://latex.codecogs.com/gif.latex?Y_t%20%3D%20T_t%20&plus;%20S_t%20&plus;%20R_t" />

The decomposition is *multiplicative* if,

<img src="https://latex.codecogs.com/gif.latex?Y_t%20%3D%20T_t%20%5Ccdot%20S_t%20%5Ccdot%20R_t " /> 

![](figs/registered_decomposition.png)
<p align="center"><sub>Decomposition for registered rides</sub></p>

![](figs/casual_decomposition.png)
<p align="center"><sub>Decomposition for casual rides</sub></p>

Testing residual for stationarity:

![](figs/registered_resid.png)
<p align="center"><sub>Testing residuals for stationarity for registered rides</sub></p>

![](figs/casual_resid.png)
<p align="center"><sub>Testing residuals for stationarity for casual rides</sub></p>

The residuals still satisfy the stationarity tests.

### Modelling Time Series

A common approach to modelling a time series is to assume that past observations somehow influence future ones. For instnace, customers who are satisfied by using the bike sharing service will more likely recommend it, producing, in this way, a positive impact on the service and a higher number of customers (any negative feedback has the opposite effect, reducing the number of customers). 

Hence, increasing the number of customers and the quality of the service increases the number of recommendations and, therefore, then number of new customers. In this way, a positive feedback loop is created, in which the current number of rides correlates with its past values.

#### ARIMA MODELS
**Autoregressive Integrated Moving Average (ARIMA)** models are a class of statistical models that try to explain the behaviour of a time series using its own past values. 

Being a class of models, ARIMA models are defined by a set of parameters **(p,d,q)** each one corresponsding to a different component of the ARIMA model.

<...snip...>

In practice, the ACF measures the complete correlation between the current entry, **{ $Y_t$ }** and its past entries, lagged by **k**. Note that when computing the ACF(k), the correlation between **$Y_t$** with all intermediate values (**$Y_{t-1},...,Y_{t-k}$)**, the PACF is often referred to, which only measures the impact of **$Y_{t-k}$** on $Y_t$

ACF and PACF are, in general, used to determin the order of integration when modelling a time series with an ARIMA model. For each lag, the correlation coefficient and level of significance are computed. In general, we aim at an integrated series, in which only the first few lags have correlation greater than the level of significance.