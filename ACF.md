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


