---
title: StringBuffer와 StringBuilder
date: 2021-08-03 23:00:00 +Z
tags: [java]
description: StringBuffer와 StringBuilder에 대한 탐구
---

Java에는 기본적으로 제공하는 api들이 있다.  
이에 대한 내용은 [오라클 공식 문서](https://docs.oracle.com/javase/8/docs/api/)에 자세히 기재되어 있다.  
java의 기본 패키지들은 따로 import 하지 않아도 기본적으로 사용할 수 있기 떄문에 이를 잘 활용하는 것이 필요하다. 오늘은 **String**, **StringBuffer**, **StringBuilder**에 대해 알아보려고 한다.

<img src="https://user-images.githubusercontent.com/60170616/128120351-f2a398c5-34ad-4814-a1f8-63a31f99279d.png"/>
> **java.lang** package에 포함되어 있는 **String**, **StringBuffer**, **StringBuilder** classes

# - String

<img src="https://user-images.githubusercontent.com/60170616/128121581-ff4bb10c-d656-4c86-85cb-d0a277a137f9.png"/>

먼저 **String**에 대한 문서 내용을 확인해보면 Serializable, Comparable, CharSequence 인터페이스가 상속되어 있고 public final class로 되어 있다. serialize가 가능하며 문자열이고 비교가능한 값이라는 것을 알 수 있다. 또한 final class이기 때문에 String class를 상속받을 수는 없다.

<img src="https://user-images.githubusercontent.com/60170616/128123912-f44d001b-7f58-44fd-9ff0-75aad35843b6.png"/>

그리고 문자열을 더할때는 **StringBuffer** 또는 **StringBuilder**를 사용하여 구현한다고 한다.  
왜 단순히 문자열에 계속 문자를 더하지 않고, **StringBuffer**와 **StringBuilder**을 이용하는 것일까?  
좀 더 자세히 알아보자.

# - StringBuffer

**StringBuffer**는 문자열을 추가하거나 변경 할 때 주로 사용하는 자료형이다.  
실제로 어떻게 다른지 확인해보자.

```java
// 문자열 test1의 값과 주소값 확인
String test1 = "TEST 1";
System.out.println("test1: " + test1);
System.out.println("test1 reference: " + test1.hashCode());

// 문자열에 한 글자씩 더하여 주소값 확인
for (int a = 0; a = 3; a++) {
    test1 += a;
    System.out.println("test1: " + test1);
    System.out.println("test1 reference: " + test1.hashCode());
}

// StringBuffer 생성
StringBuffer sb = new StringBuffer();

// StringBuffer에 문자를 append 하고 주소값 확인
for (int a = 0; a = 3; a++) {
    sb.append(a);
    System.out.println("sb: " + sb);
    System.out.println("sb reference: " + sb.hashCode());
}
```
```java
test1: TEST 1
test1 reference: -1823841245
test1: TEST 10
test1 reference: -704503699
test1: TEST 101
test1 reference: -364778140
test1: TEST 1012
test1 reference: 1576779598
sb: 0
sb reference: 471910020
sb: 01
sb reference: 471910020
sb: 012
sb reference: 471910020
```

위의 경우처럼 단순히 문자열에 한 글자를 더하게 되면, 새로운 주소값을 갖는 새 문자열을 생성하여 저장하게 된다. 이렇게 문자열에 문자를 반복적으로 수정이 이뤄지는 상황은 비효율적이다.  
이러한 경우 **StringBuffer**를 사용하여 문자를 append 해주면 같은 주소값을 참조하여 하나의 문자열에 계속 추가를 해주게 되어 더 효율적인 것이다.

그럼 **String**에 새로운 문자를 추가하면 왜 새로운 문자열을 만들어 버리는것일까?  
여기서 알아야될 것은 사실 **String**은 char형의 배열 형태라는 점이다.  
**String**은 `private final char value[];` 라고 선언이 되어있어서 한 번 만들어지면 변경이 불가하다. 때문에 새로운 문자를 추가하면 새로운 문자열로 만들어지는 것이다.

# - StringBuilder와 StringBuffer

그렇다면 **StringBuilder**는 또 무엇인가? **StringBuilder**와 **StringBuffer** 둘 다 변경 가능한 문자열이지만 차이점이 존재한다.

#### StringBuilder
  - synchronization 적용되지 않음(비동기적)
  - 단일 스레드에서 StringBuffer보다 연산처리가 빠르다.

#### StringBuffer
  - synchronization 적용(동기적)
  - 여러 스레드에서 사용하기에 안전

결론적으로 하나의 스레드라면 지역 변수로 **StringBuilder**를, 여러개의 스레드라면 전역 변수로 **StringBuffer**를 사용하는 것이 효과적이다.

---
# - References
<a href="https://docs.oracle.com/javase/8/docs/api/" target="_blank" rel="noopener noreferrer">오라클 api 공식 문서</a>  
<a href="https://novemberde.github.io/2017/04/15/String_0.html" target="_blank" rel="noopener noreferrer">Java에서 String, StringBuilder, StringBuffer의 차이</a>