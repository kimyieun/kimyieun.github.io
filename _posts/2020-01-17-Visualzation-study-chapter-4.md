---
title : "Analysis : Four Levels for Validation"

categories :
    - visualization

tags :
    - visualization

---


### Book : Visualization Analysis and Design - Tamara Munzner (Chapter 4)


### 4.1 The Big Picture

### 4.2 Why Validate?
- validation 의 중요성
  - _the vis design space is huge, and most designs are ineffective._
- design process 처음부터 validation 방법을 고려하는 것이 좋다.
  
### 4.3 Four Levels of Design
![Validation](/assets/images/fourlevel.png){:width="400px" height="200px"}{: .center}

  1. **Domain situation level**
      - details of a particular application domain for vis.
  2. **_What-why_ abstraction level**
      - domain 과 독립된 형태로 domain-specific problem 과 데이터를 매핑시킨다.
  3. **_How_ level** - idiom design
  4. **Algorithm level**
  
  

- Nested levels
  - _the **output** from an upstream level above is **input** to the downstream level below._
  - block
    - 하나의 level에서의 outcome of design process
  - upstream level 에서 잘못된 block을 선택하면, 그 다음 단계들에서도 잘못된 block을 선택하게 된다.
- 4단계로 분리하는 이유?
  - 각 단계에서 다루는 문제를 독립적으로 분석할 수 있고, design decision 을 생성하는 순서를 독립적으로 진행할 수 있다.
  - 현실적으로 한 단계를 완전하게 끝내고 다음 단계로 넘어가는 것은 어렵다. 
    - **_highly iterative refinement process_**
  
#### 4.3.1 Domain Situation
- domain
  - a particular field of interest of the target users of a vis tool
- A group of target users, their domain of interest, their questions, and their data.
- situation blocks are **_identified_**
  - user의 needs 를 받아들인다. 사용자가 직접 자신의 needs를 말하는 것은 쉽지 않기 때문에, inverviews 외에도 observation, research 를 통해서 관찰한다.
- Outcome
  - a detailed set of questions asked about or actions carried out by the target users.
  - 일반적인 질문보다는 domain-specific 한 형태의 질문을 선택해라.

#### 4.3.2 Task and Data Abstraction
- 매우 다양한 domain-specific 한 needs들은 동일한 abstract vis task에 매핑될 수 있다.
  - e.g., summarization, browsing, and comparing.
- abstract data block is **_designed_.**
  - domain situation 단계에서의 데이터를 그대로 사용하기도 하지만, transformation/derivation 을 통해서 새로운 데이터를 만들어내기도 한다.
- task/data를 abstraction 하는 것을 정당화하지 않고 implicit 하게 진행하는 것은 매우 위험한다.
  - 초기 web vis papers (_“lost in hyperspace”_ problem)
    - 웹 하이퍼링크간의 연결성을 보여주는 그래프 형태에서 topological structure 를 보여주기 위한 문제에 굉장히 집중하였다.  
    그러나 실제 사용자는 관심있는 페이지를 찾기 위해 이와 같이 복잡한 구조의 내부 멘탈 모델을 필요로 하지 않았고, 얼마나 정보를 명확하게 encoding 을 했는지와는 관계없이 사용자에게 부가적인 cognitive load 만 지어주게 됨.

#### 4.3.3 Visual Encoding and Interaction Idiom
- 이전 단계에서 선택된 abstract task 에 따라 abstract data block의 visual representation을 생성 및 처리한다. 이 때 이 방식을 **idiom** 이라 부른다.
- idiom design
  - visual encoding idiom - 사용자가 보는 것을 다룬다.
  - interaction idiom - 사용자가 자신이 보는 것을 어떻게 바꿀지를 다룬다.
- 두 디자인을 독립적으로 생각해도 되지만, 결과를 하나의 통합된 idiom이라 생각하는 것이 가장 좋다.
- 디자인을 결정할 때 인간의 능력을 잘 이해하는 것이 필요하다. 특히, visual perception and memory 관점에서.

#### 4.3.4 Algorithm
- A detailed procedure that allows a computer to automatically carry out a desired goal.
- Nested model은 인간의 인지적인 관심을 가장 우선으로 다루는 idiom 디자인과 computational 관심을 우선으로 다루는 algorithm 디자인을 분리하는 역할을 한다.


