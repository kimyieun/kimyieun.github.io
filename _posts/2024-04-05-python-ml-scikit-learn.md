---
title : "scikit-learn"

categories :
    - machinelearning

tags :
    - machinelearning

---

## sklearn

```python
from sklearn.datasets import load_iris
from sklearn.tree import DecisionTreeClassifier
from sklearn.model_selection import train_test_split

import pandas as pd

iris = load_iris()
iris_data = iris.data
iris_label = iris.target

iris_df = pd.DataFrame(data=iris.data, columns=iris.feature_names)
iris_df['label'] = iris_label
iris_df.head(3)

# 학습 데이터와 테스트 데이터 셋 분리
X_train, X_test, y_train, y_test = train_test_split(iris_data, iris_label, test_size=0.2, random_state=11)

dt_clf = DecisionTreeClassifier(random_state=11)
dt_clf.fit(X_train, y_train)

pred = dt_clf.predict(X_test)

from sklearn.metrics import accuracy_score
accuracy_score(y_test, pred)

```


### sklearn 기반 framework - estimator 와 fit(), predict()
- estimator (최상위 부모 클래스)
  - classifier
    - 분류 구현 클래스
    - DecisionTreeClassifier
    - RandomForestClassifier
    - GradientBoostingClassifier
    - GaussianNB
    - SVC
  - regressor
    - 회귀 구현 클래스
    - LinearRegression
    - Ridge
    - Lasso
    - RandomForestRegressor
    - GradientBoostingRegressor


### sklearn 내장 예제 데이터

```python
from sklearn.datasets import load_iris

iris_data = load_iris()
print(type(iris_data)) # sklearn.utils.Bunch

keys = iris_data.keys()
print(keys) # dict_keys(['data', 'target', 'target_names', 'DESCR', 'feature_names'])
```
- data : feature dataset
- target : 분류 시 레이블 값, 회귀시 숫자 결괏값
- target_names : 개별 label 의 이름
- feature_names : feature 이름
- DESCR : 데이터 셋에 대한 설명과 각 feature의 설명

### Model Selection 
- train_test_split()

```python
import pandas as pd

iris_df = pd.DataFrame(iris_data.data, columns=iris_data.feature_names)
iris_df['target'] = iris_df.target

feature_df = iris_df.iloc[:, :-1]
target_df = iris_df.iloc[:, -1]

X_train, X_test, y_train, y_test = train_test_split(feature_df, target_df, test_size=0.3, random_state=121)

print(type(X_train), type(X_test), type(y_train), type(y_test)) # DataFrame, DataFrame, Series, Series

dt_clf = DecisionTreeClassifier()
dt_clf.fit(X_train, y_train)
pred = dt_clf.predict(X_test)
```

## K 폴드 교차 검증
- 학습 데이터를 분할하여 학습 데이터와 검증 데이터로 나눔
  - 검증 데이터 : 학습된 모델의 성능을 1차 평가
- 테스트 데이터 : 모든 학습/검증 과정이 오나료된 후 최종적으로 성능을 평가하기 위한 데이터 셋

- 검증 데이터를 여러번 돌려서 검증해보고 테스트 셋으로는 최종적으로 한 번만 평가한다.

1. 일반 K 폴드
2. Stratified K 폴드
   1. label 분포가 불균형일 때 학습 데이터와 검증 데이터셋이 분포도가 유사하도록 검증 데이터 추출

```python
from sklearn.model_selection import KFold

iris = load_iris()
features = iris.data
label = iris.label

kfold = KFold(n_splits=5)
cv_accuracy = []

n_iter = 0

for train_index, test_index in kfold.split(features):
  X_train, X_test = features[train_index], features[test_index]
  y_train, y_test = label[train_index], label[test_index]

  dt_clf.fit()
```

- Stratified K Fold

```python
import pandas as pd

iris = load_iris()

iris_df = pd.DataFrame(data=iris.data, columns=iris.feature_names)
iris_df['label'] = iris.target
iris_df['label'].value_counts() # 0 - 50개, 1 - 50개, 2 - 50개

kfold = KFold(n_splits=3)

for train_index, test_index in kfold.split(iris_df):
  label_train = iris_df['label'].iloc[train_index]
  label_test = iris_df['label'].iloc[test_index]

  print(label_train.value_counts()) # 0, 1 label 만 존재
  print(label_test.value_counts()) # 2 label 만 존재

from sklearn.model_selection import StratifiedKFold

skfold = StratifiedKFold(n_splits=3)

for train_index, test_index in skfold.split(iris_df, iris_df['label']):
  label_train = iris_df['label'].iloc[train_index]
  label_test = iris_df['label'].iloc[test_index]

  print(label_train.value_counts()) # 0, 1, 2 - 33, 33, 34개
  print(label_test.value_counts()) # 0, 1, 2 - 16, 17, 17개
```

## cross_val_score()
- cross_val_score(estimator, X, y=None, scoring=None, cv=None, n_jobs=1, verbose=0, fit_params=None)
- **stratified K Fold 사용**

```python
from sklearn.model_selection import cross_val_score

scores = cross_val_score(dt_clf, data, label, scoring='accuracy', cv=3)
print(scores) # [0.95, 0.94, 0.98]
print(np.round(np.mean(scores), 4)) # 0.9667
```


## GridSearchCV 
- 교차 검증과 최적 하이퍼 파라미터 튜닝을 한번에 수행
- (오래걸릴 수 있음)

