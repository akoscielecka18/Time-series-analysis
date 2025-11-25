## Forecasting in Time Series

In time series analysis, **forecasting** means using past observations of a process to predict its future values.  
Given a series \((X_1, X_2, \dots, X_T)\), the goal is to construct forecasts for \(X_{T+h}\) for horizons \(h = 1, 2, \dots\).

There are two main ingredients:

1. **A model for the data-generating process**  
   We assume that the series follows a stochastic model, such as AR, MA, ARMA, ARIMA, etc.  
   For example, in this lab we work with an ARMA(2,1) model. Once the parameters of the model are estimated from the first \(T\) observations, the model can be used to compute future values.

2. **Forecasts and prediction intervals**  
   - A **point forecast** \(\hat{X}_{T+h}\) is our best single guess for the future value at horizon \(h\).  
   - A **prediction interval** (e.g. \(\hat{X}_{T+h} \pm 1.96 \cdot \text{SE}\)) quantifies the uncertainty of the forecast and gives a range in which the future observation is likely to lie.

In practice, we often compare forecasts under different modelling assumptions (for example, assuming that the data follow an AR(1) model vs. using an automatically selected ARMA model) and check how well the forecasts match the actual future observations.

````r
armapq<-arima.sim(list(order=c(2,0,1),ar=c(-1,-.25),ma=.49),n=110)
modelll<-arima(armapq[1:100],order=c(1,0,0),include.mean =FALSE)
future <- predict(modelll,n.ahead = 10)
plot.ts(armapq)
lines(future$pred, col="red")
````
<img width="849" height="522" alt="image" src="https://github.com/user-attachments/assets/81e0d185-b47d-4bfb-a52a-305dcc9a037a" />

Add limits lines 

````r
U<-future$pred+future$se; L<-future$pred-future$se
xx<-c(time(U),rev(time(U))); yy<-c(L,rev(U))

plot.ts(armapq)
polygon(xx,yy,col="lightgray",border="gray")
lines(future$pred, col="red"); lines(as.ts(armapq[1:110]),col="green")
````
<img width="862" height="507" alt="image" src="https://github.com/user-attachments/assets/72d16e25-ff5e-43a7-a391-2320c67fe914" />

````r
future1 <- predict(arima(armapq[1:100],order=c(1,0,0),include.mean =TRUE),n.ahead = 10)

plot.ts(armapq)
lines(future1$pred, col="brown")
lines(future$pred, col="red")
````
<img width="844" height="521" alt="image" src="https://github.com/user-attachments/assets/d013826d-a308-44a4-a31c-1366699edf39" />

When ````include.mean = TRUE```` matches the model with a free expression. 

### Forecasting with using the forecast function 

````r
library(forecast)
future1<-forecast(modelll,h=10,level=2*pnorm(1)-1 )
plot(future1)
lines(armapq)
````

<img width="800" height="527" alt="image" src="https://github.com/user-attachments/assets/cd67ff8f-1084-41a4-a4c6-b2c69e1f3863" />

Above is the forecast plot obtained from the ARIMA(1,0,0) model (i.e. an AR(1) model without intercept) fitted to the series `armapq`:

- The **black line** shows the historical observations of the time series.
- The **turquoise line** at the end represents the **point forecasts** for the next 10 time points.
- The **dark and light shaded areas** around the forecasts are **prediction intervals** for the chosen confidence level (here ≈ 68%, corresponding to ±1 standard deviation).

Because the model is ARIMA(1,0,0) **with zero mean**, the long–run mean of the process is constrained to 0. As a result, the multi–step–ahead forecasts gradually **shrink back towards zero** as the forecast horizon increases. At the same time, the prediction intervals become **wider** the further we forecast into the future, reflecting increasing uncertainty about distant observations.


##### We also can plot 95% confident interval 

````r
future1<-forecast(modelll,h=10,level=95 )
plot(future1)
lines(armapq)
lines(future1$x, col="green")
lines(future1$fitted,col="navyblue")
lines(future1$residuals,col="red")
lines(future1$lower, col="orange")
````
**Conclusion**

The plot suggests that the ARIMA(1,0,0) model with zero mean provides a reasonable description of the dependence structure in the data: the fitted values (navy line) follow the observed series, while the residuals (red line) fluctuate around zero and behave roughly like white noise. The forecasts for the next 10 periods (blue line) gradually shrink back toward zero, which is consistent with the zero-mean assumption of the model. At the same time, the grey prediction intervals widen as the forecast horizon increases, reflecting growing uncertainty the further we project into the future.

````r
future2<-forecast(armapq[1:100], h=10)
plot(future2)
````
<img width="825" height="538" alt="image" src="https://github.com/user-attachments/assets/14c20d47-34ce-4a2f-b512-da5b73c0eb0a" />

**Conclusion**

Here the `forecast()` function is applied directly to the first 100 observations of the series, and it automatically selects an **ETS(A,N,N)** model (additive error, no trend, no seasonality). The resulting forecast plot shows that:

- The model treats the data as fluctuations around a **constant level**, with no systematic trend or seasonal pattern.
- The **blue horizontal line** represents the point forecasts for the next 10 periods; they are essentially **flat**, equal to the estimated level of the series.
- The **shaded prediction intervals** around the forecasts are symmetric and widen slightly with the horizon, reflecting forecast uncertainty but keeping the future values centered around the same level.

This behaviour is typical for ETS(A,N,N) / simple exponential smoothing: when no trend or seasonality is detected, all future forecasts converge to a constant value (the estimated level of the process), with uncertainty represented by the growing prediction intervals.

### Coefficients of MA representation

````r
MA.infty<-ARMAtoMA(ar=c(3/2,-17/16, 3/4, -9/32), ma= -2/3,lag.max=100)
plot(MA.infty,type="h")
````
<img width="862" height="511" alt="image" src="https://github.com/user-attachments/assets/61dbd743-6045-4556-b61c-55ded8cdf427" />



