---
title: "Blackboard Pattern"

categories:
  - architecturestyle

tags:
  - architecturestyle
---

### 공유 데이터 스타일
- 데이터베이스, 지식 기반 시스템(github repository)
- 다중 접근자와 하나 이상의 공유 데이터 저장소로 구성
- Blackboard - 공유 데이터 저장소가 접근자에게 데이터가 변경됨을 통지
- Repository - 접근자가 데이터 검색에 책임을 짐
- 요소 
  - 컴포넌트 타입 : shard data repository, data accessor
  - 커넥터 타입 : 데이터 읽기/쓰기
- 관계
  - attach 관계 : 어떤 접근자가 어떤 데이터 저장소와 연결되는지 표시
- 연산 모델 : 접근자들 간의 통신은 공유 데이터에 의해 중재된다.
- 토폴로지 : 접근자는 데이터 저장소에 부착된 커넥터에 부착된다.
- 용도
  - 지속성을 가지는 데이터 항목들이 다양한 접근자 가질 때
    - 데이터 생산자와 소비자를 분리해서 modifiability 향상
  - 성능, 보안, 프라이버시, 신뢰성, 호환성에 집중 가능하다
  - 데이터 분산이나 중복은 성능은 올라가지만 복잡성도 올라가고 신뢰성과 보안성이 낮아진다.
- N-tier Client-server 스타일과 유사
- Pub-sub 스타일과 유사 - 이 경우에는 데이터를 지속적으로 유지하지 않는다.


### Blackboard Pattern
- shared data, database 와 같은 데이터 중심 패턴 중 하나
- 명확히 정의된 문제 해법이 없는 경우 풀어가는 하나의 방식을 정의한 패턴
  - 근사적인 문제를 풀거나 특수한 서비스 시스템의 지식을 조합하는 패턴
  - AI 시스템, 신호 처리, Vision, speech processing
- 구성 컴포넌트
  - blackboard
    - 중앙 데이터 관리, 문제의 현재 상태를 보여준다(knowledge source 들이 읽고 쓸 수 있는 인터페이스 제공)
  - knowledge sources
    - blackboard 상태를 업데이트
    - 특정 도메인의 해법 제시
    - blackboard 에 그 해법을 적용한다 (적용 가능성은 자체 평가)
  - contrl
    - blackboard 변경 사항을 모니터링하고 다음 동작 수행을 결정하는 루프 반복
    - knowledge source 스케쥴 관리 및 실행시킴
- Context
  - 도메인의 해법이 명확하지 않을 때
- Problem
  - 가공되지 않은 raw data 를 상위 수준의 데이터 구조로 변환하기 위한 결정적 해법이 없음
  - 하나의 문제를 분해하면 여러 전문 분야가 관여된 여러 하위 문제로 나뉨
- Solution
  - 각 독립적인 프로그램은 일부를 해결하기 위한 특수화된 작업 수행
  - 서로 호출하지 않고, 미리 정의된 순서도 없음
  - control 컴포넌트에 의해 모든 독립적 프로그램이 협력하여 동작한다.


### 동작 방식
1. control loop 를 실행한다.
2. control 에서 next source 를 찾는다. 가능한 next source 를 지정하기 위해 blackboard inspect 함수호출하여 상태를 파악하고 해당하는 knowledge source의 execcondition 을 수행한다. - 적용 가능성 파악
3. knowledge source 에서 다시 blackboard inspect 해서 적용 가능성을 본다.
4. 가능하면 control 이 knowledge source 의 execaction 을 invoke 한다.
5. knowledge source 는 updateblackboard 를 수행한다. - blackboard inspect 하고 update 수행한다.


### Discussion
- 적용 예제
  - 센서가 많은 상황에서 필요한 데이터만 계기판에 표시해줄 때
- 장점
  - 완벽한 해법 찾기 어려울 때 다양한 실험 가능
  - knowledge source 재사용 좋음
  - KS, Control, blackboard 독립적 동작으로 가변성이나 유지보수성이 좋다
  - fault tolerance(장애 허용)과 robustness 지원
    - 솔루션이 여러개라서!
- 단점
  - 계산 결과가 항상 동일하다고 보장할 수가 없다. 오류 발생 시 테스트 재현 불가
  - 많은 시간에 걸쳐 수정되므로 개발에 노력이 많이 필요하다.