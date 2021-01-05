# DOM

### DOM (Document Object Model)
- **HTML 문서의 계층적 구조와 정보를 표현하며 이를 제어할 수 있는 API, 즉 프로퍼티와 메소드를 제공하는 트리 자료구조**이다.

### Node

### HTML 요소와 노드 객체
- HTML 요소란 HTML 문서를 구성하는 개별적인 요소를 의미한다.
- HTML 요소의 구조 : <시작태그 어트리뷰트 이름="어트리뷰트 값">콘텐츠</종료태그>
- **HTML 요소는 렌더링 엔진에 의해 파싱되어 DOM을 구성하는 요소 노드 객체로 변환된다.** 어트리뷰트는 어트리뷰트 노드로, 텍스트 콘텐츠는 텍스트 노드로 변환된다.
- HTML 요소는 중첩 관계를 갖는다. 콘텐츠 영역에 다른 HTML 요소를 포함할 수 있다.
- 노드 객체들로 구성된 트리 자료구조를 DOM, DOM 트리라 부르기도 한다.

### 노드 객체의 타입
- 노드 객체는 총 12개의 노드 타입이 있다. 중요한 노드 타입은 4가지이다.
- Document node
  - DOM 트리의 최상위에 존재하는 루트 노드. document 객체를 가리킨다. document 객체는 브라우저가 렌더링한 HTML 문서 전체를 가리키는 객체로서 전역 객체 window의 document property에 바인딩되어 있다. 
  - 브라우저 환경의 모든 자바스크립트 코드는 script 태그에 의해 분리되어 있어도 하나의 window 전역 객체를 공유한다. 즉, HTML 당 document 객체는 유일하다.
- Element node
  - HTML 요소를 가리키는 객체. 문서의 구조를 표현한다.
- Attribute node
  - HTML 요소의 어트리뷰트를 가리키는 객체. 어트리뷰트 노드는 어트리뷰트가 지정된 HTML 요소의 요소 노드와 형제 관계를 갖는다. 
  - 단 요소 노드는 부모와 연결되어 있지만, 어트리뷰트 노드는 형제 노드인 요소 노드에만 연결되어 있다. 따라서 attribute 참조/변경시, 먼저 형제 노드인 요소 노드에 접근해야 한다.
- Text node
  - HTML 요소의 텍스트를 가리키는 객체. 요소 노드의 자식 노드이면서 리프 노드이다. DOM 트리의 최종단이다. 

### Element node 취득
- HTML 의 구조나 내용, 스타일을 동적으로 조작하려면 요소 노드 취득이 필요하다.
  
#### 1. id를 이용한 요소 노드 취득
-  Document.prototype.getElementById 메소드는 인수로 전달한 id 어트리뷰트 값을 갖는 하나의 요소 노드를 반환한다. getElementById 메소드는 Document.prototype 의 프로퍼티이므로, 반드시 document 를 통해 호출해야 한다.
-  id 는 HTML 문서 내에 유일한 값이어야 하며, class 와는 다르게 공백 문자로 구분해 여러 개의 값을 가질 수 없다. 
-  id 값을 갖는 HTML 요소가 없는 경우 null을 반환한다.
-  id attribute 를 부여하면 id 값과 동일한 이름의 전역 변수가 암묵적으로 선언되고, 해당 노드 객체가 할당된다.
   ```html
    <div id="foo"></div>
    <script>
        console.log(foo === document.getElementById('foo')); // true
    </script>
    ```
- 단, id 값과 동일한 이름의 전역 변수가 이미 선언되어 있으면 이 전역 변수에 노드 객체가 재할당되지 않는다.

#### 2. 태그 이름을 이용한 요소 노드 취득
- Document.prototype/Element.prototype.getElementsByTagName 메소드는 인수로 전달한 태그 이름을 갖는 모든 요소 노드들을 반환한다. 여러 개의 요소 노드 객체를 갖는 DOM 컬렉션 객체인 HTMLCollection 객체를 반환한다. 
- HTMLCollection 객체는 유사 배열 객체이면 이터러블이다.

```html
<body>
  <ul id="fruit">
    <li id="apple">apple</li>
    <li id="banana">banana</li>
    <li id="orange">orange</li>
  </ul>
  <script>
  const $elems = document.getElementsByTagName('li');
  [...$elems].forEach(elem => {elem.style.color = 'red'; });
  </script>
</body>
```s
- HTML  문서의 모든 요소 노드를 취득하려면 getElementByTagName 메소드의 인수로 '*'를 전달한다.
  
