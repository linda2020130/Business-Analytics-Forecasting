# Data Analysis and Forecasting in Time Series

  1. [Time Series Analysis and Forecasting Procedure](#procedure)
  2. [Time Series Components](#components)
  3. [Visualizing Time Series](#visualizing)
  4. [Data Preprocessing](#preprocessing)
  5. [Data Partitioning](#partitioning)
  6. [Forecasting Models](#forecasting)
      1. [Naive Forecast](#naive)
      2. [Regression](#regression)
          1. [Moving Average(MA)](#MA)
          2. [Autoregressive(AR)](#AR)
          3. [Autoregressive Moving Average(ARMA)](#ARMA)
          4. [Autoregressive Integrated Moving Average(ARIMA)](#ARIMA)
          5. [Others(Linear/Exponential/Polynomial)](#others)
      3. Exponential Smoothing
          1. Simple Exponential Smoothing(SES)
          2. Holt's Exponential Smoothing
          3. Holt-Winter's Exponential Smoothing
  7. [Evaluation](#evaluation)

<br>
<br>

<h2 id="procedure">1. Time Series Analysis and Forecasting Procedure</h2>
        
* Step 1 : Get time series data
* Step 2 : Partition into training and validation
* Step 3 : Estimate the model on training (visualizing --> preprocessing --> modeling)
* Step 4 : Evaluate predictive performance
* Step 5 : Generate forecasts for future periods

<br>

<h2 id="components">2. Time Series Components</h2>

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
--> vary by a constant amount


* A time series with *__multiplicative components__* can be written as:
```
yt = Level * Trend * Seasonality * Noise
```
--> vary by a percentage

<br>

<h2 id="visualizing">3. Visualizing Time Series</h2>

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

<h2 id="preprocessing">4. Data Preprocessing</h2>

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
5. **Differencing**:
    * Difference of two time periods in the time series; to remove trend and/or seasonality from a series.
    * Note that differencing is often used as a pre-processing step before applying a forecasting model to a series.
    * Types of Differencing:
        1. **Lag-1 differencing**(De-Trending): useful for removing trend. (May apply twice if the trend is exponential)
        2. **Lag-M differencing**(Deseasonalizing): useful for removing seasonality with M seasons. 
        3. **Double-Differencing**: perform the differencing operation twice(difference the differenced series). (e.g. Lag-M first, then Lag-1)

<br>

<h2 id="partitioning">5. Data Partitioning</h2>

1. Split the data into **training** `t = 1, 2, ..., n` and **validation** `t = n+1, n+2, ...`sets to avoid overfitting.
2. Select the model with minimal error on the validation set.
3. Use the selected model to rerun on the whole data and see how that works.
4. Apply final ideal model to forecast future values.

<br>

* How to choose the validation period? ==> Depends on **forecast goal**, **data frequency**, and **forecast horizon**.
* Note that **cross validation** (creating multiple training-validation partitions) is often used to avoid bias.

<br >

<h2 id="forecasting">6. Forecasting</h2>

<h3 id="naive">i. Naive Forecast</h3>

* Usage:
  1. **Short forecasting horizons**: Naive Forecast is simple and often accurate for this scenario.
  2. **Benchmark**: We should always compute them for comparison against other more complex forecasting models.
* Formula:
  1. **No seasonality**: Use latest value to forecast "all" future forecasting values. `F_t+k = yt`
  2. **With seasonality**: Use value of latest season to forecast future season. `F_t+k = y_t-M+k`
  
<br>

<h3 id="regression">ii. Regression</h3>

<h3 id="MA">a. Moving Average(MA)</h3>

> The label "moving average" is technically incorrect since the MA coefficients may be negative and may not sum to unity. This label is used by convention.

* Assumption: Demand is stable(no trend and seasonality)
* Usage: 
    1. **Time Series Visualization**: to identify time series components by supressing seasonality and noise.
    2. **Forecasting**: to forecast time series that do not have trend and seasonality.
    3. **Second-layer model**: to forecast errors, usually in ARMA or ARIMA model.
* Argument: Order(q) --> Width of window
* Formula: 
```
y_t = μ + θ_1 * ε_t-1 + θ_2 * ε_t-2 + ... + θ_q * ε_t-q + ε_t

μ: average of the series
θ_1, θ_2, ... , θ_q: parameters of the model (can be negative and need not sum to unity!!!)
ε_t, ε_t-1, ..., ε_t-q: white noise error terms
```

#### Two Types of Windows

1. **Centered Moving Average**: for *time series visualization*; based on a window centered around time t.
```
Example:

Odd width (e.g. w=5):
MA_t = μ + θ_2 * y_t-2 + θ_1 * y_t-1 + θ_0 * y_t + θ_-1 * y_t+1 + θ_-2 * y_t+2

Even width (e.g. w=4):
MA_t = μ + 1/2 X [(θ_2 * y_t-2 + θ_1 * y_t-1 + θ_0 * y_t + θ_-1 * y_t+1) + (θ_1 * y_t-1 + θ_0 * y_t + θ_-1 * y_t+1 + θ_-2 * y_t+2)]
```

2. **Trailing Moving Average**: for *forecasting*; based on a window from time t and backwards.
```
Since we cannot use data in validation period to forecast itself, 
the forecasted value in the validation period will be the average of last w time periods in training period.

Example:

y_t+1, y_t+2, ... = θ_w+1 * y_t-w-1 + ... + θ_1 * y_t-1 + θ_0 * y_t
```

#### Width of Window

To decide the degree of smoothing (How much smoothing do we want?).
1. Longest window(over-smoothing):
    * w = length of training period
    * MA = average of entire series
2. Shortest window(under-smoothing):
    * w = 1
    * MA = naive forecast

<br>

<h3 id="AR">b. Autoregressive(AR)</h3>

> We forecast the variable of interest (e.g. y_t or e_t)using a linear combination of past p values of the variable. 
The term autoregression indicates that it is a regression of the variable against itself.

* Assumption: Demand is stable(no trend/seasonality)
* Usage:
    1. **Forecasting**: to forecast time series that do not have trend and seasonality.
    2. **Second-layer model**: to capture autocorrelation by constructing a second-layer forecasting model to forecast errors.
* Argument: Order(p)
* Formula: 
```
AR(p) only useful for improving forecasts in the next p periods.

y_t = C + β_1 * y_t-1 + β_2 * y_t-2 + ... + β_p * y_t-p + ε_t

C: constant
β_1, β_2, ... , β_p: parameters of the model
ε_t: white noise
```

#### Two-Step Model

1. Use *any forecasting model* (e.g. regression, smoothing, ...) to generate F_t
2. Use *AR model* to generate e_t (input actual errors and output forecasted errors) --> Usually use AR(1) is good enough
3. Combine the two to get an improved forecast `F*_t+1 = F_t+1 + e_t+1`

<br>

<h3 id="ARMA">c. Autoregressive Moving Average(ARMA)</h3>

> The ARMA model is a tool for predicting future values in this series. The AR part involves regressing the variable on its own lagged (i.e., past) values. The MA part involves modeling the error term as a linear combination of error terms occurring contemporaneously and at various times in the past.

* Assumption: Demand is stable(no trend and seasonality)
* Usage: 
    1. **Forecasting**: to forecast time series that do not have trend and seasonality.
* Argument: Order(p, q) --> p for AR, q for MA
* Formula:
```
y_t = C + ε_t + (β_1 * y_t-1 + β_2 * y_t-2 + ... + β_p * y_t-p) + (θ_1 * ε_t-1 + θ_2 * ε_t-2 + ... + θ_q * ε_t-q)

C: constant
β_1, β_2, ... , β_p, θ_1, θ_2, ... , θ_q: parameters of the model(can be negative and need not sum to unity!!!)
ε_t, ε_t-1, ..., ε_t-q: white noise error terms
```

<br>

<h3 id="ARIMA">d. Autoregressive Integrated Moving Average(ARIMA)</h3>

> The ARIMA model can be thought as a particular case of an ARMA(p+d,q) process having the autoregressive polynomial with d unit roots.

* Assumption: Demand can be stable or unstable but can be applied differencing to eliminate the non-stationarity.
* Usage:
    1. **Forecasting**: to forecast time series where data show evidence of non-stationarity.
* Argument: Order(p, d, q) --> p for AR, d for differencing, q for MA
* Formula:
```
[1 - (β_1 * L + β_2 * L^2 + ... + β_p * L^p)] * (1 - L)^d * y_t = [1 + (θ_1 * L + θ_2 * L^2 + ... + θ_q * L^q)] * ε_t

L: lag operator --> L * y_t = y_t-1
d: d-order differencing
β_i: autoregression parameter
θ_i: moving average parameter
```

#### Autocorrelation

Autocorrelation is the *linear correlation between **the series** and **a lagged version of itself***(e.g. Lag-1: Take last period as current period).
Computing the autocorrelation of the errors can be used to *check if trend and seasonality are captured correctly*.
(e.g. strong positive lag-1 in autocorrelation of errors --> trend wasn't captured)

```
r = [(X1 - avg_X)(Y1 - avg_Y) + ... + (Xn - avg_X)(Yn - avg_Y)] 
    / {[(X1 - avg_X)^2 + ... + (Xn - avg_X)^2]^(1/2) * [(Y1 - avg_Y)^2 + ... + (Yn - avg_Y)^2]^(1/2)}
```

1. Strong autocorrelation (positive or negative): can identify a cyclical pattern 
  * e.g. strong autocorrelation at lag-12, lag-24, lag-36 in monthly data --> an annual seasonality
2. Positive lag-1 autocorrelation (stickness)
  * Series with strong linear trend tends to have strong lag-1 autocorrelation
3. Negative lag-1 autocorrelation (swings)
  * High value comes before and after low value and vice versa.
  
#### Choosing d (Differencing)

1. Rule 1: High positive autocorrelation at lag-k where k is large --> Need larger d (Be careful of overdifferencing!)
2. Rule 2: Autocorrelation at lag-1 is 0 or negative or autocorrelation at all lags are small and patternless --> No more differencing
3. Rule 3: Optimal d usually makes RMSE the smallest
4. Rule 4: 
  * d=0 --> original series is stationary
  * d=1 --> original series has a constant average trend
  * d=2 --> original series has a time-varying trend

#### Choosing p, q (AR, MA)

Use **ACF**(*Autocorrelation Function*) and **PACF**(*Partial Autocorrelation Function*) to decide values of p and q. The **partial autocorrelation** at lag-k is the autocorrelation between y_t and y_t-k that is not accounted for by lag-1 through lag-k-1.

1. **ACF plot**: a bar chart of the correlation coefficients between the time series and the lags of itself.
  * Rule-of-thumb for choosing **q**: ACF plot has a sharp peak or autocorrelation at lag-1 is negative --> Consider adding a **MA** term to the model
  * The lag at which the ACF cuts off is the indicated number of MA terms
2. **PACF plot**: a bar chart of the partial correlation coefficients between the time series and the lags of itself.
  * Rule-of-thumb for choosing **p**: PACF plot has a sharp peak or autocorrelation at lag-1 is positive --> Consider adding an **AR** term to the model
  * The lag at which the PACF cuts off is the indicated number of AR terms
  
<br>

<h3 id="others">e. Others(Linear/Exponential/Polynomial)</h3>
  
* Assumption: Demand needs to be deseasonalized first
* Usage:
    1. **Time Series Analysis**: to identify/capture the trend of the time series.
    2. **Time Series Forecasting**: to forecast time series where data show evidence of trend.
* Argument: predictor(s) is t or functions of t
* Formula:
  * **Linear**: `y_t = β_0 + β_1 * t + ε`
  * **Exponential**: `y_t = β_0 * e^(β_1 * t) * ε` --> `Log(y_t) = β_0 + β_1 * t + ε`
  * **Polynomial**: e.g. `y_t = β_0 + β_1 * t + β_2 * t^2 + ε`

<br>

<h2 id="evaluation">7. Evaluation</h2>

It is recommended to use **charts** to evaluate how the model performs on both *training period* and *validation period*.
1. Line Chart: Actual values and forecasting values vs time periods(training + validation).
2. Line Chart: Forecasting errors vs time periods.
3. Histogram: Frequency of each error range; more info on forecast errors of different sign and size.

### Any error that is less preferable?

1. **One large** error vs **Many small** errors
2. Errors **above/below threshold**
3. **Positive** errors(under-forecast) vs **Negative** errors(over-forecast)

### Common Predictive Accuracy Measures

1. **et**: Average error; Values with different sign will offset each other.
2. **|et|**: Mean Absolute Error(MAE) or Mean Absolute Deviation(MAD); Doesn't matter sign, only care about size.
3. **(et)^2**: Mean Squared Error(MSE); Larger error gets larger penalty.
4. **((et)^2)^(1/2)**: Root Mean Squared Error(RMSE).
5. **|et/yt|x100%**: Mean Absolute Percentage Error(MAPE); Can't apply to series with actual value equals to 0.
