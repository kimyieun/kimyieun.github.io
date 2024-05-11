---
title: "카이제곱검정"

categories:
  - datascience

---


## 카이제곱검정
- 독립변수와 종속변수 모두 범주형 데이터일 때
- 분할표 (contingency table)

![Validation](/assets/images/chi1.PNG){:width="900px" height="400px"}{: .center}


- 적합도 검정, 독립성 검정, 동질성 검정
  - 모두 카이제곱 식을 사용한다.


![Validation](/assets/images/chi3.PNG){:width="900px" height="400px"}{: .center}

- O : 관측 도수, E : 기대 도수
- df : (열 수-1)*(행 수-1)


## 카이제곱 검정의 제약
- 분할표에서 각 칸의 기대 도수가 **5이하인 cell 이 20% 이상**이면 사용할 수 없다.
  - 수가 적으면 카이제곱 검정의 신뢰도가 떨어지기 때문에 사용 불가
  - 카이제곱 검정은 대표본에서만 유효하다.
  - 그 때는 **피셔의 정확 검정**을 사용한다.
    - 소표본일 때, 빈도수가 적은 것이 많이 나올 때.
- 


## 적합도 검정 (goodness of fit test)
- 한 개의 요인
- 관측값들이 어떤 **이론적 분포**를 잘 따르고 있는지 검정
- 귀무가설 : 관측값의 도수와 기대 관측도수가 동일하다. 
- 대립가설 : 적어도 하나의 범주의 도수가 가정한 이론 도수와 다르다.


![Validation](/assets/images/goodness.PNG){:width="900px" height="400px"}{: .center}
- df : 그룹수-1

## 적합도 검정 예제
- 멘델의 유전 법칙 - 9:3:3:1

![Validation](/assets/images/goodness2.PNG){:width="900px" height="400px"}{: .center}

- df : 3

## 적합도 검정 예제 코드

- df_titanic dataframe 에서 성별이 0.5, 0.5 비율로 남, 여 동일한지 검정하라.

```python
from scipy.stats import chi2

n = len(df_titanic)
chi = df_titanic.groupby('Sex')['Sex'].agg('count').apply(lambda x : (x - 0.5*n)**2/(0.5*n)).sum()
n, chi # (891, 77.63075196408529)

chi2.sf(chi, 1) # sf = 1 - cdf / df = 성별(2) - 1
# 1.2422095313910336e-18 (pvalue)

# 2. 더 간단한 방법
from scipy.stats import chisquare

n = len(df_titanic)
obs = df_titanic.groupby('Sex')['Sex'].agg('count').tolist()
exp = [0.5*n, 0.5*n]
chisquare(obs, exp) 
# Power_divergenceResult(statistic=77.63075196408529, pvalue=1.2422095313910336e-18)

```


## 독립성 검정 (test of independence)
- 2 개의 요인
- 2개의 요인이 관찰값에 영향을 주는지 아닌지? 서로 연관이 있는지 없는지?
- 귀무가설 : 두 변수 X, Y 는 서로 독립이다.
- 대립가설 : 두 변수 X, Y 는 서로 독립이 아니다.

![Validation](/assets/images/independence.PNG){:width="900px" height="400px"}{: .center}

## 독립성 검정 예제
- 성별이 드라마 구매 여부에 연관성이 있는가?
- 귀무가설 : 두 변수는 서로 독립이다.
- 대립가설 : 두 변수는 서로 독립이 아니다.
- 성별이 드라마 구매 여부와 연관성이 있다면, 성별 구분 없는 전체 구매 비율이 각 성별에도 동일하게 나타날 것이다.
- 연관성이 있으려면 얼마나 차이가 나야 할까?
- df = (행수-1)(열수-1) = (성별-1)(구매/비구매-1) = 1

![Validation](/assets/images/chi4.PNG){:width="900px" height="400px"}{: .center}

## 독립성 검정 예제 코드

- df_titanic 에서 Survived와 Embarked가 독립인지 카이제곱 독립성 검정으로 검정해봅니다.
(contingency : 우연성)

```python
from scipy.stats import chi2_contingency
o_conti = pd.crosstab(df_titanic['Survived'], df_titanic['Embarked'])
chi2_contingency(o_conti)

# (25.964452881874784,
#  2.3008626481449577e-06,
#  2,
#  array([[103.51515152,  47.44444444, 398.04040404],
#         [ 64.48484848,  29.55555556, 247.95959596]]))

```

![Validation](/assets/images/contingency.PNG){:width="300px" height="200px"}{: .center}
- chi2_contingency return value
  - chi2 statistics, pvalue, dof, expected_freq

## 동질성 검정 (test of homogeneity)
- 2 개의 요인
- 관측값들이 주어진 범주 내에서 서로 비슷하게 나타나는지를 검정
- 속성 A,B를 가진 모집단으로부터 정해진 표본만큼 자료를 추출했을 때 분할표에서 모집단의 **비율**이 동일한지를 검정
  - 키워드 : 비율!!!
- 귀무가설 : 비율이 동일하다.
- 대립가설 : 동일하지 않다.

## 동질성 검정 예제
- 남녀 어린이의 tv 프로그램 시청에 대한 선호도 비교
- 100명 남자, 200명 여자에게 3가지 프로그램 중 가장 좋아하는 한가지를 고르게 함.
- 귀무가설 : 남자, 여자 어린이는 tv 프로그램에 대한 선호도가 동일하다.
- 대립가설 : 선호도가 다르다.

![Validation](/assets/images/homo.PNG){:width="900px" height="400px"}{: .center}
- 아래 표인 기대값을 계산할 줄 알아야 한다.
- df : 2 * 1 = 2

## 독립성 검정 vs 동질성 검정
- 동일한 과정인데 귀무가설이 다르다.
- 독립성 귀무가설 : 두 변수가 독립인지 아닌지
- 동질성 귀무가설 : 부모집단의 비율과 동일한지를 검정