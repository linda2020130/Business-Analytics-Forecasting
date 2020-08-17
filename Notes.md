# Data Analysis in Time Series

  1. Time Series Components
  2. Visualizing Time Series
  3. Data Preprocessing
  4. Data Partitioning
  5. Forecasting and Evaluation
      1. Naive Forecast
      2. Time Series
          1. Moving Average(MA)
          2. Autoregression(AR)
          3. Autoregressive Moving Average(ARMA)
          4. Autoregressive Integrated Moving Average(ARIMA)
      3. Exponential Smoothing
          1. Simple Exponential Smoothing(SES)
          2. Holt's Exponential Smoothing
          3. Holt-Winter's Exponential Smoothing
      4. Regression
          1. Linear Regression
          2. Exponential Regression
          3. Polynomial Regression

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

1. **Simplest form**: A line chart of the series **values** `y1, y2, ...` over **time** `t = 1, 2, ...` with temporal labels (e.g. calendar date) on the horizontal axis.
2. **Zooming in**: To a **shorter** period within thr series can reveal patterns.
3. **Changing the scale**: e.g. To change the vertical scale `y` to a **logarithmic** scale.
4. **Adding trend lines**: By trying different trend lines, one can see what type of trend best approximates the data.
5. **Suppressing seasonality**: Easier to see trends when seasonality is suppressed.
    * Methods include: 
        1. Aggregating monthly data into years.
        2. Plot seperate time plots for each season.
        3. Use moving average plots

<br >

## Data Preprocessing

1. **Missing values**: 
    * Imputation approach:
        1. Averaging neighboring values.
        2. Creating forecasts of missing values using earlier values or external data.
    * Note that *Time series* and *Smoothing models* **cannot** apply to data with missing values, but *Regression models* and *Neural network* **can**.
2. **Unequally spaced series**: 
    * Same solutions as missing values.
3. **Extreme values**: 
    * Possible approaches:
        1. Remove/Replace extreme values.
        2. Generate two sets of forecasting (one with extreme values and the other without).
4. **Choice of time span**: 
    * Concerns:
        1. Too short: limit of data information.
        2. Too long: scenario/situation may change a lot when time span is long.

<br>

## Data Partitioning

1. Split the data into **training** `t = 1, 2, ..., n` and **validation** `t = n+1, n+2, ...`sets to avoid overfitting.
2. Select the model with minimal error on the validation set.
3. Use the selected model to rerun on the whole data and see how that works.
4. Apply final ideal model to forecast future values.

<br>

* How to choose the validation period? ==> Depends on **forecast goal**, **data frequency**, and **forecast horizon**.
* Note that **cross validation** (creating multiple training-validation partitions) is often used to avoid bias.

<br >

## Forecasting and Evaluation




