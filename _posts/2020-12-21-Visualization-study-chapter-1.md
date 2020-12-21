---
title : "What's Vis, and Why Do It?"

---

## What's Vis, and Why Do It?

### 1.1 The Big Picture

### 1.2 Why have a human in the loop?
- vis는 사용자가 정확히 자신이 가진 문제점이 뭔지 모르는 상황에서 데이터 분석시 도움을 준다.
- 사용자가 well-defined question을 가지고 있다면, 통계학이나 머신러닝을 사용해 해결 가능하다.
- 완전하게 자동화되는 솔루션이 필요하다면, human judgement가 필요없고 결과적으로 vis tool이 필요 없다.

- 그러나, 많은 분석 문제들은 ill specified이다. 어떻게 문제에 접근하는지 모른다.
- 이런 케이스에서 가장 좋은 방법은 사용자를 분석 과정의 loop안에 집어 넣는 것이다.
- Vis systems are appropriate for use when your goal is to **augment human capabilities**, rather than completely replace the human in the loop.

- 여러 사용 방법
    - Transitional use
        - goal : "work itself out of a job" 
        - 예를 들어, 수학적인 모델을 고안하기 이전에 requirements를 이해하기 위한 vis tool이 있다고 가정해보자. 이는, transition process의 가장 초기 단계에 사용된다. 이 분석 툴의 결과는 사용자 task에 대한 보다 명확한 이해를 하는데 도움을 준다.
        - middle stage에서는, computational solution을 refine, debug, extend 하거나 알고리즘의 parameters에 따른 효과를 이해하기 위해 사용한다.

    - Long-term use
        - no intention of replacing the human
        - 연구 과정이 완전하게 자동화되기 힘든 분야에서 사용.
    - For presentation
        - 이미 알고 있는 지식이나 사실을 다른 사람들에게 설명하기 위한 수단으로 사용한다.


### 1.3 Why have a computer in the loop?
- 사람의 attention span은 굉장히 한정되어 있다. 이에 반해 real-world datasets는 방대하고, 시시각각 변화한다. computer-based tool을 사용함으로써 manual creation에 비해 노력이 상대적으로 덜 필요하다.


### 1.4 Why use an external representation?
- 인간의 내부 인지와 메모리는 제한되어 있다. Vis는 external representation을 사용해서 사용자의 internal cognition과 memory의 필요성을 없애준다.
- 다양한 형태의 external representation이 있지만, 이 책에서는 2D display surface로만 언급하겠다.

### 1.5 Why depends on vision?
- visual system은 매우 높은 대역폭 channel을 제공한다. 시각적 정보 처리의 양은 preconscious level와 거의 parallel하게 발생한다. visual popout.
- 소리는 vision에 비해 많은 양의 정보의 overview를 제공하는데 적합하지 않다. 
- taste, smell, haptic 등의 감각들은 아직 기술적인 한계로 인해 쉽지 않다.

### 1.6 Why show the data in detail?
- dataset의 간단한 summary를 제공하는 것보다, 구조에 대해 자세한 정보를 제공하는 것이 좋다.
- 예를 들어, dataset의 통계적인 특징은 가장 흔한 접근 방법 중 하나이다. 그러나, summarization을 함으로써 정보의 손실이 발생한다는 한계가 있다. Anscombe's Quartet.
- 그림 설명

### 1.7 Why use interactivity?
- Interactivity 는 시각화 툴에서 가장 강력한 무기이다. dataset이 커질수록 사용자와 dispaly의 한계로 인해 모든 데이터를 한번에 보여줄 수 없다. 
- 거의 대부분의 상황에서 interaction은 필수적이다. 
- 예를 들어, 고차원적인 overview에서부터 작은 부분의 detailed view까지 모두 지원가능하다.


### 1.8 Why is the vis idiom design space huge?
- vis idiom : a distinct approach to creating and manipulationg visual representations.
- visual encoding
- 

### 1.9 Why focus on tasks?
- 동일한 dataset에서 하나의 task에 적합한 툴은 다른 테스크에는 적절하지 않을 수 있다.
domain-specific한 형태의 사용자의 테스크를 abtract한 형태로 변경하는 작업을 통해, 사용자가 실제환경에서 필요로 하는 것들간의 공통점과 차이점을 알 수 있다.
- Discovery : 새로운 dataset에서 새로운 가정을 생성하거나, 이미 알고 있는 dataset에서 존재하는 가설을 검증하기 위한 목적으로 사용됨.

