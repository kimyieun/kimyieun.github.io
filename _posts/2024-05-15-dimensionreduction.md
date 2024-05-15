---
title: "차원 축소 - PCA, LDA"

categories:
  - datascience

---

## 차원의 저주
- 차원이 커질수록 데이터 포인트들간 거리가 크게 늘어난다.
- 차원 축소의 장점
  - 학습 데이터 크기를 줄여서 학습 시간 절약 가능
  - 불필요한 피쳐들을 줄여서 모델 성능 향상에 기여 가능할 수도 있다
  - 시각적으로 보다 쉽게 데이터 패턴 인지 가능
- 그러면 어떻게 원본 데이터 정보를 최대한 유지하면서 차원을 축소할 것이냐?


1. 피쳐 선택 (feature **selection**)
   1. 불필요한 피쳐를 제거하고, 데이터 특징을 잘 나타내는 주요 피쳐만 선택한다.
2. 피쳐 추출 (feature **extraction**)
   1. 기존 피쳐의 특성을 잘 요약해서 새로운 피쳐를 만들어낸다.
   2. **latent**(잠재적인 요소)
      1. **내재된 속성을 찾아주는 것이 피쳐 추출의 핵심**이다.
      2. 추천 엔진, 이미지 분류 및 변환, 문서 토픽 모델링
      3. 선형 피쳐 추출 - PCA, LDA
      4. 비선형 피쳐 추출 - t-SNE

## PCA(principal component analysis) - 비지도 학습 기법
- 고차원의 원본 데이터를 저차원의 새로운 **부분 공간으로 투영**하여 데이터를 축소하는 기법
- 원본 데이터가 가지는 데이터 변동성(분산)을 가장 중요한 정보로 간주한다.
- 원본 d 차원 데이터를 새로운 k 차원의 부분 공간으로 변환 (k << d)

![Validation](/assets/images/pca.jpg){:width="900px" height="300px"}{: .center}


- 원본 데이터 변동성이 가장 큰 방향으로 순차적으로 축들을 생성하고, 이렇게 생성된 축으로 데이터를 투영하는 방식
- 가장 큰 데이터 변동성을 기반으로 첫 번째 벡터 축을 생성한다.
- 두 번째 축은 첫 번째 축을 제외하고 그 다음으로 변동성이 큰 축을 설정하는데, 이는 첫 번째 축에 직각이 되는 벡터 축이다.
- 세 번째 축은 다시 두 번째 축과 직각이 되는 벡터로 축을 생성한다.

## PCA 알고리즘
1. d 차원 데이터셋을 표준화 전처리한다. 
   1. **PCA 방향은 데이터 스케일에 민감하기 때문에 전처리가 필수적이다.**
2. 공분산 행렬을 만든다.
3. 공분산 행렬은 대칭 행렬이므로 고유 벡터와 고윳값으로 분해한다.
4. 고윳값을 내림차순으로 정렬하고 그에 해당하는 고유 벡터의 순위를 매긴다.
5. 고윳값이 가장 큰 K 개의 고유 벡터를 선택한다. 
6. 최상위 K개의 고유 벡터로 투영 행렬 W 를 만든다.
7. 투영 행렬 W 를 사용해서 d 차원 입력 데이터셋 X를 새로운 k 차원의 특성 부분 공간으로 변환한다.


- 고유 벡터는 PCA 주성분 벡터로서 입력 데이터의 분산이 큰 방향을 나타낸다.
- 고윳값은 바로 이 고유 벡터의 크기를 나타내며, 동시에 입력 데이터의 분산을 나타낸다.

## 공분산 행렬
- 두 변수 간의 변동을 의미한다.
- cov(x,y) > 0 이면 x가 증가할 때 y도 증가한다는 의미이다.
- 정방행렬(행과 열이 같은 행렬), 대칭행렬(정방행렬 중 대각 원소 중심으로 원소 값 대칭) 이다.


## 선형 변환
- 특정 벡터에 행렬 A 를 곱해 새로운 벡터로 변환하는 것을 의미한다.
- 고유 벡터는 행렬 A 를 곱하더라도 방향이 변하지 않고 그 크기만 변하는 벡터를 말한다.
- 고유 벡터가 바로 PCA 축이다.
  - [e1, e2, ... , en] : e1이 가장 변동성이 큰 축


```python
from sklearn.decomposition import PCA
# 1. 간단한 예제
columns = ['measurement_{}'.format(i) for i in range(0, 15)]
pca = PCA()
pca.fit_transform(X_train)
pca.transform(X_test)


# 1-2. pca 결과를 기존 dataframe 에 저장하기 (pca_0 ~ pca_14)
new_X_train = X_train.copy()
new_X_test = X_test.copy()

new_X_train[['pca_{}'.format(i) for i in range(0, 15)]] = pd.DataFrame(pca.fit_transform(X_train), index=X_train.index)
# index 를 동일하게 설정하는 것이 진짜 중요함 !!!!
new_X_test[['pca_{}'.format(i) for i in range(0, 15)]] = pd.DataFrame(pca.transform(X_test), index=X_test.index)


# 2. 설명 가능한 분산의 비율 = 0.95 가 되도록 PCA 수행
pca = PCA(n_components=0.95)

print(pca.n_components) # 10
print(np.sum(pca.explained_variance_ratio_)) # 0.96~~
```

- PCA
  - parameters
    - n_components : PCA 축의 개수
      - 설명 가능한 분산의 비율(0~1)을 입력하면 자동으로 그 비율을 달성하기 위해 필요한 주성분 개수만 선택한다.
  - attributes
    - explained_variance_ratio_ : 전체 변동성에서 개별 PCA 컴포넌트별로 차지하는 변동성 비율

## LDA(Linear Discriminant Analysis)
- 개별 class 를 분별할 수 있는 기준을 최대한 유지하면서 차원을 축소하는 방법
  - PCA 는 입력 데이터의 변동성이 가장 큰 축을 찾았지만, LDA 는 입력 데이터의 결정 값 클래스를 최대한 분리할 수 있는 축을 찾는다.
- 클래스 간의 분산은 최대화하고, 클래스 내부 분산은 최소화한다.


## LDA 과정
- 공분산 행렬이 아니라 클래스 간 분산과 클래스 내부 분산 행렬을 생성하고, 이 행렬에 기반해 고유벡터를 구하고 입력 데이터를 투영한다.


## LDA 예제

```python
from sklearn.discriminant_analysis import LinearDiscriminantAnalysis

lda = LinearDiscriminantAnalysis()
result = lda.fit_transform(X_train[columns], y_train)
# LDA 는 학습할 때 y label 도 필요하다!
```