#to load data
library(quantmod)    #package needed for data
wilsh<-getSymbols("DEXUSAL",src="FRED",auto.assign=FALSE) #obtain symbols for data
wilsh <- na.omit(wilsh) #omit NA values
wilsh <- wilsh["1979-12-31/2017-12-31"]  #to use specific dates
names(wilsh) <- "TR"

logret <- diff(log(wilsh)) #formula to obtain logaritmic return
round(mean(logret),8) #estimated mean of daily log returns
round(sd(logret),8 ) #estimated sd of daily log returns
head(logret,3) 
logret.m <- apply.monthly(logret,sum) #for obtaining monthly returns (or other timeframes)
ret.m <- exp(logret.m)-1 # to get discrete returns, "-1" eliminates 1st value
round(head(ret.m,3),6) #round result to 6 decimals 
----------------
  #TO OBTAIN VAR AND ES FROM ACTUAL DATA
library(quantmod)
getSymbols("GOLDPMGBD228NLBM",src="FRED")
wilsh <- na.omit(GOLDPMGBD228NLBM)
wilsh <- wilsh["1979-12-31/2017-12-31"]
names(wilsh) <- "TR"
logret <- diff(log(wilsh))[-1]
#for inverse
library(quantmod)
wilsh<-getSymbols("DEXSZUS",src="FRED")
wilsh <- na.omit(DEXSZUS) 
wilsh<-1/wilsh # INVERSE
wilsh <- wilsh["1979-12-31/2017-12-31"]
names(wilsh) <- "TR"
logret <- diff(log(wilsh))[-1]
----------------
mu <- mean(logret)
sig <- sd(logret)
var <- qnorm(0.05,mu,sig) #formula to obtain var at 95% CL
HFvar <- 1000 * ( exp(var)-1 ) #Hedge fund var
es <- mu-sig*dnorm(qnorm(0.05,0,1),0,1)/0.05 #formula to obtain ES at 95% CL
HFes <- 1000 * ( exp(es)-1 ) #Hedge fund ES
----------------------------------------
  #VAR AND ES SIMULATIONS
  #simulation from randomly distribution
set.seed(123789) # to get random results
rvec <- rnorm(100000,mu,sig) #vector with 100k random outcomes (NORMAL DIST)
VaR <- quantile(rvec,0.05)
ES <- mean(rvec[rvec<VaR])
---------------
  #simulation from emipirical distribution of the data
set.seed(123789) 
rvec <- sample(as.vector(logret),100000,replace=TRUE)
VaR <- quantile(rvec,0.05)
ES <- mean(rvec[rvec<VaR])
--------------------
#skewness, kurtosis, and Jarque-Bera  test 
library(moments)
rvec <- as.vector(logret) #put the log returns inside a vector
round(skewness(rvec),2) #calculate skewness coefficients (to know symmetry of distribution)
round(kurtosis(rvec),2) #calculate kurtosis coefficients (to know how tails are)
jarque.test(rvec)  #to test if distribution is normal
--------------------
#student t distributions, heavier tailed (more real)
library(MASS) #(needed for scaled t distribution parameters)
rvec <- as.vector(logret)
t.fit <- fitdistr(rvec,"t")  #to obtain the best possible ouctomes of the distribution
round(t.fit$estimate,6) #show estimates (m,s,df)
#to estimate VAR and ES under a t distriution
alpha <- 0.05
set.seed(123789)
library(metRology)
rvec <- rt.scaled(100000,mean=t.fit$estimate[1],sd=t.fit$estimate[2],df=t.fit$estimate[3])
VaR <- quantile(rvec,alpha)
ES <- mean(rvec[rvec<VaR])
round(VaR,6)
round(ES,6)
#Estimate VaR and ES at 10-day horizon 
#Simulation 1: the scaled student-t distribution
alpha <- 0.05
set.seed(123789)
rvec <- rep(0,100000)
for (i in 1:10) {
  rvec <- rvec+rt.scaled(100000,mean=t.fit$estimate[1],sd=t.fit$estimate[2],df=t.fit$estimate[3])
}
VaR <- quantile(rvec,alpha)
ES <- mean(rvec[rvec<VaR]) 
#Simulation 2: randomly samples ten 1-day observations from the empirical distribution and add them up
alpha <- 0.05
set.seed(123789)
rvec <- rep(0,100000)
for (i in 1:10) {
  rvec <- rvec+ sample(as.vector(logret),100000,replace=TRUE)
}
VaR <- quantile(rvec,alpha)
ES <- mean(rvec[rvec<VaR]) 
#Simulation 3: randomly draws blocks of ten consecutive 1-day outcomes and add them up
alpha <- 0.05
set.seed(123789)
rdat <- as.vector(logret)
rvec <- rep(0,100000)
posn <- seq(from=1,to=length(rdat)-9,by=1)
rpos <- sample(posn,100000,replace=TRUE)
for (i in 1:10) {
  rvec <- rvec+ rdat[rpos]
  rpos <- rpos+1
}
VaR <- quantile(rvec,alpha)
ES <- mean(rvec[rvec<VaR]) 
---------------------------
  #Serial Correlation, Volatility Clustering, GARCH
acf(logret) #graph the autocorrelation function of log returns 
acf( abs(logret) ) #graph absolute autocorrelation function of log returns
#to estimate GARCH t model
library(rugarch)
uspec <- ugarchspec( variance.model = list(model = "sGARCH",garchOrder = c(1,1)),
                     mean.model = list(armaOrder = c(0,0), include.mean = TRUE),
                     distribution.model = "std")
fit.garch <- ugarchfit(spec = uspec, data = logret[,1],solver ='hybrid')
#to save estimates
save1 <- cbind( logret[,1], fit.garch@fit$sigma, fit.garch@fit$z )
names(save1) <- c( "logret", "s", "z" )  
acf(save1$z)
acf(abs(save1$z))
fit.garch <- ugarchfit(spec = uspec, data = logret[,1])
-------------------
set.seed(123789) #set seed value
boot.garch <- ugarchboot(fit.garch,
                         method=c("Partial","Full")[1], # ignore parameter uncertainty
                         sampling="raw", # draw from standardized residuals
                         n.ahead=1, # 1-day ahead
                         n.bootpred=100000, # number of simulated outcomes
                         solver="solnp")
rvec <- boot.garch@fseries #simulated outcomes are then saved in the vector "rvec" 
VaR <- quantile(rvec,0.05)
ES <- mean(rvec[rvec<VaR])
