# Quantitative Stock Analysis & Volatility Modeling (PayPal)

**Project Status:** Completed (Oct 2025)
**Tech Stack:** Python, Statsmodels, ARCH, TensorFlow, Scikit-learn

## 1. Executive Summary
This project performs a comprehensive quantitative analysis of PayPal (PYPL) stock over a 5-year period (2020-2025). The goal was to determine the predictability of stock prices versus stock volatility using advanced time-series techniques.

Using a rigorous statistical workflow, I demonstrated that while PYPL's price follows a **Random Walk** (making directional forecasting ineffective), its volatility is **highly predictable** and persistent. I further tested for asymmetric volatility (the "Leverage Effect") using a GJR-GARCH model and found it to be statistically insignificant, concluding that a symmetric GARCH(1,1) model is the optimal risk tool for this asset.

## 2. Key Findings

### Finding #1: Price is a Random Walk (The "Baseline")
I established a baseline using an **ARIMA(0,1,0)** model.
* **Result:** The stock price is non-stationary. Returns are stationary but show zero autocorrelation.
* **Implication:** Historical price data contains no predictive signal for future price direction, consistent with the **Efficient Market Hypothesis (EMH)**.
* **Baseline RMSE:** $9.17

### Finding #2: Deep Learning Failed to Beat the Baseline
To test if non-linear patterns existed, I trained a multivariate **LSTM (Long Short-Term Memory)** neural network using additional features (NASDAQ, 10-Yr Treasury Yields).
* **Result:** The LSTM failed to generalize to the test set (RMSE: $26.22), performing significantly worse than the naive baseline.
* **Conclusion:** Complexity does not equal accuracy in efficient markets.

### Finding #3: Volatility is Predictable & Symmetric
While price direction is random, the *magnitude* of returns (risk) is not. I modeled this using **GARCH** methodology.
* **GARCH(1,1):** Proved that volatility is statistically persistent (clustering).
* **GJR-GARCH (The Advanced Test):** I tested for the "Leverage Effect"—the hypothesis that bad news causes higher volatility than good news.
    * **Result:** The asymmetry coefficient (`gamma[1]`) had a p-value of **0.498** (not significant).
    * **Conclusion:** PayPal's volatility response is **symmetric**. The simpler GARCH(1,1) model is the superior choice for risk modeling (Occam's Razor).

## 3. Visualizations

### Volatility Clustering (GARCH Model)
*The red line shows the model's risk estimate reacting to market turbulence.*
![GARCH Volatility Model](images/pypl_garch_volatility.jpg)

### The "Random Walk" Proof
*The LSTM (green) attempts to find a smooth trend, failing to capture real-world chaos (orange).*
![LSTM Forecast vs Actual](images/pypl_lstm_forecast.png)

### Asymmetric Volatility Test (GJR-GARCH)
*The GJR-GARCH model (red) closely tracks the GARCH model, confirming symmetry.*
![GJR-GARCH Volatility](images/pypl_gjr_garch_volatility.jpg)

## 4. Project Structure
* `analysis.ipynb`: The core notebook containing the end-to-end analysis (Data Acquisition, EDA, ARIMA, LSTM, GARCH, GJR-GARCH).
* `images/`: Folder containing all generated plots and charts.
* `requirements.txt`: List of Python dependencies for reproducibility.

## 5. How to Run
1.  Clone the repo:
    ```bash
    git clone [https://github.com/davidagbaje/Stock-Volatility-Prediction.git](https://github.com/davidagbaje/Stock-Volatility-Prediction.git)
    ```
2.  Install dependencies:
    ```bash
    pip install -r requirements.txt
    ```
3.  Open `analysis.ipynb` in Jupyter or VS Code to run the analysis.