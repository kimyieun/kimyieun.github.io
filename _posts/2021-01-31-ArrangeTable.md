---
title : "Arrange Tables"

categories :
    - visualization

tags :
    - visualization

---


### Book : Visualization Analysis and Design - Tamara Munzner (Chapter 7)

### 7.2 Why Arrange?
- The arrange design choice covers all aspects of the use of spatial channels for visual encoding.
- quantitative/ordered attributes 에서 가장 효과적인 채널 top3 모두 spatial position과 관련 있다.
  - planar position against a common scale, planar position along an unaligned scale, and length.
- categorical attributes 에서 가장 효과적인 채널은 동일한 위치에 아이템들을 그루핑하는 것이다.

### 7.3 Arrange by Keys and Values
- key
  - independent attribute. table에서 item 검색 시 사용하는 unique index.
  - categorical or ordinal.
- value
  - dependent attribute. table 에서 cell의 값.
  - categorical, ordinal, or quantitative.
- level
  - categorical / ordered attribute 의 unique value. (value 랑 혼동 주의)
- how many keys and how many values does it have?
  - no key - scatterplot(two values), one key and one value - bar chart, two keys and one value - heatmap

### 7.4 Express: Quantitative Values
- quantitative attributes 를 표현하기 위해 공간을 사용하는 것은 직관적이다. attribute는 축을 따라 공간적인 위치에 mark로 매핑이 된다. 
- 좀 더 복잡한 케이스는 glyph 를 사용한다. glyph를 subregion에 multiple attributes 표현 가능하다.

- Scatterplot
![Validation](/assets/images/scatterplot.png){:width="300px" height="300px"}{: .center}

![Validation](/assets/images/scatterplotDescription.png){:width="500px" height="400px"}{: .center}
  - point 를 알아볼 수 있을 정도가 되어야 한다. (수십개에서 수백개 정도가 적합)
  - 부가적인 attribute 를 보여주기 위해서 color coding 을 사용할 수 있다. 또한, size coding 을 사용하는 경우는 특수하게 **bubble plot** 이라 부른다.


### 7.5 Separate, Order, and Align: Categorical Regions
- categorical attributes 표현을 위해서 space 를 사용하는 경우는 quantitative attributes 보다 더 복잡하다. 그 이유는 아래와 같다.
  - Spatial position is an ordered magnitude visual channel, but categorical attributes have **unordered identity semantics.**
- categorical value 를 spatial position에 매핑하면 expressiveness 원칙을 어기는 것이므로, 이를 대신해 spatial region을 사용한다.
- regions
  - contiguous bounded areas that are distinct from each other.
  - 동일한 value 를 갖는 categorical attribute 에 대해서 동일한 region에 그림으로써 spatial proximity 를 사용해서 그들간의 유사성을 표현할 수 있다. expressiveness principle 에 적절하다.
  - 3가지 operation 으로 나눌 수 있다. 
    - **separating** into regions - categorical 한 attribute 에 맞게 separate 되어야 한다.
    - **aligning** the regions, and **ordering** the regions - ordered attribute 에 의해 align, ordered 되어야 한다.

#### 7.5.1 List Alignment: One Key
- key가 하나일 때, item 별로 각자의 region을 갖는다. 이 때, region은 주로 horizontal 또는 vertical 하게 1D list alignment 에 배치된다.

- Bar chart

![Validation](/assets/images/barchart.png){:width="500px" height="300px"}{: .center}

![Validation](/assets/images/barchartInfo.png){:width="500px" height="300px"}{: .center}

  - Each line mark is indeed in a separate region of space, and there is one for each level of the categorical attribute.
  - x축 정렬 기준 다양하다. (값, 알파벳 순 등등). data-driven ordering 을 잘하면 dataset trends 를 보기 용이하다.
  - screen width 한계로 인해 key 개수가 수백 개 정도만 표현할 수 있다.

- Stacked bar chart

![Validation](/assets/images/stackedBarChart.png){:width="400px" height="300px"}{: .center}

![Validation](/assets/images/stackedBarChartInfo.png){:width="500px" height="300px"}{: .center}
  - stack 의 가장 바닥의 요소 외에는 bar 간의 비교가 어렵다. 이유는 starting points 가 common scale 에 맞춰 align 되어있지 않기 때문이다.
  - stacking 순서에 따라 중요한 패턴을 쉽게 볼 수 있느냐 없느냐가 결정된다.
  
- Streamgraphs

![Validation](/assets/images/streamgraph.png){:width="400px" height="300px"}{: .center}

