---
title: "Stream"
description: ""
date: 2022-10-21
update: 2022-10-21
tags:
  - java
---

## 1.Stream 이란?

스트림이란 순차 또는 병렬 집계 작업을 지원하는 데이터의 흐름이자 반복자 이다.
(A sequence of elements supporting sequential and parallel aggregate operations.)
이러한 개념을 토대로 나온 StreamAPI는 java에서도 함수형프로그래밍을 가능하도록 하기위해 나온 오퍼레이션들의 모임이라고
할 수 있다.

## 2. 특징

그렇다면 스트림의 특징은 무엇이 있을까?

### 스트림은 원본 데이터를 변경하지 않는다.

원본의 데이터를 조회하여 원본의 데이터가 아닌 별도의 요소들로 Stream을 생성한다.
그렇기 때문에 원본의 데이터로부터 읽기만 할 뿐,정렬이나 필터링 등의 작업은 별도의 Stream 요소들에서 처리가 된다.

### 내부 반복으로 작업을 처리한다.

for이나 while문과는 다르게 반복문법을 메소드 내부에 숨기고 있어 코드가 간결해진다.

### 스트림으로 처리하는 데이터는 오직 한번만 처리한다.(일회성)

Interator와 같이 처리하고 지나가는 것이기에 재사용이 불가능하다.
그렇기 때문에 다시 사용하려면 다시 stream을 생성하여야한다.

### 중개 오퍼레이션은 Lazy Evolution.

Lazy Evoution이란 실제로 필요해지는 경우에 연산을 시작하는 것인데 Stream의 중개 오퍼레이션이 이에 해당한다.

아래는 그 예시이다.

```java
        List<String> names = new ArrayList<>();
        names.add("a");
        names.add("bb");
        names.add("ccc");
        names.add("dddd");
        names.add("eeeee");
        names.add("ffffff");

        Integer[] result = names.stream()
                .map(s -> {
                    System.out.println(s);
                    return s.length();
                })
                .filter(i -> {
                    System.out.println("filter!");
                    return (i % 2) == 0;
                })
                .limit(2)
                .toArray(Integer[]::new);

```

```
a
filter!
bb
filter!
ccc
filter!
dddd
filter!
```

위의 코드 결과 처럼 lazy하게 하나씩 실행되며 조건에 만족한다면 뒤의 나머지 값들은 실행되지 않는다.
또 다른 에시를 보자.

```java
List<String> names = new ArrayList<>();
names.add("aaa");
names.add("bbb");
names.add("ccc");
names.add("ddd");

names.stream().map(s -> {
    System.out.println("s");
    return s.toUpperCase();
})
```

해당 코드의 print는 실행되지 않는다.
왜냐하면 위에서 언급한 것과 같이 중개오퍼레이션은 종료오퍼레이션을 만나야 실행되기때문이다.
우리는 이러한 구조를 스트림 파이프라인이라고 부른다.

## 3. 스트림 파이프라인

파이프라인이란 컴퓨터 과학에서 한 데이터 처리 단계의 출력이 다음 단계의 입력으로 이어지는 형태로 연결된 구조를 말한다.
스트림은 중개오퍼레이션과 종료오퍼레이션을 가지고 이러한 파이프라인을 형성한다.
여기서 스트림파이프 라인의 특징은 종료오퍼레이션이 시작되기 전까지 중개오퍼레이션은 지연(lazy)된다는 것이다.
종료오퍼레이션이 시작하면 비로소 컬렉션에서 요소가 하나씩 중개오퍼레이션에서 처리되고 종료오퍼레이션까지 오게 된다.

- 0 또는 다수의 중개 오퍼레이션 (intermediate operation)과 한개의 종료 오퍼레이션 (terminal operation)으로 구성한다.
- 스트림의 데이터 소스는 오직 터미널 오퍼네이션을 실행할 때에만 처리한다.

### 중개 오퍼레이션

- Stream을 리턴한다.
- Stateless / Stateful 오퍼레이션으로 더 상세하게 구분할 수도 있다.
  (대부분은 Stateless지만,distinct나 sorted처럼 이전 이전 소스 데이터를 참조해야 하는 경우는Stateful 오퍼레이션.)
- filter, map, limit, skip, sorted, ...

### 종료 오퍼레이션

- Stream을 리턴하지 않는다.
- collect, allMatch, count, forEach, min, max, ...

### 참고

- https://docs.oracle.com/javase/8/docs/api/java/util/stream/Stream.html

- https://mangkyu.tistory.com/112
