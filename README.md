# Stock-Volatility-Prediction

Executive Summary
This project performed a comprehensive quantitative analysis of PayPal (PYPL) stock, using 5 years of market data (Nov 2020 - Nov 2025). The analysis followed a professional "quant" workflow, moving from baseline statistical models to advanced machine learning.

The key findings are:

Price is a "Random Walk": The stock price itself is non-stationary and follows a Random Walk, as confirmed by an Augmented Dickey-Fuller (ADF) test.

Baseline Model (ARIMA): A baseline ARIMA(0,1,0) model (which functionally predicts "today's price is the best guess for tomorrow's price") was established, yielding a Root Mean Squared Error (RMSE) of $9.17.

Volatility is Predictable: While price direction is random, price magnitude (volatility) is not. The returns exhibit significant volatility clustering (heteroskedasticity), which was successfully modeled with a GARCH(1,1) model. This is the most valuable finding for risk management.

Advanced Model (LSTM): A complex, multivariate Long Short-Term Memory (LSTM) neural network was built to try and beat the baseline. This model used PYPL's returns, NASDAQ (^IXIC) returns, and 10-Yr Treasury (^TNX) returns as inputs.

Final Conclusion: The advanced LSTM model failed to beat the baseline (RMSE: $26.22), reinforcing the Efficient Market Hypothesis (EMH). The analysis concluded that for public data on a liquid stock, predicting risk (volatility) is a far more viable and useful endeavor than predicting price.

Phase 1: Data Acquisition & Exploratory Data Analysis (EDA)

The initial phase involved acquiring 5 years of daily price data for PYPL.

Finding 1: Price is Non-Stationary
The 'Close' price chart shows a clear, non-constant mean and variance. It wanders and does not revert to a central point. This is visually a non-stationary series.

Finding 2: Returns are Stationary
To create a model-ready series, we converted the price data into daily percent changes ("returns"). The returns are stationary, hovering around a constant mean of zero.

Statistical Proof (ADF Test):

Close Price: p-value: 0.698. We fail to reject the null hypothesis; the series is non-stationary.

Daily Returns: p-value: 0.0. We reject the null hypothesis; the series is stationary.

Phase 2: Baseline Price Model - ARIMA(0,1,0)

Before building a complex model, a baseline was established.

ACF/PACF Analysis: Plots for Autocorrelation (ACF) and Partial Autocorrelation (PACF) on the stationary returns showed no significant lags.

Model Choice: This lack of autocorrelation is the classic signature of a Random Walk. The correct statistical model is an ARIMA(0,1,0).

Baseline Score: This model was trained on 80% of the data and tested on the final 20%.

Baseline RMSE: $9.1746

This is our "score to beat." Any advanced model must have a lower price-forecast error to be considered useful.

Phase 3: Volatility Model - GARCH(1,1)

The ARIMA model's summary statistics showed a Prob(H): 0.00, a p-value that strongly indicates the volatility of the returns is not constant (i.e., it's heteroskedastic). We modeled this using GARCH.

Model: A GARCH(1,1) model was fit to the training data.

Finding: The model's parameters (alpha[1] and beta[1]) were highly significant (p < 0.05), proving that volatility clusters.

Conclusion: The plot below shows the GARCH model (red line) successfully capturing periods of high and low risk. This is the project's most practically useful model, as it can be used to forecast risk (e.g., for VaR calculations).

Phase 4: Advanced Multivariate Model - LSTM

The final phase attempted to beat the $9.17 baseline RMSE using a more powerful AI model with more data.

Feature Selection: We analyzed the correlation between PYPL's returns and other market factors.

NASDAQ (^IXIC): 0.62 (Strong positive correlation)

10-Yr Rates (^TNX): -0.44 (Moderate negative correlation)

VIX (^VIX): -0.05 (No correlation; discarded)

Model Architecture: A multivariate LSTM was built to use the past 60 days of PYPL, NASDAQ, and 10-Yr Rate returns to predict the next day's PYPL return.

Final Results: The model was trained and evaluated on the test set.

Baseline ARIMA RMSE: $9.17

Advanced LSTM RMSE: $26.22

The complex model was significantly worse than the simple "random walk" baseline. The plot below shows why: the LSTM (green) learned a smooth, general trend and was completely unable to predict the chaotic, real-world volatility (orange) of the test period.