### 4.4 Angles of Attack
1. top down
    - problem-driven work. 4.3에서 살펴본 레벨 구조.
    - real-world user가 가지는 문제점을 효과적으로 해결할 수 있는 solution 을 고안하는 작업. 이러한 작업을 design study 라 부른다.
    - 많은 경우 문제들은 새로운 visual encoding이나 interaction 이 아니라 기존의 visual encoding 이나 interaction 을 사용해서 해결되기도 한다. 
    - 대부분의 challenge는 abstraction level에 있으므로, abstraction 단계를 소홀하게 하면 안된다.
      - 이 단계를 제대로 진행해야 앞단계에서 outcome이 충분하지 않은 문제점도 짚고 넘어갈 수 있다. 저자가 이 챕터에서 계속해서 강조하는 점은 vis design 은 iterative refinement 가 필수적이라는 것이다. 한 단계가 끝나면 그 단계로 다시 안돌아가는 것이 절대 아니다.
2. bottom up
    - technique-driven work. 4.3 에서 살펴본 구조와 반대로 진행되는 구조.
    - idiom or algorithm design 부터 시작하는 것으로 목적은 기존의 abstraction 을 더 잘 지원하는 new idiom 을 만들거나 기존의 idiom 을 더 잘 지원하는 새로운 algorithm 을 고안하는 것이다.
    - starting point : an idea for a new visual encoding or interaction idiom, or a new algorithm.
    - algorithms 과 idioms 의 관계를 명확하게 밝혀내기가 까다로울 수 있다.
      - 새롭게 제안하는 알고리즘이 기존 알고리즘과 대체 가능하면서 동일한 visual encoding 을 더 빨리 제공할 수 있나?
      - 새롭게 제안하는 알고리즘이 인간의 능력과 태스크를 충분히 잘 수행하는 것을 보여주기 위해서 justification이 필요한 새로운 idiom을 필요로 하는 만큼 다른 시각적 인코딩을 생성하는가?
      - does your new algorithm result in a visual encoding different enough to constitutes a new idiom that requires justification to show it’s a good match for human capabilities and the intended task?

### 4.5 Threats to Validity
- 각 레벨에서의 선택에 대한 validity를 독립적으로 고려할 필요가 있다.
- Wrong problem: You misunderstood their needs.
- Wrong abstraction: You’re showing them the wrong thing.
- Wrong idiom: The way you show it doesn’t work.
- Wrong algorithm: Your code is too slow.

### 4.6 Validation Approaches
![Validation](/assets/images/validation.png){:width="600px" height="700px"}{: .center}

- Immediate validation approach
  - 각 level에 대한 부분적인 validation 만 진행한다.
- Downstream validation approach
  - Downstream dependencies
    - outer level 의 validation은 그 아래 levels 의 결과가 없이는 immediately 진행될 수 없다.
    - 만약 아래 level validation 이 제대로 되지 않았다면, outer level validation 에서 실패했을 때 어떤 level 에서 문제가 발생한 것인지 파악이 어렵기 때문이다.
- Rapid prototyping methodology
  - Paper prototype, Wizard of Oz test
    - 알고리즘을 디자인하거나 구현하기 전에 abstraction 과 encoding design 에 대해 user 에게 피드백 받을 수 있다.

#### 4.6.1 Domain Validation
- Problem is mischaracterized.
  - 정의한 문제가 실제로 유저가 갖고 있는 문제가 아닌 경우.
  - Immediate form of validation
    - 가정에만 의존하지 않고 실제 target users 인터뷰 또는 관찰.
    - Field study
      - 사용자를 lab이 아닌 실제 사용 환경에 두고, 어떻게 행동하는지를 관찰하는 스터디 방식
      - Gathering qualitative data through semi-structured interviews.
    - Contextual inquiry
      - 사용자가 real-world context 에서 행동하는 것을 관찰하면서 부가적인 설명이 필요한 부분은 중간에 interrupt 하여 필요한 질문을 한다. 끝까지 관찰한 후에 질문하는 것보다 특정 행동이 나왔을 때 바로 물어보는 것이 vis designer 에게 더 적합하다.
  - Downstream form of validation
    - Adoption rates - 모든 것을 설명하지는 못한다.

