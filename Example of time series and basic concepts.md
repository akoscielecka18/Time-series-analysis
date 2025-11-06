### Introduction

Time series analysis is the study of data points collected or recorded at successive points in time to uncover patterns, trends, and dependencies. 
It involves using statistical and mathematical models to understand the underlying processes and make forecasts about future values.

#### Example 
“Nile” is a built-in data set in R that is often used in time series analysis. It shows the annual water flow in the Nile River (measured at Ashwan) from 1871 to 1970.
````r
data("Nile")  
nile_df <- data.frame(
  Year = as.integer(time(Nile)),
  Flow = as.numeric(Nile)
)
head(nile_df,5)
````
<img width="155" height="116" alt="image" src="https://github.com/user-attachments/assets/76555cb0-a675-423e-bba3-0f260922fcc9" />

````r
nile_df <- nile_df[, 1:2]
nile_num <- as.numeric(nile_df$Flow)
nile_ts <- ts(nile_num, start = c(1871), frequency = 1) # frequency = 1 - annual data
plot(nile_ts,
     main = "Annual Flow of the River Nile (1871–1970)",
     xlab = "Year",
     ylab = "Flow (10^8 m^3)",
     col  = "purple")
````
<img width="630" height="475" alt="image" src="https://github.com/user-attachments/assets/b359f0c7-81d0-4155-a9ff-01a4633163d5" />


### Stationary Time Series
A time series is stationary if its mean, variance, and correlations do not change over time.
In other words, its behavior is stable and does not depend on when we observe it.

````r
set.seed(123)

n <- 300

## 1) STACJONARNY (white noise i AR(1) z |phi|<1)
wn  <- ts(rnorm(n, 0, 1))                 # Gaussian white noise ~ stationary
phi <- 0.6
ar1 <- ts(filter(rnorm(n), filter = phi, method = "recursive"))  # stationary AR(1)

## 2) NIESTACJONARNE
trend     <- ts(0.02*(1:n) + rnorm(n))    # liniowy trend (zmienia się średnia)
hetero    <- ts(rnorm(n, 0, sd = seq(0.5, 3, length.out = n)))  # rosnąca wariancja
rw        <- ts(cumsum(rnorm(n)))         # random walk (wariancja rośnie z t)
seasonal  <- ts(sin(2*pi*(1:n)/24) + rnorm(n, 0, 0.3))          # silna sezonowość

## --- Wykresy szeregu ---
op <- par(no.readonly = TRUE)
par(mfrow = c(3,2), mar = c(3.2,3.6,2.2,0.8), oma = c(0,0,1,0), cex.main=.95)

plot(wn, col="steelblue", main="Stationary: White noise", ylab="value"); abline(h=0, lty=2)
plot(ar1, col="steelblue4", main=sprintf("Stationary: AR(1), phi=%.1f", phi), ylab="value"); abline(h=0, lty=2)

plot(trend, col="firebrick", main="Non-stationary: trend (mean changes)", ylab="value"); abline(h=0, lty=2)
plot(hetero, col="tomato3", main="Non-stationary: changing variance", ylab="value"); abline(h=0, lty=2)

plot(rw, col="purple4", main="Non-stationary: random walk", ylab="value"); abline(h=0, lty=2)
plot(seasonal, col="darkorange3", main="Non-stationary: strong seasonality", ylab="value"); abline(h=0, lty=2)

mtext("Stationary vs Non-stationary time series", outer=TRUE, line=-1, cex=1.1)
````

<img width="1210" height="888" alt="image" src="https://github.com/user-attachments/assets/df4dac14-bc4c-4f2a-a8b8-83fc54615410" />



