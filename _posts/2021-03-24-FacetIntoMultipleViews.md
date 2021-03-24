---
title: "Facet into Multiple Views"

categories:
  - visualization

tags:
  - visualization
---

- 이 챕터에서는 display 을 multiple views/layers 로 나누는 design choices 를 탐색한다.


## 12.2 Why Facet?
- facet means **to split.**
- 5 approaches to handling visual complexity
  - **juxtaposing coordinated views side by side and superimposing layers within a single view.**
  - deriving new data to include in a view.
  - changing a single view over time.
  - reducing the amount of data to show in a view.
  - embedding focus and context information within the same view.

## 12.3 Juxtapose and Coordinate Views
- how to coordinate between them to create **linked views.**


### 12.3.1 Share Encoding : Same / Different
- shared encoding views
  - all channels are handled the same way for an idintical visual encoding.
- multiform views
  - some aspects of the visual encoding differ between the two views.
- Interactivity
  - **linked highlighting**
    - items that are interactively selected in one view are immediately highlighted in all other views using in the same highlight color.
    - good for seeing how a region that is contiguous in one view is distributed within another.
- single view 는 visual clutter 를 발생시키지 않으면서 동시에 보여줄 수 있는 attributes 의 개수가 많지 않다.
- 복잡한 태스크의 경우에는 multiple view 를 사용해 각 view가 attribute 의 일부만 보여줌으로써 visual clutter 를 피할 수 있다. 
  - 이 때 중요한 것은 spatial position 이 어떻게 인코딩 되느냐이다.
    - 가장 핵심적인 채널이 view를 거의 차지하고, 잘 지원할 수 있는 task 가 무엇인지에 엄청난 영향을 줄 수 있다.

### Exploratory Data Visualizer (EDV)

![Validation](/assets/images/edv.png){:width="500px" height="500px"}{: .center}

### 12.3.2 Share Data : All, Subset, None
-  how much data is shared between the two views.
   -  shared data.
   -  overview-detail.
   -  small multiples.

- shared data
  - common data but different encoding.
- overview-detail
  - one of the views shows information about the entire dataset to provide an overview of everything.
  - one or more additional views show more detailed information about a subset of the data.
  - 가장 대표적인 예시로는 shared encoding 과 data 를 사용하면서 navigation support 를 제공하는 idiom 이다.
    - main view 는 detail 을 탐색하기 위한 용도이고, small view 는 zoomed-out overview 로 사용된다. 
    - 또 다른 경우로는 large view 가 overview 를 제공하고, smaller view 가 details 를 제공한다.

- chooing how many views to use in total.
  - a common choice - have only two view. one for overview and one for detail.
  - dataset 이 multilevel 구조이면, multiple detail views 가 적절할 수 있다.

### Bird's-Eye Maps

![Validation](/assets/images/birdseye.png){:width="400px" height="400px"}{: .center}

- combining the choices of overview-detail for data sharing with multiple views. 
  - **detail-on-demand view.**
  - detail view 는 main view 에서 선택된 몇 가지의 items 에 대해서만 부가적인 정보를 제공한다. popup 형태나, 고정된 위치.

![Validation](/assets/images/detailondemand.png){:width="500px" height="500px"}{: .center}

- small multiples
  - small multiples 는 list 나 matrix 형태로 align 한다. 
  - multiform views 랑 다른 점은, encoding 은 동일하나 data 가 다르다.
  - 단점 - screen size 가 제한이 있다.
  - 장점 - dataset 의 다른 partitions 을 side by side 로 동시에 볼 수 있어서 사용자의 interaction, memory cost 가 덜 든다. 
  - animations 대신 사용되기도 한다. animation 은 memory load 를 많이 필요로 한다는 단점이 있다.
- Figure 12.5



### 12.3.3 Share Navigation : Synchronize
- With linked navigation, moving the viewpoint in one view is synchronized to movement in the others.
- small window 에서의 interaction 으로 large window 의 viewpoint 를 변경한다.

### 12.3.4 Combinations
![Validation](/assets/images/combination.png){:width="500px" height="500px"}{: .center}

![Validation](/assets/images/combination2.png){:width="500px" height="500px"}{: .center}

  - multiform views. sharing the same color encoding. small-multiple views. linked highlighting. overview-detail views.

### 12.3.5 Juxtapose Views
- 일반적인 juxtaposition 방식은 모든 view 가 사용자에게 계속해서 노출되는 것이다. 반대로 잠깐 pop up 형태로만 지속될 수도 있다.
- views arrangement 도 중요하다.
  - 사용자에게 manually arrange 하도록 할 수 있지만, 수가 너무 많으면 burden 이 될 수 있다.
  - 자동화도 가능.

