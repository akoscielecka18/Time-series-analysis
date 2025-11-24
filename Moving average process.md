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

<img width="922" height="553" alt="image" src="https://github.com/user-attachments/assets/841a9c54-2905-46c8-9f5f-86c0f04a9d5f" />



### AR(p) – Autoregressive Process of Order p

An **Autoregressive process of order p**, denoted by AR(p), is a time series model in which the current value of the series is expressed as a linear combination of its own past values plus a white-noise error term. Formally, a process \((X_t)\) is AR(p) if

$$
X_t = c + \phi_1 X_{t-1} + \phi_2 X_{t-2} + \dots + \phi_p X_{t-p} + \varepsilon_t,
$$

where:
- $\(c\)$ is a constant,
- $\(\phi_1, \dots, \phi_p\)$ are autoregressive parameters,
- $\((\varepsilon_t)\) is a white-noise process with zero mean and constant variance,
- $\(p\)$ is the order of the process.

In an AR(p) model, dependence is captured through past values of the series itself. Under appropriate conditions on the parameters (e.g. the roots of the characteristic polynomial lie outside the unit circle), the process is stationary, meaning it fluctuates around a constant mean with constant variance. A key identifying feature of AR models is that their partial autocorrelation function (PACF) cuts off after lag \(p\), while the autocorrelation function (ACF) typically decays gradually.

````r
zz<- rnorm(100,0,1)
phi<-.9
ar1<-ts(c(0:100)); rw<-ts(c(0:100))
for(t in 1:100){
  ar1[t+1]<-phi*ar1[t]+zz[t]
  rw[t+1]<-rw[t]+zz[t]
}
dd=min(c(ar1,rw));uu=max(c(ar1,rw))
plot(ar1,col="blue",ylab=NULL,ylim=c(dd-.1,uu+.1)); lines(rw,col="red")
````
<img width="847" height="510" alt="image" src="https://github.com/user-attachments/assets/a1f76c0c-2c16-47dc-9b4c-3a40d27fe3af" />
