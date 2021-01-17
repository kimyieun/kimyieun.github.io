---
title : "Why : Task Abstraction"

categories :
    - visualization

tags :
    - visualization

---


### Book : Visualization Analysis and Design - Tamara Munzner (Chapter 3)

### 3.1 The Big Picture
- 저자가 책 중간중간에 강조하는 점으로는 Why와 How는 독립적이라는 것이다. 내가 presentation action을 지원한다고 해서 how가 정해지는 것이 아니라, 이 안에서도 엄청나게 다양한 vis idiom 선택지가 있다고 한다.


### 3.2 Why Analyze Tasks Abstractly?
- 이 책에서는 domain-specific하게 주어진 사용자 태스크를 abstract form 으로 바꾸는 것이 중요하다고 한다.
- abstract form 으로 변경하였을 때, 태스크들간의 similarities와 differences를 파악하기가 훨씬 용이하다.

_**“Contrast the prognosis of patients who were intubated in the ICU more than one month after exposure to patients hospitalized within the first week”.**_  


_**“See if the results for the tissue samples treated with LL-37 match up with the ones without the peptide”.**_

- 위 두 태스크는 얼핏 보기에는 굉장히 다른 성격의 태스크를 수행하는 것 같지만, 이를 generic term으로 바꿔보면 _**"compare values between two groups"**_ 라는 동일한 성격의 태스크를 수행한다는 것을 알 수 있다.
- 태스크 분석이 필요한 또 다른 이유는 사용자의 원본 데이터를 다른 형태로 변경할지, 그리고 어떤 형태로 변경할지를 이해하기 위해서이다. 
  - _task abstraction can and should guide the data abstraction._

### 3.3 Who : Designer or User
- 누가 Visualization Design 을 선택할지에 대해서 두 가지 측면으로 나눠볼 수 있다.
- Specific, narrow, specialized tools
  - designer 가 미리 많은 design choice를 만들어낸다. 사용자의 입장에서는 제한된 데이터와 태스크를 가지는 반면에, 많은 design choice를 고려하지 않아도 되는 장점이 있다.
- General, broad tools
  - flexible 하기 때문에, 다양한 형태의 dataset, transformation도 사용 가능하고 사용자들이 자유로운 결정을 내릴 수 있다. 
  - 반면, design issues에 대한 전문성이 깊지 않은 사용자들은 비효율적인 결정을 내릴 수 있다.
   
### 3.4 Actions
- user goal을 정의할 때 총 3단계의 action이 있다.
  - high-level : analyze - consume, produce
  - mid-level : search - lookup, browser, locate, explore
  - low-level : query - identify, compare, summarize
- 각 단계에서의 choice는 서로 독립적이다. 각 단계에서의 actions를 설명해보는 것이 좋다.

#### Analyze
- Consume
  - Existing information
  - Discover(Explore)
    - 이전에 알려지지 않은 새로운 지식을 발견하는 것. 새로운 가설을 만들어내거나, 존재하는 가설을 증명하거나.
    - 일반적으로 과학적인 질문을 해결하기 위해서 사용된다. 
  - Present(Explain)
    - 기존에 알고 있는 정보를 전달하기 위한 목적으로 사용한다. decision making, planning, forecasting, 교육적인 목적 등으로 사용된다.
    - 일반적인 예시로는 뉴스의 diagram이나 블로그 이미지 같은 static information graphics.
    - dynamic vis idiom으로도 표현되는 경우가 있다.
  - Enjoy
    - _casual encounters with vis._
    - Name Voyaher
      - 시대별로 유행했던 아기 이름.
      - enjoy와 present는 명확하게 구별하기 어렵다. 이 예시에서도 presentation을 위한 목적이라고 볼 수도 있기 때문이다.
- Produce
  - New information
  - Annotate
    - 기존에 존재하는 visualization elements에 graphical/textual한 annotation을 다는 것.
    - 주로 사용자의 manual한 행동에 의해 수행된다. 
  - Record
    - vis element를 안정적인 형태로 캡쳐하거나 보관하는 것.
  - Derive
    - 기존의 data elements를 기반으로 새로운 data elements를 생성하는 것.
    - **vis design process에서 굉장히 중요한 부분이다. design space를 넓힐 수 있다.**
    - changing types : quantitative(temperature) -> ordinal(hot, warm, or cold)
    - transformation with an additional information : categorical(city name) -> quantitative(latitude, longitude)
    - arithmetic, logical, statistical operations : two quantitative value (export, import) -> new quantitative difference (trade balance)

#### Search
- 관심 있는 element를 search하는 action. target 여부/location 정보 여부에 따라 총 4가지로 분류된다.
- lookup
  - 타겟이 어딘지 알고 위치가 어딘지 아는 경우. 
  - 인간을 찾고 싶고, mammal species에 속한다는 것을 아는 상황.
- locate
  - 타겟은 알지만, 위치를 모르는 경우.
  - 토끼를 찾고 싶지만, 토끼가 어디과에 속하는지 모르는 경우.
- browse
  - 타겟은 모르는데, 위치는 아는 경우.
  - mammal subtree의 모든 leaf를 다 보는 경우.
- explore
  - 타겟도 모르고, 위치도 모르는 경우.
  - scatterplot에서 outliers 찾는 것. 가장 많은 종을 가지는 과를 찾아라?

- lookup/locate 와 browse/explore 를 구별하는 방법은, 타겟을 아는 경우에는 (california, id = 10) lookup/locate에 해당되고, 타겟의 특성을 아는 경우에는 (missing value, x > 10) browse/explore 에 해당된다.
- 예를 들어, 지도에서 캘리포니아의 값을 찾는 search 태스크를 수행할 때, 캘리포니아의 위치를 아는 사람에게는 lookup이고, 모르는 사람에게는 locate이다.
  
#### Query
- 타겟이 발견되고 나면, low-level user goal은 이 타겟을 query하는 것이다. 3단계의 레벨이 있다.
- identify : single target, compare : multiple target, summarize : full set of targets
- example) chropleth map of US election results
  - 사용자는 한 주의 election result를 identify, 한 주의 결과와 다른 주의 결과를 compare하고, 전체 주에서 A후보에 대한 지지자가 얼마나 있는 summarize한다.

### Targets
- Action의 목표가 되는 것으로, 사용자가 관심있는 데이터의 특성이다.
- all data level
  - 3가지 high-level target : trends, outliers, and features.
  - trend
    - _high-level characterization of a pattern in the data._
    - increase, decrease, peak, trough, plateaus, ...
  - outliers
    - anomalies, novelties, deviants, surprises
  - features
    - 정확한 의미는 task dependent하다. 관심있는 데이터의 특별한 구조를 의미한다.
- attribute level
  - one attribute
    - find an individual value
    - find the extremes : min, max
  - multiple attributes level
    - dependency
      - 하나의 attribute가 다른 attrbute에 직접적으로 의존하는 것.
    - correlation
      - 하나의 attribute가 다른 attribute에 연관되어 있는 경향.
    - similarity
      - 두 attribute가 얼마나 similar, different 한지에 대한 quantitative measurement.
- specific types of datasets
  - network : topology
  - spatial data : geometric shape


- 책에서는 여러 시각화 예제를 사용해서 what-why-how 를 설명하고 있다. 