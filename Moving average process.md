### MA(q) – Moving Average Process of Order q

A **Moving Average process of order q**, denoted by MA(q), is a time series model in which the current value of the series is expressed as a linear combination of a finite number of past white-noise shocks (error terms). Formally, a process \((X_t)\) is MA(q) if it can be written as

\[
X_t = \mu + \varepsilon_t + \theta_1 \varepsilon_{t-1} + \theta_2 \varepsilon_{t-2} + \dots + \theta_q \varepsilon_{t-q},
\]

where:
- \(\mu\) is a constant mean level,
- \((\varepsilon_t)\) is white noise with zero mean and constant variance,
- \(\theta_1, \dots, \theta_q\) are real-valued parameters,
- \(q\) is the order of the process.

In an MA(q) model, the dependence structure comes from the past error terms rather than from past values of the series itself. The process is stationary, and its autocorrelation function (ACF) is nonzero only up to lag \(q\) and cuts off to zero for higher lags, which is a key property used for identifying MA models in practice.

````r
sdfg
````
