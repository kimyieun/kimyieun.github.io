---
title : "Pytorch Tutorial - Autograd"

categories :
    - recommendation

tags :
    - recommendation

---

## Autograd
- tensor 의 모든 연산에 대해 자동 미분 제공한다.
- backpropagation
  - 출력층에서부터 입력층으로 오차를 역으로 전파하면서 각 층의 가중치와 편향을 조정하는 과정
- backpropagation 을 위해서 미분값(gradient)을 자동으로 계산한다.
  - 가중치와 편향을 조정할 때 각 파라미터에 대한 loss 의 gradient 를 계산하여 사용한다.
- tensor 의 gradient 를 구하려면?
  - **requires_grad 속성을 True 로 설정**하면 해당 텐서에서 이루어지는 모든 연산을 추적한다.
  - backpropagation 을 시작할 지점의 output 은 scalar 형태여야 한다.
- 중단하려면 .detach() 를 호출하여 연산 기록으로부터 분리한다.


```python

a = torch.randn(3,3)
a = a * 3
print(a)
# tensor([[ 4.9723,  3.6259,  1.2418],
#         [-2.4350,  0.3685,  1.8943],
#         [ 1.4829, -3.6253, -2.8453]])

print(a.requires_grad) # False (기본적으로는 False)

# requires_grad_(...) 는 기존 텐서의 requires_grad 값을 in-place 하여 변경
# grad_fn : 텐서가 어떤 연산을 하였는지 연산 정보를 담고 있음
a.requires_grad_(True)
print(a.requires_grad) # True

b = (a*a).sum()
print(b) # tensor(72.5043, grad_fn=<SumBackward0>)
print(b.grad_fn)  # <SumBackward0 object at 0x7b853d7a5240>
```

## Gradient 기울기

```python
x = torch.tensor(2.0, requires_grad=True)
y = 9*x**4 + 2*x**3 + 3*x**2 + 6*x + 1
y.backward() # y.backward(torch.tensor(1.)) 와 동일하다
print(x.grad) #tensor(330.)


# output 은 scalar 여야 backward 수행 가능하다.
x = torch.randn(2, 2, requires_grad=True)
y = x + 2
z = (y * y)
z.backward() # RuntimeError: grad can be implicitly created only for scalar outputs (z : (2,2))
```

```python
# output z 를 scalar 로 만들어줌.
x = torch.randn(2, 2, requires_grad=True)
y = x + 2
z = (y * y).sum()
z.backward() 

print(x.grad)
# tensor([[4.5571, 2.4908],
#         [3.3370, 4.4436]])
print(y.grad) # None
print(z.grad) # None

# tensor x 는 grad 가 저장됨. 중간 과정인 y,z 는 실제 grad 가 저장되지 않고 backward 연산에만 참여한다.
```

```python
# output z 를 scalar 로 만들어줌.
x = torch.randn(2, 2, requires_grad=True)
y = x + 2
z = (y * y).sum()
z.backward() 

print(x.grad)
# tensor([[4.5571, 2.4908],
#         [3.3370, 4.4436]])
print(y.grad) # None
print(z.grad) # None

# tensor x 는 grad 가 저장됨. 중간 과정인 y,z 는 실제 grad 가 저장되지 않고 backward 연산에만 참여한다.
```

```python
x = torch.ones(3, requires_grad=True)
y = x**2
z = y*3
z.backward(torch.Tensor([0.1,1,10])) # dz/dx = 6x * [0.1,1,10] = [0.6,6,60] 

print(x.grad) # tensor([ 0.6000,  6.0000, 60.0000])
print(y.grad) # None
print(z.grad) # None
```

- with torch.no_grad() - 기울기 업데이트 하지 않음
- 모델 평가할 때는 모델을 업데이트 하지 않기 때문에 기울기 업데이트도 하지 않는다

```python
print(x.requires_grad) # True
print((x**2).requires_grad) # True

with torch.no_grad():
    print((x**2).requires_grad) # False
```

```python
# detach - gradient 계산을 멈춘다
print(x.requires_grad) # True
y = x.detach()
print(y.requires_grad) # False
print(x.eq(y).all()) # tensor(True) - x의 모든 element 와 y의 모든 element 가 동일한지 확인
```

## 자동 미분 흐름 예제
- a -> b -> c -> out
- dout/da = ?
- backward() 를 통해서 a<-b<-c<-out 을 계산하면 dout/da 값이 a.grad 에 채워진다.

```python
a = torch.ones(2,2,requires_grad=True)
print(a)

print(a.data) # 실제 데이터 값
print(a.grad) # None
print(a.grad_fn) # None

b = a+2
print(b)

# tensor([[3., 3.],
#         [3., 3.]], grad_fn=<AddBackward0>)

c=b**2
print(c)

# tensor([[9., 9.],
#         [9., 9.]], grad_fn=<PowBackward0>)

out = c.sum()
print(out) # tensor(36., grad_fn=<SumBackward0>)

out.backward()

print(a.data)
print(a.grad)
# tensor([[1., 1.],
#         [1., 1.]])
# tensor([[6., 6.],
#         [6., 6.]])
print(a.grad_fn) # None - 직접적으로 a에서 계산한 부분이 없기 때문에

print(b.grad) # None
print(c.grad) # None
print(out.grad) # None
```

- backpropagation 에서는 최종 출력에 대한 입력의 기울기만 필요하다.

## Reference
- https://gaussian37.github.io/dl-pytorch-gradient/
- https://www.youtube.com/watch?v=k60oT_8lyFw&t=966s
