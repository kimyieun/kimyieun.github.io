---
title : "Pytorch Tutorial - Tensor"

categories :
    - recommendation

tags :
    - recommendation

---

## Pytorch
- GPU 를 사용해 텐서 조작 및 동적 신경망 구축 가능한 딥러닝 프레임워크
- 파이썬답게 만들어졌다!
- Low level 에서는 C, CUDA 와 같은 빠른 언어로 만들어짐
- Mid level 에서는 C++
- Top level 에서는 python API 로 래핑함

## Pytorch 구성요소
- torch : 메인 네임스페이스
- torch.autograd : 자동 미분 기능
- torch.nn : 신경망 구축을 위한 데이터 구조나 레이어
- torch.multiprocessing
- torch.optim : SGD를 중심으로 한 파라미터 최적화 알고리즘 제공
- torch.utils

## Tensor
- 다차원 데이터 표현을 위한 기본 구조
- 일반적으로 수치형 데이터 저장
- numpy 에서 ndarray 와 유사한 개념
- Pytorch 에서는 GPU 를 사용해서 tensor 연산 가속 가능
- 0D Tensor (scalar) - rank(축) : 0, shape(모양) : ()
- 1D Tensor (Vector) - rank : 1, shape : (3, )
- 2D Tensor (Matrix) - rank : 2, shape : (3,3)



```python
import torch

torch.__version__ # 2.1.0+cu121 (torch : 2.1.0 버전 cuda : 12.1) - colab 기준

# 1. 초기화 되지 않은 행렬
x = torch.empty(4, 2)
print(x)

# 값은 랜덤(초기화되지 않은 값)
# tensor([[ 0.0000e+00,  0.0000e+00],
#         [ 1.8367e-40,  3.6734e-40],
#         [ 1.0561e-38,  5.0251e-23],
#         [-2.3510e-38,  5.7864e+17]])


# 2. 무작위로 초기화된 텐서
x = torch.rand(4, 2)
print(x)

# 값은 랜덤을 기준으로 초기화됨
# tensor([[0.0811, 0.1964],
#         [0.0879, 0.5398],
#         [0.9949, 0.3862],
#         [0.3749, 0.7373]])


# 3. 데이터 타입(dtype)이 long 이고, 0으로 채워진 텐서
x = torch.zeros(4, 2, dtype=torch.long)
print(x)

# tensor([[0, 0],
#         [0, 0],
#         [0, 0],
#         [0, 0]])


# 4. 사용자가 입력한 값으로 텐서 초기화
x = torch.tensor([3, 2.3])
print(x)

# tensor([3.0000, 2.3000])

# 5. 2 x 4 크기, double 타입, 1로 채워진 텐서
x = x.new_ones(2, 4, dtype=torch.double)
print(x)

# tensor([[1., 1., 1., 1.],
#         [1., 1., 1., 1.]], dtype=torch.float64)


# 6. x와 같은 크기, float 타입, 무작위로 채워진 텐서
x = torch.randn_like(x, dtype=torch.float)
print(x)

# tensor([[-0.0511,  1.6857, -0.6656,  0.6340],
#         [-1.5450,  0.0941, -0.1857,  1.9778]])

# 7. 텐서의 크기 계산
print(x.size())

# torch.Size([2, 4])
```

- torch.rand : 0과 1 사이의 숫자를 균등하게 생성
- torch.randn : 평균이 0이고 표준편차가 1인 가우시안 정규분포에서 생성

## Data type

- 일반 float 는 float32, double 은 float64, half 는 float16 비트
  
```python
import torch

ft = torch.FloatTensor([1,2,3])
print(ft)
print(ft.dtype)

# tensor([1., 2., 3.])
# torch.float32

print(ft.short()) # tensor([1, 2, 3], dtype=torch.int16)
print(ft.int()) # tensor([1, 2, 3], dtype=torch.int32)
print(ft.long()) # tensor([1, 2, 3])

it = torch.IntTensor([1,2,3])
print(it) # tensor([1, 2, 3], dtype=torch.int32)
print(it.dtype) # torch.int32
```

## CUDA Tensors

- .to 메소드를 사용해서 텐서를 cpu, gpu 로 옮길 수 있다

```python
x = torch.randn(1)

print(x) 
print(x.item())
print(x.dtype)

# tensor([-0.5603])
# -0.5603185892105103 (실제 값은 이렇게 길게 나온다)
# torch.float32

device = torch.device('cuda' if torch.cuda.is_available() else 'cpu')
print(device) # cuda
y = torch.ones_like(x, device=device)
print(y) # cuda tensor([1.], device='cuda:0')

x = x.to(device) 
print(x) # tensor([1.7316], device='cuda:0')

z = x+y
print(z) # tensor([2.7316], device='cuda:0')
print(z.to('cpu', torch.double)) # tensor([2.7316], dtype=torch.float64)

```

## 다차원 텐서 표현

