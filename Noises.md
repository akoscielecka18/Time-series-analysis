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

### White Noise 

White noise stands for a sequence of random variables that are:

- have an expected value of 0, 𝐸[𝑍𝑡]=0
- have a constant finite variance, 𝑉𝑎𝑟(𝑍t)=𝜎2
- are uncorrelated with each other, meaning 𝐶𝑜𝑣(𝑍𝑡,𝑍𝑠)=0 for 𝑡≠𝑠

This means that the variables do not show any linear dependence over time — their correlation is zero for all non-zero lags.
If, in addition, each variable has a normal distribution, then the process is called Gaussian white noise.
In white noise variables are only uncorrelated might not be independent. 

