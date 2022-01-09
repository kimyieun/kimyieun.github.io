---
title: "Interpreter Pattern"

categories:
  - designpattern

tags:
  - designpattern
---

### Interpreter Pattern
- 23가지 패턴 중 가장 어려운 패턴이다.
- **문법 규칙을 클래스로 표현한다.**
- 프로그램이 해결하고자 하는 문제를 간단한 미니 언어로 표현한다. 
- 미니 언어로 적힌 미니 프로그램은 자바 언어로 interpret 하는 역할을 하는 프로그램을 생성한다.
- interpreter 는 미니 언어를 이해하고, 미니 프로그램을 해석 및 실행한다.
- 사용자가 프로그램을 변경하고 싶을 때는 자바 코드를 수정하지 않고, 미니 프로그램을 수정한다.

### 미니 언어 문법

```
<program> : : = program <command list>
<command list> : : = <command> * end
<command> : : = <repeat command> | <primitive command>
<repeat command> : : = repeat <number> <command list>
<primitive command> : : go | right | left 
```

- 재귀적인 정의
  - 어떤 것을 정의하는데 자기 자신이 등장하는 경우.
  - command list 는 command list 를 포함할 수 있다.
  - 재귀적인 메소드 : 메소드가 자기 자신을 호출한다. 


- terminal expression
  - 더 이상 전개되지 않는 expression. primitive command
- non-terminal expression
  - 계속해서 다시 전개되는 expression. program, command, repeat command, command list 
- 토큰은 구문 해석 시 처리의 기본 단위를 말한다. 



![Validation](/assets/images/interpreterpattern.png){:width="500px" height="300px"}{: .center}

- Context class
  - Node interface 에 대한 지식이 전혀 없다.
  - interpret 를 위해 토큰과 관련된 필요한 메소드를 제공한다. 
  - 현재 어느 토큰까지 해석이 진행되었는지 알고 있다.
- Node abstract class
  - 구문 트리의 각 부분을 대표하는 최상위 클래스
  - parse 함수는 구문 해석을 실행하기 위한 메소드이다.


```java
public class Context{ // interpreter 가 구문 해석을 수행하도록 정보 제공하는 역할(토큰 관리)
  private StringTokenizer tokenizer;
  private String currentToken;

  public String nextToken(){
    // 다음 토큰을 얻는다.
  }

  public String currentToken(){
    // 현재 토큰을 얻는다.
  }

  public void skipToken(String token) throws ParseException {
    // 현재 토큰 체크하고 나서 다음 토큰을 얻는다.
    // token 문자열과 currentToken 문자열이 같은지 확인한다.
  }
}

public abstract class Node { // 구문 트리의 각 부분(노드)을 대표하는 최상위 클래스
	public abstract void parse(Context context) throws ParseException;
}

public class ProgramNode extends Node {
  private Node commandListNode;
  public void parse(Context context) throws ParseException{
    context.skipToken("program");
    commandListNode = new CommandListNode(); // <program> 뒤에 나오는 <command list> 노드 객체를 생성하고 parsing 을 맡긴다.
    commandListNode.parse(context);
  }
}

public class Main {
  public static void main(String[] args) {
    while((text = reader.readLine()) != null){
      Node node = new ProgramNode(); // 매 줄 마다 ProgramNode 부터 시작한다.
      node.parse(new Context(text)); // 파싱을 시도한다.
    }
  }
}

```
