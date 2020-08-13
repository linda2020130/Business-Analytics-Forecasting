# Data Analysis in Time Series

  1. Time Series Components
  2. Visualizing Time Series
  3. Data Preprocessing
  4. Data Partitioning
  5. Forecast Methods<br>
      5.1 Naive Forecast<br>
      5.2 Moving Average<br>
      5.3 Exponential Smoothing<br>
          5.3.1 Simple Exponential Smoothing(SES)<br>
          5.3.2 Holt's Exponential Smoothing<br>
          5.3.3 Holt-Winter's Exponential Smoothing<br>
      5.4 Regression<br>
          

<br>
<br>

## Time Series Components
1. Systematic part:
    * **level**: the average value of the series
    * **trend**: change in the series from one period to the next
    * **seasonality**: a shorterm cyclical behavior that can be observed several times within the given series
2. Non-systematic part: 
    * **noise**: random variation that results from measurement error or other causes that are not accounted for

### Examples

* A time series with *__additive components__* can be written as:
```
yt = Level + Trend + Seasonality + Noise
```
=> vary by a constant amount


* A time series with *__multiplicative components__* can be written as:
```
yt = Level * Trend * Seasonality * Noise
```
=> vary by a percentage

<br>

## Visualizing Time Series