```python
from sklearn.model_selection import GridSearchCV, train_test_split


iris = load_iris()
X_train, X_test, y_train, y_test = train_test_split(iris_data.data, iris_data.target, test_size=0.2, random_state=121)

dt_cl = DecisionTreeClassifier()

parameters = {'max_depth' : [1,2,3], 'min_samples_split':[2,3]}

grid_dtree = GridSearchCV(dt_cl, param_grid=parameters, cv=3, refit=True, return_train_score=True)
# parameter 조합이 총 6개 x cross validation 3개 = 총 18번 수행

grid_dtree.fit(X_train, y_train)

search_df = pd.DataFrame(grid_dtree.cv.results_) 
# grid_dtree.cv.results_ - dictionary 형태로 저장되므로 보기 좋도록 dataframe 으로 변경

print(grid_dtree.best_params_)
print(grid_dtree.best_scores_)

pred = grid_dtree.predict(X_test)
print(accuracy_score(y_test, pred))


estimator = grid_dtree.best_estimator_
pred = estimator.predict(X_test)
print(accuracy_score(y_test, pred))
```

- refit=True 가 default 로 True 이면 가장 좋은 파라미터 설정으로 재학습 시킨다.

## 데이터 전처리
- 데이터 클린징
- 결손값 처리
- 데이터 인코딩(레이블, one-hot encoding)
- 데이터 스케일링
- outlier 제거
- feature 선택, 추출/가공
  

## 데이터 인코딩
- label encoding
  - label : tv, 냉장고, 전자렌지, 컴퓨터, 선풍기, 믹서
  - label encoding : 0, 1, 4, 5, 3, 2
  - 숫자간의 크기 비교가 가능하게 인코딩됨. 그걸 보완한 것이 원핫 인코딩
  - LabelEncoder class 사용
- one-hot encoding
  - OneHotEncoder class 사용
  - pd.get_dummies(DataFrame) 사용

```python
from sklearn.preprocessing import LabelEncoder

items=['TV', '냉장고', '전자렌지', '컴퓨터', '선풍기', '선풍기', '믹서', '믹서']

encoder = LabelEncoder()
encoder.fit(items)
labels = encoder.transform(items)
print(labels) # [0, 1, 4, 5, 3, 3, 2, 2] - ndarray
print(encoder.classes_) # ['TV', '냉장고', '전자렌지', '컴퓨터', '선풍기', '믹서']
print(encoder.inverse_transform([4, 5, 3, 2, 1, 0]))
```


```python
from sklearn.preprocessing import OneHotEncoder
import numpy as np

items=['TV', '냉장고', '전자렌지', '컴퓨터', '선풍기', '선풍기', '믹서', '믹서']

items = np.array(items).reshape(-1,1) # 2d array 형태로 변경

oh_encoder = OneHotEncoder()
oh_encoder.fit(items)
oh_labels = oh_encoder.transform(items)

print(oh_labels) # sparse matrix
  # (0, 0)	1.0
  # (1, 1)	1.0
  # (2, 4)	1.0
  # (3, 5)	1.0
  # (4, 3)	1.0
  # (5, 3)	1.0
  # (6, 2)	1.0
  # (7, 2)	1.0
print(oh_labels.toarray()) # dense matrix
# [[1. 0. 0. 0. 0. 0.]
#  [0. 1. 0. 0. 0. 0.]
#  [0. 0. 0. 0. 1. 0.]
#  [0. 0. 0. 0. 0. 1.]
#  [0. 0. 0. 1. 0. 0.]
#  [0. 0. 0. 1. 0. 0.]
#  [0. 0. 1. 0. 0. 0.]
#  [0. 0. 1. 0. 0. 0.]]
print(oh_labels.shape) # (8,6)


labels = encoder.fit_transform(items)
```


```python
import pandas as pd

df = pd.DataFrame({'items' : ['TV', '냉장고', '전자렌지', '컴퓨터', '선풍기', '선풍기', '믹서', '믹서']})

ohe_df = pd.get_dummies(df)
ohe_df
```


## feature scaling
- 표준화 : 피쳐 각각을 평균 0, 분산 1 인 가우시안 정규 분포를 가진 값으로 변환
- 정규화 : 서로 다른 피쳐의 크기를 통일하기 위해 크기를 변환
- StandardScaler, MinMaxScaler
- logistic regression, svm 은 어떤 스케일러냐에 따라 영향을 많이 받는다.
- **scaler class 의 fit, transform 은 2차원 이상의 데이터만 가능하므로 reshape(-1,1)로 차원 변경해야 한다.**

```python
from sklearn.preprocessing import StandardScaler

scaler = StandardScaler()
scaler.fit(iris_df)
iris_scaled = scaler.transform(iris_df)
# iris_scaled = scaler.fit_transform(iris_df)

iris_df_scaled = pd.DataFrame(data=iris_scaled, columns=iris.feature_names)

iris_df_scaled.mean(), iris_df_scaled.var()

```

```python
from sklearn.preprocessing import MinMaxScaler

scaler = MinMaxScaler()
iris_scaled = scaler.fit_transform(iris_df)
```

- scaler 에서 fit_transform 쓸 때 주의할 점

```python
train_array = np.arange(0,11).reshape(-1,1)
test_array = np.arange(0,6).reshape(-1,1)774724552

scaler = MinMaxScaler()

data_array = np.concatenate((train_array, test_array), axis=0)
scaler.fit(data_array) 
```
- 학습 데이터와 테스트 데이터를 합친 데이터에 대해서 scaler.fit 을 수행해야 한다.
- 테스트 데이터가 공개되어 있지 않다면 학습 데이터 대상으로 scaler.fit 을 해야 한다.