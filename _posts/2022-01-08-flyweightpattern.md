---
title: "Flyweight Pattern"

categories:
  - designpattern

tags:
  - designpattern
---

### Flyweight Pattern
- 메모리 사용량을 줄이자!
- new Something() 을 실행하면 클래스의 인스턴스가 메모리 공간에 할당된다.
- 인스턴스를 가능한 한 **공유시켜서**, 쓸모없는 new 를 줄이자!


![Validation](/assets/images/flyweight.png){:width="500px" height="300px"}{: .center}


### Example Program
- 특정 a 조건을 만족하면 현재 상태를 보존하는 게임
- 특정 b 조건을 만족하면 이전에 보존해둔 상태로 복귀한다.


```java
public class BigChar{ // 메모리를 많이 차지하는 class
    private char charname;
    private String fontdata;
    public void print(){
        System.out.print(fontdata);
    }
}

public class BigCharFactory{ // singleton 패턴(공장은 1개)
    private Hashtable pool = new Hashtable(); // 이미 생성된 BigChar 인스턴스를 관리한다.

    privat static BigCharFactory singleton = new BigCharFactory();

    private BigCharFactory(){}

    public static BigCharFactory getInstance(){
        return singleton;
    }

    public synchronized BigChar getBigChar(char charname){
        BigChar bc = (BigChar)pool.get(""+charname);
        if(bc == null){
            bc = new BigChar(charname);
            pool.put(""+charname, bc);
        }
        return bc;
    }
}

public class BigString{
    private BigChar[] bigchars;
    public BigString(String string){
        bigchars = new BigChar[string.length()];
		BigCharFactory factory = BigCharFactory.getInstance();

        for (int i = 0; i < bigchars.length; i++) {
			bigchars[i] = factory.getBigChar(string.charAt(i));
		}

        // flyweight pattern 적용하지 않고 매번 instance 를 생성하는 코드
        /* 
        for (int i = 0; i < bigchars.length; i++) {
	        bigchars[i] = new BigChar(string.charAt(i));
	    }
        */
    }

    public void print() {
		for (int i = 0; i < bigchars.length; i++) {
			bigchars[i].print();
		}
	}
}

public class Main {
    public static void main(String[] args) {
        BigString bs = new BigString(args[0]);
        bs.print();
    }
}

```


### Discussion
- 여러 곳에서 공유하고 있기 때문에 변경 사항이 생기면 여러 곳에 영향을 미친다.
- 채색된 큰 문자열을 사용하고 싶다. 색 정보는 어느 클래스에 둘까?
  - BigChar 에 두면 공유되는 모든 BigChar 인스턴스는 동일한 색
  - BigString 에 두면 다르게 표현 가능하다.
- Intrinsic 한 정보 - 공유되어야 하는 정보. 상태에 의존하지 않는 정보
- Extrinsic 한 정보 - 공유하지 않는 정보. 상태에 따라 바뀌는 정보 