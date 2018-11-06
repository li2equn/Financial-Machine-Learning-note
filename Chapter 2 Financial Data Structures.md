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
Consider a sequence of ticks ![](https://latex.codecogs.com/gif.latex?%7B%28p_t%2Cv_t%29%7D_%7Bt%3D1%2C...%2CT%7D) where ![](https://latex.codecogs.com/gif.latex?p_t) is the price associated with tick t and ![](https://latex.codecogs.com/gif.latex?v_t) is the volume associated with tick t. The so-called tick rule defins a sequence ![](https://latex.codecogs.com/gif.latex?%5C%7Bb_t%5C%7D_%7Bt%3D1%2C...%2CT%7D) where

![](https://latex.codecogs.com/gif.latex?b_t%20%3D%5Cleft%5C%7B%5Cbegin%7Bmatrix%7D%20b_%7Bt-1%7D%20%26%5Ctext%7Bif%7D%20%26%5CDelta%20p_t%20%3D%200%20%5C%5C%20%5Cfrac%7B%5Cleft%20%7C%20%5CDelta%20p_t%20%5Cright%20%7C%7D%7B%5CDelta%20p_t%7D%26%5Ctext%7Bif%7D%26%20%5CDelta%20p_t%20%5Cneq%200%20%5Cend%7Bmatrix%7D%5Cright.)

with ![](https://latex.codecogs.com/gif.latex?b_t%20%5Cin%20%5C%7B-1%2C1%5C%7D), and the boundary condition ![](https://latex.codecogs.com/gif.latex?b_0) is set to match the terminal value ![](https://latex.codecogs.com/gif.latex?b_T) from the immediately preceding bar
### Volume/dollar imbalance bars
### Tick runs bars
### Volume/dollar runs bars
