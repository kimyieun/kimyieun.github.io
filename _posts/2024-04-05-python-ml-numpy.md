---
title : "Numpy"

categories :
    - machinelearning

tags :
    - machinelearning

---

## ndarray 생성


```python
array1 = np.array([1,2,3]) # 인자로 주로 python list, ndarray 입력
```

## shape
- shape : ndarray.shape, dimension : ndarray.ndim (shape 형태보고 파악할 수 있다. tuple - 2차원)
- [[1,2,3], [4,5,6]] -> shape : (2,3), 2차원
- [1,2,3] -> shape : (3,) 1차원

## type
- ndarray.dtype 으로 확인 가능
- 숫자, 문자열, boolean 값 모두 가능하다. 다만, ndarray 내의 데이터 타입은 그 연산의 특성상 같은 데이터 타입만 가능하다.
  - [1, 0.9] - 불가능
  - 이렇게 넣으면 실제로는 실수형으로 바꾼다 -> [1.0, 0.9]
- astype() 을 이용하여 데이터 타입을 변환한다.
  - 메모리 절약을 위해 형변환 고려하여야 한다.
  - array1d.astype("int32") 또는 array1d.astype(np.int32)


## axis
- ndarray 의 shape 은 axis0, axis1, axis2 로 axis 단위로 부여된다. (행, 열, 높이 단위가 아니다!)
- 1차원 - axis0
- 2차원 : (2,4) (axis0, axis1)
- 3차원 : (4, 2, 2) (axis0, axis1, axis2)

- 가장 작은 단위부터 axis 마지막부터 할당한다.

## arange, zeros, ones
- ndarray 를 편리하게 생성하기

```python
np.arange(10) # [0,1,2,3,4,5,6,7,8,9] - array range 라는 의미

np.zeros((3,2), dtype='int32') #[[0,0],[0,0],[0,0]]

np.ones((3,2)) # [[1.,1.],[1.,1.],[1.,1]] - dtype 명시하지 않으면 기본적으로 float64
```


## reshape

```python
array1 = np.array([0,1,2,3,4,5,6,7,8,9])
array1.reshape(2,5) # [[0,1,2,3,4],[5,6,7,8,9]]


array1.reshape(-1,5) # [[0,1,2,3,4],[5,6,7,8,9]]

array2 = np.array([[0],[1],[2],[3],[4]])
array2.reshape(-1,) # (num,) : 무조건 1d array 이므로 (5,) 로 변환한다. [0,1,2,3,4]

array3 = np.array([0,1,2,3,4])
array3.reshape(-1,1) # 2차원이되, axis=1 크기가 1로 고정된다. [[0],[1],[2],[3],[4]]
```

- ml api 에서 인자로 1차원 ndarray 를 2d ndarray 로 변환하여 입력하기를 원하거나 반대의 경우 사용할 수 있다.


## indexing
- 특정 위치의 단일값 추출
  - 마이너스를 인덱스로 사용하면 맨 뒤에서부터 위치 지정
  - array[-1], array[-2]

- slicing - 연속된 인덱스상의 ndarray 추출

- fancy indexing

- boolean indexing - 조건에 맞는 True/False 값으로 인덱싱하는 방법

```python
# fancy indexing
array1 = np.array([1,2,3,4,5,6,7,8,9])

array1[[2,4,7]] # fancy indexing : [3,5,8]

array2 = np.array([[1,2,3],[4,5,6],[7,8,9]])
array2[[0,1],2] # [3,6]
array2[[0,1],0:2] # [[1,2],[4,5]]
array2[[0,1]] # [[1,2,3],[4,5,6]]

# boolean indexing
array1[array1>5] = [6,7,8,9]
```
