---
title: "Command Pattern"

categories:
  - designpattern

tags:
  - designpattern
---

### Command Pattern
- Purpose
  - Encapsulate a request as an object
- Use When
  - requests 를 다양한 시간대와 순서로 관리할 수 있다.
    - queuing, callback 등 가능하다.
  - request history 를 관리할 수 있다(undo 기능 지원).
  - invoker 를 invocation 을 받는 receiver 와 분리할 수 있다.
    - invoker - command - receiver

![Validation](/assets/images/CommandPattern.png){:width="500px" height="300px"}{: .center}


### Remote Control Example
- remote control - invoker
- CeilingLight, TV, GarageDoor, ... - receiver

### Without Using Command Pattern

```java
if(command == slotOn) light.on();
else if(command == slotOff) light.off();
```

- 위 client code 의 문제점은 client 에서 구체적인 receiver의 행동에 대한 dependency 로 인해 코드 수정이 어렵다는 것이다.

### To the Command Pattern
1. client 가 command object 를 생성한다.
2. client 는 setCommand 메소드를 사용해 invoker(리모컨 버튼) 에게 command 를 연결한다.
3. client 코드에서 invoker 의 invocation 을 발생시켰다.
4. invoker 에 연결되어 있는 command 의 execute 메소드를 실행한다.
5. command 는 receiver 에게 해당하는 액션을 취한다.


### Simple Code

```java
public interface Command{
    public void execute();
}

public class LightOnCommand implements Command{
    Light light;

    public LightOnCommand(Light light){
        this.light = light;
    }

    public void execute(){
        light.on();
    }
}

public class SimpleRemoteControl{ // invoker
    Command slot;

    public SimpleRemoteControl(){}
    public void setCommand(Command command){
        slot = command;
    }
    public void buttonWasPressed(){
        slot.execute();
    }
}

public class RemoteControlTest{
    public static void main(String[] args){
        SimpleRemoteControl remote = new SimpleRemoteControl();
        Light light = new Light();
        LightOnCommand lightOn = new LightOnCommand(light);

        remote.setCommand(LightOn);
        remote.buttonWasPressed();
    }
}
```


### Assigning Command to Slots

```java
public class RemoteControl{ // invoker
    Command[] onCommands;
    Command[] offCommands;

    public RemoteControl(){
        onCommands = new Command[7];
        offCommands = new Command[7];

        Command noCommand = new NoCommand();
        for(int i=0;i<7;i++){
            onCommands[i] = noCommand;
            offCommands[i] = noCommand;
        }
    }
    public void setCommand(int slot, Command onCommand, Command offCommand){
        onCommands[slot] = onCommand;
        offCommands[slot] = offCommand;
    }

    public void onButtonWasPressed(int slot){
        onCommands[slot].execute(); // invoker 가 직접 receiver 에 접근하지 않고, command 에게 delegation 한다.
    }

    public void offButtonWasPressed(int slot){
        offCommands[slot].execute();
    }
}

// 그 외 추가적인 device 들의 on/off concrete command 를 구현했다고 가정하자.

public class RemoteControlTest{
    public static void main(String[] args){
        RemoteControl remote = new RemoteControl();
        Light lovingRoomLight = new Light("livingroom");
        Light kitchenLight = new Light("kitchen");
        Stereo stereo = new Stereo("livingroom");

        LightOnCommand livingRoomLightOn = new LightOnCommand(lovingRoomLight);
        LightOffCommand livingRoomLightOff = new LightOffCommand(lovingRoomLight); 
        LightOnCommand kitchenLightOn = new LightOnCommand(kitchenLight);
        LightOffCommand kitchenLightOff = new LightOffCommand(kitchenLight);
        //...

        remoteControl.setCommand(0, livingRoomLightOn, livingRoomLightOff);
        remoteControl.setCommand(1, kitchenLightOn, kitchenLightOff);

        remote.onButtonWasPressed(0);
        remote.onButtonWasPressed(1);
        remote.offButtonWasPressed(0);
    }
}
```

