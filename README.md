## Financial Risk Management using R

### Introduction

I will show how to calculate the return of a portfolio of securities as well as quantify the market risk of that portfolio. I will use the two main tools for calculating the market risk of stock portfolios: Value-at-Risk (VaR) and Expected Shortfall (ES).

### Required libraries

```` markdown
library(quantmod) 
library(moments)
library(MASS)
library(metRology)
library(rugarch)
````

### Topics discused

```markdown
# TO OBTAIN VAR AND ES FROM ACTUAL DATA
# VAR AND ES FROM SIMULATIONS:
1. different distributions (randon, normal, t)
2. Calculate skewness, kurtosis, and Jarque-Bera test 
# ESTIMATE VaR and ES at 10-DAY HORIZON
# ACF AND VOLATILITY CLUSTERING USING GARCH MODELS
```


### Roadmap

```markdown
# Data source (FRED at the Federal Reserve Bank of St. Louis), and the calculation of returns 
## How to calculate value-at-risk (VaR) and expected shortfall (ES) when returns are normally distribute
### How to calculate VaR and ES when returns aren't normally distributed (random and t distributed)
#### Test for the presence of volatility clustering (Jarque-Bera test ), and how to calculate VaR and ES when returns exhibit volatility clustering
```
___

### References

Financial Risk Management with R, Duke University
