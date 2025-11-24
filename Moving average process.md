### MA(q) – Moving Average Process of Order q

A **Moving Average process of order q**, denoted by MA(q), is a time series model in which the current value of the series is expressed as a linear combination of a finite number of past white-noise shocks (error terms). Formally, a process $(X_t)$ is MA(q) if it can be written as

$$
X_t = \mu + \varepsilon_t + \theta_1 \varepsilon_{t-1} + \theta_2 \varepsilon_{t-2} + \dots + \theta_q \varepsilon_{t-q},
$$

where:

- $\mu$ is a constant mean level,
- $(\varepsilon_t)$ is white noise with zero mean and constant variance,
- $\theta_1, \dots, \theta_q$ are real-valued parameters,
- $q$ is the order of the process.

In an MA(q) model, the dependence structure comes from the past error terms rather than from past values of the series itself. The process is stationary, and its autocorrelation function (ACF) is nonzero only up to lag $q$ and cuts off to zero for higher lags, which is a key property used for identifying MA models in practice.

````r
theta=c(.7,-.1)
maq<-arima.sim(list(order=c(0,0,2), ma=theta), n=200)

layout(rbind(c(1,1),c(2,3)),widths=c(1,1), heights=c(2,3))
plot(maq, ylab="x", col="navyblue")
acf(maq,40)
plot(ARMAacf(0, ma=theta, 40),type="h", xlab="lag", main="theoretical ACF"); abline(h=0)
````
The most importatnt thing is ```` MA(q) ```` where ````order(0,0,q)````. Value ````q```` The value of q tells us how much delay we are going to have.


