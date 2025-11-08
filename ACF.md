### ACF

ACF (autocorrelation function) – a function that shows how strongly the values of a time series are related to each other at different time intervals (lags).
In other words, ACF measures whether and how much the current value depends on previous values in the series.
For white noise, autocorrelations are close to zero, and for data with dependencies (e.g., a trend), ACF shows higher values for successive lags.

````r
normalWN<- ts(rnorm(200,sd=2 ))   
par(mfrow=c(2,1),mar=c(4,4,.1,1))  
plot(normalWN, col="red",ylab=NULL) ;abline(h=mean(normalWN),lty=2);    
acf(normalWN, lag.max=40, ci=.95, main=" ")    
````
Here we have a data set that is generated randomly. 
The ```rnorm()``` function generates 200 random numbers from a normal distribution with a mean of 0 and a standard deviation of 2. 
Then, the ```ts()``` function converts a regular vector of numbers into a time series object. 

<img width="1200" height="893" alt="image" src="https://github.com/user-attachments/assets/026978ac-5603-4f7e-87f6-403743de0f49" />

The first graph shows the generated time series, and below we see the ACF analysis, which shows how successive observations are correlated with each other. 
The first high bar at lag = 0 is always 1, because each observation is 100% correlated with itself, while the other bars (for lag = 1, 2, 3...) show how strongly the current value depends on the previous ones. 
The blue lines are confidence limits (e.g., 95%). If the bar is inside these lines, it means that the correlation is small and statistically insignificant. 
Based on the graph, we can conclude that there is no significant relationship between successive observations over time. 

### “chi-squared” noise

````r
chi2WN<- ts(rchisq(200,df=2 )) 
chi2WN<-chi2WN-2  

par(mfrow=c(2,1),mar=c(4,4,.1,1))
plot(chi2WN, col="navyblue",ylab=NULL); abline(h=mean(chi2WN),lty=2); 
acf(chi2WN,  lag.max=40, ci=.95, main=" ") #
````
<img width="1204" height="877" alt="image" src="https://github.com/user-attachments/assets/4ba23b73-e9e7-423b-81c9-fca2d4c4d329" />



Here we have white noise, because each observation is independent of the others, but it is not normal, only coming from a chi-square distribution. Again, we generate 200 random numbers from the chi2 distribution and subtract 2 from the mean so that it equals 0. The distribution is asymmetric, i.e., skewed. 

The top graph shows that the data jumps randomly without any trend or seasonality. In the bottom graph, we can see that almost all bars are between the blue lines, there is no clear pattern, i.e., there is no autocorrelation, which means that despite the non-normal distribution, the data are independent over time. The process is stationary because the mean and variance are constant over time. 


### Increasing variance

````r
UU<-ts(runif(100,min=-1,max=1)) 
UU<-UU*c(sqrt(1:100)) 

par(mfrow=c(2,1),mar=c(4,4,.1,1))
plot(UU, col="2022",ylab=NULL); abline(h=mean(UU),lty=2); 
acf(UU,  lag.max=40, ci=.95, main=" ")
````
<img width="1197" height="867" alt="image" src="https://github.com/user-attachments/assets/4c711a34-d349-43f0-9d84-500d1ee49c25" />

We generate 100 random values from a uniform distribution on the interval (-1,1). From the basic properties, we know that the mean of this distribution is 0 and the variance is 1/3 for each variable. However, at this point, UU*sqrt(1:100) multiplies each subsequent observation by an increasingly larger number.
The conclusion is that the process has variable variance over time (the further in the series, the greater the dispersion), i.e., it is not stationary.

