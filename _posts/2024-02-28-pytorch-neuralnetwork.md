---
title : "Pytorch Tutorial - Neural Network"

categories :
    - recommendation

tags :
    - recommendation

---

## Dataset

- pytorch 에서는 torch.utils.data 의 Dataset, DataLoader 사용 가능
- Dataset
  - vision, text, audio dataset 존재

```python
from torch.utils.data import Dataset, DataLoader
```

- torchvision 은 pytorch 에서 제공하는 데이터셋이 모여있는 패키지
  - transforms - 전처리할 때 사용하는 메소드
  

```python
import torchvision.transforms as transforms
from torchvision import datasets

# torchvision 은 PIL Image 형태로만 입력을 받기 때문에 데이터 처리를 위해 Tensor 형으로 변환이 필요하다.
mnist_transform = transforms.Compose([transforms.ToTensor(),
                                      transforms.Normalize(mean=(0.5,), std=(1.0,))])

trainset = datasets.MNIST(root='/content/', train=True, download=True, transform=mnist_transform)
testset = datasets.MNIST(root='/content/', train=False, download=True, transform=mnist_transform)
```

- DataLoader 는 데이터 전체를 보관했다가 실제 모델 학습할 때 batch_size 크기만큼 데이터를 가져온다.

```python
train_loader = DataLoader(trainset, batch_size=8, shuffle=True, num_workers=2)
test_loader = DataLoader(testset, batch_size=8, shuffle=False, num_workers=2) # 테스트할 때는 데이터를 섞을 필요가 없다

dataiter = iter(train_loader)
images, labels = next(dataiter)
images.shape, labels.shape #(torch.Size([8, 1, 28, 28]) - 28x28 grayscale 8 images, torch.Size([8])) 

torch_image = torch.squeeze(images[0]) # images[0].shape : torch.Size([1,28,28])
torch_image.shape # torch.Size([28,28])
```


## 신경망
- 레이어 - 하나 이상의 텐서를 입력받아서 하나 이상의 텐서를 출력
- 모듈 - 한 개 이상의 layer 가 모여서 구성
- 모델 - 한 개 이상의 모듈이 모여서 구성

- torch.nn 패키지
  - weight, bias 값들이 내부에서 자동으로 생성되는 레이어들을 사용할 때 사용
  
- torch.nn 와 torch.nn.functional 의 차이?
  - Conv2d 에서 사용하려면 functional 에서는 weight 를 직접 명시해줘야 한다.
  - 

```python
import torch.nn as nn

# nn.Linear 계층
input = torch.randn(128,20)

m = nn.Linear(20, 30) # in_features = 20, out_features = 30
print(m) # Linear(in_features=20, out_features=30, bias=True)

output = m(input)
print(output)
print(output.size()) # torch.Size([128, 30])


# nn.Conv2d 계층
input = torch.randn(20,16,50,100)

m1 = nn.Conv2d(16,33,3, stride=2)
m2 = nn.Conv2d(16,33,(3,5), stride=(2,1), padding=(4,2))
m3 = nn.Conv2d(16,33,(3,5), stride=(2,1), padding=(4,2), dilation=(3,1))

print(m3) # Conv2d(16, 33, kernel_size=(3, 5), stride=(2, 1), padding=(4, 2), dilation=(3, 1)) (16 : in_channels, 33 : out_channels)
output = m3(input)
print(output.size()) #  torch.Size([20, 33, 26, 100])
```

## Convolution Layers

```python
nn.Conv2d(in_channels=1, out_channels=20, kernel_size=5, stride=1)
# Conv2d(1, 20, kernel_size=(5, 5), stride=(1, 1))

layer = nn.Conv2d(1,20,5,1).to(torch.device('cpu'))
layer

weight = layer.weight
weight.shape # torch.Size([20, 1, 5, 5])

# weight 는 detach() 통해 꺼내줘야 numpy() 변환 가능하다
weight = weight.detach()
weight = weight.numpy()
weight.shape # (20,1,5,5)

input_image = torch.squeeze(images[0])
input_data = torch.unsqueeze(images[0], dim=0)
print(input_data.size()) # torch.Size([1,1,28,28])

output_data = layer(input_data)
output = output_data.data
output_arr = output.numpy()
output_arr.shape # (1,20,24,24)

```


## Pooling Layers

```python
import torch.nn.functional as F

pool = F.max_pool2d(output,2,2)
pool.shape # torch.Size([1,20,12,12])
```