### Supporting Undo - Simple Case

```java
public interface Command{
    public void execute();
    public void undo();
}

public class LightOnCommand implements Command{
    Light light;

    public LightOnCommand(Light light){
        this.light = light;
    }

    public void execute(){
        light.on();
    }

    public void undo(){
        light.off();
    }
}
```


### Supporting Undo

```java
public class RemoteControl{ // invoker
    Command[] onCommands;
    Command[] offCommands;
    Command undoCommand;

    public RemoteControl(){
        onCommands = new Command[7];
        offCommands = new Command[7];

        Command noCommand = new NoCommand();
        for(int i=0;i<7;i++){
            onCommands[i] = noCommand;
            offCommands[i] = noCommand;
        }
        undoCommand = noCommand;
    }
    public void setCommand(int slot, Command onCommand, Command offCommand){
        onCommands[slot] = onCommand;
        offCommands[slot] = offCommand;
    }

    public void onButtonWasPressed(int slot){
        onCommands[slot].execute(); 
        undoCommand = onCommands[slot]; // 가장 마지막에 실행한 command 를 저장한다.
    }

    public void offButtonWasPressed(int slot){
        offCommands[slot].execute();
        undoCommand = offCommands[slot]; // 가장 마지막에 실행한 command 를 저장한다.
    }

    //undo 지원하는 기능
    public void undoButtonWasPushed(){
        undoCommand.undo();
    }
}
```

### Using State to Implement Undo

```java
public class CeilingFan{
    public static final int HIGH = 3;
    public static final int MEDIUM = 2;
    public static final int LOW = 1;
    public static final int OFF = 0;
    String location;
    int speed;

    public CeilingFan(String location){
        this.location = location;
        speed = OFF;
    }

    public void high(){
        speed = HIGH;
    }

    // ...
}

public class CeilingFanHighCommand implements Command{
    CeilingFan ceilingfan;
    int prevSpeed;

    public CeilingFanHighCommand(CeilingFan ceilingfan){
        this.ceilingfan = ceilingfan;
    }

    public void execute(){
        prevSpeed = ceilingfan.getSpeed(); // 실행하기 전에 이전 상태를 저장한다.
        ceilingfan.high();
    }

    public void undo(){
        if(prevSpeed = CeilingFan.HIGH){
            ceilingFan.HIGH();
        }
        else if()
        //...
    }
}
```

### Party Mode!
- Composite pattern 이용
  - MacroCommand 객체 생성 (class diagram 참고)
    - MacroCommand 내부에는 단일 command 도 포함하고 있어서 Command 인터페이스와 composition 관계를 가진다.
    - MacroCommand 도 단일 Command 와 동일하게 보여야 하므로 상속 관계도 가진다.

```java
public class MacroCommand implements Command{
    Command[] commands;

    public MacroCommand(Command[] commands){
        this.commands = commands;
    }

    public void execute(){
        for(int i=0;i<commands.length;i++){
            commands[i].execute();
        }
    }
}

public class RemoteControlTest{
    public static void main(String[] args){
        RemoteControl remoteControl = new RemoteControl();
        Command[] partyOn = {lightOn, stereoOn, tvOn, hottubOn};
        Command[] partyOff = {lightOff, stereoOff, tvOff, hottubOff};

        MacroCommand partyOnMacro = new MacroCommand(partyOn);
        MacroCommand partyOffMacro = new MacroCommand(partyOff);
        
        remoteControl.setCommand(0, partyOnMacro, partyOffMacro); // MacroCommand 도 Command 를 implements 하므로 타입 에러 발생하지 않는다.
    }
}
```

### Review
- Command 객체는 operation 을 invoke 하는 invoker 와 receiver 에 액션을 취하는 객체를 분리한다.
- commands 를 하나의 composite command 로 만들 수 있다.
  - MacroCommand class, Composite pattern 의 객체 형태
- 새로운 Command 를 추가하기 용이하다. 