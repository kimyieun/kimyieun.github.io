---
title : "Convolutional Neural Network"

categories :
    - datascience

tags :
    - datascience

---

## CNN 등장 배경? 
- overfitting
  - DNN 에서는 깊게 만들수록 error 를 0으로 만들 수 있다!
  - 근데 이걸 잘했다고 할 수 있나?
  - 차라리 적당하게 맞췄다고 하면 실제 데이터에서는 error 가 더 작을 수도 있다.
  - Fully Connected Layer
    - overfitting 문제가 있다. 너무 training data 에 fitting 되는 것이 문제.
    - 불필요하게 너무 계산이 많아진다. 너무 복잡하게 생각한다!
    - 효율적이고 융통적인 특징을 뽑아내고 싶다! -> CNN 등장 배경


## Convolution?
- image 는 dicrete data (픽셀로 구성)
- 이미지에 convolution 한다? 필터링 한다? -> 이미지의 특징을 추출한다!
- 