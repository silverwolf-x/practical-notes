
ARIMA建模中，往往需要通过ADF检验确定差分平稳的阶数。
ADF检验中，重要的一步是确定滞后阶数p，然后计算检验统计量$\gamma=a_1+\cdots+a_p-1$

> 为了使检验能适用于AR(p)过程的平稳性检验，人们对检验进行了一定的修正，得到增广检验（Augmented Dickey－Fuller），简记为ADF检验

但是，不同的程序中默认设定的参数p不同，导致最后的平稳性的判断不一致。

## 案例背景

题目来自教材习题5.4中发人口出生率。

### statsmodels(python)
```python
from statsmodels.tsa.stattools import adfuller
ts = [18.21, 20.91, 22.28, 20.19, 19.9 , 21.04, 22.43, 23.33, 22.37, 21.58, 21.06, 19.68, 18.24, 18.09, 17.7 , 17.12, 16.98, 16.57, 15.64, 14.64, 14.03, 13.38, 12.86, 12.41, 12.29, 12.4 , 12.09, 12.1 , 12.14, 11.95, 11.9 , 11.93, 12.1 , 12.08, 12.37, 12.07, 12.95, 12.45]

print(adfuller(timeseries, autolag="AIC", store=True)[-1].__dict__)
```

> {'resols': <statsmodels.regression.linear_model.RegressionResultsWrapper at 0x1bbe5f9aff0>,
>  'maxlag': 10,
>  'usedlag': 9,
>  =='adfstat': -4.513910481964258,==
>  'critvalues': {'1%': -3.6889256286443146,
>   '5%': -2.9719894897959187,
>   '10%': -2.6252957653061224},
>  'nobs': 28,
>  'H0': 'The coefficient on the lagged level equals 1 - unit root',
>  'HA': 'The coefficient on the lagged level < 1 - stationary',
>  'icbest': 3.274087515710903,
>  '_str': 'Augmented Dickey-Fuller Test Results'}

 
这里说明ADF检验0阶平稳，取lag=9 ^a729fe
 
### pmdarima(python) == tseries(R)
pmdarima是tseries这个R包的python版本，程序实现方式和所用默认参数一致
```python
from pmdarima.arima.stationarity import ADFTest
from pmdarima.arima.utils import ndiffs
adf_test = ADFTest(alpha=0.05)
print(adf_test.should_diff(ts))
n_adf = ndiffs(ts, test='adf')
print(n_adf)
```

> (0.4446150727323349, True)
> 2

```r
library(tseries)
ts <- c(18.21, 20.91, 22.28, 20.19, 19.9, 21.04, 22.43, 23.33, 22.37, 21.58, 21.06, 19.68, 18.24, 18.09, 17.7, 17.12, 16.98, 16.57, 15.64, 14.64, 14.03, 13.38, 12.86, 12.41, 12.29, 12.4, 12.09, 12.1, 12.14, 11.95, 11.9, 11.93, 12.1, 12.08, 12.37, 12.07, 12.95, 12.45)
ts <- ts(ts)
adf_test <- adf.test(ts)
print(adf_test)
```

> Augmented Dickey-Fuller Test
> data:  ts
> ==Dickey-Fuller = -2.3293, Lag order = 3, p-value = 0.4446==
> alternative hypothesis: stationary

这里说明ADF检验0阶不平稳，差分到2阶平稳。取lag=3 ^e5623a


### aTSA(R)
**教材使用的包**
```r
library(aTSA)
ts <- c(18.21, 20.91, 22.28, 20.19, 19.9, 21.04, 22.43, 23.33, 22.37, 21.58, 21.06, 19.68, 18.24, 18.09, 17.7, 17.12, 16.98, 16.57, 15.64, 14.64, 14.03, 13.38, 12.86, 12.41, 12.29, 12.4, 12.09, 12.1, 12.14, 11.95, 11.9, 11.93, 12.1, 12.08, 12.37, 12.07, 12.95, 12.45)
ts <- ts(ts)
adf_test <- adf.test(ts)
```


> Augmented Dickey-Fuller Test 
> alternative: stationary 
> Type 1: no drift no trend 
>       lag   ADF p.value
>  [1,]   0 -1.16  0.2597
>  [2,]   1 -1.87  0.0609
>  [3,]   2 -2.43  0.0181
>  [4,]   3 -1.16  0.2564
>  [5,]   4 -1.50  0.1361
>  [6,]   5 -2.10  0.0378
>  [7,]   6 -3.12  0.0100
>  [8,]   7 -3.94  0.0100
>  [9,]   8 -1.05  0.2984
>  [10,]   9  0.349  0.7375
> Type 2: with drift no trend 
>       lag    ADF p.value
>  [1,]   0 -0.442  0.8817
>  [2,]   1 -1.249  0.6012
>  [3,]   2 -1.029  0.6775
>  [4,]   3 -0.765  0.7694
>  [5,]   4 -1.175  0.6268
>  [6,]   5 -1.962  0.3439
>  [7,]   6 -2.663  0.0939
>  [8,]   7 -4.577  0.0100
>  [9,]   8 -3.849  0.0100
>  ==[10,]   9 -4.514  0.0100==
> Type 3: with drift and trend 
>       lag    ADF p.value
>  [1,]   0 -2.335   0.430
>  [2,]   1 -1.619   0.715
>  [3,]   2 -0.789   0.954
>  ==[4,]   3 -2.329   0.432==
>  [5,]   4 -2.016   0.554
>  [6,]   5 -1.600   0.723
>  [7,]   6 -0.769   0.956
>  [8,]   7  0.654   0.990
>  [9,]   8 -0.457   0.979
>  [10,]   9 -2.126   0.510
> Note: in fact, p.value = 0.01 means p.value <= 0.01

