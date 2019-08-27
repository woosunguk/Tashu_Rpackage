
<!-- README.md is generated from README.Rmd. Please edit that file -->
Tashu
=====

This package provides bike rental history and analysis function of public bike system('Tashu') in Daejeon, South Korea.

Installation
------------

Package 'tashudata' should be installed by using 'drat' package before calling analysis function.
tashudata contains some data such as bicycle rental history, weather and bicycle station information for 3 years.
Functions of package 'tashu' uses data on 'tashudata'.

``` r
library(drat)
addRepo("zeee1")
install.packages("tashudata")
```

## 1. Introduction
 The public bicycle system is spreading globally as a green public transportation. In major cities around the world, public bicycle systems are leading to the effects of distribution of public transportation demand and the increase of citizens' exercise.  Bicycle arrangement on time is needed for operating this system smoothly. To know time when bicycle would be moved, Analyzing citizens' bicycle rental history is neccessary.
This package provides real bicycle rental/return history which has been recorded on public bicycle system(called 'Tashu') in Daejeon city, South Korea for 3 years, external data affecting bicycle rental(such as weather) and analyzing functions. Through this package, Users can analyze citizens' bicycle rental/return pattern free.


## 2. Data
1.    Tashu Data
: Bicycle rental history on Tashu from 2013 to 2015. Data is consisted of "RENT STATION, RENT DATE, RETURN STATION, RETURN DATE"
* tashu : Rental/Return record from 2013 to 2015.

```{r}
head(tashudata::tashu, n = 5)
```

2.    Tashu Station Data
: By 2014 there were 144 bicycle stations on "Tashu" system. In this package, only the existing data of 144 stations are considered. Data is consisted of "NUM(Number of station), NUMOFBIKE_RACK(Number of Bike rack), GEODATA_lat(Latitude of station), GEODATA_lon(Longitude of station)"

* tashuStationData : data of 144 Stations on Tashu system. 
```{r}
head(tashudata::tashuStation, n = 5)
```

3.    Weather Data
: Weather Data In Daejeon from 2013 to 2015. Data is consisted of "datetime, temperature, rainfall, windspeed, wind direction, humidity, Dew point temperature, Local Pressure, Sea surface Pressure, Sunshine, Solar Radiation, Snowfall, Ground Temperature"

* weather : weather data in Daejeon from 2013 to 2015

```{r}
head(tashudata::weather, n = 5)
```

## 3. Analysis function

This package provides some functions that analyze bicycle rental/return history. We implements functions showing popular top 10 stations and paths and change of rental amount according to day and month.

1.    top10_stations()
: This function presents top 10 stations that have most number of bike rental for 3 years.

```{r, fig.width=7,fig.height=8}
library(tashu)
top10_stations()
```

2.    top10_paths()
: This function presents top 10 paths that have mostly used for 3 years.

```{r, fig.width=7,fig.height=8}
library(tashu)
top10_paths()
```

3.    daily_bike_rental()
: This function shows average amount of bike rental in each months.

```{r, fig.width=7,fig.height=8}
library(tashu)
daily_bike_rental()
```

4.    monthly_bike_rental()
: This function shows bike usage pattern in each day of week.

```{r, fig.width=7,fig.height=8}
library(tashu)
monthly_bike_rental()
```


## 4. Prediction stations

 This package provides functions that create train/test dataset and predict bicycle rental amount. Bicycle rental/return history and weather data would be preprocessed to train/test dataset for prediction by create_train_dataset() and create_test_dataset(). After creating train/test dataset, Users can create prediction model(create_train_model()) and predict hourly rental amount of test dataset(predict_bike_rental()). 

1.    create_train_dataset(station_number)
: This function generates training dataset for the random forest prediction model. This function preprocesses the bicycle rental history and weather data so that compute hourly rental amount on "station_number" station from 2013 to 2014. The example source code is below.

```{r}
library(tashu)
train_dataset <- create_train_dataset(1)
head(train_dataset, n = 5)
```

2.    createTestData(station_number)
: This function generates test dataset that would be used for prediction. Bicycle rental history at the "station_number" station and weather in 2015 are used in this function.
 
```{r}
library(tashu)
test_dataset <- create_test_dataset(1)
head(test_dataset, n = 5)
```

3.    create_train_model(trainData, testData, isImportance, numOfTree, type)
: This function takes trainData and testData as parameters and performs prediction about amount of bike rental at stationNum stop. First, It implements the random forest prediction model with trainData and input testData to predict the demand of bike rental in 2015. And below source code is example.
 
```{r}
library(tashu)
rf_model <- create_train_model(train_dataset)
```

4.    predict_bike_rental(rf_model, test_dataset)
: This function predict amount of bicycle rental in 2015 by using rf_model trained with dataset in 2013, 2014. 

```{r}
library(tashu)
predict_result <- predict_bike_rental(rf_model, test_dataset)
head(predict_result, n = 5)
```
