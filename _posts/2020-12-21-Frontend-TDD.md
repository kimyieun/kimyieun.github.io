---
title : "Frontend - TDD(Test Driven Development)"

categories :
    - frontend
    - tdd

tags :
    - frontend

---

테스트 코드 작성하시나요?
왜 안하게 되는가요?
- 힘들고 일정에 치여서...나중에 하자.
- 나중은 결코 오지 않는다.

TDD의 궁극적 목표
- Clean Code that works

Test First Programming

TDD Cycle

왜 어려운가?
- 코드가 Testable 하지 않다.
- Testable code를 작성하면 해결된다.

Separation of Concerns
- 각 요소가 자신이 관심이 있는 부분만 해결

Example(To-Do App)
- Testcode 먼저 만들기
- 브라우저 rendering 확인은 스타일이나 레이아웃 잡을 때 외에는 거의 사용하지 않게 됨
- testcode가 빠른 feedback을 주기 때문에

Redux
왜 쓰는가?
- 상태 관리
- React의 관심사 : state reflection! 반면 state management에는 관심이 없다.

컴포넌트를 만들 때, 각 컴포넌트를 작게 만들도록 주의를 기울여라.
이게 과연 이 컴포넌트의 관심사가 맞는가? 

SRP (Single Responsibility Priciple) 단일 책임 원칙

고정된 데이터
fixtures folder에 저장

BDD (Behavior driven development)
- 테스트 짤 때 행위 중심으로 생각

상황에 따라 다르게 행동 (describe context it template 이 유용)

with tasks, render tasks
without tasks, renders not task message

fire event

tdd할 때 중복 제거가 중요
- 빠르게 제거. 문제가 되어도 test에서 바로 찾을 수 있으니 바로바로 지워라.