```python

# 0D Tensor (Scalar) - rank, shape 없음
t0 = torch.tensor(0)
print(t0.ndim, t0.shape, t0) # 0 torch.Size([]) tensor(0)

# 1D Tensor (Vector) - rank : 1
t1 = torch.tensor([1,2,3])
print(t1.ndim, t1.shape, t1) # 1 torch.Size([3]) tensor([1, 2, 3])

# 2D Tensor (Matrix) 
t2 = torch.tensor([[1,2,3], [4,5,6], [7,8,9]])
print(t2.ndim, t2.shape, t2)

# 2 torch.Size([3, 3]) tensor([[1, 2, 3],
        # [4, 5, 6],
        # [7, 8, 9]])

# 3D Tensor (Cube)
t3 = torch.tensor([[[1,2,3],
                   [4,5,6],
                   [7,8,9]],
                   [[1,2,3],
                   [4,5,6],
                   [7,8,9]],
                   [[1,2,3],
                   [4,5,6],
                   [7,8,9]]])

print(t3.ndim, t3.shape, t3) # 3 torch.Size([3,3,3])

# 4D Tensor - color image (sample, height, width, color channel)
# 5D Tensor - video data (sample, frame, height, width, color channel)
```

##  Tensor Operations

```python
import math

a = torch.rand(1,2) * 2 - 1
print(a) # tensor([[0.4255, 0.6422]])
print(torch.abs(a)) # tensor([[0.4255, 0.6422]])
print(torch.ceil(a)) # 올림 tensor([[1., 1.]])
print(torch.floor(a)) # 내림 tensor([[0., 0.]])
print(torch.round(a)) # 반올림 tensor([[0., 1.]])
print(torch.clamp(a, -0.5, 0.5)) # -0.5~0.5 범위로 찝어버림 tensor([[0.4255, 0.5000]])

print(a) # tensor([[0.4255, 0.6422]])
print(torch.min(a)) # tensor(0.4255)
print(torch.max(a)) # tensor(0.6422)
print(torch.mean(a)) # tensor(0.5338)
print(torch.std(a)) # tensor(0.1532)
print(torch.prod(a)) # tensor(0.2733)
print(torch.unique(torch.tensor([1,2,3,1,2,2]))) # tensor([1, 2, 3])

```

### max, min operation
- max, min 은 dim 인자를 줄 경우 argmax, argmin 도 함께 리턴한다.
  - argmax : 최대값을 가진 index
  - argmin : 최소값을 가진 index


```python
x = torch.rand(2,2)
print(x)
print(x.max(dim=0))
print(x.max(dim=1))


# tensor([[0.3410, 0.0981],
#         [0.7258, 0.3690]])
# torch.return_types.max(
# values=tensor([0.7258, 0.3690]),
# indices=tensor([1, 1]))
# torch.return_types.max(
# values=tensor([0.3410, 0.7258]),
# indices=tensor([0, 0]))


x = torch.rand(2,2)
print(x)
y = torch.rand(2,2)
print(y)

print(x+y) 
print(torch.add(x,y)) # 둘 다 동일한 결과

result = torch.empty(2,4)
torch.add(x,y,out=result) # x+y 값을 result 에 넣음

# in-place 방식 - operation 뒤에 _ 를 붙이면 그 값을 변경한다
print(x)
print(y)
y.add_(x) # y값이 변경됨
print(y) 

# 아래 3가지 방법 모두 동일한 결과 리턴
print(x-y)
print(torch.sub(x,y))
print(x.sub(y))

print(x*y)
print(torch.mul(x,y))
print(x.mul(y))

print(x/y)
print(torch.div(x,y))
print(x.div(y))

# dot product 내적
print(torch.matmul(x,y))
print(torch.mm(x,y))
z = torch.mm(x,y)

print(torch.svd(z))

# torch.return_types.svd(
# U=tensor([[-0.3391, -0.9407],
#         [-0.9407,  0.3391]]),
# S=tensor([1.0282, 0.0067]),
# V=tensor([[-0.8390,  0.5442],
#         [-0.5442, -0.8390]]))

```


## Tensor Manipulations
- indexing (numpy 와 동일)

```python
x = torch.Tensor([[1,2],
                  [3,4]])
print(x[0,0]) # tensor(1.)
print(x[0,1]) # tensor(2.)
print(x[1,0]) # tensor(3.)
print(x[1,1]) # tensor(4.)

# Slicing
print(x[:,0]) # tensor([1.,3.])
print(x[:,1]) # tensor([3.,4.])
print(x[0,:]) # tensor([1.,2.])
print(x[1,:]) # tensor([3.,4.])

```
- view : 텐서의 size(크기), shape(모양) 변경
  - 기본적으로 변경 전과 후에 텐서 안의 원소 개수가 유지되어야 한다.
  - -1 로 설정되면 계산을 통해 해당 크기 값을 유추한다.