#### 4.6.2 Abstraction Validation
- Identified task abstraction blocks 와 designed data abstraction blocks 이 앞 단계에서 정의된 타겟 유저의 문제를 해결하지 못하는 경우.
- 이 문제를 위한 validation은 최종 시스템을 실제 사용자가 테스트하는 방법이다.
- Downstream form of validation
  - 타겟 유저로부터 tool이 유용하다는 anectodal evidence를 수집하는 것.
    - e.g., 발견한 insight, 확인된 가설 등..
    - 이 과정은 최종 implementation 까지 완료되어야 검증 가능하다.
  - Field study 
    - 사용자가 실제로 시스템을 real-world 에서 어떤 플로우로 얼마나 잘 수행하는지를 관찰한다.
    - Domain validation 에서의 field study 와의 차이점은 여기서는 deployed system 를 사용할 때 사용자의 행동 변화를 관찰한다는 점이다.

#### 4.6.3 Idiom Validation
- 선택된 idiom이 사용자에게 효과적이지 않은 경우.
- Known perceptual and cognitive principles 에 따라 idiom design을 정당화하는 것이 필요하다.
  - Evaluation methods such as heuristic evaluation and expert review
    - 우리의 디자인이 어떠한 가이드라인도 어기지 않았다.
- Downstream form of validation
  - Lab study - a controlled experiment in a laboratory setting.
    - Study designer 가 선택한 abstract task 에 대한 퍼포먼스 측정을 함으로써 idiom design 선택의 효과 입증
    - Qualitative / quantitative measurements
      - Questionnaires / logging, eye movements, ...
    - Objective measurements of the time spent and errors / subjective measurements such as their preferences are also popular.
    - 실험 참여자의 수가 충분히 많을수록, 그들간의 편차가 적을수록 신뢰할만한 실험이라고 볼 수 있다.
  - Presentation of and qualitative discussion of results 
    - 이미지나 비디오
    - Usage scenario
  - Quantitative measurement of result images
    - Quality metrics
      - Network 상에서 edge crossing 이나 edge bends 개수

#### 4.6.4 Algorithm Validation
- Suboptimal in terms of time or memory performance
  - Immediate form of validation
    - Analyze the computational complexity of the algorithm
  - Downstream form of validation
    - 알고리즘 실행시 시간과 memory performance 측정한다.
    - Scalability 가 중요하다. 어떤 종류, 사이즈의 데이터를 쓸거냐? 이전 논문 benchmarks 
- Incorrectness
  - Vis paper 에서 주로 implicit 한 방식으로 다뤄진다. 이미지나 비디오를 논문에 넣음으로써 독자가 correctness 를 판단할 수 있도록 한다.

#### 4.6.5 Mismatches
- 각 level에 맞는 validation methods 를 선택하는 것이 중요하다. 
- 논문에서 validation 파트를 작성할 때 한 명의 연구자가 4가지 level 에 대해 모두 다루는 것은 시간/공간적으로 한계가 있다. 그래서 대부분은 논문의 contribution 이 담긴 디자인 level에 대한 validation methods 만 일부 진행한다.


### 4.7 Validation Examples
- 책에서는 여러 시각화 예제를 사용해서 what-why-how 를 설명하고 있다. 
  
#### 4.7.1 Genealogical Graphs
![Genealogical Graph](/assets/images/genealogicalGraph.png){:width="600px" height="700px"}{: .center}

  - 가계도 시각화 - 다양한 새로운 visual encoding idioms (dual-tree)과 복잡한 interaction idioms (automatic camera framing, animated transition, ..) 사용.
  - 이 논문에서는 4 level 을 모두 다루고 있다. 
    - Domain situdation 
      - Domain - genealogy, genealogical hobbyists 가 주로 사용하는 tools과 그들의 needs
    - Abstraction level
      - _family tree_ is highly misleading.
        - 실제로 데이터 타입은 구조에 특별한 제약이 있는 일반적인 그래프이다. 
        - 저자들은 data type 이 tree, multitree, DAG 각각의 조건에 대해 논의했고, 실제 사용자들의 task인 "핵가족 구조를 파악하고 싶다." 를 abstract task인 subgraph structure 를 결정하는 것으로 매핑하였다.
    - Visual encoding / interaction idioms
      - 다양한 대안들의 장/단점에 대해 discussion 하였다.
    - Algorithm design
      - Algorithmic details of dual-tree layout.
  - 3가지 validation methods 가 사용되었다.
    - Immediate justification of encoding and iteraction idiom design decisions
    - Downstream method of a qualitative discussion of result images and videos
    - Abstraction level - downstream informal testing (anecdotal evidence 수집)

![Validation](/assets/images/genealogyvalidation.png){:width="600px" height="400px"}{: .center}

 