---
title: "상관분석 - Correlation Analysis"

categories:
  - datascience

---

## 상관분석


## 피어슨 상관계수
- 정규분포일 때, 선형관계일 때, outlier 가 많이 없을 때 주로 사용한다.
- 귀무가설 : 두 변수간의 모상관계수는 0이다.
- 대립가설 : 두 변수간의 모상관계수는 0이 아니다.

- 모상관계수의 추정량인 표본상관계수를 이용하여 모상관계수가 0인지 여부를 판단한다.

![Validation](/assets/images/pearson.PNG){:width="900px" height="400px"}{: .center}

## 예제 1
- df_titanic 에서 Age 와 Fare의 피어슨 상관계수를 구해봅니다. 둘 중에 하나라도 결측인 경우는 제외 합니다.

```python
from scipy.stats import pearsonr

df_new = df_titanic.loc[df_titanic['Age'].notna() & df_titanic['Fare'].notna(), ['Age', 'Fare']]
pearsonr(df_new['Age'], df_new['Fare'])
# (0.09606669176903891, 0.010216277504442105)

df_new[['Age', 'Fare']].corr('pearson')
```


## 스피어만 상관계수
- 비정규분포일 때, 비선형 관계일 때, outlier 가 많을 때 주로 사용한다.
- 귀무가설, 대립가설은 피어슨 상관계수와 동일하다.


## 예제 2
```python
from scipy.stats import spearmanr

df_new = df_titanic.loc[df_titanic['Age'].notna() & df_titanic['Fare'].notna(), ['Age', 'Fare']]
spearmanr(df_new['Age'], df_new['Fare'])

df_new[['Age', 'Fare']].corr('spearman')
```

## 켄달 상관계수


## 예제 3
- df_titanic 에서 Age 와 Fare의 켄달(Kendall) 상관계수를 구해봅니다. 둘 중에 하나라도 결측인 경우는 제외하고, 중복된 값은 한 건만 남기고 제외합니다.


```python
from scipy.stats import kendalltau

df_new = df_titanic.loc[(df_titanic['Age'].notna()) & (df_titanic['Fare'].notna())][['Age', 'Fare']]
kendalltau(df_new['Age'], df_new['Fare'])

df_new.drop_duplicates().corr('kendall')
```
