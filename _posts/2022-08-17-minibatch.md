---
title: "Mini Batch and Data Load"

categories:
  - recommendation

tags:
  - recommendation
---

### Negative Sampling
- Neural Collaborative Filtering 알고리즘을 사용할 때, 학습 데이터로 Negative Sampling 을 사용한다. 
  - Negative Sampling
    - 제품 추천 같은 경우에는 사용자와 아이템간의 positive interaction 은 있으나, negative interaction 은 알 수 없다.
    - positive interaction 뿐만 아니라 일부 negative interaction 도 학습하면 성능 향상에 도움이 될 수 있다.
- 

### Mini Batch
- Negative Sampling 을 수행할 때 전체 데이터에 대해서 미리 Negative Samples 을 생성할 수도 있지만 이는 방대한 양의 데이터에서 생성할 때는 시간이 오래 소요되고, 많은 양의 메모리를 차지하게 된다. (실제로 데이터 개수가 늘어나니, Negative Sampling 비율만큼 늘어나 커널이 죽는 문제가 발생하였다.)
- 그래서 Mini Batch 내에서 Negative Sampling 을 수행하면 메모리나 속도가 향상될 수 있다.
- 전체 데이터가 크다면, 한번에 계산하기엔 메모리 한계로 인해 불가능할 수 있다. 그래서 전체 데이터를 작은 단위로 나눠서 해당 단위별로 학습하는데, 이 때 단위를 mini batch 라 한다.
- Epoch 는 전체 training data 가 학습에 한 번 사용되는 주기를 말하기 때문에 mini batch 개수만큼 학습을 완료해야 하나의 Epoch 이 종료되었다고 할 수 있다.

### Batch Size
- Mini Batch 개수는 Mini Batch 의 크기에 따라 달라진다. 이때 Mini Batch 의 크기를 Batch Size 라고 한다.
- Iteration 은 한 번의 Epoch 내에서 이루어지는 가중치의 업데이트 횟수로, 전체 데이터가 1000개, batch size 가 200이면 iteration 수는 5개가 된다.

### Load Data
- 