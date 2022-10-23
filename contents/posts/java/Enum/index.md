---
title: "Enum"
description: ""
date: 2022-10-23
update: 2022-10-23
tags:
  - java
---

## Enum 이란?

enum은 열거형(enumerated type)이라고 부르며,
서로 연관된 상수들의 집합이다.

## Enum의 사용 이유

그렇다면 왜 Enum을 사용할까?
여러 상수 정의 방법들을 통해 우리는 enum의 등장 배경과 사용 이유를 알 수 있다.

### 상수 정의 방법(Enum의 등장 배경)

#### 1) 단순 변수명

```java
    private static final int APPLE = 1;
    private static final int PEACH = 2;
    private static final int BANANA = 3;

    public static void main(String[] args) {
        int type = APPLE;

        switch (type){
            case APPLE:
                System.out.println("apple!");
                break;
            case PEACH:
                System.out.println("peach!");
                break;
            case BANANA:
                System.out.println("banana!");
                break;
        }
    }
```

우선 우리는 단순한 변수명으로 상수들을 만들어 보았다.
하지만 만약 여기서 company 데이터타입으로 google과 apple 변수를 추가하면 어떻게 될까?
apple이라는 변수명이 겹치기 때문에 컴파일 에러가 발생할 것이다.
이럴경우 우리는 접두어를 사용할 수도 있다.

```java
    private static final int FRUIT_APPLE = 1;
    private static final int FRUIT_PEACH = 2;
    private static final int FRUIT_BANANA = 3;

    private static final int COMPANY_GOOGLE = 1;
    private static final int COMPANY_APPLE = 2;
    private static final int COMPANY_SAMSUNG = 3;
```

컴파일 에러는 해결하였지만,코드가 지저분해 졌다.만약 다른 데이터 타입이 더 추가된다면 가독성면에서는 점점 더 나빠질 것이다.
그래서 다른 방법으로 인터페이스를 사용하는 경우도 있는데 그것을 사용해 보자.

#### 2) 인터페이스

```java
interface FRUIT{
    public static final int APPLE = 1;
    public static final int PEACH = 2;
    public static final int BANANA = 3;
}
interface COMPANY{
    public static final int APPLE = 1;
    public static final int GOOGLE = 2;
    public static final int SAMSUNG = 3;
}
public class Main {
    public static void main(String[] args) {
        int type = FRUIT.APPLE;

        switch (type){
            case FRUIT.APPLE:
                System.out.println("apple!");
                break;
            case FRUIT.PEACH:
                System.out.println("peach!");
                break;
            case FRUIT.BANANA:
                System.out.println("banana!");
                break;
        }
    }
}
```

인터페이스를 사용하니 이전과 비교해 볼 떄 상대적으로 가독성이 좋아졌다. 하지만 만약 아래의 코드가 추가된다면 어떻게 될까?

```java
if(FRUIT.APPLE == COMPANY.APPLE){
            System.out.println("어라? 이게 비교가 되면 안되는데..");
        }
```

우리의 처음 설계목적은 FRUIT 데이터 타입과 COMPANY 데이터 타입이 전혀 다른 것이었다.
하지만 위의 경우,비교가 가능하여 자칫 의도하지 않은 결과를 초래할 수도 있다.우리는 안정성을 고려하여 이러한 경우 컴파일 에러를 발생시키고 싶다.

#### 3) 클래스

```java
class FRUIT{
    public static final FRUIT APPLE = new FRUIT();
    public static final FRUIT PEACH = new FRUIT();
    public static final FRUIT BANANA = new FRUIT();
}

class COMPANY{
    public static final COMPANY GOOGLE = new COMPANY();
    public static final COMPANY APPLE = new COMPANY();
    public static final COMPANY SAMSUNG = new COMPANY();
}
```

위 코드 처럼 클래스 형태로 각각의 데이터 타입을 만들어 준다면,비교하는 과정에서 컴파일 에러를 발생시킬 수 있다.
하지만 switch문에서 컴파일 에러가 발생한다.
그 이유는 switch문에서 사용할 수 있는 데이터 타입이 다소 제한적이기 때문이다.
(byte,short,char,int,enum,String,Character,Byte,Short,Integer)

![class방식 error](class_error.png)

#### 4) Enum

```java
/*
class FRUIT{
    public static final FRUIT APPLE = new FRUIT();
    public static final FRUIT PEACH = new FRUIT();
    public static final FRUIT BANANA = new FRUIT();
}
 */
enum FRUIT{
    APPLE, PEACH, BANANA;
}
enum COMPANY{
    APPLE, GOOGLE, SAMSUNG;
}
public class Main {
    public static void main(String[] args) {
        FRUIT type = FRUIT.APPLE;

        switch (type){
            case APPLE:
                System.out.println("apple!");
                break;
            case PEACH:
                System.out.println("peach!");
                break;
            case BANANA:
                System.out.println("banana!");
                break;
        }
    }
}
```

드디어 enum 방식으로 변경하여 보았다. 이전 보다 훨씬 간결하게 원하는 바를 달성할 수 있었다.
사실 위 주석과 새로 추가한 FRUIT enum의 코드의 의미는 같다.그렇다 enum은 클래스이다.위 주석과 같은 클래스 형식의 패턴이 enum 등장 이전까지 사람들이 일반적으로 사용하던 상수 표현 방식이다.그래서 java측에서 이를 문법적으로 지원한 것이 바로 enum인 것이다.(약속)

## Enum 사용 및 특징

### 생성자

```java
enum FRUIT{
    APPLE, PEACH, BANANA;
    FRUIT(){
        System.out.println("Call constructor "+ this);
    }
}
public class Main {
    public static void main(String[] args) {
        FRUIT type = FRUIT.APPLE;
        switch (type){
            case APPLE:
                System.out.println("apple!");
                break;
            case PEACH:
                System.out.println("peach!");
                break;
            case BANANA:
                System.out.println("banana!");
                break;
        }
    }
```

![실행 결과](constructor_result.png)

클래스형태이기에 위 코드처럼 각 인스턴스의 생성자가 실행되는 것을 볼 수 있다.
필드 역시 추가 가능하다.

```java
enum FRUIT{
    APPLE("red"), PEACH("pink"), BANANA("yellow");
    String color;
    FRUIT(String color){
        this.color = color;
        System.out.println("Call constructor "+ this+", color: "+this.color);
    }
}

```

![필드명 추가 실행 결과](field_result.png)

### 특징

Enum은 클래스이기에 생성자와 필드값 그리고 관련 메서드까지 설정 가능하다.
하지만 Enum은 고정된 상수값들의 집합이므로 컴파일 타임에 모든 값을 알고 있어야 한다.
즉,안정성 측면에서 동적으로 값을 정해주거나 변경할 수 없어야 한다는 뜻이다.
따라서 생성자의 접근제어자는 반드시 private이어야 하며,이는 enum의 인스턴스 생성을 제어하며 외부적으로 사용되기에 싱글톤 방식으로도 볼 수 있다.
마지막으로, 관련된 상수들을 모아 두는 것은 물론 클래스형태이기에 연관된 메소드나 필드값을 한곳에 저장할 수 있어서 설계 목적 등의 문맥을 담기에도 용이하다.

## 참고 자료

- https://opentutorials.org/module/516/6091
- https://www.nextree.co.kr/p11686/
