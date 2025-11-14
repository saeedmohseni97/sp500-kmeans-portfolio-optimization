# 📈 Unsupervised Trading Strategy on the S&P 500  
### *Clustering + Factor Modeling + Portfolio Optimization (10-Year Backtest)*  

This project builds a fully systematic **quantitative trading strategy** using an **unsupervised machine learning pipeline** on 10 years of S&P 500 data.  
It combines **feature engineering**, **K-Means clustering**, **Fama–French factor modeling**, and **maximum Sharpe ratio portfolio optimization** to create a robust, data-driven equity selection and allocation framework.

The goal is to demonstrate end-to-end quant research skills:  
**data acquisition → alpha signal generation → ML clustering → factor modeling → portfolio construction → backtesting → benchmarking.**

---

## 🚀 Project Highlights  
- **10 years of S&P 500 data** automatically fetched from Yahoo Finance  
- Full suite of **technical indicators**: RSI, ATR, MACD, Bollinger Bands, GK volatility, rolling returns  
- **Liquidity filtering** to maintain realistic tradability  
- **K-Means clustering** with custom centroid initialization to group stocks by momentum and volatility  
- **Fama–French 5-factor rolling betas** integrated as additional quantitative features  
- Selection of high-momentum (high RSI) stocks **every month**  
- **Efficient Frontier optimization** (PyPortfolioOpt) to compute maximum Sharpe ratio weights  
- **Monthly backtest** comparing strategy performance vs **SPY (S&P 500 benchmark)**  
- Professional visualizations for cluster behavior & cumulative returns

---

## 📊 Methodology Overview  

### 1. Data Acquisition  
- Scrapes the latest S&P 500 ticker list from Wikipedia  
- Downloads daily OHLCV data for all stocks (2015–2025)  
- Converts daily data to monthly aggregates  

### 2. Feature Engineering  
For each stock/month:
- **Technical indicators** (RSI, ATR, MACD, Bollinger Bands, GK Volatility)  
- **Monthly returns** at 1m, 2m, 3m, 6m, 9m, 12m horizons  
- **Dollar-volume liquidity filtering** (top 150 most liquid only)  

### 3. Factor Modeling  
- Loads **Fama–French 5 Factors**  
- Computes **rolling betas** for each stock using Rolling OLS  
- Fills missing exposures with cross-sectional means  

### 4. Unsupervised Clustering  
- Runs **K-Means (k=4)** on normalized features  
- Custom centroid initialization to emphasize RSI separation  
- Produces stable momentum/volatility clusters  
- Scatter visualization: **RSI vs ATR**, colored by cluster  

### 5. Portfolio Construction  
Each month:
1. Select stocks with **highest RSI** from the target cluster  
2. Estimate expected returns & covariance matrix  
3. Solve **maximum Sharpe ratio** portfolio using Efficient Frontier  
4. Compute next-month realized returns  

### 6. Backtest & Benchmarking  
- Track monthly strategy returns  
- Compare cumulative returns vs **SPY buy-and-hold**  
- Plot performance & evaluate alpha  

---

## 🧠 Skills Demonstrated  

### **Quantitative Finance**
- Factor modeling (Fama–French)  
- Portfolio optimization (Markowitz / Efficient Frontier)  
- Multi-period return engineering  
- Liquidity constraints  

### **Machine Learning / AI**
- Unsupervised learning (K-Means clustering)  
- Feature scaling & custom centroid initialization  
- Time-series ML pipeline design  

### **Software & Data Engineering**
- Automated data pipelines  
- MultiIndex financial data structures  
- Vectorized pandas workflows  
- Clean modular Jupyter notebook design  

---

## 🧰 Tech Stack  
- **Python**, **NumPy**, **Pandas**, **Matplotlib**  
- **scikit-learn**  
- **statsmodels**  
- **PyPortfolioOpt**  
- **yfinance** & **pandas-datareader**  
- **Jupyter Notebook**

---

## 📁 Repository Structure  

```
│
├── notebook/
│   └── Unsupervised_Trading_Strategy.ipynb
│
├── data/                # (Optional) cached price data
├── images/              # Plots for README
│
├── README.md            # Project documentation
└── requirements.txt     # Environment setup
```

---

## ▶️ How to Run  
```bash
git clone https://github.com/<your-username>/<your-repo>.git
cd <your-repo>
pip install -r requirements.txt
jupyter notebook
```

---

## 📬 Contact  
If you’re a recruiter or researcher and would like to discuss this project:

**Your Name**  
**Email:** your_email_here  
**LinkedIn:** https://linkedin.com/in/your-profile  
**GitHub:** https://github.com/your-username  

---

## ⭐ If you find this interesting, consider starring the repo!  
