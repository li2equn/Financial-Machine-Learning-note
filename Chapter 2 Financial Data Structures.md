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
- TIBs, VIBs, DIBs, monitor order flow imbalance, as measured in terms of ticks, volumns, and dollar values exchanged.
- TRBs, VRBs, DRBs:
Large traders will sweep the order book, use iceberg orders, or slice a parent order into multiple children, all of which leave a trace of runs in the ![](https://latex.codecogs.com/gif.latex?%5C%7Bb_t%5C%7D_%7Bt%3D1%2C...%2CT%7D) sequence. For this reason, it can be useful to monitor the sequence of buys in the overall volume, and take samples when that sequence diverges from our expectation
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
1. we define the length of the current run as

![](https://latex.codecogs.com/gif.latex?%5Ctheta_T%20%3D%20%5Cmax%5C%7B%5Csum_%7Bt%7Cb_t%3D1%7D%5ETb_t,-%5Csum_%7Bt%7Cb_t%3D-1%7D%5ETb_t%5C%7D)

2. we compute the expected value of ![](https://latex.codecogs.com/gif.latex?%5Ctheta_T) at the beginning of the bar

![](https://latex.codecogs.com/gif.latex?E_0%5B%5Ctheta_T%5D%20%3D%20E_0%5BT%5D%5Cmax%5C%7BP%5Bb_t%3D1%5D%2C1-P%5Bb_t%3D1%5D%5C%7D)

  In practice, we can estimate:
  - ![](https://latex.codecogs.com/gif.latex?E_0%5BT%5D) as an exponentially weighted moving average of T values from prior bars
  - ![](https://latex.codecogs.com/gif.latex?%282v%5E&plus;-E_0%5Bv_t%5D%29) as an exponentially weighted moving average of the proportioin of buy ticks from prior bars
  
3. we define a tick runs bar(TRB) as a ![](https://latex.codecogs.com/gif.latex?T%5E*)-contiguous subset of ticks such that the following condition is met:

![](https://latex.codecogs.com/gif.latex?T%5E*%20%3D%20%5Carg%20%5Cmin_T%20%5C%7B%5Ctheta_T%20%5Cge%20E_0%5BT%5D%5Cmax%5C%7BP%5Bb_t%3D1%5D%2C1-P%5Bb_t%3D1%5D%5C%7D%5C%7D)

- in this definition of runs, we allow for sequence breaks.
  - instead of measuring the length of the longest sequence, we count the number of ticks of each side, without offsetting them.

### Volume/dollar runs bars
- we wish to sample bars whenever the volumes or dollars traded by one side exceed our expectation for a bar.
- Determine the index T of the last observation in the bar
  1. we define the length of the current run as

![](https://latex.codecogs.com/gif.latex?%5Ctheta_T%20%3D%20%5Cmax%5C%7B%5Csum_%7Bt%7Cb_t%3D1%7D%5ET%20b_tv_t%2C%20-%5Csum_%7Bt%7Cb_t%3D-1%7D%5ET%20b_tv_t%5C%7D)

2. we compute the expected value of ![](https://latex.codecogs.com/gif.latex?%5Ctheta_T) at the beginning of the bar

![](https://latex.codecogs.com/gif.latex?E_0%5B%5Ctheta_T%5D%20%3D%20E_0%5BT%5D%5Cmax%5C%7BP%5Bb_t%3D1%5DE_0%5Bv_t%7Cb_t%3D1%5D%2C%281-P%5Bb_t%3D1%5D%29E_0%5Bv_t%7Cb_t%3D-1%5D%5C%7D)

  In practice, we can estimate:
  - ![](https://latex.codecogs.com/gif.latex?E_0%5BT%5D) as an exponentially weighted moving average of T values from prior bars
  - ![](https://latex.codecogs.com/gif.latex?%282v%5E&plus;-E_0%5Bv_t%5D%29) as an exponentially weighted moving average of the proportioin of buy ticks from prior bars
  - ![](https://latex.codecogs.com/gif.latex?E_0%5Bv_t%7Cb_t%3D1%5D) as an exponentially weighted moving average of the buy volumns from prior bars
  
3. we define a tick runs bar(TRB) as a ![](https://latex.codecogs.com/gif.latex?T%5E*)-contiguous subset of ticks such that the following condition is met:

![](https://latex.codecogs.com/gif.latex?T%5E*%20%3D%20%5Carg%20%5Cmin_T%20%5C%7B%5Ctheta_T%20%5Cge%20E_0%5BT%5D%5Cmax%5C%7BP%5Bb_t%3D1%5DE_0%5Bv_t%7Cb_t%3D1%5D%2C%281-P%5Bb_t%3D1%5D%29E_0%5Bv_t%7Cb_t%3D-1%5D%5C%7D)

# Dealing with Multi-Product Series
- modelling a time series of instruments, where the weights need to be dynamically adjusted over time.
- dealing with products that pay irregular coupons or dividends, or that are subject to corporate actions.
## The ETF Trick
We can model a basket of futures as if it was a signle non-expiring cash product.
produce a time series that reflects the value of $1 invested in a spread. Changes in the series will reflect changes in the PnL, the series will be strictly positive, and the implementation shortfall will be taken into account. This will be the series used to model, generate signals, and trade, as if it were an ETF.

Suppose that we are given a history of bars. These bars contain the following columns:
- ![](https://latex.codecogs.com/gif.latex?o_%7Bi%2Ct%7D) is the raw open price of instrument i =1,...,I at bar t = 1,...T
- ![](https://latex.codecogs.com/gif.latex?p_%7Bi%2Ct%7D) is the raw close price of instrument i =1,...,I at bar t = 1,...T
- ![](https://latex.codecogs.com/gif.latex?%5Cvarphi_%7Bi%2Ct%7D) is the USD value of one point of instrument i =1,...,I at bar t = 1,...T
- ![](https://latex.codecogs.com/gif.latex?v_%7Bi%2Ct%7D) is the volume of instrument of instrument i =1,...,I at bar t = 1,...T
- ![](https://latex.codecogs.com/gif.latex?d_%7Bi%2Ct%7D) is the carry, dividend, or coupon paid by instrument i ar bar t. This variable can also be used to charge margin costs, or costs of funding.
All instruments i = 1,...I were tradeable at bar t = 1,...T. In other words, even if some instruments were not tradeable over the entirely of the time interval [t-1,t], at least they were tradeable at times t-1 and t.

For a basket of futures characterized by an allocation vector ![](https://latex.codecogs.com/gif.latex?%5Comega_t) rebalanced(or rolled) on bars ![](https://latex.codecogs.com/gif.latex?B%20%5Csubseteq%20%5C%7B1%2C...T%5C%7D), the $1 investment value ![](https://latex.codecogs.com/gif.latex?%5C%7BK_t%5C%7D) is derived as

![](https://latex.codecogs.com/gif.latex?h_%7Bi%2Ct%7D%20%3D%20%5Cleft%5C%7B%5Cbegin%7Bmatrix%7D%20%5Cfrac%7B%5Comega_%7Bi%2Ct%7DK_t%7D%7Bo_%7Bi%2Ct&plus;1%7D%5Cvarphi_%7Bi%2Ct%7D%5Csum_%7Bi%3D1%7D%5EI%7C%5Comega_%7Bi%2Ct%7D%7C%7D%20%26%5Ctext%7Bif%7D%20%26t%20%5Cin%20B%20%5C%5C%20h_%7Bi%2Ct-1%7D%20%26%5Ctext%7Botherwise%7D%20%26%20%5Cend%7Bmatrix%7D%5Cright.)

![](https://latex.codecogs.com/gif.latex?%5Cdelta_%7Bi%2Ct%7D%20%3D%20%5Cleft%5C%7B%5Cbegin%7Bmatrix%7D%20p_%7Bi%2Ct%7D-o_%7Bi%2Ct%7D%20%26%5Ctext%7Bif%7D%20%26%5C%28t-1%5C%29%20%5Cin%20B%20%5C%5C%20%5CDelta%20p_%7Bi%2Ct%7D%20%26%5Ctext%7Botherwise%7D%20%26%20%5Cend%7Bmatrix%7D%5Cright.)

![](https://latex.codecogs.com/gif.latex?K_t%20%3D%20K_%7Bt-1%7D&plus;%5Csum_%7Bi%3D1%7D%5EIh_%7Bi%2Ct-1%7D%5Cvarphi_%7Bi%2Ct%7D%5C%28%5Cdelta_%7Bi%2Ct%7D&plus;d_%7Bi%2Ct%7D%5C%29)

- ![](https://latex.codecogs.com/gif.latex?K_0%3D1) in the initial AUM
- Variable ![](https://latex.codecogs.com/gif.latex?h_%7Bi%2Ct%7D) represents the hodlings(number of securities or contracts) of instrument i at time t.
- Variable ![](https://latex.codecogs.com/gif.latex?%5Cdelta_%7Bi%2Ct%7D) is the change of market value between t-1 and t for instrument i
- profts and losses are being reinvested whenever ![](https://latex.codecogs.com/gif.latex?t%20%5Cin%20B), hence preventing the negative prics.
- Dividends are already embedded in K_t so there is no need for the strategy to know about them

Let ![](https://latex.codecogs.com/gif.latex?%5Ctau_i) be the transaction cost associated with trading $1 od instrument t. There are three additional variables that the strategy need to know for every observed bar t:
1. Rebalance cost: ![](https://latex.codecogs.com/gif.latex?c_t%3D%5Csum_%7Bi%3D1%7D%5EI%5C%28%7Ch_%7Bi%2Ct-1%7D%7Cp_%7Bi%2Ct%7D&plus;%7Ch_%7Bi%2Ct%7D%7Co_%7Bi%2Ct&plus;1%7D%5C%29%5Cvarphi_%7Bi%2Ct%7D%5Ctau_i%2C%20%5Cforall%20t%20%5Cin%20B)
2. Bid-ask spread: ![](https://latex.codecogs.com/gif.latex?%5Ctilde%7Bc_t%7D%3D%5Csum_%7Bi%3D1%7D%5EIh_%7Bi%2Ct-1%7Dp_%7Bi%2Ct%7D%5Cvarphi_%7Bi%2Ct%7D%5Ctau_i)
When a unit is boutght or sold, the strategy must vharge this cost, which is equivalent to crossing the bid-ask spread of this cirtual ETF
3. Volume: ![](https://latex.codecogs.com/gif.latex?v_t%20%3D%20%5Cmin_i%5C%7B%5Cfrac%7Bv_%7Bi%2Ct%7D%7D%7B%7Ch_%7Bi%2Ct-1%7D%7C%7D%5C%7D)

Transaction cost are not necessarily linear, and those non-linear costs can be simulated by the strategy based on the above information
## PCA Weights
Consider an IID multivariate Gaussian process characterized by a vector of means \mu, of size Nx1, and a covariance matrix V, of size NxN. This stochastic process describes an invariant random variable(like the returns of stocks, the changes in yield of bonds, or changes in options' volatilities) for a portfolio of N instruments. We would like to compute the vector of allocation w that conforms to a particular distribution of risks across V's principal components.

1. We perform a spectral decomposition ![](https://latex.codecogs.com/gif.latex?VW%3DV%5CLambda), where the columns in W are reordered so that the elements of ![](https://latex.codecogs.com/gif.latex?%5CLambda)'s diagonal are sorted in descending order
2. Given a vector of alloction w, we can compute the portfolio's risk as ![](https://latex.codecogs.com/gif.latex?%5Csigma%5E2%20%3D%20%5Comega%27V%5Comega%20%3D%20%5Comega%27W%5CLambda%20W%27%5Comega%3D%5Cbeta%27%20%5CLambda%20%5Cbeta%20%3D%20%28%5CLambda%5E%7B1/2%7D%20%5Cbeta%29%27%28%5CLambda%5E%7B1/2%7D%20%5Cbeta%29), where ![](https://latex.codecogs.com/gif.latex?%5Cbeta) represents the projection of w on the orthogonal basis
3. is a diagonal matrix, thus ![](https://latex.codecogs.com/gif.latex?%5Csigma%5E2%20%3D%20%5Csum_%7Bn%3D1%7D%5EN%5Cbeta%5E2_n%5CLambda_%7Bn%2Cn%7D) and the risk attributed to the nth component is ![](https://latex.codecogs.com/gif.latex?R_n%3D%5Cbeta_n%5E2%5CLambda_%7Bn%2Cn%7D%5Csigma%5E%7B-2%7D%20%3D%20%5BW%27%5Comega%5D_n%5E2%5CLambda_%7Bn%2Cn%7D%5Csigma%5E%7B-2%7D). You can interpret ![](https://latex.codecogs.com/gif.latex?%5C%7BR_n%5C%7D_%7Bn%3D1%2C...%2CN%7D) as the distribution of risks across the orthogonal components.
4. we would like to compute the vector w that delivers a usder-defined risk distribution R. From earlier steps, ![](https://latex.codecogs.com/gif.latex?%5Cbeta%20%3D%20%5C%7B%5Csigma%5Csqrt%7B%5Cfrac%7BR_n%7D%7B%5CLambda_%7Bn%2Cn%7D%7D%7D%5C%7D_%7Bn%3D1%2C...%2CN%7D), which represents the allocation in the new orthogonal basis.
5. the allocation in the old basis is given by ![](https://latex.codecogs.com/gif.latex?%5Comega%20%3D%20W%20%5Cbeta) Re-scaling w merely re-scales ![](https://latex.codecogs.com/gif.latex?%5Csigma), hence keeping the risk distribution constant
```python
def pcaWeights(cov, RiskDist = None, riskTarget=1.):
    eVal,eVec = np.linalg.eigh(cov)#must be Hermitian
    indices = eVal.argsort()[::-1] # sorting eVal desc
    eVal,eVec = eVal[indices],eVec[:,indices]
    if riskDist = None:
        riskDist = np.zeros(cov.shape[0])
        riskDist[-1]=1
    loads = riskTarget*(riskDist/eVal)**.5
    wghts = np.dot(eVec,np.reshape(loads,(-1,1)))
    return wghts
```
## Single Future Roll

# Sampling Features