```javascript
const $all = document.getElementsByTagName(*);
// -> HTMLCollection(8) [html, head, body, ul, li#apple, ....]
```

- getElementByTagName 메소드는 Document.prototype 에 정의된 메소드랑 Element.prototype 에 정의된 메소드가 있다.
  - Document.prototype.getElementByTagName
    - DOM 의 root node 인 document 를 통해 호출하며 DOM 전체에서 요소 노드를 탐색하여 반환한다.
  - Element.prototype.getElementByTagName
    - 특정 요소 노드를 통해 호출하며, 특정 요소 노드의 자손 노드 중에서 요소 노드를 탐색하여 반환한다.

```javascript
const $fruits = document.getElementById('fruit');
const $lisFromFruits = $fruits.getElementByTagName('li');
console.log($lisFromFruits); // HTMLCollection(3) [li, li, li]
```

#### 3. class를 이용한 요소 노드 취득
- Document.prototype/Element.prototype.getElementsByClassName 메소드는 인수로 전달한 class 값을 갖는 모든 요소 노드들을 탐색하여 반환한다. 인수로 전달할 class 값은 공백으로 구분하여 여러 개의 class 를 지정할 수 있다.
- HTMLCollection 객체 반환한다.

```html
<body>
  <ul>
    <li class="fruit apple">apple</li>
    <li class="fruit banana">banana</li>
    <li class="fruit orange">orange</li>
  </ul>
  <script>
  const $elems = document.getElementsByClassName('fruit');
  [...$elems].forEach(elem => {elem.style.color = 'red'; });

  const $apples = document.getElementsByClassName('fruit apple');
  [...$apples].forEach(elem => {elem.style.color = 'red'; });
  </script>
</body>
```
- getElementsByClassName 메소드는 Document.prototype 에 정의된 메소드랑 Element.prototype 에 정의된 메소드가 있다.
  - Document.prototype.getElementsByClassName
    - DOM 의 root node 인 document 를 통해 호출하며 DOM 전체에서 요소 노드를 탐색하여 반환한다.
  - Element.prototype.getElementsByClassName
    - 특정 요소 노드를 통해 호출하며, 특정 요소 노드의 자손 노드 중에서 요소 노드를 탐색하여 반환한다.

#### 4. css selector를 이용한 요소 노드 취득
- css selector는 스타일을 적용하고자 하는 HTML 요소를 특정할 때 사용하는 문법이다.
  ```css
  * { ... } /* 전체 선택자 */
  p { ... } /* 태그 선택자 */
  #foo { ... } /* id 선택자 */
  .foo { ... } /* class 선택자 */
  input[type=text] { ... } /* attribute 선택자 */
  div p { ... } /* 후손 선택자 */
  div > p { ... } /* 자식 선택자 */ 
  p + ul { ... } /* 인접 형제 선택자 */
  p ~ ul { ... } /* 일반 형제 선택자 */
  a:hover { ... } /* 가상 클래스 선택자 */ 
  p::before { ... } /* 가상 요소 선택자 */ 
  ```

- Document.prototype/Element.prototype.querySelector 메소드는 인수로 전달한 css selector를 만족시키는 하나의 요소 노드를 탐색하여 반환한다.
  - 인수로 전달한 css selector 를 만족시키는 요소 노드가 여러 개인 경우 첫 번째 요소 노드만 반환한다. 만족하는 요소 노드가 없는 경우 null 을 반환한다.
- Document.prototype/Element.prototype.querySelectorAll 메소드는 인수로 전달한 css selector를 만족시키는 모든 요소 노드를 탐색하여 반환한다.
  - DOM 컬렉션 객체인 NodeList 객체를 반환한다. 이 객체는 유사 배열 객체이면서 이터러블이다.

```html
<body>
  <ul>
    <li class="apple">apple</li>
    <li class="banana">banana</li>
    <li class="orange">orange</li>
  </ul>
  <script>
  const $elems = document.querySelectorAll('ul > li');
  $elems.forEach(elem => {elem.style.color = 'red'; }); //NodeList는 forEach 메소드를 제공한다.
  </script>
</body>
```

```javascript
const $all = document.querySelectorAll(*);
// -> NodeList(8) [html, head, body, ul, li#apple, ....]
```

