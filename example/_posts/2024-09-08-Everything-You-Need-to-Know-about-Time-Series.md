---
layout: post
description: >
  This guide severs as a comprehensive container for various key topics in time series analysis. It highlights only the essential concepts and headings you need to be familiar with before addressing a time series problem. If you're unsure about any topic, you can easily google it and look it up for further details.
image:
  path: /assets/img/blog/timeseries.jpg
excerpt_separator: <!--more-->
sitemap: false
---

<!--more-->

## Introduction

This guide severs as a comprehensive container for various key topics in time series analysis. It highlights only the essential concepts and headings you need to be familiar with before addressing a time series problem. If you're unsure about any topic, you can easily google it and look it up for further details.

## Definition

Time Series is a series of observations taken at specified time intervals usually equal intervals.

## Resampling

- Upsampling - Time series is resampled from low frequency to high frequency (Monthly to daily frequency). It involves filling or interpolating missing data
- Downsampling - Time series is resampled from high frequency to low frequency (Weekly to monthly frequency). It involves aggregation of existing data.

## **Handling Missing Values**

- Fill NaN with Mean Value
- Fill NaN with Last Value with .ffill()
- Fill NaN with Linearly Interpolated Value
- Seasonal Interpolation: For data with seasonality, use seasonal patterns to fill in gaps.
- Imputation with models: Use time series models (e.g., ARIMA, Kalman filters) to predict missing values based on surrounding data.

## **Identifying outliers**

- Statistical techniques like Z-scores.
- Other techniques: see the last section of the guide.

## **Comparing two time series**

To compare two time series by normalizing them such that they both start with the same value. Dividing them with the initial value and then multiply it by a reference value.

## **Autocorrelation**

The autocorrelation function (ACF) measures how a series is correlated with itself at different lags. Lag: The delay between a value and its previous value(s). Time series models like ARIMA depend on choosing the correct number of lags. Plotting ACF helps identify appropriate lags. If a series is significantly autocorrelated, that means, the previous values of the series (lags) may be helpful in predicting the current value.

## **Partial Autocorrelation**

The partial autocorrelation function can be interpreted as a regression of the series against its past lags. The terms can be interpreted the same way as a standard linear regression, that is the contribution of a change in that particular lag while holding others constant.

## **Components of a time series**

- Trend - Consistent upwards or downwards slope of a time series
- Seasonality - Clear periodic pattern of a time series (like sine function)
- Noise - Outliers or missing values. White noise has Constant mean, variance, and zero auto-correlation at all lags
- Cyclicity - behavior that repeats itself after large interval of time, like months, years etc. It happens when the rise and fall pattern in the series does not happen in fixed calendar-based intervals.

## **Modeling time series**

- Additive time series: Value = Base Level + Trend + Seasonality + Error
- Multiplicative Time Series: Value = Base Level x Trend x Seasonality x Error
  If seasonality tends to be constant over time, the additive model is suggestive. If the seasonality seems to increase with time then the multiplicative model is suggestive.

## **Random walk**

A random walk is a mathematical object, known as a stochastic or random process, that describes a path that consists of a succession of random steps on some mathematical space such as the integers.
Pt = Pt-1 + εt. Example: Today's Price = Yesterday's Price + Noise
Random walks can't be forecasted because well, noise is random.

## **Stationary**

A stationary time series is one whose statistical properties such as mean, variance, autocorrelation, etc. are all constant over time. So, the values are independent of time. Stationarity is important as non-stationary series that depend on time have too many parameters to account for when modelling the time series.

## **Tests to check if a time serie is stationary or not**

Rolling Statistics - Plot the moving avg or moving standard deviation to see if it varies with time. It’s a visual technique.
ADCF Test - Augmented Dickey–Fuller test is used to gives us various values (Test Statistics & some critical values for some confidence levels) that can help in identifying stationarity. The Null hypothesis says that a TS is non-stationary. The Null Hypothesis to be rejected and accepting that the time series is stationary, there are 2 requirements:

1. Critical Value (5%) > Test Statistic
2. p-value < 0.05

## **Transformations from non- stationary to stationary**

- Differencing the Series (once or more): If the autocorrelations are positive for many numbers of lags (10 or more), then the series needs further differencing. On the other hand, if the lag 1 autocorrelation itself is too negative, then the series is probably over-differenced.
- Take the log of the series
- Take the nth root of the series
- Combination of the above
- Exponential decay

