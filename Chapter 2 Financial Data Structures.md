# Essential Types of Financial Data
## Fundamental Data
- accounting data
- reported with a lapse
- often backfilled or reinstated
- low frequency
## Market Data
- all trading activity
- very abundant
## Analytics
- derivated data
- positive aspect: the signal has been extracted
- negative aspect: analytics may be costly, methodology used may be biased or opaque, and you will not be the sole customer
## Alternative Date
- information that has not made it to the other sources
- expensive
- privacy concerns
- hard-to-process

# Bars
## Standard Bars
### Time bars
- sampling information at fixed time intervals
  - Market do not process information at a constant time interval
    - time bars oversample information during low-activity periods and undersample information during high-activity periods
  - Time-sampled series often exhibit poor statistical properties
    - serial correlation
    - heteroscedasticity
    - non-normality of returns

### Tick bars
- sampling as a function of the number of transactions exhibited
  - tick bars allow for better inference than time bars
- outliers
  - order gragmentation introduces some arbitratiness in the number of ticks

### Volume bars
- sampling every time a pre0defined amount of the security's units have been exchanged
  - even better statistical properties than sampling by tick bars(closer to IID Gaussian distribution)

### Dollar bars
- sampling an observation every time a pre-difined market value is exchanged
  - when number of shares traded is a function of the actual value exchanged
  - the number of outstanding shares often changes multiple times over the course of a security's life, as a result of corporate actions.
  
## Information-driven Bars
### Tick information bars
Consider a sequence of ticks ![](https://latex.codecogs.com/gif.latex?%7B%28p_t%2Cv_t%29%7D_%7Bt%3D1%2C...%2CT%7D)
### Volume/dollar imbalance bars
### Tick runs bars
### Volume/dollar runs bars
