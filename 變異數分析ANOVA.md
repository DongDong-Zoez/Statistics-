# 變異數分析 ANOVA

## 單因子變異數分析

當我們期望檢定多組數據平均數是否相等時，我們手上的工具只有學生t分配檢定，但是t檢定只能兩兩相互比較，當比較數量很大時(n)，我們總共需要$\binom{n}{2}$次比較，過程相當冗長，所以我們引進一種新的檢定方法，**單因子變異數分析**檢定法

## 何謂"因子" ?

因子指的是在試驗中會影響反應變數的變數，例如我們想知道不同大學大學生的身高，則我們給定一個大學後會有一組來自該大學大學生的身高資料，則大學為因子，而身高資料則是反應變數

## 原理

- 我們想檢定群$1,\cdots,k$之間的平均是否有不同，需要有三個條件
1. 群之間必須是相互獨立的 
2. 近似常態分配(詳見 [假設檢定 Hyphothesis testing](https://hackmd.io/M7ti5jX0RZqC41sWiyXYJg) 常態性檢定)
3. 各組的方差相等 $(\sigma_1^2 = \sigma_2^2 = \cdots =  \sigma_k^2)$
- 虛無假設$H_0 : \mu_1 = \mu_2  =\cdots =  \mu_k$，對立假設$H_1 :$ 存在一組 $\mu_i \neq \mu_j$

:::info
:notebook_with_decorative_cover: 即使我們有顯著的證據拒絕虛無假設，我們仍不知道有哪些群的平均與其他群不同
:::

- 檢定統計量為 $F = \frac{MSG}{MSE}$，$MSG$為組間方差和，$MSE$為組內方差和，在虛無假設下近似$F_{k-1,N-k}$，$k-1$為$SSG$的自由度，$N-k$為$SSE$的自由度
- 我們期望組內方差和與組間方差和相近，換句話說，我們期望比值為$1$
- $F$ 越大，則 p-value 越小，越要拒絕虛無假設

## 資料表格

1. 為了更方便我們視覺化處理資料，我們把資料放成表格如下 : 



|    群    |    1     | 2        | $\cdots$ |   $i$    | $\cdots$ |    k     |
|:--------:|:--------:| -------- | -------- |:--------:|:--------:|:--------:|
| 觀測值$1$  | $x_{11}$ | $x_{21}$ | $\cdots$ | $x_{i1}$ | $\cdots$ | $x_{k1}$ |
| 觀測值$2$  | $x_{12}$ | $x_{22}$ | $\cdots$ | $x_{i2}$ | $\cdots$ | $x_{k2}$ |
| $\vdots$ | $\vdots$ | $\ddots$ | $\ddots$ | $\vdots$ | $\vdots$ | $\vdots$ |
| 觀測值$j$  | $x_{1j}$ | $x_{2j}$ | $\cdots$ | $x_{ij}$ | $\cdots$ | $x_{kj}$ |
| $\vdots$ | $\vdots$ | $\ddots$ | $\ddots$ | $\vdots$ | $\vdots$ | $\vdots$ |
| 觀測值$n_j$  | $x_{1n_1}$ | $x_{2n_2}$ | $\cdots$ | $x_{in_i}$ | $\cdots$ | $x_{kn_k}$ |

或是我們可以用以下形式表示(有可能非方陣) : 

\begin{bmatrix}
 1 & 2 & \cdots & i & \cdots & k\\
 x_{11} & x_{21} & \cdots & x_{i1} & \cdots & x_{k1}\\
 x_{12} & x_{22} & \cdots & x_{i2} & \cdots & x_{k2}\\
 \vdots &  \vdots & \ddots &  \ddots & \vdots & \vdots\\
 x_{1j} & x_{2j} & \cdots & x_{ij} & \cdots & x_{ki}\\
 \vdots &  \vdots & \ddots &  \ddots & \vdots & \vdots\\
 x_{1n_1} & x_{2n_2} & \cdots & x_{in_i} & \cdots & x_{kn_k}
\end{bmatrix}

2. 利用表格計算資料 (ANOVA Table) : 

| 標題 | 自由度 | SS | MS | F |
| -------- | -------- | -------- | --- | --- |
| 組間$(G)$  |  $k-1$ |   $SSG = \sum_{i=1}^{k}n_i(\large\bar X_i - \large\bar X)^2$   |  $MSG = \frac{SSG}{k-1}$   |   $\frac{MSG}{MSE}$  |
| 組內$(E)$  |  $N-k$ |    $SSE = \sum_{i=1}^{k}\sum_{j=1}^{n_i}\large({x_{ij} - \bar X_i})^2$      |  $MSE = \frac{SSE}{N-k}$   |     |
| 總和$(T)$  |  $N-1$ |  $SST = \sum_{i=1}^{k}\sum_{j=1}^{n_i}\large({x_{ij} - \bar X})^2$   |     |     |

- $x_{ij}$ 即為第 $i$ 群的第 $j$ 個觀測值
- $N = \sum_{i=1}^{k}n_i$，為總共的觀測值個數
- $\bar X_i = \sum_{j=1}^{n_j}x_{ij}/n_i$ 即第 $i$ 群的觀測值平均，$\bar X = \sum_{i=1}^{k}\sum_{j=1}^{n_i}x_{ij}$，即所有觀測值的平均
- $SST = SSG + SSE$，即 總方差=組間方差+組內方差
- 我們注意到$F$分配的檢定統計量定義為$F = \frac{s_x^2}{s_y^2}\iff \frac{\large\chi^2_{k-1}/(k-1)}{\large\chi^2_{N-k}/(N-k)}$，事實上
1. $SST = N\cdot Var(x_{ij})$
2. $SSE = \sum_{i=1}^kn_iVar(x_{i\cdot})$，在虛無假設下$MSE \sim \chi^2_{N-k}$
3. $SSG = \sum_{i=1}^kn_i(\bar X_i - \bar X)^2$，在虛無假設下$MSG \sim \chi^2_{k-1}$
4. 所以在虛無假設下$\frac{MSG}{MSE} \sim F_{k-1,N-k}$

## 事後檢定 Post-hoc

當我們拒絕了虛無假設後，我們會想要知道究竟是哪些群平均與其他不同，我們可以用T檢定

### Bonferroni correction

如果虛無假設$H_0 : \mu_1 = \mu_2 = \cdots = \mu_k$，則T檢定總共要做$\binom{k}{2}$次，假設我們要比較群$X_i$與$X_j$

1. 如果$\sigma_i$和$\sigma_j$已知，則$Var(\bar X_i - \bar X_j) = Var(\bar X_i) + Var(\bar X_j) = \frac{\sigma_i^2}{n_i} + \frac{\sigma_j^2}{n_j}$
2. 如果$\sigma_i$和$\sigma_j$未知，則$Var(\bar X_i - \bar X_j) = Var(\bar X_i) + Var(\bar X_j) = \frac{s_i^2}{n_i} + \frac{s_j^2}{n_j}$
3. 如果$\sigma_i$和$\sigma_j$已知相等，則$Var(\bar X_i - \bar X_j) = Var(\bar X_i) + Var(\bar X_j) = s^2(\frac{1}{n_i} + \frac{1}{n_j})$，
其中$s^2 = \frac{(n_i - i)s_i^2 + (n_j - i)s_j^2}{n_i+n_j-2}$
- 在ANOVA中我們假設方差相等，且$s = \sqrt{MSE}$，型一誤差為$\alpha_k = \frac{\alpha}{\binom{k}{2}}$，其中$\alpha$為$F$檢定的顯著水準
- 虛無假設為$H_0 : \mu_i = \mu_j$，對立假設為$H_1 : = \mu_i \neq \mu_j$
- 檢定統計量為$t = \frac{\bar X_i - \bar X_j - 0}{\sqrt{MSE(\frac{1}{n_i}+\frac{1}{n_j})}}$，在虛無假設下近似$t_{N-k}$分配，有自由度$N-k$
- 信賴區間為 $X_i - X_j \pm t_{\alpha_k}\sqrt{MSE(\frac{1}{n_i}+\frac{1}{n_j}})$

### Scheffé method

Scheffé法與Bonferroni法大致上相同，我們注意到 $t_{n-1} = \frac{z}{\sqrt{\frac{\chi^2}{n-1}}} \to t_{n-1}^2 = \frac{\chi^2(n-1)}{\chi^2_{n-1}} \sim F_{1,n-1}$，事實上Scheffé法就是把Bonferroni法的顯著水準$\alpha_k$改成$\sqrt{(k-1)F_{k-1,N-k,2\alpha_k}}$

- 虛無假設為$H_0 : \mu_i = \mu_j$，對立假設為$H_1 : = \mu_i \neq \mu_j$
- 檢定統計量為$F = (\frac{\bar X_i - \bar X_j - 0}{\sqrt{MSE(\frac{1}{n_i}+\frac{1}{n_j})}})^2$，在虛無假設下近似$F$分配，有自由度$k-1,N-k$
- 信賴區間為 $X_i - X_j \pm \sqrt{(k-1)F_{k-1,N-k,2\alpha_k}}\sqrt{MSE(\frac{1}{n_i}+\frac{1}{n_j}})$

### 最小顯著性差異法 LSD (Least Significant Difference)

顧名思義，即使兩組平均有些微差異一樣會檢定出來，但是他不會調整多重比較後的型一誤差(即虛無假設為假接受虛無假設之機率)，使得我們犯型一誤差的機率較其他事後比較方法高，其概念類似於逐一對兩樣本做T檢定

- 虛無假設為$H_0 : \mu_i = \mu_j$，對立假設為$H_1 : = \mu_i \neq \mu_j$
- 檢定統計量為$t = \frac{\bar X_i - \bar X_j - 0}{\sqrt{MSE(\frac{1}{n_i}+\frac{1}{n_j})}}$，在虛無假設下近似$t_{N-k}$分配，有自由度$N-k$
- 信賴區間為 $X_i - X_j \pm t_{\frac{\alpha}{2}}\sqrt{MSE(\frac{1}{n_i}+\frac{1}{n_j}})$
- 顯著水準默認為 $0.05$

:::warning
此檢定法會堆疊型一誤差的發生概率，實作上我們不常用此方法做事後比較
:::

### TukeyHSD Test 成對樣本事後比較

TukeyHSD Test 又稱誠實顯著差異性檢定，除了要求獨立、同質、常態性之外，還要要求每一組的**樣本數量相同**

- equal sample size 
- 虛無假設為$H_0 : \mu_i = \mu_j$，對立假設為$H_1 : = \mu_i \neq \mu_j$
- 檢定統計量為$T = q_{\alpha,k,N-k}\sqrt{\frac{MSE}{n_i}}$，其中$q$為Tukey值，可以查Tukey Table，$n_i$為抽樣數量
- $q$ 統計量是組間最大平均值檢最小平均值再除以組間平均值的標準差

:::success
Tukey檢定法的檢定力沒有其他方法來的高，而且無法處理複雜問題
:::

### Dunnetts's Test

- 虛無假設為$H_0 : \mu_i = \mu_j$，對立假設為$H_1 : = \mu_i \neq \mu_j$
- 檢定統計量為$D = t\sqrt{\frac{MSE}{n_i}}$，其中$t$為Dunnetts值，可以查Dunnett Table，$n_i$為抽樣數量

## R語言實現

- ```~```為運算子，放在反應變數與解釋變數之間，```反應 ~ 解釋 + 解釋```

```{R}
library(DescTools)                                                 #Dunnett's Test      
library(agricolae)                                                 #Scheffé's Method
apple <- read.csv(file = "C:/Users/ASUS/Desktop/rdata/apple.csv")  #匯入資料
appleData <- stack(apple, c("apple1", "apple2", "apple3"))         #堆疊資料
names(appleData) <- c("apple", "group")                            #為資料標題命名
leveneTest(apple ~ group, data = appleData)                        #檢定同質性方法一
bartlett.test(apple ~ group, data = appleData)                     #檢定同質性方法二
shapiro.test(apple$apple1)                                         #常態性檢定資料apple1
shapiro.test(apple$apple2)                                         #常態性檢定資料apple2
shapiro.test(apple$apple3)                                         #常態性檢定資料apple3
apple_aov <- aov(apple ~ group, data = appleData)                  #aov變異數分析方法一
oneway.test(apple ~ group, data = appleData, var.equal = TRUE)     #aov變異數分析方法二
summary(apple_aov)                                                 #查看ANOVA_Table
scheffe.test(apple_aov, trt = "group", group = FALSE, console = TRUE)  #Scheffé法
bf_apple <- LSD.test(apple_aov, trt = "group", p.adj = "bonferroni")   #Bonferroni法
TukeyHSD(apple_aov)                                                    #TukeyHSD法
holm_apple <- LSD.test(apple_aov, trt = "group", p.adj = "holm")       #Holm法
DT_apple <- DunnettTest(x = appleData$apple, g = appleData$group)      #Dunnett法
```

## 雙因子變異數分析

當我們的因子變數兩個的時候，我們可以使用雙因子變異數分析，其數學概念與單因子變異數分析類似，我們也需要有以下前提假設

1. 獨立性 : 各組因子之間是相互獨立的
2. 常態性 : 假設每組都是來自常態分配
3. 同質性 : 每組之間具有相同的變異數

### 資料表格

1. 這裡假設每一組都抽出同樣數量的樣本$(K)$ : 

| 因子2/因子1 |            1            |            2            | $\cdots$ |            I            |
|:-----------:|:-----------------------:|:-----------------------:| -------- |:-----------------------:|
|     $1$     | $x_{111}\cdots x_{11K}$ | $x_{211}\cdots x_{21K}$ | $\cdots$ | $x_{I11}\cdots x_{I1K}$ |
|     $2$     | $x_{121}\cdots x_{12K}$ | $x_{221}\cdots x_{22K}$ | $\cdots$ | $x_{I21}\cdots x_{I2K}$ |
|  $\vdots$   |        $\vdots$         |        $\vdots$         | $\ddots$ |        $\vdots$         |
|     $J$     | $x_{1J1}\cdots x_{1JK}$ | $x_{2J1}\cdots x_{2JK}$ | $\cdots$ | $x_{IJ1}\cdots x_{IJK}$ |

2. 利用表格計算資料(Anova Table)



| Column 1 | Column 2 | Column 3 |
| -------- | -------- | -------- |
| Text     | Text     | Text     |



### 因子變數

在單因子變異數分析時，我們有$x_{ij} = \mu + \alpha_i + \epsilon_{ij}$，其中$\mu$為整體平均，$\alpha_i$為第$i$組與整體平均之差，$\epsilon_{ij}$則為第$i$群的第$j$個觀測值與$\mu$和$\alpha_i$之和的差，我們稱$\alpha_i$為因子，當然，我們可以有等價的虛無假設

- 虛無假設$H_0 : \alpha_1 = \alpha_2  =\cdots =  \alpha_k$，對立假設$H_1 :$ 存在一組 $\alpha_i \neq \alpha_j$ 

在雙因子變異數分析也有類似結論，$x_{ijk} = \mu + \alpha_i + \beta_j + \delta_{ij} + \epsilon_{ijk}$，其中
1. $\mu = \bar X$
2. $\alpha_i = \bar X_i - \bar X$，且 $\sum_i \alpha_i = 0$
3. $\beta_j = \bar X_j - \bar X$，且 $\sum_j \beta_j = 0$
4. $\delta_{ij} = \bar X_{ij} - \bar X_i - \bar X_j + \bar X$，且 $\sum_i \delta_{ij} = \sum_j \delta_{ij} = 0$
5. $\epsilon_{ijk}$為殘差值，且有$\epsilon_{ijk} \sim N(0,\sigma^2)$

- 虛無假設有以下三條，對立假設$H_1$為至少有一組因子變數不相等
  1. $H_0 : \alpha_i = 0,\space i = 1,\cdots,I$，即因子1不影響
  2. $H_0 : \beta_j = 0,\space j = 1,\cdots,J$，即因子2不影響
  3. $H_0 : \delta_{ij} = 0,\space i = 1,\cdots,I,\space j = 1,\cdots,J$，即因子間沒有交互作用

## R語言實現

```{r}
data <- read.table(header = TRUE, text = "                     #讀資料
 id Sex     Genotype  Activity
  1 male    ff        1.884
  2 male    ff        2.283
  3 male    fs        2.396
  4 female  ff        2.838  
  5 male    fs        2.956
  6 female  ff        4.216
  7 female  ss        3.620
  8 female  ff        2.889  
  9 female  fs        3.550
 10 male    fs        3.105
 11 female  fs        4.556
 12 female  fs        3.087
 13 male    ff        4.939
 14 male    ff        3.486
 15 female  ss        3.079
 16 male    fs        2.649
 17 female  fs        1.943
 19 female  ff        4.198
 20 female  ff        2.473
 22 female  ff        2.033 
 24 female  fs        2.200
 25 female  fs        2.157
 26 male    ss        2.801
 28 male    ss        3.421
 29 female  ff        1.811
 30 female  fs        4.281
 32 female  fs        4.772
 34 female  ss        3.586
 36 female  ff        3.944
 38 female  ss        2.669
 39 female  ss        3.050
 41 male    ss        4.275 
 43 female  ss        2.963
 46 female  ss        3.236
 48 female  ss        3.673
 49 male    ss        3.110
")
interaction.plot(data$Sex, data$Genotype, data$Activity)        #劃出交互作用圖
aov_data <- aov(Activity ~ Sex * Genotype, data = data)         #雙因子變異數分析方法一
anova_model <- lm(Activity ~ Sex * Genotype, data = data)       #雙因子變異數分析方法二
Anova(anova_model)                                              #雙因子變異數分析方法二
aov_residuals <- residuals(object = aov_data)                   #利用residuals計算殘差值
shapiro.test(aov_residuals)                                     #檢定殘差值是否為常態分配
leveneTest(Activity ~ Sex * Genotype, data = data)              #檢定同質性
summary(aov_data)                                               #查看Anova_Table
TukeyHSD(aov_data)                                              #事後檢定
```

## 隨機區組設計 RBD (Randomized Block Design)

### 試驗三大守則

為了使試驗是公正且具可靠性的，Fisher在1925年提出實驗設計的三大標竿，隨機化(Randomization)、重複化(Replication)、區組化(Blocking)

1. **隨機化**，隨機分配每個試驗單位，合理的分配會使得該試驗具有獨立性
2. **重複化**，即任意群至少含有兩個或以上的觀測值，重複化可以最大化觀測值的可信度
3. **區組化**，依據不同的劃分方法來區別不同類型的試驗樣本，目的在於精確化結果

在試驗上，因子分為**可控**與**不可控**因子，即經由人為可以影響的試驗因子稱為可控，舉例來說，我們想要知道不同年齡的狗吃不同的飼料是否對跑步速度有所提升，在此例中，因子為飼料以及年齡，年齡不是人為可控因素，所以年齡為不可控因子

### 使用時機

當我們在處理雙因子試驗時，如果兩個因子都是可控因子，則我們使用雙因子變異數分析，如果其中一個為不可控因子時，則使用隨機區處組設計

## 原理

隨機區組設計是用來檢驗**處理影響**和**區組影響**是否存在，將有著相似的可控因子試驗樣本先分到不同區組，再從各區組隨機分配到不同的處理組中，即不可控因子。隨機區組設計的前提條件與雙因子變異數分析是相同的，且兩因子中有一個是不可控因子。隨機區組設計可以看做雙因子變異數的特例。

## R語言實現

```{r}
# 這裡資料使用與前一例一樣
linear_model <- lm(Activity ~ Sex + Genotype, data = data)
anova(object = linear_model)
```

## 無母數變異數分析

### 單因子

如果我們的前提條件不滿足時(三個之中有一條或以上不符合)，可以使用無母數變異數分析，Kruskal-Walis Rank Sum Test，用來檢驗母體的中位數

- 事後檢定使用逐一比對的Pairwise Wilcoxon rank sum test

### R語言實現

```{r}
apple <- read.csv(file = "C:/Users/ASUS/Desktop/rdata/apple.csv")    #寫入資料
appleData = stack(apple, c("apple1", "apple2", "apple3"))            #堆疊資料
names(appleData) <- c("apple", "group")                              #資料命名
kruskal.test(apple ~ group, data = appleData)                        #無母數變異數分析
pairwise.wilcox.test(apple, group, p.adjust.method = "bonferroni")   #事後檢定Post-hoc
```

### 隨機區組設計

如果我們的前提條件不滿足時(三個之中有一條或以上不符合)，可以使用無母數變異數分析，Friedman Test，用來檢驗母體的中位數

### R語言實現

```{r}
data_ana <- matrix(c(3,1,2,3,1,
                     1,2,1,1,2,
                     4,4,4,4,3,
                     2,3,3,2,4), byrow = FALSE, 5)
data_ana <- as.data.frame(data_ana)
names(data_ana) <- c("V1", "V2", "V3", "V4")
data_ana
local({
  .Responses <- na.omit(with(Dataset, cbind(V1, V2, V3, V4)))
  cat("\nMedians:\n") 
  print(apply(.Responses, 2, median)) 
  friedman.test(.Responses)
})
```



#### 參考資料

1. [ANOVA 統計不求人](https://statistics-using-r.blogspot.com/2018/04/one-way-anova.html)
2. [Colin Rundel 教學講義](http://www2.stat.duke.edu/~cr173/)
3. [R data](https://rcompanion.org/rcompanion/d_08.html)