## 12.4 Partition into Views
- how to partition a multiattribute dataset into meaningful groups
  - **what kind of patterns are visible to the user**
- how many splits to carry out
- the order in which attributes are used to split things up
- how many views to use

### 12.4.1 Regions, Glyphs, and Views
- partionings 간의 연결을 위해서는 partitioned group 이 region of space 에 함께 있어야 한다. 
- multiple keys - several possibilities for separation.
- mark - a single geometric primitive.
- glyph - an object with internal structure that arises from multiple marks.
- view - showing a complete visual encoding of marks and attirubtes.


### 12.4.2 List Alignments
- Grouped bar chart - multibar glyph
- Small multiple bar chart
- grouped bar chart 는 attributes 간의 비교하는 태스크에 적합하고, small multiple bar chart 는 하나의 attribute 내에서의 비교하는 태스크에 적합하다.
- glyph point of view
  - grouped bars idiom - smaller multibar glyph.
  - small-multiple bars idiom - larger bar-chart glyph.
- partitioning point of view
  - two levels of partitioning.
  - grouped bars idiom - second-level regions are interleaved within the first-level regions.
  - small-multiple bars idiom - second-level regions are contiguous within a single first-level region.

### 12.4.3 Matrix Alignments

![Validation](/assets/images/matrixalignment.png){:width="400px" height="400px"}{: .center}

![Validation](/assets/images/matrixalignment2.png){:width="400px" height="300px"}{: .center}
- Main effects ordering
  - partitioning 으로 나눠진 group 간의 derived attribute 를 만들고, 이것을 기준으로 ordering 을 한다.
  - 공간적으로 정보를 ordering 하는 data-driven way 이므로, trends, outliers 발견이 쉽다. 

### 12.4.4 Recursive Subdivision

![Validation](/assets/images/recursiveSubdivision.png){:width="400px" height="400px"}{: .center}

- (a)
  - top-level : type attribute, second-level : neighborhood attribute, bottom-level : time
  - house type 별로 굉장히 다르다. 그러나 neighborhood 는 일관적이다. 동일한 neighborhood 인 집 가격은 비슷한 경향이 있다.
- (b)
  - top-level : neighborhood, second-level : type
  - expensive neighborhood 를 찾기 쉽다. 그리고 type 중 오른쪽 아래의 type이 다른 types 에 비해 비싼 경향이 있다.
- Figure 12.12
- (a)
  - spatial arrangement 가 다르다. sales 개수에 따라 size encoding. 
- (b)
  - second-level 에서 choropleth maps 를 사용해 geographic 정보 제공한다.


## 12.5 Superimpose Layers
- Superimpose
  - Combining multiple layers together by stacking them directly on top of each other in a single composite view.
- Visual layer
  - a set of objects spread out over a region.
  - 각 layer 의 object sets 은 시각적으로 구별 가능하다.
- design choices
  - How many layers are used? 
  - How are the layers visually distinguished from each other? 
  - Is there a small static set of layers that do not change, or are the layers constructed dynamically in response to user selection?
  - How to partition items into layers?

### 12.5.1 Visually Distinguishable Layers
- 각 layer 가 서로 다르고 겹치지 않는 범위의 visual channel 을 사용하면 된다.
- e.g., foreground vs. background
- layer 개수가 늘어날수록 시각적으로 구별하기 어려워진다. 

### 12.5.2 Static Layers
- 모든 layer 가 동시에 화면에 보여진다. 사용자는 어떤 layer 에 focus 할 지 결정해야 한다.
![Validation](/assets/images/superimposedlayers.png){:width="400px" height="400px"}{: .center}

- (a)
  - area mark - background layer, color - water, parks, land 구별. 
- (b)
  - luminance contrast 를 제공하므로 각 layer 가 구별되어 보인다.

- Superimposed Line Charts
![Validation](/assets/images/groupedbarchart.png){:width="400px" height="400px"}{: .center}

![Validation](/assets/images/superimposedlinechart.png){:width="400px" height="400px"}{: .center}

  - Superimposed line charts vs. juxtaposed small multiples (area chart)
  - visual clutters vs. available vertical space
  - comparison within a local visual span vs. large visual span.

### Hierarchical Edge Bundles
- Figure 12.16
- Edge bundling
  - reduce occlusion. 

### 12.5.3 Dynamic Layers
- With dynamic layers, a layer with different salience than the rest of the view is constructed interactively, typically in response to user selection.
![Validation](/assets/images/dynamiclayers.png){:width="400px" height="400px"}{: .center}

  - 사용자가 커서를 움직일 때마다 foreground layer 가 변경된다.