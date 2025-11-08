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
Let us assume that the time series BC_AP satisfies 
𝑋
𝑡
=
𝑚
𝑡
+
𝑠
𝑡
+
𝑌
𝑡
X
t
	​

=m
t
	​

+s
t
	​

+Y
t
	​
