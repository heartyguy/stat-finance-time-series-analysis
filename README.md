# Statistical Finance Time Series Analysis

[![R](https://img.shields.io/badge/R-4.0+-blue.svg)](https://www.r-project.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Finance](https://img.shields.io/badge/Domain-Finance-green.svg)](https://github.com/)

A comprehensive statistical analysis framework for financial time series data, focusing on the S&P 500 Composite Index (^GSPC). This project implements and compares multiple time series forecasting methods including Holt-Winters exponential smoothing, ARIMA models, GARCH volatility models, and regression-based approaches.

## 🎯 Overview

Financial time series analysis is crucial for understanding market behavior, risk assessment, and investment decision-making. This project provides a complete toolkit for analyzing stock market data using both traditional econometric methods and modern statistical techniques.

### Key Features

- **Multiple Forecasting Methods**: Holt-Winters, ARIMA, Moving Averages, and Regression models
- **Volatility Modeling**: GARCH(1,1), GARCH(1,1)-M, and TARCH(1,1) implementations
- **Comprehensive Evaluation**: RMSE, MAE, MAPE, and other accuracy metrics
- **Visual Analysis**: Time series plots, ACF/PACF analysis, and forecast visualization
- **Statistical Testing**: Normality tests, ARCH effects, and model diagnostics
- **Economic Variables**: Integration of macroeconomic indicators (CPI, GDP, unemployment, etc.)

## 📊 Implemented Models

### Time Series Models

| Model | Description | Implementation |
|-------|-------------|----------------|
| **ARIMA(p,d,q)** | Autoregressive Integrated Moving Average | `time_series.ipynb` |
| **Holt-Winters** | Exponential smoothing with trend/seasonality | `smoothing.ipynb` |
| **Moving Averages** | Simple Moving Average (SMA) with various windows | `smoothing.ipynb` |
| **GARCH(1,1)** | Generalized Autoregressive Conditional Heteroskedasticity | *Coming Soon* |
| **TARCH(1,1)** | Threshold ARCH for asymmetric volatility | *Coming Soon* |

### Regression Models

| Model | Description | Implementation |
|-------|-------------|----------------|
| **Linear Regression** | OLS with macroeconomic variables | `regression.ipynb` |
| **Ridge Regression** | L2 regularized regression | `regression.ipynb` |
| **LASSO Regression** | L1 regularized regression | `regression.ipynb` |
| **Subset Selection** | Forward/backward selection methods | `regression.ipynb` |

## 🚀 Quick Start

### Prerequisites

- R 4.0 or higher
- RStudio (recommended)
- Internet connection for data download

### Required R Packages

```r
# Install required packages
install.packages(c(
  "tseries",      # Time series analysis
  "CombMSC",      # Combined model selection criteria
  "zoo",          # Time series objects
  "forecast",     # Forecasting methods
  "TTR",          # Technical trading rules
  "TSA",          # Time series analysis
  "caret",        # Classification and regression training
  "leaps",        # Regression subset selection
  "fBasics"       # Financial basics
))
```

### Quick Setup

1. **Clone the repository**
   ```bash
   git clone https://github.com/your-username/stat-finance-time-series-analysis.git
   cd stat-finance-time-series-analysis
   ```

2. **Open R/RStudio and load libraries**
   ```r
   library(tseries)
   library(CombMSC)
   library(forecast)
   library(zoo)
   ```

3. **Run the analysis**
   - Open any of the Jupyter notebooks or R scripts
   - Execute cells sequentially for complete analysis

## 📈 Data and Analysis

### Dataset

- **Primary Data**: S&P 500 Index (^GSPC) from Yahoo Finance
- **Time Period**: January 2000 - December 2011
- **Frequency**: Daily closing prices
- **Train/Test Split**: ~2,500 training days, 504 test days

### Macroeconomic Variables

The regression analysis incorporates key economic indicators:

- **CPI**: Consumer Price Index
- **GDP**: Gross Domestic Product
- **Unemployment Rate**: Labor market indicator
- **Gold Prices**: Alternative investment metric
- **Oil Prices**: Energy sector indicator
- **Government Debt**: Fiscal policy measure
- **Political Variables**: Party control and election cycles

## 🔬 Usage Examples

### Time Series Analysis

```r
# Load and prepare data
spy <- get.hist.quote(instrument = "^GSPC", 
                     start = "2000-01-01", 
                     end = "2011-12-31",
                     quote = "AdjClose",
                     provider = "yahoo")

# Split into train/test
test_len <- 504
spy_ts <- ts(coredata(spy), frequency = 1)
spy_split <- splitTrainTest(spy_ts, numTrain = length(spy_ts) - test_len)

# Fit ARIMA model
fit_arima <- Arima(spy_split$train, order = c(2,1,2))
forecast_arima <- forecast(fit_arima, h = test_len)

# Evaluate accuracy
accuracy(forecast_arima, spy_split$test)
```

### Holt-Winters Smoothing

```r
# Fit Holt-Winters model
hw_model <- HoltWinters(spy_split$train, beta = FALSE, gamma = FALSE)
hw_forecast <- forecast.HoltWinters(hw_model, h = 504)

# Plot results
plot(hw_forecast)
```

### Regression Analysis

```r
# Load regression data with economic indicators
reg_data <- read.csv("regression_data.csv")

# Fit linear model
lm_model <- lm(AdjClose ~ cpi + debt + gdp + gold + 
               labour + party + election + petrol + unemploy, 
               data = train_data)

# Make predictions
predictions <- predict(lm_model, test_data)
```

## 📊 Model Performance

### Forecasting Accuracy (RMSE on Test Set)

| Model | RMSE | MAE | MAPE | Notes |
|-------|------|-----|------|-------|
| ARIMA(2,1,2) | 93.63 | 71.43 | 5.86% | Best overall performance |
| ARIMA(7,1,7) | 122.49 | 99.61 | 7.93% | More complex, higher error |
| Holt-Winters | 123.55 | 100.52 | 7.99% | Simple exponential smoothing |
| SMA(10) | 119.82 | 97.40 | 7.75% | Short-term moving average |
| SMA(50) | 125.45 | 102.30 | 8.13% | Medium-term moving average |
| Linear Regression | 525.22 | - | - | With economic indicators |

### Key Findings

- **ARIMA(2,1,2)** provides the best forecasting accuracy
- **Moving averages** show consistent but limited performance
- **Economic indicators** add explanatory power but may overfit
- **Volatility clustering** suggests GARCH models would be beneficial

## 🛠️ Project Structure

```
stat-finance-time-series-analysis/
├── README.md                    # This file
├── time_series.ipynb           # ARIMA and time series analysis
├── smoothing.ipynb             # Holt-Winters and moving averages
├── regression.ipynb            # Linear and regularized regression
├── regression_data.csv         # Economic indicators dataset
├── data/                       # Data directory
│   └── processed/              # Cleaned datasets
├── results/                    # Output plots and tables
│   ├── forecasts/              # Forecast plots
│   └── diagnostics/            # Model diagnostic plots
└── utils/                      # Utility functions
    └── analysis_helpers.R      # Common analysis functions
```

## 📋 Model Diagnostics

### Statistical Tests Implemented

- **Augmented Dickey-Fuller Test**: Stationarity testing
- **ARCH-LM Test**: Heteroskedasticity testing  
- **Shapiro-Wilk Test**: Normality testing
- **McLeod-Li Test**: ARCH effects testing
- **Ljung-Box Test**: Autocorrelation testing

### Diagnostic Plots

- ACF/PACF plots for model identification
- Residual analysis plots
- Q-Q plots for normality assessment
- Time series decomposition plots

## 🔍 Advanced Features

### Model Selection

- **AIC/BIC Criteria**: Automated model selection
- **Cross-Validation**: K-fold validation for regression models
- **EACF Tables**: Extended ACF for ARIMA identification
- **Subset Selection**: Best subset regression using leaps

### Visualization

- Interactive time series plots
- Forecast confidence intervals
- Residual diagnostic plots
- Economic variable correlation analysis

## 📚 Dependencies

### Core R Packages

```r
# Time series analysis
tseries (>= 0.10)
forecast (>= 8.0)
zoo (>= 1.8)

# Statistical modeling
CombMSC (>= 1.4)
TSA (>= 1.3)
fBasics (>= 3.0)

# Machine learning
caret (>= 6.0)
leaps (>= 3.1)

# Technical analysis
TTR (>= 0.24)
```

## 🤝 Contributing

Contributions are welcome! Areas for improvement include:

### Planned Enhancements

- [ ] GARCH volatility modeling implementation
- [ ] Multivariate time series analysis (VAR models)
- [ ] Machine learning approaches (Random Forest, SVM)
- [ ] Real-time data integration
- [ ] Interactive dashboard development
- [ ] Additional economic indicators
- [ ] Regime-switching models
- [ ] High-frequency data analysis

### How to Contribute

1. **Fork the repository**
2. **Create a feature branch** (`git checkout -b feature/new-model`)
3. **Add your improvements** with proper documentation
4. **Include unit tests** for new functions
5. **Update documentation** as needed
6. **Submit a pull request**

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

- **Yahoo Finance** for providing historical market data
- **R Core Team** for the excellent statistical computing environment
- **CRAN Contributors** for the comprehensive package ecosystem
- **Quantitative Finance Community** for methodology inspiration

## 📞 Contact & Support

- **Issues**: [GitHub Issues](https://github.com/heartyguy/stat-finance-time-series-analysis/issues)
- **Discussions**: [GitHub Discussions](https://github.com/heartyguy/stat-finance-time-series-analysis/discussions)

## 📖 References

### Key Papers and Books

- **Time Series Analysis**: Box, G. E. P., Jenkins, G. M., & Reinsel, G. C. (2015)
- **Financial Econometrics**: Campbell, J. Y., Lo, A. W., & MacKinlay, A. C. (1997)
- **GARCH Models**: Bollerslev, T. (1986). "Generalized autoregressive conditional heteroskedasticity"
- **Forecasting**: Hyndman, R. J., & Athanasopoulos, G. (2018). "Forecasting: principles and practice"

### Useful Resources

- [R for Data Science](https://r4ds.had.co.nz/)
- [Forecasting: Principles and Practice](https://otexts.com/fpp3/)
- [Quantitative Finance in R](https://www.quantmod.com/)

---

⭐ **Star this repository if you find it useful for your financial analysis projects!**

🔔 **Watch this repository** to stay updated with new models and features.