![Validation](/assets/images/streamgraphInfo.png){:width="500px" height="300px"}{: .center}

  - a more complex generalized stacked graph display idiom. 
  - layout 은 전체 모양의 외부 실루엣과 baseline 으로부터 각 레이어의 편차, baseline 의 높낮이 정도를 고려하여 절충안으로 최적화된다. 
  - layer의 순서는 derived value 를 강조하도로고 하는 알고리즘에 의해 배치된다.
  - Stacked bar chart 와 비교해서 categories 개수를 더 많이 사용할 수 있다. width 에 영향을 주는 것이 아니기 때문.

- Dot and Line Charts
![Validation](/assets/images/dotlinechart.png){:width="500px" height="300px"}{: .center}

![Validation](/assets/images/dotchartInfo.png){:width="500px" height="300px"}{: .center}

![Validation](/assets/images/linechartInfo.png){:width="500px" height="300px"}{: .center}

#### 7.5.2 Matrix Alignment : Two Keys

- 2D matrix alignment 를 주로 사용한다.
- Heatmaps
  - heatmap idiom : matrix alignment 의 가장 단순한 형태.
  - bioinformatics datasets
  - benefits
    - quantitative data 를 시각적으로 작은 area mark에 color를 사용해 표현하는 것이 매우 compact 하다. -> overview 를 보기에 용이하다. (high information density)
    - area mark 의 가장 작은 단위는 하나의 픽셀이므로, 1M 개의 데이터를 보여줄 수 있다.
  - color perception 의 한계 (noncontiguous regions에서 3 - 11 bins)로, 다양한 level을 구별하기는 어려울 수 있다.
- Cluster Heatmap
  - basic heatmap with matrix reordering
  - matrix reordering 목표 - 비슷한 셀을 grouping 함으로써, attributes 간의 큰 패턴을 보고자 한다.
  - juxtaposed combination of a heatmap and two dendrograms showing the derived data of the cluster hierarchies.
  - dendrogram - tree data.

![Validation](/assets/images/clusterHeatmap.png){:width="500px" height="400px"}{: .center}

- Scatterplot Matrix (SPLOM)
  - 하나의 cell이 scatterplot chart 를 포함하는 matrix. attributes 의 모든 가능한 pairwise 조합을 제공한다.
  - key 는 rows, columns 에 동일한 attribute 이다.
  - correlations, trends, outliers 를 찾는 task 에서 많이 활용한다.
  - one dozen attributes and hundreds of items.

#### 7.5.3 Volumetric Grid : Three Keys

- non-spatial data 에서는 권장하지 않는다. perceptual problems 을 발생시키기 때문이다. (occlusion, perspective distortion 등)

#### 7.5.4 Recursive Subdivision : Multiple Keys

-

### 7.6 Spatial Axis Orientation

#### 7.6.1 Rectilinear Layouts

- regions or items are distributed along two perpendicular axes.
- most common statistical charts.

#### 7.6.2 Parallel Layouts

- rectilinear layouts 은 two data attributes 에서만 사용 가능하다.
- parallel coordinates
  - many quantitative attributes at once using spatial position.
  - checking for correlation between attributes.
    - positive - parallel / negative - cross over each other / uncorrelated - mix of crossing angles
    - 실제로는 SPLOM이 correlation 을 보는데에 더 적합하다. parallel coordinates 는 전체 attributes 에 대한 overview를 보거나, 각 attribute의 range 를 보거나, outlier detection, items의 범위 선택 등에 유용하다.
  - scalability - attributes 개수가 dozens is common / items : 수백만. occlusion 발생
  - pattern 이 잘 보이게 하려면, 이웃하는 축간의 관계를 잘 알아야 한다. 축의 순서를 정하는 것이 중요하지만, 모든 조합을 시도해보는데에는 시간이 굉장히 많이 소요된다.
  - limitation - training time (처음 보는 사용자는 패턴의 의미에 대한 직관이 없다. 보통 하나의 view로만 사용하지 않고, 여러 개를 조합해서 보는 방식이다.)

![Validation](/assets/images/parallelCoordinate.png){:width="500px" height="400px"}{: .center}

![Validation](/assets/images/parallelCoordiDescription.png){:width="500px" height="400px"}{: .center}

#### 7.6.3 Radial Layouts

- items 원을 그리며 배치된다.
- Radial Bar Charts
- Pie Charts
  - 인기는 많지만, 문제가 많다.
  - angle judgements 가 length judgements 보다 부정확하다.
  - polar area chart - pie chart 처럼 각도로 비교하지 않고, bar length 로 비교한다.
  - 가장 유용한 특징으로는 전체에 대한 부분의 상대적인 비율을 비교할 수 있다는 것이다. 하지만, pie chart 에서만 보여줄 수 있는 건 아님. normalized stacked bar chart. 심지어 pie chart 보다 더 잘 보여줌.
  - 공간도 많이 차지한다.

![Validation](/assets/images/pieChart.png){:width="500px" height="400px"}{: .center}

![Validation](/assets/images/piechartComparison.png){:width="500px" height="400px"}{: .center}