### 1.10 Why focus on effectiveness?
- user tasks를 얼마나 잘 지원하는지에 대한 평가
- correctness, accuracy, truth
- "it's not just about making pretty pictures" - vis designer는 artists가 아니다. 결과에 대한 목표는 예쁜 것이 아니라, 효과적이어야 한다.
- 시각화에 있어서 correctness - 데이터에 대한 모든 묘사는 abstraction이다. 모든 결정이 어떠한 면을 강조할지에 따라 이루어지기 때문에 correctness를 평가하기 어렵다.

### 1.11 Why are most designs ineffective?
- visual design space는 매우 방대하다. 그러나 대부분은 ineffective하다. 그 이유로는 인간의 인지적, 지각적 시스템에 적합하지 않아서, 또는 의도하는 task에 맞지 않아서 등등. 아주 일부만이 좋은 디자인이라 할 수 있다. 
- 이 때 randomly 디자인을 탐색하는 것은 정말 비효율적이다. 아래 그림을 보자.

### 1.12 Why is validation difficult?
- vis design에 대한 validation은 어렵다. goal을 만족하는지 판단할 때 할 수 있는 질문이 무궁무진하기 때문에...
- 이 디자인이 다른 디자인에 비해 좋거나 안좋은 이유가 뭘까? better이라는 의미는? 사용자가 태스크를 빠르게 수행하나? 사용자들이 이 tool을 통해 더 재밌는 것을 많이 하나? 효과적으로 일을 하게 되었나? effectively하다는 것은 여기서 무슨 의미인가? 등 등....
- 사용자는 누구인가? 이 태스크를 수년이상 수행해본 전문가인가? 아니면 시작하기전에 태스크가 무엇인지 설명을 들어야하는 수준의 novice인가? 이 시스템을 처음 보는 사람인가? 아니면 익숙한 사람인가?

### 1.13 Why are there resource limitations?
- 시각화 시스템을 설계할 때 3가지 limitation에 대한 고려가 필요하다.
    - computational capacity
    - human perceptual and cognitive capacity
    - display capacity

- 시각화 시스템은 필연적으로 방대한 양의 데이터를 다루어야 하므로 **scalability** 가 주요한 이슈이다.
    - 여러 다양한 이유로 dataset 사이즈는 계속해서 증가하는 추세.
    - computer capacity 또한 향상되어 가고 있다.

- Computer side
- Human side
    - memory and attention 이 제한된 자원이다.
    - 인간은 놀랄만큼 굉장히 적은 정보를 저장할 수 있다. **change blindness**
    - 하나의 view에서 무언가 일을 수행하고 있을 때 다른 부분에서 엄청난 변화가 발생해도 우리는 이를 눈치채지 못한다.
- Display capacity
    - 모든 정보를 동시에 보여주기에는 해상도가 충분하지 못하다.
    - Information density : 사용하지 않는 공간 대비 encoded된 정보의 양에 대한 measure 방법
    - 모든 정보를 한번에 제공함으로써 navigation과 exploration의 필요성을 줄이는 장점과 비용 간의 trade-off가 있다. 좋은 디자인의 목표는 이 두가지 중에 적절한 balance를 찾는 것이다.

### 1.14 Why analyze?
- high-level framework for analyzing vis use according to three questions
    - **What** data the user sees,
    - **Why** the user intends to use a vis tool,
    - **How** the visual encoding and interaction idioms are constructed in terms of design choices
- **what-why-how  = data-task-idiom**
- 이 3단계를 하나의 instance라 한다.
- 단순한 vis tool은 독립된 instance로도 구성할 수 있지만, 복잡한 vis tool은 dependencies가 있는 instances chaining을 필요로 한다. output이 또다른 instance의 input이 되는 형태이다.
- 예를 들어, 사용자가 vis tool에서 보이는 항목들을 소팅한다. 이는 소팅 그 자체가 결과물로 소팅된 list를 갖는 것이 목적일 수도 있지만, 이를 통해 outlier를 발견하기 위한 수단이 될 수도 있다.

### 1.15 Further Reading
