# 假設檢定 Hypothesis testing 

## 常態性檢定

當我們想要知道一組樣本是否來自於常態分配時，我們可以做一些檢定 : 

- 虛無假設$H_0 :$ 分配來自常態**顯著**，對立假設$H_1 :$ 分配來自常態**不顯著**

### Shapiro-wilk 檢定法

R 有內建函式```shapiro.test(x = data)```
- x 即為你想要檢定的資料樣本，允許缺失值NA(數量必須介於3~5000)
- 最好用在樣本數小於50時
- 不適用於樣本值相同的數量很多時(與順序統計量有關)

```{r}
shapiro.test(rnorm(100, mean = 10, sd = 2))
shapiro.test(runif(100, min = 0, max = 10))
```
![](https://i.imgur.com/AonNc1b.png)

p-value 大於0.05則接受虛無假設，小於0.05則拒絕虛無假設

## 單一樣本平均檢定

### t.test
當小樣本來自**常態分配**且方差**未知**時，可以使用t.test
- 如果$X_1,\cdots,X_n \sim N(\mu,\sigma^2)$，則$\bar X \sim N(\mu, \frac{\sigma^2}{n})$
- 虛無假設$H_0 : \mu = \mu_0$，對立假設$H_1: \mu >\neq< \mu_0$
- 檢定統計量為$T = \frac{\bar X - \mu_0}{s/\sqrt{n}}$，在虛無假設下近似學生t分配有自由度$n-1$，$t_{n-1}$
```{r}
x <- rnorm(25, mean = 10, sd = 4)
shapiro.test(x)                             #t.test必須確定資料來自常態分配
t.test(x, mu = 5, alterenative = "two.sided", conf.level = 0.95)
```
![](https://i.imgur.com/ZVUKsGD.png)

資料來自常態分配 
p-value = 1.383e-06 < 0.05，拒絕虛無假設

### z.test

當小樣本來自**常態分配**且方差**已知**時，可以使用z.test
- 如果$X_1,\cdots,X_n \sim N(\mu,\sigma^2)$，則$\bar X \sim N(\mu, \frac{\sigma^2}{n})$
- 虛無假設$H_0 : \mu = \mu_0$，對立假設$H_1: \mu >\neq< \mu_0$
- 檢定統計量為$Z = \frac{\bar X - \mu_0}{\sigma/\sqrt{n}}$，在虛無假設下近似標準常態分配$N(0,1)$
R沒有內建z.test函式，需安裝套件```install.packages("BSDA")```
```{r}
library(BSDA)
x <- rnorm(25, mean = 10, sd = 4)
shapiro.test(x)
z.test(x, mu = 10, alternative = "two.sided", conf.level = 0.95, sigma = 4)
```
![](https://i.imgur.com/UxW89y5.png)

p-value = 0.2401 > 0.05，不拒絕虛無假設

當資料是**大樣本**時且方差**已知**，無論母體分配如何，可以使用z.test

## 單一樣本方差檢定

R 並沒有內建單一樣本方差檢定的函式，作檢定時，樣本必須來自**常態分配**
- 如果$X_1,\cdots,X_n \sim N(\mu,\sigma^2)$，則$\frac{(n-1)s^2}{\sigma^2} \sim \chi^2_{n-1}$
- 虛無假設$H_0 : \sigma^2 = \sigma^2_0$，對立假設$H_1: \sigma^2 >\neq< \sigma^2_0$
- 檢定統計量為$\chi^2 = \frac{(n-1)s^2}{\sigma^2}$，在虛無假設下近似卡方分配，有自由度$n-1$，$\chi_{n-1}^2$

## 單一樣本比例檢定

### prop.test

如果樣本來自於**伯努力試驗**，我們關心樣本試驗發生的比例是否與我們的猜想相同

- 如果$X_1,\cdots,X_n \sim Bernoulli(p)$，則$\bar X= \hat p \sim N(p,\frac{p(1-p)}{n})$
- 虛無假設$H_0 : p = p_0$，對立假設$H_1: p >\neq< p_0$
- 檢定統計量為$Z = \frac{\hat p - p_0}{\sqrt{\frac{p_0(1-p_0)}{n}}}$，在虛無假設下近似標準常態分配$N(0,1)$

```{python}
prop.test(x = 29, n = 50, p = 0.5, alternative = "greater", conf.level = 0.95)
#x為觀測值，n為試驗次數
```
![](https://i.imgur.com/4IgxWDW.png)

p-value = 0.1611 > 0.05，不拒絕虛無假設

## 單樣本無母數檢定

### wilcox.test

當我們發現樣本是來自**對稱連續分布**，但是我們不知道道母體分配時，可以用Wilcoxon-signed test
- $X_1,\cdots,X_n$來自連續機率分布，且以均值或中位數為對稱
- 虛無假設$H_0 : \mu = \mu_0$，對立假設$H_1: \mu >\neq< \mu_0$
- 檢定統計量為$W_+ = \sum_{i=1}^n rank(|x_i - \mu_0|)1_{x_i \geq \mu_0}$

```{python}
x <- c(66,78,87,90,100,112,135,140,144,145,146)
wilcox.test(x, mu = 80, alternative = "two.sided", conf.level = 0.95)
```
![](https://i.imgur.com/yA3zgQZ.png)

p-value = 0.009766 < 0.05，拒絕虛無假設

### SIGN.test

當我們的樣本不是來自常態分配時，我們可以使用SIGN.test

```{r}
x <- c(7.8, 6.6, 6.5, 7.4, 7.3, 7., 6.4, 7.1, 6.7, 7.6, 6.8)
SIGN.test(x, md = 6.5, alternative = "two.sided", conf.level = 0.95)
```
![](https://i.imgur.com/RHLD916.png)

p-value = 0.02148 < 0.05，拒絕虛無假設 



## 雙樣本平均數檢定

### z.test (無論樣本數)

如果$\sigma$已知，且兩組樣本為相互獨立，我們可以使用標準常態分配檢定

- 如果 $X \sim N(\mu_x,\sigma^2_x)$，$Y \sim N(\mu_y,\sigma^2_y)$，則$\bar X - \bar Y \sim N(\mu_x - \mu_y, \frac{\sigma^2_x}{n} + \frac{\sigma^2_y}{m})$
- 虛無假設$H_0 : \mu_x = \mu_y$，對立假設$H_1: \mu_x >\neq< \mu_y$
- 檢定統計量為$Z = \frac{\bar X - \bar Y - 0}{\sqrt{\frac{\sigma^2_x}{n} + \frac{\sigma^2_y}{m}}}$，在虛無假設下近似標準常態分配$N(0,1)$

```{python}
library(BSDA)
z.test(x = rnorm(100, mean = 5, sd = 2), y = rnorm(150, mean = 5.5, sd = 2.5), mu = 0,
       alternative = "two.sided", conf.level = 0.95, sigma.x = 2, sigma.y = 2.5)
```

![](https://i.imgur.com/uJjTyP1.png)

p-value = 0.0159 < 0.05，拒絕虛無假設

### z.test (大樣本)

在大樣本下，即使我們不知道$\sigma$，我們可以根據中央極限定理使用標準常態分配檢定

- 如果 $X \sim N(\mu_x,\sigma^2_x)$，$Y \sim N(\mu_y,\sigma^2_y)$，則$\bar X - \bar Y \sim N(\mu_x - \mu_y, \frac{\sigma^2_x}{n} + \frac{\sigma^2_y}{m})$
- 虛無假設$H_0 : \mu_x = \mu_y$，對立假設$H_1: \mu_x >\neq< \mu_y$
- 檢定統計量為$Z = \frac{\bar X - \bar Y - 0}{\sqrt{\frac{s^2_x}{n} + \frac{s^2_y}{m}}}$，在虛無假設下近似標準常態分配$N(0,1)$


### t.test (方差相等)

當樣本來自**常態分配**時，且$\sigma_x = \sigma_y$和樣本**相互獨立**時，我們可以使用t分配

- 如果 $X \sim N(\mu_x,\sigma^2_x)$，$Y \sim N(\mu_y,\sigma^2_y)$，則$\bar X - \bar Y \sim N(\mu_x - \mu_y, \frac{\sigma^2_x}{n} + \frac{\sigma^2_y}{m})$
- 虛無假設$H_0 : \mu_x = \mu_y$，對立假設$H_1: \mu_x >\neq< \mu_y$
- 檢定統計量為$t = \frac{\bar X - \bar Y - 0}{\sqrt{Spool^2}\sqrt{\frac{1}{m} + \frac{1}{n}}}$，在虛無假設下近似t分配，有自由度$n+m-2$，$t_{n+m-2}$
- $Spool^2 = \frac{(n-1)s^2_x + (m-1)s^2_y}{n+m-2}$

```{python}
t.test(x = rnorm(100, mean = 5, sd = 2), y = rnorm(150, mean = 5.5, sd = 2.5), mu = 0,
       alternative = "two.sided", conf.level = 0.95, var.equal = TRUE)
```
![](https://i.imgur.com/Z1fkOeM.png)

p-value = 0.2537 > 0.05，不拒絕虛無假設



### t.test (不知道方差是否相等)

當樣本來自**常態分配**時，且**我們不知道方差是否相等**和樣本**相互獨立**時，我們可以使用t分配

- 如果 $X \sim N(\mu_x,\sigma^2_x)$，$Y \sim N(\mu_y,\sigma^2_y)$，則$\bar X - \bar Y \sim N(\mu_x - \mu_y, \frac{\sigma^2_x}{n} + \frac{\sigma^2_y}{m})$
- 虛無假設$H_0 : \mu_x = \mu_y$，對立假設$H_1: \mu_x >\neq< \mu_y$
- 檢定統計量為$t = \frac{\bar X - \bar Y - 0}{\sqrt{Spool^2}\sqrt{\frac{1}{m} + \frac{1}{n}}}$，在虛無假設下近似t分配，有自由度$v$，$t_v$
- $Spool^2 = \frac{(n-1)s^2_x + (m-1)s^2_y}{n+m-2}$，$v = \frac{(\frac{s^2_x}{n} + \frac{s^2_y}{m})^2}{\frac{s^4_x}{n^2v_x} + \frac{s^2_y}{m^2v_y}}$，其中 $v_x = n - 1, v_y = m - 1$

```{python}
t.test(x = rnorm(100, mean = 5, sd = 2), y = rnorm(150, mean = 5.5, sd = 2.5), mu = 0,
       alternative = "two.sided", conf.level = 0.95, var.equal = FALSE)
```
![](https://i.imgur.com/ZiXIvVq.png)

p-value =  0.01117 < 0.05，拒絕虛無假設

### t.test (成對樣本平均數檢定)

當每一個樣本會產生兩筆資料$X$和$Y$，且樣本來自**常態分配**和**相互獨立**時，我們可以使用t分配

- 虛無假設$H_0 : \mu_x = \mu_y$，對立假設$H_1: \mu_x >\neq< \mu_y$
- 檢定統計量為$t = \frac{\bar X - \bar Y - 0}{\sqrt{s^2/n}}$，在虛無假設下近似t分配，有自由度$n-1$，$t_{n-1}$

```{python}
x <- rnorm(15, mean = 10, sd = 2)
y <- rnorm(15, mean = 17, sd = 1.2)
t.test(x, y, mu = 0, alternative = "two.sided", conf.level = 0.95, paired = TRUE)
```

![](https://i.imgur.com/UHN0Qq4.png)

p-value = 2.314e-09 < 0.05，拒絕虛無假設

## 雙樣本比例檢定

### t.test

當樣本來自伯努力分配且相互獨立時，我們關心兩樣本的平均比例是否有差異，可以使用z比例檢定
- 如果$X_1,\cdots,X_n \sim Bernoulli(p_x)，Y_1,\cdots,Y_n \sim Bernoulli(p_y)$，則$\bar X - \bar Y \sim N(p_x - p_y,p(1-p)(\frac{1}{n} + \frac{1}{m}))$
- 虛無假設$H_0 : p_x = p_y$，對立假設$H_1: p_x >\neq< p_y$
- 檢定統計量為$Z = \frac{\hat p_x - \hat p_y - 0}{\sqrt{\hat p (1 - \hat p)(\frac{1}{n} + \frac{1}{m})}}$，在虛無假設下近似標準常態分配$N(0,1)$
- 其中$\hat p$是$n+m$個樣本中的觀測值總數占比
- 大樣本時誤用t檢定結果差異不大

```{r}
x <- rbinom(p = 0.45, n = 40, size = 1)
y <- rbinom(p = 0.5, n = 50, size = 1)
t.test(x, y, mu = 0, alternative = "two.sided", conf.level = 0.95, var.equal = TRUE)
```
![](https://i.imgur.com/uEH835g.png)

p-value = 0.5146 > 0.05，不拒絕虛無假設


## 雙樣本方差檢定

### var.test

當樣本來自**常態分配**且**相互獨立**時，我們關心兩個樣本的方差是否相等，我們可以使用F檢定

- 如果 $X \sim N(\mu_x,\sigma^2_x)$，$Y \sim N(\mu_y,\sigma^2_y)$，則$\frac{(n-1)s^2_x}{n-1}/\frac{(m-1)s^2_y}{m-1} \sim \frac{\chi^2_{n-1}/(n-1)}{\chi^2_{m-1}/(m-1)} \sim F_{n-1,m-1}$
- 虛無假設$H_0 : \sigma^2_x = \sigma^2_y$，對立假設$H_1: \sigma^2_x >\neq< \sigma^2_y$
- 檢定統計量為$F = \frac{s^2_x}{s^2_y}$，在虛無假設下近似F分配，有自由度$n-1,m-1$，$F_{n-1,m-1}$

```{r}
x <- rnorm(15, mean = 10, sd = 2)
y <- rnorm(15, mean = 17, sd = 1.2)
var.test(x, y, alternative = "less", conf.level = 0.95)
```
![](https://i.imgur.com/70MKxXY.png)

p-value = 0.8033 > 0.05，不拒絕虛無假設


## 雙樣本無母數檢定(mu或md)

### wilcoxon.test (成對樣本)

當我們不知道相互獨立的**成對且形狀相同的樣本**是來自甚麼分配時，可以使用wilcoxon檢定

```{r}
x <- c(25,29,60,27)
y <- c(27,25,59,37)
wilcox.test(x - y, mu = 0, alternative = "two.sided", conf.level = 0.95, paired = TRUE)
```
![](https://i.imgur.com/XpeTUrI.png)

p-value = 0.875 > 0.05，不拒絕虛無假設

### wilcoxon.test (非成對樣本)

如果我們不知道樣本來自甚麼母體分配，且兩組樣本相互獨立，我們可以使用wilcoxon.test

```{r}
x <- c(0.80, 0.83, 1.89, 1.04, 1.45, 1.38, 1.91, 1.64, 0.73, 1.46)
y <- c(1.15, 0.88, 0.90, 0.74, 1.21)
wilcox.test(x, y, mu = 0, alterenative = "greater", conf.level = 0.95, exact = TRUE)
#小樣本使用精確性
```

![](https://i.imgur.com/JF2kuv9.png)

p-value = 0.2544 > 0.05，不拒絕虛無假設


### SIGN.test (成對樣本)

如果我們不知道樣本來自甚麼分配，且樣本是獨立且成對的來自同個分配時，我們可以使用SIGN.test

- $(x_1,y_1),\cdots,(x_n,y_n)$是來自同一個母體分配
- 虛無假設$H_0 : \mu_x = \mu_y$，對立假設$H_1: \mu_x >\neq< \mu_y$
- 檢定統計量為$K = \sum_{i =1}^{n}1_{x_i > y_i}$，在虛無假設下近似二項式分配，$Bin(n,0.5)$
R 沒有內建SIGN.test函式，需打開套件```library(BSDA)```

```{r}
library(BSDA)
x <- runif(15, min = 10, max = 25)
y <- runif(15, min = 0, max = 5)
SIGN.test(x, y, mu = 0, alternative = "two.sided", conf.level = 0.95)
```
![](https://i.imgur.com/UsxBfp6.png)