## **Granger causality test**

It is used to determine if one time series will be useful to forecast another.

## **Facebook Prophet**

A tool for forecasting time series with trends and seasonality, particularly for irregular data.

## **ARIMA**

ARIMA (Auto Regressive Integrated Moving Average) is a combination of 2 models AR(Auto Regressive) & MA(Moving Average). It has 3 hyperparameters: P(auto regressive lags),d(order of differentiation),Q(moving avg.). The AR part is correlation between prev & current time periods. AR is a model that uses the dependency between an observation and a residual error from a moving average model applied to lagged observations. The I part binds together the AR & MA parts. Basically, Today's return = mean + Yesterday's return + noise + yesterday's noise. It does not support seasonality.

## **SARIMA**

SARIMA models are useful for modeling seasonal time series, in which the mean and other statistics for a given season are not stationary across the years. The SARIMA model defined constitutes a straightforward extension of the nonseasonal autoregressive-moving average (ARMA) and autoregressive integrated moving average (ARIMA) models presented.

## **Find value of P & Q for ARIMA**

We need to take help of ACF(Auto Correlation Function) & PACF(Partial Auto Correlation Function) plots. ACF & PACF graphs are used to find value of P & Q for ARIMA. We need to check, for which value in x-axis, graph line drops to 0 in y-axis for 1st time.
From PACF(at y=0), get P
From ACF(at y=0), get Q

## **Other Models**

- Transformers with time series: [Link for Transformers with Time Series]
- Regression after extracting features from the date:

1. Rolling Statistics: Calculate rolling mean, median, or standard deviation over a moving window to capture trends and variations.
2. Lagged Variables: Create lagged versions of your time series to capture historical patterns and correlations.
3. Fourier Transforms: Convert time-domain data into frequency-domain data using Fourier transforms to identify periodic patterns.
4. ‘day_of_week', 'month', 'quarter', 'year'…
5. LSTM

## **Accuracy metrics**

- Mean Absolute Percentage Error (MAPE)
- Mean Error (ME)
- Mean Absolute Error (MAE)
- Mean Percentage Error (MPE)
- Root Mean Squared Error (RMSE)

## **Univariate vs Multivariate**

Univariate Time Series: Models are trained on a single variable

Multivariate Time Series: Involves multiple variables. The model learns from the interactions between these variables to improve predictions. Capture both autocorrelations and inter-variable correlations. Models to be used: VAR (Vector Autoregressive Models), LSTM, Transformers models. Multi variate time series, a special Time series approach:

1. The idea is to use a Keras Conv2D (usually used for image analysis) on this time series
2. Prep the data as chunks or buckets of x time steps and y features each
3. Exposing a Conv2D to the above prepped data, which should hopefully make the algorithm task easier

## **Use Case: Anomalies Detection in time series**

<img src="/assets/img/blog/timeseriesanomalies.png" alt="drawing" width="800"/>

Anomalies are data points that deviate significantly from the underlying pattern of the time series. These deviations can be caused by various factors such as sudden events, errors in data collection, or changes in the underlying process.

Moving average and exponential smoothing are techniques used to smooth out noise and fluctuations in time series data. Anomalies can be detected by comparing the observed values to the smoothed values. Sudden deviations between the two may indicate the presence of anomalies. These methods are especially useful for detecting anomalies in data with seasonal patterns.

Anomalies can be identified by comparing the predicted values of a model like ARIMA OR LSTM with the actual values. Significant differences may indicate the presence of anomalies.

We can also use classification after extracting features from the date.

In addition, Anomaly detection in time series data may be accomplished using unsupervised learning approaches like clustering, PCA (Principal Component Analysis), and autoencoders. The autoencoder is an unsupervised neural network that learns to reconstruct its input data by first compressing input data into a lower-dimensional representation and then extending it back to its original dimensions. An autoencoder may be trained on typical time series data to learn a compressed version of the data for anomaly identification. The anomaly score may then be calculated using the reconstruction error between the original and reconstructed data.

[Link for Transformers with Time Series]: https://medium.com/intel-tech/how-to-apply-transformers-to-time-series-models-spacetimeformer-e452f2825d2e