可以认为在某个type的某个lag下0阶平稳，但整体来看很难判断






## 源码与讨论

三个程序的默认参数对比如下

|参数/选项|statsmodels|pmdarima/tseries|aTSA|
| --- | --- | --- | --- |
|默认模式|constant only<br>可选`{“c”,”ct”,”ctt”,”n”}`|constant and a linear trend<br>无法更改|直接输出`no drift no trend`<br>`with drift no trend`<br>`with drift with trend`三种模式|
|默认lag定阶|AIC<br>可选`{“AIC”, “BIC”, “t-stat”, None(直接取maxlag)}`|直接取`maxlag` |自己看自己选|
|默认`maxlag`|$12(\frac{n}{100})^{1/4}$|$(n-1)^{1/3}$|$4(\frac{n}{100})^{2/9}$|
|网站|[statsmodels](https://www.statsmodels.org/dev/generated/statsmodels.tsa.stattools.adfuller.html)|[tseries](https://cran.r-project.org/web/packages/tseries/tseries.pdf)|[adf.test](https://www.rdocumentation.org/packages/aTSA/versions/3.1.2.1/topics/adf.test)

在aTSA的结果中，高亮部分分别对应statsmodels和tseries的检验结果。上述默认参数表得到验证。


> [!NOTE] [Augmented Dickey–Fuller test - Wikipedia](https://en.wikipedia.org/wiki/Augmented_Dickey%E2%80%93Fuller_test)
> By including lags of the order _p_ the ADF formulation allows for higher-order autoregressive processes. This means that the lag length _p_ has to be determined when applying the test. One possible approach is to test down from high orders and examine the [_t_-values](https://en.wikipedia.org/wiki/T-value "T-value") on coefficients. An alternative approach is to examine information criteria such as the [Akaike information criterion](https://en.wikipedia.org/wiki/Akaike_information_criterion "Akaike information criterion"), [Bayesian information criterion](https://en.wikipedia.org/wiki/Bayesian_information_criterion) or the [Hannan–Quinn information criterion](https://en.wikipedia.org/wiki/Hannan%E2%80%93Quinn_information_criterion "Hannan–Quinn information criterion").  
> 通过包含 p 阶滞后，ADF 公式允许更高阶的自回归过程。这意味着在应用测试时必须确定滞后长度 p。一种可能的方法是从高阶开始测试并检查系数的 t 值。另一种方法是检查信息准则，例如 Akaike 信息准则、贝叶斯信息准则或 Hannan-Quinn 信息准则。

- 根据Wikipedia，从自动化的角度看，statsmodels的做法更加通用：在$p=0,\cdots,maxlag$范围中分别建模，以AIC准则选择最优AR(p)模型，然后用这个p阶数的ADF检验统计量，判断序列平稳性。
- tseries的做法很冒险，直接选定lag = $(n-1)^{1/3}$有点一视同仁
- aTSA最全面，即”程序只负责计算，判断由人决定“。但新手难以做出判断，陷入摸棱两可的境地

## 提问
1. 为了确定ARIMA**差分**的阶数，即?阶差分平稳，哪个包的源码逻辑更加符合统计学思维呢？当这些包的结论冲突时（如[案例背景](#案例背景) 中的[statsmodels(python)](#statsmodels(python))和[pmdarima(python) == tseries(R)](#pmdarima(python)%20==%20tseries(R)) ），应该怎么处理呢？
2. 从ARIMA建模视角看，我认为可以跳过平稳性检验，直接遍历ARIMA(0-5,0-5,0-5)共$5^3$=125个模型，或者贝叶斯搜参的方式，以某个信息准则（如AIC）为标准，看哪个模型拟合效果最好。抛开计算资源需求的问题，这种思路可行吗？


## 附录：ACF和PACF
左列:ACF;右列:PACF
第一行:原序列
第二行:一阶差分
第三行:二阶差分
> [!R]
> ![](Pasted%20image%2020240516222516.png)

> [!python]
>![](Pasted%20image%2020240516222158.png)

> 注意到R的PACF横轴从1开始,而python的PACF横轴从0开始.在计算上,python和R的数值相同

从图可知,ARIMA(1,0,0)是一个不错的选择.据此我们可以下结论"0阶平稳"吗!?
