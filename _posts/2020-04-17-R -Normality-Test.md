---
title: R 다변량 통계 분석 - 1. 일변량 정규성 검정(Normality Test) Q-Q plot, qqplotr, Kolmogorov-Smirnov test, Shapiro-Wilk test
author: Lee
date: 2020-04-17. 10:51
categories: [Blogging, Demo]
tags: [Kolmogorov-Smirnov test, Multivariate, Normality Test, Q-Q plot, qqplotr, Shapiro-Wilk test, 다변량, 다변량 통계, 일변량, 정규성 검정]
math: true
mermaid: true
image:
  src: - Default (with caption)

![R book](../assets/img/items/{image}.png)
_R과 함께하는 다변량 자료분석을 위한 추정과 검정, 최용석 지음, 2019_

---

아래 내용은 <R과 함께하는 다변량 자료분석을 위한 추정과 검정, 최용석 지음, 2019>에서 대부분 발췌하였습니다.

## 라이브러리 
---

![full armor unicorn gundam](../assets/img/items/{image}.png)

다변량 통계 분석에서 정규성 검정, 시각화 방법 등에 필요한 라이브러리 입니다

<span style="color:red">
*library(MVT)
library(MVN)
library(dplyr)
library(car)
library(multifluo)*
 </span>.

---
<br>
## 데이터 불러오기
---
examScor 데이터를 불러옵니다

원활한 분석을 위해 결측치를 제거하고

데이터 형태와 요약 정보를 확인해봅시다

'''R
data(examScor)

examScor <- examScor %>% na.omit()

examScor %>% head()

examScor %>% summary()
'''

![r table1](../assets/img/items/{image}.png)
---
<br>
## 각 과목별 일변량 정규성 검정

과목별로 데이터 프레임을 분리합시다

'''r
메카 <- examScor %>% select(mechanics)
벡터 <- examScor %>% select(vectors)
대수 <- examScor %>% select(algebra)
분석 <- examScor %>% select(analysis)
통계 <- examScor %>% select(statistics)
'''
그리고 이들을 numeric으로 변환합니다

편의상 "메카(mechanics)"에 대해서만 실행해보겠습니다

'''r
메카_vector <- 메카 %>% as.vector()
메카_vector <- 메카_vector %>% unlist()
메카_num <- 메카_vector %>% as.numeric()
'''

각각에 대해 정규성 검정 및 시각화를 해봅시다

### Q-Q plot (1)

Q-Q plot에 대해서는 아래 위키백과를 참고해주세요

<https://en.wikipedia.org/wiki/Q%E2%80%93Q_plot>

'''r
n <- 메카_num %>% length()
p <- (1:n)/(n+1)
q <- qnorm(p, mean(메카_num), sd(메카_num))

sort_q <- q %>% sort()
메카_sort <- 메카_num %>% sort()

메카_QQ <- plot(sort_q, 메카_sort, xlab = "Quantiles from Normal Distribution", ylab = "Sample Quantiles", main = "Q-Q plot-mechanics")
abline(0,1,col="Red")
'''
![r q-q plot1](../assets/img/items/{image}.png)

얼마나 abline(0,1,col="Red")에 적합한지 상관계수로 알아봅시다
'''r
cor(sort_q, 메카_sort)
'''
<span style="color:red"> *[1] 0.9886617* </span>.


### Q-Q plot (2)
qqnorm()으로 Q-Q plot을 그려보겠습니다
'''r
메카_norm <- qqnorm(메카_num, pch = 1, main = "Q-Q plot(2)-mechanics")
qqline(메카_num, col = "Blue", lwd = 2)
'''
![r q-q plot2](../assets/img/items/{image}.png)

얼마나 qqline(메카_num, col = "Blue", lwd = 2)에 적합한지 상관계수로 알아봅시다
'''r
메카_sort_norm_x <- 메카_norm$x %>% sort()
메카_sort_norm_y <- 메카_norm$y %>% sort()

cor(메카_sort_norm_x, 메카_sort_norm_y)
'''
<span style="color:red"> *[1] 0.9873895* </span>.

### Q-Q plot (3)
좀더 다양한 옵션이 추가된 (이상치, 신뢰구간 등) 추가된 Q-Q plot을 그려봅시다
'''r
메카_qqplot <- 메카_num %>% qqPlot(main = "Q-Q Normal Q-Q plot(2)-mechanics")
'''
![r q-q plot3](../assets/img/items/{image}.png)
#### 참고 : qqplot2

ggplot2를 이용한 Q-Q을 그리면 좀더 다양한 옵션을 시각화 할 수 있습니다

추가로 필요한 라이브러리는 다음과 같습니다

<span style="color:red">
*library(qqplotr)
library(tidyverse)*
 </span>.
 
 numeric형태인 메카_num을 변수 메카_score를 가지는 Data Frame으로 변화하여 따로 저장합니다
 '''
 메카_df <- data.frame(메카_score = 메카_num)
 '''
 
 그리고 Confidence Band(보통 Confidence Interval로 많이 쓰는데 시각화하면 Band형태가 되기에 이렇게 부르는것 같습니다)를 Normal, MLEs, Kolmogorov-Smirnov test, Aldor_Noiman으로 계산하여 시각화 해봅시다.

아래표는 bandType에 대한 정리입니다

