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
- sample bars whenever tick imbalances exceed our expectations
- We can understand TIBs as buckets of trades containing equal amounts of information
- We wish to determine the tick index, T, such that the accumulation of signed ticks exceeds a given threshold.
- Procedure to determine T
  1. defined the tick imbalance at T as
  
  ![](https://latex.codecogs.com/gif.latex?%5Ctheta_T%20%3D%20%5Csum_%7Bt%3D1%7D%5E%7BT%7D%20b_t)
  
  2. compute the expected value of ![](https://latex.codecogs.com/gif.latex?%5Ctheta_T) at the beginning of the bar, ![](https://latex.codecogs.com/gif.latex?E_0%5B%5Ctheta_T%5D%20%3D%20E_0%5BT%5D%28P%5Bb_t%3D1%5D-P%5Bb_t%20%3D%20-1%5D%29) where 
    - ![](https://latex.codecogs.com/gif.latex?E_0%5BT%5D) is the expected size of the tick bar.
    - ![](https://latex.codecogs.com/gif.latex?P%5Bb_t%20%3D%201%5D) is the unconditional probability that a tick is classified as a buy
    - ![](https://latex.codecogs.com/gif.latex?P%5Bb_t%20%3D%20-1%5D) is the unconditional probability that a tick is classified as a sell
    ![](https://latex.codecogs.com/gif.latex?E_0%5B%5Ctheta_T%5D%20%3D%20E%5BT%5D%282P%5Bb_t%20%3D%201%5D-1%29)
    
    In practice, we can estimate
    - ![](https://latex.codecogs.com/gif.latex?E_0%5BT%5D) as an exponentially weighted moving average of T values from prior bars
    - ![](https://latex.codecogs.com/gif.latex?2P%5Bb_t%20%3D%201%5D-1) as an exponentially weighted moving average of ![](https://latex.codecogs.com/gif.latex?b_t) values from prior bars
  3. defined a tick imbalance bar(TIB) as a ![](https://latex.codecogs.com/gif.latex?T%5E*)-contiguous subset of ticks such that the following condition is met:
  ![](https://latex.codecogs.com/gif.latex?T%5E*%20%3D%20%5Carg%20%5Cmin%20_T%20%5C%7B%7C%5Ctheta_T%7C%20%5Cge%20E_0%5BT%5D%7C2P%5Bb_t%3D1%5D-1%7C%5C%7D)
  
### Volume/dollar imbalance bars
- sample bars when volume or dollar imbalance diverge from our expectations.
- the bar size is adjusted dynamically
- Procedure to determine T
  1. defined the tick imbalance at T as
  ![](https://latex.codecogs.com/gif.latex?%5Ctheta_T%20%3D%20%5Csum_%7Bt%3D1%7D%5ET%20b_tv_t)
  where ![](https://latex.codecogs.com/gif.latex?v_t) may represent either the number of securities traded(VIB) or the dollar amount exchanged(DIB). 
  2. we compute the expected value of ![](https://latex.codecogs.com/gif.latex?%5Ctheta_T) at the beginning of the bar
  ![](https://latex.codecogs.com/gif.latex?E_0%5B%5Ctheta_T%5D%3DE_0%5B%5Csum_%7Bt%7Cb_t%3D1%7D%5ET%20v_t%5D-E_0%5B%5Csum_%7Bt%7Cb_t%3D-1%7D%5ET%20v_t%5D%20%3D%20E_0%5BT%5D%28P%5Bb_t%3D1%5DE_0%5Bv_t%7Cb_t%3D1%5D-P%5Bb_t%3D-1%5DE_0%5Bv_t%7Cb_t%3D-1%5D%29)
  Let us denote
    - ![](https://latex.codecogs.com/gif.latex?v%5E&plus;%3DP%5Bb_t%3D1%5DE_0%5Bv_t%7Cb_t%3D1%5D)
    - ![](https://latex.codecogs.com/gif.latex?v%5E-%3DP%5Bb_t%3D-1%5DE_0%5Bv_t%7Cb_t%3D-1%5D)
    you can think of ![](https://latex.codecogs.com/gif.latex?v%5E&plus) and ![](https://latex.codecogs.com/gif.latex?v%5E&-) as decomposing the initial expectation of ![](https://latex.codecogs.com/gif.latex?v_t) into the component contributed by buys and the component contributed by sells, then 
    ![](https://latex.codecogs.com/gif.latex?E_0%5B%5Ctheta_T%5D%3D%20E_0%5BT%5D%28v%5E&plus;-v%5E-%29%20%3D%20E_0%5BT%5D%282v%5E&plus;-E_0%5Bv_t%5D%29)
    In practice we can estimate
    - ![](https://latex.codecogs.com/gif.latex?E_0%5BT%5D) as an exponentially weighted moving average of T values from prior bars
    - ![](https://latex.codecogs.com/gif.latex?%282v%5E&plus;-E_0%5Bv_t%5D%29) as an exponentially weighted moving average of ![](https://latex.codecogs.com/gif.latex?b_tv_t) values from prior bars
  3. we define VIB or DIB as a ![](https://latex.codecogs.com/gif.latex?T%5E*)-contiguous subset of ticks such that the following condition is met:
  ![](https://latex.codecogs.com/gif.latex?T%5E*%20%3D%20%5Carg%20%5Cmin_T%20%5C%7B%7C%5Ctheta_T%7C%5Cge%20E_0%5BT%5D%7C2v%5E&plus;-E_0%5Bv_t%5D%7C%5C%7D)
### Tick runs bars
### Volume/dollar runs bars
