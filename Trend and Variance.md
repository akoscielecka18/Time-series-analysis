### Preparation for forecasting

Let us consider AirPassangers the time series from datasets package.

The AirPassengers dataset shows an upward trend, as the number of passengers increases over time. There is also seasonality (patterns repeat every year) and variable variance, with fluctuations increasing over time. 
There is no normal distribution, so we will perform a Box-Cox transformation, which will stabilize the variance so that the amplitude of the fluctuations does not increase with the level and, over time, 
approximate the distribution to normal.

````r
par(mfrow=c(2,1))
plot(AirPassengers)
BC_AP<-BoxCox(AirPassengers,lambda="auto")
plot(BC_AP)
````
<img width="845" height="547" alt="image" src="https://github.com/user-attachments/assets/88a95771-2cb3-47f8-8c38-ec2af5359318" />

### Estimation of Trend and Seasonal Components

Let us assume that the time series BC_AP satisfies Xt=mt+st+Yt these are the three basic components of any time series, where:

- mt - A trend is a general tendency to change over time.It shows whether data tends to increase, decrease, or remain more or less constant.
- st - A seasonal/cyclical component describes regular, recurring fluctuations around a trend. These are cycles that repeat at fixed intervals (year, quarter, month, week, etc.).
- Yt - random component remaining after removing trend and seasonality

#### mt
We are looking for a trend, i.e., the general tendency of changes over time, whether the data is increasing or decreasing. 
We guess that d=12 and hence q=6

````r
w= c(.5,rep(1,11),.5)/12    #weights of moving average filter
MAF_AP<-filter(BC_AP,filter=w,sides=2) # moving average filter
plot(MAF_AP)
````
You use a moving average for this — it's a way to “smooth out” the data so that short-term fluctuations (e.g., seasonal ones) disappear and only the main direction of change remains.
<img width="865" height="517" alt="image" src="https://github.com/user-attachments/assets/d5f4f853-3ce5-4120-a552-916575832927" />

The graph shows a smooth line that rises steadily, which means that the number of air passengers has grown year on year.

#### st
You look at how the data deviates from the trend in individual months to find a seasonal pattern.
In other words, how much January, February, March, etc. differ from the trend value.

````r
​w_AP<- c(rep(0,12)) # It will be w_k k from the lecture. Later also s_k for k = 1, .., 12; k=1,..., d; d=12

for(k in 1:6){
  for(j in 1:11 ){
    w_AP[k]<- w_AP[k]+BC_AP[k+j*12]-MAF_AP[k+j*12]
  }
  
}

for(k in 7:12){
  for(j in 0:10 ){
    w_AP[k]<- w_AP[k]+BC_AP[k+j*12]-MAF_AP[k+j*12]
  }
  
}
  w_AP=w_AP/12
````
In this step, we calculate how much each month differs from the trend —
this allows you to identify seasonal patterns (i.e., when data is higher or lower during the year).
Then compute st
````r
w_AP=w_AP-mean(w_AP) # s_k for k=1, ... , d
s_AP<-rep(w_AP,12) #s_k for all d
````

#### Estimate the trend

In this step, we want to remove seasonality from the data to see the pure trend after deseasonalization.

````r
  res_AP<-BC_AP-s_AP  # deseasonalized data
  trend_AP<-filter(res_AP,filter=rep(1/7,7),sides=2) # Estimation of trend from deseasonalized data
````

#### Final 

Estimate and plot the noise series 

````r
residuals_AP<-BC_AP-trend_AP-s_AP
plot(residuals_AP)
````

<img width="821" height="507" alt="image" src="https://github.com/user-attachments/assets/fc7f43a9-db2a-4478-a079-4a48d8110669" />

So the trend has been removed, and what remains is white noise, random, unpredictable deviations. This graph shows the remainder after subtracting the trend and seasonality, i.e., what is purely random in the data. 