| bandType =                   | Option                  | 
|------------------------------|-------------------------|
| pointwise                    | Normal                  | 
| boot                         | MLEs                    | 
| ks                           | Kolmogorov-Smirnov test | 
| ts                           | Aldor_Noiman            |

자세한 설명은 아래  CRAN Page를 참조하시면 되겠습니다

<https://cran.r-project.org/web/packages/qqplotr/qqplotr.pdf>

그리고 qqplotr로 Q-Q plot를 그려봅시다
'''r
메카_qqplot2 <- ggplot(data = 메카_df , mapping = aes(sample = 메카_score)) +
  ggtitle("Q-Q plot-ggplotr-mechanics") +
  geom_qq_band(bandType = "pointwise", mapping = aes(x = 메카_score, fill = "Normal"), alpha = 0.5) +
  geom_qq_band(bandType = "boot", mapping = aes(x = 메카_score, fill = "Bootstrap"), alpha = 0.5) +
  geom_qq_band(bandType = "ks", mapping = aes(x = 메카_score, fill = "KS"), alpha = 0.5) +
  geom_qq_band(bandType = "ts", mapping = aes(x = 메카_score, fill = "TS"), alpha = 0.5) +
  stat_qq_line() +
  stat_qq_point() +
  labs(x = "Theoretical Quantiles", y = "Sample Quantiles") +
  scale_fill_discrete("Bandtype")
'''
![r q-q plot4](../assets/img/items/{image}.png)

#### Kolmogorov-Smirnov Test

<span style="color:red">
Kolmogorov-Smirnov Test는 표본의 정규성을 검정하는 비모수 검정입니다. 비모수 검정은 데이터의 수가 정규성을 만족할 만큼 충분하다고 생각하지 않을때 적용하는 방법입니다
</span>
정규성을 만족하는 개수를 어떻게 구하는가는 또 다른 수리통계학적 방법이 있는데

**"표본 크기에 따른 검정력함수의 비교(최강력 검정 : Most Powerful Test, MP test)"**로 필요한 표본의 크기를 산출 할수 있습니다

저는 이론적으로만 접해본적 있고 구현하지는 못해봤기에 다음기회에 다뤄보기로 하겠습니다

 

조금 말이 길어졌는데 비모수 검정을 적용할 경우는 일반적(경험적)으로 표본의 수가 30이하이면 해봄직 합니다
'''
ks.test(메카_num, pnorm, mean(메카_num), sd(메카_num), alternative = "two.sided") 
'''
<span style="color:red">
*One-sample Kolmogorov-Smirnov test*

*data:  메카_num*
**D = 0.092019, p-value = 0.4455**
*alternative hypothesis: two-sided*
</span>

여기서 H0 : 관측한 표본과 비교대상(정규분포)에 차이가 없다 입니다

H0을 기각하지 못합니다


#### Shapiro-Wilk Test

Shapiro-Wilk Test는 표본의 정규성을 검정하는 모수적 검정입니다. 표본의 수가 충분할 경우 적용합니다

'''
shapiro.test(메카_num)
'''
<span style="color:red">
*Shapiro-Wilk normality test

data:  메카_num*
</span>
**W = 0.97241, p-value = 0.05708**

 

여기서 H0 : 정규분포(Normal distrubution)을 따른다 입니다

아슬아슬하게 H0을 기각합니다


## Images

- Default (with caption)

![Desktop View](https://cdn.jsdelivr.net/gh/cotes2020/chirpy-images/posts/20190808/mockup.png)
_Full screen width and center alignment_

<br>

- Specify width

![Desktop View](https://cdn.jsdelivr.net/gh/cotes2020/chirpy-images/posts/20190808/mockup.png){: width="400"}
_400px image width_

<br>

- Left aligned

![Desktop View](https://cdn.jsdelivr.net/gh/cotes2020/chirpy-images/posts/20190808/mockup.png){: width="350" .normal}

<br>

- Float to left

  ![Desktop View](https://cdn.jsdelivr.net/gh/cotes2020/chirpy-images/posts/20190808/mockup.png){: width="240" .left}
  "A repetitive and meaningless text is used to fill the space. A repetitive and meaningless text is used to fill the space. A repetitive and meaningless text is used to fill the space. A repetitive and meaningless text is used to fill the space. A repetitive and meaningless text is used to fill the space. A repetitive and meaningless text is used to fill the space. A repetitive and meaningless text is used to fill the space. A repetitive and meaningless text is used to fill the space. A repetitive and meaningless text is used to fill the space. A repetitive and meaningless text is used to fill the space. A repetitive and meaningless text is used to fill the space. A repetitive and meaningless text is used to fill the space."

<br>

- Float to right

  ![Desktop View](https://cdn.jsdelivr.net/gh/cotes2020/chirpy-images/posts/20190808/mockup.png){: width="240" .right}
  "A repetitive and meaningless text is used to fill the space. A repetitive and meaningless text is used to fill the space. A repetitive and meaningless text is used to fill the space. A repetitive and meaningless text is used to fill the space. A repetitive and meaningless text is used to fill the space. A repetitive and meaningless text is used to fill the space. A repetitive and meaningless text is used to fill the space. A repetitive and meaningless text is used to fill the space. A repetitive and meaningless text is used to fill the space. A repetitive and meaningless text is used to fill the space. A repetitive and meaningless text is used to fill the space. A repetitive and meaningless text is used to fill the space."

<br>