```python
x = torch.randn(4,5)
y = x.view(20)
print(x)
print(y)

# (4,5)
# tensor([[-0.5467,  0.6650,  1.7325,  0.2206, -0.1909],
#         [-0.6393,  0.9939,  1.3450, -1.2246, -0.5261],
#         [-2.0878, -0.1982,  0.1467, -1.2262, -0.3144],
#         [ 2.2357,  0.2597,  0.4481, -0.0068, -0.4211]])
#(20)
# tensor([-0.5467,  0.6650,  1.7325,  0.2206, -0.1909, -0.6393,  0.9939,  1.3450,
#         -1.2246, -0.5261, -2.0878, -0.1982,  0.1467, -1.2262, -0.3144,  2.2357,
#          0.2597,  0.4481, -0.0068, -0.4211])

z = x.view(5, -1)
print(z)

# (5,4)
# tensor([[-0.5467,  0.6650,  1.7325,  0.2206],
#         [-0.1909, -0.6393,  0.9939,  1.3450],
#         [-1.2246, -0.5261, -2.0878, -0.1982],
#         [ 0.1467, -1.2262, -0.3144,  2.2357],
#         [ 0.2597,  0.4481, -0.0068, -0.4211]])

# item - 텐서의 원소가 하나일 때(스칼라 값 하나만 존재) 숫자 값 가져온다
x = torch.randn(1)
print(x) # tensor([0.7539])
print(x.item()) # 0.7539131045341492 
print(x.dtype) # torch.float32


# squeeze - 차원이 1인 차원을 제거해준다. 따로 dim 설정하지 않으면 1인 차원을 모두 제거
tensor = torch.rand(1,3,3)
print(tensor)
print(tensor.shape)

# tensor([[[0.7305, 0.3415, 0.7240],
#          [0.8102, 0.3840, 0.5232],
#          [0.7305, 0.2301, 0.8029]]])
# torch.Size([1, 3, 3])

t = tensor.squeeze()
print(t)
print(t.shape)

# tensor([[0.7305, 0.3415, 0.7240],
#         [0.8102, 0.3840, 0.5232],
#         [0.7305, 0.2301, 0.8029]])
# torch.Size([3, 3])


print(torch.rand(2,1,2))
print(torch.rand(1,2,2))
print(torch.rand(2,2,1))

# tensor([[[0.7132, 0.2701]],
#         [[0.9864, 0.0961]]])
# tensor([[[0.4636, 0.0638],
#          [0.1116, 0.0168]]])
# tensor([[[0.3254],
#          [0.2083]],
#         [[0.7310],
#          [0.6569]]])


# unsqueeze - 차원을 증가(생성) dim 인자 필수
t = torch.rand(3,3)
print(t)
print(t.shape)

# tensor([[0.1268, 0.1530, 0.0923],
#         [0.4029, 0.8750, 0.8664],
#         [0.7195, 0.3122, 0.9643]])
# torch.Size([3, 3])

tensor = t.unsqueeze(dim=0)
print(tensor)
print(tensor.shape)

# tensor([[[0.1268, 0.1530, 0.0923],
#          [0.4029, 0.8750, 0.8664],
#          [0.7195, 0.3122, 0.9643]]])
# torch.Size([1, 3, 3])


# stack - 텐서 간 결합 (새로운 차원으로)
x = torch.FloatTensor([1,4]) # torch.Size([2])
print(x)
y = torch.FloatTensor([2,5])
print(y)
z = torch.FloatTensor([3,6])
print(z)

print(torch.stack([x,y,z]))
print(torch.stack([x,y,z]).shape) # torch.Size([3,2])
# tensor([[1., 4.],
#         [2., 5.],
#         [3., 6.]])

# cat - 주어진 차원을 기준으로 텐서를 붙이는 메소드 (concatenate)
c = torch.cat((x,y,z), dim=0)
print(c, c.shape) # tensor([1., 4., 2., 5., 3., 6.]) torch.Size([6])
```


## torch, numpy 변환
- torch tensor 를 numpy array 로 변환 가능하다
- **tensor 가 cpu 상에 있으면** numpy 배열은 메모리 공간을 공유하므로 하나가 변하면, 다른 하나도 변한다. (gpu 에 있으면 gpu 는 별도 메모리가 있기 때문에 공유하지 않는다!)
- numpy를 tensor 로 변환할 때는 torch.from_numpy() - 메모리 공유, torch.tensor() - 메모리 공유 X 사용 가능하다.

```python
a = torch.ones(7)
print(a) # tensor([1., 1., 1., 1., 1., 1., 1.])

b = a.numpy()
print(b) # [1. 1. 1. 1. 1. 1. 1.]

a.add_(1)
print(a) # tensor([2., 2., 2., 2., 2., 2., 2.])
print(b) # [2. 2. 2. 2. 2. 2. 2.]

import numpy as np
 
a = np.ones(7)
b = torch.from_numpy(a)
np.add(a, 1, out=a) # np.add_ 와 같은 기능
print(a) # [2. 2. 2. 2. 2. 2. 2.]
print(b) # tensor([2., 2., 2., 2., 2., 2., 2.], dtype=torch.float64)
```