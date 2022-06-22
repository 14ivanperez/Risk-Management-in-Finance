## Financial Risk Management using R

### Introduction

I will show how to calculate the return of a portfolio of securities as well as quantify the market risk of that portfolio. I will use the two main tools for calculating the market risk of stock portfolios: Value-at-Risk (VaR) and Expected Shortfall (ES).

### Installation

```` markdown
#load data
library(quantmod)    #package needed for data
wilsh<-getSymbols("DEXUSAL",src="FRED",auto.assign=FALSE) #obtain symbols for data
wilsh <- na.omit(wilsh) #omit NA values
wilsh <- wilsh["1979-12-31/2017-12-31"]  #to use specific dates
````

### Required libraries

```` markdown
library(quantmod) 
library(moments)
library(MASS)
library(metRology)
library(rugarch)
````

### Topics discused

````markdown
# TO OBTAIN VAR AND ES FROM ACTUAL DATA
# VAR AND ES FROM SIMULATIONS:
1. different distributions (randon, normal, t)
2. Calculate skewness, kurtosis, and Jarque-Bera test 
# ESTIMATE VaR and ES at 10-DAY HORIZON
# ACF AND VOLATILITY CLUSTERING USING GARCH MODELS
````


### Roadmap

````markdown
1.Data source (FRED at the Federal Reserve Bank of St. Louis), and the calculation of returns 
                                                 ↓
2.How to calculate value-at-risk (VaR) and expected shortfall (ES) when returns are normally distribute
                                                 ↓
3.How to calculate VaR and ES when returns aren't normally distributed (random and t distributed)
                                                 ↓
4.Test for the presence of volatility clustering (Jarque-Bera test ), and how to calculate VaR and ES when returns exhibit volatility clustering
````
___


### References

Financial Risk Management with R, Duke University
