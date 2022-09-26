# Bike Sharing Analysis

Bike sharing is a fundamenta service, commonly used in the urban mobility sector. it is easily accessible (as no driving license is required to ride a bike), cheaper then normal car sharing service (since bike maintenance and insustance are substantially cheaper than automible ones), and, finally, is often a fast way to commute within the city. Therefore, understanding the driving factors of bike sharing requests is essenential for both companies and users.

From a company's perspeqctive, identifying the expected bike demand in a specific area, within a specific time frame, can significantly increase revenue and customer satisfaction. Moreover, bike relocation can be optimized to futher reduce operational costs. From a user's perwspective, probable the most important factor is bike availability in the shortest wait time, which is obviously in alignment with the company's interests.

## Project Objective
In this project, I will analyze bike sharing data from Capital Bikeshare Washington, D.C., USA, for the period between January 1, 2011, and December 21, 2012. The data is aggregated on an hourly basis. This means that no initial and final locations of the individual rides are available, but only the total number of rides per hour. Nevertheless, additional meteorological information is available in the data, which could serve as a driving factor for identifying the total number of requests for a specific time frame (bad weather conditions could have a substantial impact on bike sharing demand).

Although the conducted analysis is related to bike sharing, the provided techniques could be easily transferred to other types of sharing business models, such as car or scooter sharing.

## Dataset
The original dataset is available at https://archive.ics.uci.edu/ml/datasets/Bike+Sharing+Dataset#.

According to the description of the original data, provided in the [Readme.txt](data/README.txt), the columns can be split into three main groups:

- **temporal features**: This contains information about the time at which the record was registered. This group contains the **dteday, season, yr, mnth, hr, holiday, weekday** and **workingday** columns.
- **weather related features**: This contains information about the weather conditions. The **weathersit, temp, atemp, hum** and **windspeed** columns are included in this group.
- **record related features**: This contains information about the number of records for the specific hour and date. This group included the **casual, registered** and **cnt** columns.

> Note: **instant** wasn't included in any of the aforementioned groups. The reason for this is that it is an idex column and will be exluded from the analysis, as it does not contain any information relevant to this analysis.

### Preprocessing Details
1. `seasons`: each of the four divisions of the year
    - `1`: Windter
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


![](figs/rides_daily.png)
<p align="center"><sub>Number of rides daily for registered and casual customers</sub</p>

- The number of registered rides is always significantly higher than the number of casual rides per day.
- During the winter, the overal number of rides decreases (as bad weather and low tempreatures have a negative impact on ride sharing services)

