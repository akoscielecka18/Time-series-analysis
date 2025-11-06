### IID Noise

IID stands for a sequence of random variables that are:

- I - independent
- I - identically distributed
- D - distributed

This means that each variable has an expected value of 0 E[Zt​]=0 and a common variance Var(Zt​)=σ2 , and is independent of the others. 
If, in addition, each variable has a normal distribution, then the process is called IID Gaussian noise.


````r
normalWN<- ts(rnorm(200,sd=2 )) 

plot(normalWN, col="red",ylab=NULL) ;abline(h=0); abline(h=mean(normalWN),lty=2) 

````
<img width="1163" height="776" alt="image" src="https://github.com/user-attachments/assets/9b8f8bf6-2c50-4e98-9a8c-d38e4c039a74" />


To generate observations from a normal distribution, you can use the rnorm function. In this example, we can see that the variables come from a normal distribution with a mean of 0 and a standard deviation of 2. 


### DrunkManWalk 

````r
DrunkManWalk<-ts(c(0:500))
for(t in 1:500){
  DrunkManWalk[t+1]<-DrunkManWalk[t]+rchisq(1,2)-2
}
plot(DrunkManWalk, col=1410,main="Random walk with positive skewed white noise")
````

Generates a random number from a chi-square distribution with 2 degrees of freedom. 
This distribution is not symmetrical because it is skewed to the right.
The loop builds a random wandering process, where successive values are added together with a random component.

<img width="1199" height="822" alt="image" src="https://github.com/user-attachments/assets/f76663a6-6dfa-42f8-ba35-36d9c1acc5cb" />


### White Noise 

White noise stands for a sequence of random variables that are:

- have an expected value of 0, 𝐸[𝑍𝑡]=0
- have a constant finite variance, 𝑉𝑎𝑟(𝑍t)=𝜎2
- are uncorrelated with each other, meaning 𝐶𝑜𝑣(𝑍𝑡,𝑍𝑠)=0 for 𝑡≠𝑠

This means that the variables do not show any linear dependence over time — their correlation is zero for all non-zero lags.
If, in addition, each variable has a normal distribution, then the process is called Gaussian white noise.
In white noise variables are only uncorrelated might not be independent. 

````r
white_noise <- ts(rnorm(200, mean = 0, sd = 1))

# Plot the time series
plot(white_noise, col = "steelblue",
     main = "Example of Gaussian White Noise",
     ylab = "Value", xlab = "Time")
abline(h = 0, lty = 2)  # add the mean line
````
<img width="1195" height="828" alt="image" src="https://github.com/user-attachments/assets/fe1b691c-bd3d-4b18-9b43-9489a73f001e" />

White noise is a sequence of random values that fluctuate around the mean, have no upward or downward trend, and have a constant spread (variance), meaning that the amplitude of the fluctuations is roughly the same throughout. They are not related to each other, meaning that no value depends on previous ones—there is no pattern and no correlation. 



