# Introducing the data 
The data comes from DEFRA’s (Department for Environment, Food and Rural Affairs) Automatic Urban and Rural Network (AURN), which reports the level of nitrogen dioxide (among other pollutants) in the air every hour at 164 different locations. For this analysis, we considered data recorded between November 6, 2018 to November 6, 2022.

The data can be downloaded from [uk-air.defra.gov.uk/data/data_selector_service.](https://uk-air.defra.gov.uk/data/data_selector_service)

# Functional approaches to modelling
Now that we’ve tidied up the data and dealt with the missing values, it’s time to get stuck into the modelling process! First of all, let’s have another look at our data but in a slightly different way:

<img align="centre" src="https://github.com/sydneybandi/air_pollution/blob/main/plots/daily_levels.png">

The nitrogen dioxide levels follow a similar pattern each day, with higher levels around rush hour in the morning and evening. Functional analysis treats the pollution pattern for each day as an observation of a function over time. This allows us to compare the differences between patterns on different days, without complications from the varying pollution levels throughout each day.

We still need to deal with seasonal and trend components of the functional data. For example, since the higher levels of pollution around 8am and 5pm are most likely caused by people travelling to work, this raises the question of whether pollution levels are lower on weekends, when there are generally fewer people travelling to work. Are there different pollution patterns on different days of the week?

<img align="centre"  src="https://github.com/sydneybandi/air_pollution/blob/main/plots/daily_average.png">

Visualising the average daily patterns suggests that pollution patterns on weekdays are generally very similar, with Saturdays and Sundays being lower on average but still different from each other. To test this formally, a functional ANOVA test can be applied. This tests whether the average pollution pattern on a weekday is significantly different from the average pollution pattern on a Saturday or Sunday. The test returns the probability that, if the two pollution patterns were equal, the observed patterns would be this different. For this analysis, the probability was 0. Therefore, we can conclude there is a significant difference in the pollution levels on weekdays compared to Saturdays and Sundays. We can either analyse Saturdays, Sundays, and weekends separately, or fit a model to remove the seasonality and analyse the residuals instead.

A simplified version of the regression model might look something like this:

y_{n} = I_{weekday, n} + I_{Saturday, n} + e_{n}
y 
n
​
 =I 
weekday,n
​
 +I 
Saturday,n
​
 +e 
n
​
 
where I_{weekday, n}I 
weekday,n
​
  is 1 if the n^{th}n 
th
  day is a weekday and 0, otherwise. We can use an analogous method to test if different months follow different daily patterns, and if there is a significant different between years. Interestingly, November, December, and January all exhibit higher level of nitrogen dioxide, particularly in the evening. One possible explanation for why these months have higher levels of nitrogen dioxide is fireworks. Fireworks are commonly set off on Bonfire Night, around Christmas, and New Year’s Eve. Fireworks have been shown to contribute to elevated levels of gaseous pollutants, including nitrogen dioxide.

We can account for all three types of variation (weekday, monthly, and annual) within a functional regression model.

We can then analyse the residuals, that look a little bit like this:

<img align="centre"  src="https://github.com/sydneybandi/air_pollution/blob/main/plots/residuals.png">

# Finding outliers in the data 

To describe how normal (or abnormal) the (residual) pollution pattern for each day is, we calculate the functional depth. A depth measurement attributes a sensible ordering to observations, such that observations which are close to average have higher depth and those far from the average have lower depth. Days which have very low depth can be classified as outliers. Functional depth takes into account the abnormality both in the magnitude (for example, overall higher pollution levels throughout the day), and shape (for example, pollution levels peak earlier in the day than normal) of the pollution pattern.

Now we can take a look at the functional depths, and the associated threshold.

<img align="centre"  src="https://github.com/sydneybandi/air_pollution/blob/main/plots/depths.png">

Much like when inspecting residuals, a scatter plot of functional depths over time should appear random. If there is evidence of seasonality, or trend in the functional depths, we would want to revisit the functional regression model. Here, the depths look alright.

The days classified as outliers, are those that fall below the threshold. In this case, 16 days were classified as outliers. As we would expect, the outliers appear randomly distributed across the period of time the data covers.


