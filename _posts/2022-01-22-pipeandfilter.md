---
title: "Pipe and Filter Architecture Style/Pattern"

categories:
  - architecturestyle

tags:
  - architecturestyle
---

### Pipe and Filter Architecture Style



### Pipe and Filter Architecture Pattern

- Filter 종류
  - Active Filter
    - data stream 으로부터 data 를 pull 하고, output data stream 으로 변형된 data를 push 할 수 있다.
  - Passive Filter
    - 둘 중 하나로만 사용된다.


- 시나리오 4
  - Data source - Filter 1 pull/push - Buffering Pipe - Filter 2 pull/push - Data Sink
    - Filter 2 는 데이터 읽기를 시도하지만, 버퍼가 비어있어서 중지된다.
    - Filter 1 은 Data source 로부터 데이터를 가져와서 함수 f1을 실행한다.
    - Filter 1 은 파이프로 결과를 밀어낸다.
    - Filter 2 는 데이터를 읽어온다. Filter 1 은 버퍼가 꽉 찰때까지 계속 Data source 로부터 데이터를 가져온다.
    - Filter 2 는 함수 f2를 계산하고 결과를 data sink 에 저장한다.
  - Filter1, 2 둘 다 능동필터로 파이프의 버퍼 데이터를 읽고 쓰고 중단 가능하다. 


### Discussion
- 장점
  - 중간 결과 파일이 불 필요하다.
  - Filter 교환이나 재조합, 재사용성이 좋다.
  - 병렬 처리에 유용하다.
- 단점
  - Filter 간 전송 시간, Context Switching 시간, Synchronization 으로 인해 성능이 떨어질 수 있다.
    - 재사용, 재조합이 좋은게 보통은 성능이 안좋은 문제가 있다.
  - 에러 처리가 어렵다. (데이터 복구가 어려운 문제 등)
  - 