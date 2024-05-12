---
title: "표본분산의 분포, 카이제곱분포, T분포, F분포"

categories:
  - datascience

---

## sampling distribution
- 통계량의 분포
  - 통계량 : 표본평균, 표본분산
    - 표본평균 통계량은 평균 = mu 이고 분산이 표준편차의 제곱/n 인 정규분포를 따른다.
    - 표본분산은 n-1 을 곱하고 표준편차의 제곱으로 나누어준 통계량을 사용한다. 이것은 카이제곱분포를 따른다.


## 표본평균의 분포

![Validation](/assets/images/sd.PNG){:width="=500px" height="300px"}{: .center}

- 모집단의 분포가 주어졌을 때 표본 X1, X2, ...., Xn 표본을 뽑는다.
- 표본평균의 평균 = 모평균, 분산 = 모분산/n
- X_bar 를 표준정규분포로 바꾼 Z 는 N(0,1) 표준정규분포를 따른다.


## 표본분산의 분포

![Validation](/assets/images/sd2.PNG){:width="=500px" height="300px"}{: .center}

- 모집단의 분포(정규분포)가 주어졌을 때, N개의 샘플을 추출한다.
- (Statistics)는 자유도가 n-1 인 카이제곱분포를 따른다.
  - 표본분산 = S


## 그럼 카이제곱분포가 뭐야?

![Validation](/assets/images/chi.PNG){:width="=500px" height="300px"}{: .center}

- 확률변수 N개가 있을 때 각각의 N(0,1) 표준정규분포를 따른다.
- 각 각을 제곱을 취한 Z 는 카이제곱분포를 따른다.
  - v : 자유도 (표준정규분포를 몇개 더해줬는지?)
- 카이제곱분포가 뭐에요?
  - 표준정규분포 제곱의 합입니다.
- 감마분포의 special case 가 카이제곱분포이다! (alpha, lambda 가 특정 값일 때)
  

- 그래프 모양은 skewed 되어있다.
  - 오른쪽으로 기울어짐. 완만한 쪽이 오른쪽이다. (skew to the right, right skewed)
- 자유도에 따라 기울어진 정도가 다르다.
  

![Validation](/assets/images/chi2.PNG){:width="=500px" height="300px"}{: .center}

- alpha(면적) : 0.95, v(df) = 5 일 때 카이제곱값은 1.145
- 카이제곱값이 1.145 보다 클 확률은? 0.95 (면적)

## 표본분산으로 다시 돌아가보자.

![Validation](/assets/images/sv.PNG){:width="=500px" height="300px"}{: .center}

- 모집단이 정규분포를 따른다고 했을 때 N개의 샘플을 뽑으면, ~ 통계량은 카이제곱분포를 따른다.
- n : 샘플의 개수


## 자유도가 뭐야?
- 카이제곱을 따르는 통계량에서 사실 X_bar 는 모평균이므로, sigma(Xi-mu)**2 = Z 제곱과 동일하다.
- mu는 모집단의 평균인데 X bar 로 우리가 샘플들로부터 추정한 값이다. 그러므로 이미 알고 있는 값이다.
  - 그래서 n개 중에 1개는 알고 있다는 의미로, n-1 개는 모르기 때문에 자유도에 해당된다. 1개는 표본평균으로부터 이미 확정된 값.
- X1 = 2, X2 = 3, X3 = 4, X4 = 5, X5 = ? 
  - sigmaXi = 20 이 주어지면, X5는? 6
  - 이 경우 자유도는? 4
  - 4개가 정해지면 나머지 1개의 값은 고정될 수 밖에 없다. 그러므로 4개는 자유롭게 정해질 수 있다. 이게 자유도.


# 표본분산의 예제
- 모집단 - 평균 = 15, 분산 = 100
- 25개의 샘플 추출 -> df = 24

![Validation](/assets/images/sv3.PNG){:width="=500px" height="300px"}{: .center}


- 표본분산이 c보다 클 확률이 0.95 인 c를 찾아라
- 표본분산(S^2)의 분포는 모르지만, 우리는 (n-1)S^2/sigma^2 통계량이 카이제곱분포를 따른다는 것은 알고 있다.



## t분포
- T 통계량은 t 분포를 따르고, v는 자유도이다.
- t분포의 기대값은 0이고, 0을 기준으로 대칭이다.
- 표준정규분포보다는 longer tails 를 가진다. 꼬리가 두껍다.
  - 자유도가 커질수록 꼬리가 얇아진다.
  - **비정규분포!**

![Validation](/assets/images/t2.PNG){:width="=500px" height="300px"}{: .center}


- 모집단이 정규분포이다.
  - 정규분포, 카이제곱분포, t분포, f분포 모두 모집단은 정규분포이다!

![Validation](/assets/images/t3.PNG){:width="=500px" height="300px"}{: .center}

- 표준정규분포 형태와 거의 동일한데, 표준편차 대신 S 를 사용한다는 차이만 있다.
  - sigma 는 모집단의 표준편차. S 는 샘플의 표준편차
  - 모집단의 표준편차를 알때는 표준정규분포를 사용하고, 모를 때는 t분포를 사용한다.


## 예제

![Validation](/assets/images/t4.PNG){:width="=500px" height="300px"}{: .center}


## F 분포
- F 통계량은 카이제곱분포의 비율로 정의된다.
  - 자유도는 v1, v2 로 2개이다.


![Validation](/assets/images/f.PNG){:width="=500px" height="300px"}{: .center}


![Validation](/assets/images/f2.PNG){:width="=500px" height="300px"}{: .center}


![Validation](/assets/images/f3.PNG){:width="=500px" height="300px"}{: .center}
- 카이제곱분포랑 비슷하게 오른쪽으로 skew 된 형태이다.
- 2개의 자유도가 1이면 거의 지수분포 형태.


![Validation](/assets/images/f4.PNG){:width="=500px" height="300px"}{: .center}


- 표본분산의 비율이 궁금하다!
- 두 모집단 사이의 분산 차이를 알고 싶을 때 보통 사용하는 식 => S1^2/S2^2
  

![Validation](/assets/images/f5.PNG){:width="=500px" height="300px"}{: .center}