- **css selector를 사용하는 querySelector, querySelectorAll 메소드는 getElementById, getElementBy--- 메소드보다 다소 느린 것으로 알려져 있다.** 하지만, css selector 를 사용하여 구체적인 조건으로 요소 노드 취득 가능하다는 장점이 있다.
- 따라서 id attribute가 있는 요소 노드를 취득할 때에는 getElementById 메소드를 사용하고, 그 외의 경우에는 querySelector, querySelectorAll 을 사용하는 것을 권장한다.  
<br/>

#### 5. 특정 요소 노드 취득할 수 있는지 확인
- Element.prototype.matches 메소드는 인수로 전달한 css selector 를 통해 특정 요소 노드를 취득할 수 있는지 확인한다.
- 이벤트 위임을 사용할 때 유용하다.
```html
<body>
  <ul id="fruits">
    <li class="apple">apple</li>
    <li class="banana">banana</li>
    <li class="orange">orange</li>
  </ul>
  <script>
  const $apple = document.querySelector('.apple');
  console.log($apple.matches('#fruits > li.apple')); //true
  </script>
</body>
```

#### 6.  HTMLCollection과 NodeList
- DOM API 가 여러 개의 결과값을 반환하기 위한 DOM Collection 객체이다.
- 둘 다 유사 배열 객체이면서 이터러블이다. for ... of 문과 스프레드 문법 사용 가능하다.
- 중요한 특징은 **노드 객체의 상태 변화를 실시간으로 반영하는 살아 있는 객체라는 것이다.**
- HTMLCollection 객체는 언제나 live 객체로 동작한다. NodeList 객체는 대부분은 non-live 객체로 동작하지만, 경우에 따라 live 객체로 동작할 때가 있다.

- HTMLCollection
  - getElementByTagName, getElementByClassName 메소드가 반환하는 HTMLCollection 객체는 노드 객체의 상태 변화를 실시간으로 반영하는 live DOM Collection 객체이다. 

```html
<body>
  <ul id="fruits">
    <li class="red">apple</li>
    <li class="red">banana</li>
    <li class="red">orange</li>
  </ul>
  <script>
  const $elems = document.getElementByClassName('red');
  for(let i = 0; i < $elems.length ; i++){
    $elems[i].className = 'blue';
  }
  console.log($elems); // HTMLCollection(1) [li.red]
  </script>
</body>
```

- 모든 li 요소의 class 가 blue로 변경되기를 기대하였으나, 가운데 li 가 그대로 red로 남아 있다. 이유는?
  - i = 0 일 때, 첫번째 li는 'blue' 로 변경되었다. 그리고 **$elems 에서 실시간으로 제거되었다.**
  - i = 1 일 때, 첫번째 li는 제거되었기 때문에 세 번째 li 요소를 가리키고 있다. 따라서 세 번째 li를 'blue' 로 변경하고 $elems 에서 마찬가지로 실시간으로 제거되었다.
  - i = 2 일 때, $elems.length 가 1이므로 반복문이 종료되었다. 따라서 두 번째 li class 값이 변경되지 않았다.

for loop 을 역방향으로 순회하거나, while 문을 사용해서 HTMLCollection 객체에 노드 객체가 남아 있지 않을 때까지 반복하는 방법으로 해결 가능하다.
```javascript
for(let i = $elems.length -1 ; i >= 0; i--){
  $elems[i].className = 'blue';
}
```
```javascript
let i = 0;
while ($elems.length > i){
  $elems[i].className = 'blue';
}
```
가장 간단한 해결책은 부작용을 발생시키는 원인인 HTMLCollection 객체를 사용하지 않는 것이다. 이 객체를 배열로 변환하면 유용한 배열의 고차 함수도 함께 사용할 수 있다.
```javascript
[...$elems].forEach(elem => elem.className = 'blue');
```

- NodeList
  - HTMLCollection 객체의 부작용을 해결하기 위해 querySelectorAll 메소드를 사용하는 방법이 있다. 이 메소드는 DOM Collection 객체인 NodeList 객체를 반환한다.
  - NodeList 객체는 대부분의 경우 non-live 객체로 동작한다. 하지만, **childNodes 프로퍼티가 반환하는 NodeList 객체는 live 객체이므로 주의가 필요하다.**

### 결론
- NodeList, HTMLCollection 객체는 예상과 다르게 동작할 때가 있어 다루기 까다롭고 실수하기 쉽다. 따라서, **배열로 변환하여 사용하는 것을 권장한다.**
- 두 객체 모두 유사 배열 객체이면서 이터러블이므로, 스프레드 문법이나, Array.from 메소드를 사용해 간단하게 배열로 변환 가능하다.
