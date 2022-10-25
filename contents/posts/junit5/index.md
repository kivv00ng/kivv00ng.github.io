---
title: "Junit5"
description: ""
date: 2022-10-25
update: 2022-10-25
tags:
  - junit5
  - java
---

## Junit5 란?

JUnit5는 자바 프로그래밍 언어용 유닛 테스트 프레임 워크로 자바8이상부터 사용가능하다.  
5버전은 이전 버전과 달리 (JUnitPlatform + JUnitJupiter + JUnitVintage)로 구성되어 있다.

- **JunitPlatform**: JUnit 플랫폼은 JVM에서 테스트 프레임워크를 시작하기 위한 기반 역할을 한다.또 TestEngine(인터페이스) 플랫폼에서 실행되는 테스트 API를 정의하여 주고,이를 바탕으로 만든 사용자 테스트 코드를 실행 가능하도록 해준다.(jUnit platfrom 엔진 제공)

- **jUnitJupiter**: JUnit Jupiter 는 JUnit 5에서 테스트 및 확장을 작성하기 위한 프로그래밍 모델 과 확장 모델 의 조합이다.

- **JunitVintage**: JUnit 3 및 JUnit 4 기반 테스트를 실행 하기 위한 를 제공합니다.

## 어노테이션

- @Test : 메서드가 테스트 메서드임을 나타냅니다.
- @ParameterizedTest : 메서드가 매개 변수가 있는 테스트임을 나타냅니다.
- @DisplayName : 테스트 클래스 또는 테스트 메서드에 대한 사용자 지정 이름을 설정합니다.
- @BeforeEach : 모든 테스트 메소드 실행 전에 실행되는 메소드입니다.
- @AfterEach : 모든 테스트 메소드 실행이 끝나면 실행되는 메소드입니다.
- @Disable : 테스트 클래스나 테스트 메소드를 사용하지 않도록 할 때 사용됩니다.
- @Timeout : 주어진 시간안에 실행을 못할 경우 실패하도록 하는데 사용됩니다.

\* **더 많이 알고싶다면** : https://junit.org/junit5/docs/current/user-guide/#writing-tests-annotations

## @Parameterized Tests

#### @ValueSource

```java
@ParameterizedTest
@ValueSource(strings = { "racecar", "radar", "able was I ere I saw elba" })
void palindromes(String candidate) {
    assertTrue(StringUtils.isPalindrome(candidate));
}

@ParameterizedTest
@ValueSource(ints = { 1, 2, 3 })
void testWithValueSource(int argument) {
    assertTrue(argument > 0 && argument < 4);
}
```

#### @NullSource, @EmptySource

```java
@ParameterizedTest
@NullSource
@EmptySource
//@NullAndEmptySource 위의 두개 합친 것도 가능
//@ValueSource(strings = { " ", "   ", "\t", "\n" })
void nullEmptyAndBlankStrings(String text) {
    assertTrue(text == null || text.trim().isEmpty());
}

```

#### @EnumSource

```java
@ParameterizedTest
@EnumSource(ChronoUnit.class)
void testWithEnumSource(TemporalUnit unit) {
    assertNotNull(unit);
}
```

#### @MethodSource

@MethodSource테스트 클래스 또는 외부 클래스의 하나 이상의 팩토리 메소드를 참조할 수 있다.

```java
@ParameterizedTest
@MethodSource("stringProvider")
void testWithExplicitLocalMethodSource(String argument) {
    assertNotNull(argument);
}

static Stream<String> stringProvider() {
    return Stream.of("apple", "banana");
}

// =================================================

@ParameterizedTest
@MethodSource("range")
void testWithRangeMethodSource(int argument) {
    assertNotEquals(9, argument);
}

static IntStream range() {
    return IntStream.range(0, 20).skip(10);
}
```

팩토리 메소드 이름을 명시적으로 제공하지 않으면 @MethodSourceJUnit Jupiter는 규칙에 따라 현재 메소드와 동일한 이름을 가진 팩토리 메소드를 검색한다.

```java
@ParameterizedTest
@MethodSource
void testWithDefaultLocalMethodSource(String argument) {
    assertNotNull(argument);
}

static Stream<String> testWithDefaultLocalMethodSource() {
    return Stream.of("apple", "banana");
}

```

**매개변수가 여러개일 경우**

```java
@ParameterizedTest
@MethodSource("stringIntAndListProvider")
void testWithMultiArgMethodSource(String str, int num, List<String> list) {
    assertEquals(5, str.length());
    assertTrue(num >=1 && num <=2);
    assertEquals(2, list.size());
}

static Stream<Arguments> stringIntAndListProvider() {
    return Stream.of(
        arguments("apple", 1, Arrays.asList("a", "b")),
        arguments("lemon", 2, Arrays.asList("x", "y"))
    );
}
```

#### @CsvSource

인수 목록을 쉼표로 구분된 값(즉, CSV 리터럴)으로 표현한다.  
기본 구분자는 쉼표( ,)이지만 'delimiter' 속성을 설정하여 다른 문자를 사용할 수 있다.

```java

@ParameterizedTest
@CsvSource(value = {"1:true", "2:true", "3:true", "4:false","5:false"}, delimiter = ':')
void set_contains_true_false(int element, boolean expected) {
    if (expected) {
        System.out.println(element); //결과 1, 2, 3
        assertTrue(element <= 5); //테스트 통과..
    }
}

```

## 참조

- https://junit.org/junit5/docs/current/user-guide/#writing-tests-parameterized-tests
