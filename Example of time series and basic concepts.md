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

