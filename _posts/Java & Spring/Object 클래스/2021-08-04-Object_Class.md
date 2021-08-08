---
title: (JAVA) Object Class
date: 2021-08-04 22:40:00 +Z
tags: [JAVA]
description: Object class와 그 method들에 대한 이해
---

java.lang 패키지에는 Object 클래스가 포함되어 있다. 그리고 이 Object 클래스는 모든 Java 클래스의 최고 상위 클래스로 모든 클래스는 Object 클래스를 상속받는다. 따라서 Java의 모든 클래스들은 Object 클래스의 모든 메소드를 바로 사용할 수 있고, 이것을 잘 익혀놓으면 다양하게 활용할 수 있다.

총 11개의 메소드로 구성되어 있고, 주로 toString(), equals(), hashCode(), clone()을 override 해서 사용한다.  
메소드들을 좀더 자세히 알아보겠다.

<br>

---
# - Object Class의 11가지 method

|Method|설명|
|:----:|----|
|`protected Object clone()`|: 해당 객체의 복제본을 생성하여 반환함.|
|`boolean equals(Object obj)`|: 해당 객체와 전달받은 객체가 같은지 여부를 반환함.|
|`protected void finalize()`|: 해당 객체를 더는 아무도 참조하지 않아 가비지 컬렉터가 객체의 리소스를 정리하기 위해 호출함.|
|`Class<T> getClass()`|: 해당 객체의 클래스 타입을 반환함.|
|`int hashCode()`|: 해당 객체의 해시 코드값을 반환함.|
|`void notify()`|: 해당 객체의 대기(wait)하고 있는 하나의 스레드를 다시 실행할 때 호출함.|
|`void notifyAll()`|: 해당 객체의 대기(wait)하고 있는 모든 스레드를 다시 실행할 때 호출함.|
|`String toString()`|: 해당 객체의 정보를 문자열로 반환함.|
|`void wait()`|: 해당 객체의 다른 스레드가 notify()나 notifyAll() 메소드를 실행할 때까지 현재 스레드를 일시적으로 대기(wait)시킬 때 호출함.|
|`void wait(long timeout)`|: 해당 객체의 다른 스레드가 notify()나 notifyAll() 메소드를 실행하거나 전달받은 시간이 지날 때까지 현재 스레드를 일시적으로 대기(wait)시킬 때 호출함.|
|`void wait(long timeout, int nanos)`|: 해당 객체의 다른 스레드가 notify()나 notifyAll() 메소드를 실행하거나 전달받은 시간이 지나거나 다른 스레드가 현재 스레드를 인터럽트 할 때까지 현재 스레드를 일시적으로 대기(wait)시킬 때 호출함.|

<br>

---
## - toString()

toString() method는 해당 인스턴스의 정보를 문자열로 반환한다.  
클래스 이름과 구분자 '@', 그 뒤에 16진수 해시 코드가 반환된다.

#### toString()의 원형
`getClass().getName()` + `'@'` + `Integer.toHexString(hashCode())`

```java
class TestMethod {}

class Main {
    public static void main(String args[]) {
        TestMethod test01 = new TestMethod();
        TestMethod test02 = new TestMethod();
        
        System.out.println(test01.toString());
        System.out.println(test02.toString());
    }
}
```
**실행 결과**
```java
TestMethod@2c7b84de
TestMethod@3fee733d
```

<br>

#### String

String 객체를 출력하면 String 객체가 저장하고 있는 문자열이 출력된다.  
jdk의 String 클래스는 toString()을 override하고 있기 때문이다.

```java
String test = new String("Test");
System.out.println(test);
```
**실행 결과**
```java
Test
```

<br>

그렇다면 이 toString()을 어떤 경우에 override해서 사용할까?  
예를 들어 보겠다.

```java
package com.object_class;

public class Song {
    private String title;
    private String singer;

    public Song(String title, String singer) {
        this.title = title;
        this.singer = singer;
    }

    @Override
    public String toString() {
        return "Song{title='" + title + "', singer='" + singer + "'}";
    }

    public static void main(String[] args) {
        Song song = new Song("노래 제목", "가수");
        System.out.println(song);
    }
}
```
**실행 결과**
```java
Song{title='노래 제목', singer='가수'}
```

이렇게 객체의 정보를 문자열 형태로 표현하고자 할 때,  
공통적으로 출력되어야 하는 형태(메뉴얼(?))가 있는 경우,  
toString()을 override해서 return 값을 바꿔줄 수 있다.

<br>

---
## - equals()

equals() method는 두 객체가 같은 객체인지 비교할 때 사용한다.  
'==' 연산자가 두 값이 같은지를 비교하는 것과 다르게,  
equals() method는 두 객체의 주소값이 일치하는, 같은 객체인지를 비교하는 것이다.

```java
public class User {
    int id;
    String name;

    public User(int id, String name) {
        this.id = id;
        this.name = name;
    }

    public static void main(String[] args) {
        User user1 = new User(1000, "최승은");
        User user2 = new User(1000, "최승은");
        
        System.out.println(user1.equals(user2));
        user1 = user2; // 두 변수가 같은 주소를 가리키게 된다.
        System.out.println(user1.equals(user2));
    }
}
```
**실행 결과**
```java
false
true
```

<br>

그렇다면 equals()는 어떤 경우에 override해서 사용할까?  
예를 들어 보겠다.

```java
public class User {
    int id;
    String name;

    public User(int id, String name) {
        this.id = id;
        this.name = name;
    }

    public int getId() {
        return id;
    }

    @Override
    public boolean equals(Object obj) {
        if (obj instanceof User) {
            return this.getId() == ((User)obj).getId();
        } else {
            return false;
        }
    }

    public static void main(String[] args) {
        User user1 = new User(1000, "최승은");
        User user2 = new User(1000, "최승은");

        System.out.println(user1.equals(user2));
    }
}
```
**실행 결과**
```java
true
```

위의 예시처럼 두 객체가 같은 해시코드 값을 갖는지 비교하는 내용으로 override를 하여 사용한다.

<br>

---
## - hashCode()

해시코드란 JVM이 인스턴스를 생성할 때 메모리 주소를 변환해서 부여하는 코드이다.  
실제 메모리 주소값과는 별개이며, 실제 메모리 주소는 System 클래스의 identityHashCode()로 확인할 수 있다.  

자바에서의 동일성은 equals()의 반환값이 true, hashCode() 반환값이 동일함을 의미한다.  
그래서 일반적으로 equals()와 hashCode()는 함께 override 한다.

```java
public class User {
    int id;
    String name;

    public User(int id, String name) {
        this.id = id;
        this.name = name;
    }

    public int getId() {
        return id;
    }

    @Override
    public boolean equals(Object obj) {
        if (obj instanceof User) {
            return this.getId() == ((User)obj).getId();
        } else {
            return false;
        }
    }

    @Override
    public int hashCode() {
        return getId();
    }

    public static void main(String[] args) {
        User user1 = new User(1000, "최승은");
        User user2 = new User(1000, "최승은");

        System.out.println("user1.equals(user2): " + user1.equals(user2));
        System.out.println("user1.hashCode(): " + user1.hashCode());
        System.out.println("user2.hashCode(): " + user2.hashCode());
        System.out.println("System.identityHashCode(user1): " + System.identityHashCode(user1));
        System.out.println("System.identityHashCode(user2): " + System.identityHashCode(user2));
    }
}
```
**실행 결과**
```java
user1.equals(user2): true
user1.hashCode(): 1000
user2.hashCode(): 1000
System.identityHashCode(user1): 901506536
System.identityHashCode(user2): 747464370
```

위의 예시는 id를 해시코드 값으로 반환하여 출력하게 된다.  
이렇게 두 개의 서로 다른 메모리에 위치한 객체가 동일성을 갖기 위해 override를 하게 된다.

<br>

#### Integer class

```java
Integer i1 = 100;
Integer i2 = 100;

System.out.println("i1.hashCode(): " + i1.hashCode());
System.out.println("i2.hashCode(): " + i2.hashCode());
```
**실행 결과**
```java
i1.hashCode(): 100
i2.hashCode(): 100
```

참고로 Integer 클래스도 Object 클래스의 hashCode()를 override하고 있다.

<br>

---
## - clone()

clone() method는 객체를 복제할 때 사용하며, private 필드도 복제할 수 있기 때문에 정보은닉에 위배될 수 있다.  
따라서 인터페이스가 명시돼있는 클래스만 clone()을 통해 객체를 복제할 수 있다.

<br>

---
## - finalize()

finalize() method는 직접 호출하는 메소드가 아니라 객체가 힙 메모리에서 해제될 때 가비지콜렉터가 호출하는 메소드이다.  
이 메소드가 override 되어있으면 가비지콜렉터가 이 메소드를 호출하여 실행한다.  
즉, finalize()에는 객체가 해제될 때 리소스 해제, 소켓 close 등의 필요한 것들을 구현해주면 된다.

<br>

---
# - References
<a href="https://docs.oracle.com/javase/8/docs/api/" target="_blank" rel="noopener noreferrer">오라클 api 공식 문서</a>  
<a href="http://tcpschool.com/java/java_api_object" target="_blank" rel="noopener noreferrer">Object 클래스</a>  
<a href="https://atoz-develop.tistory.com/entry/%EC%9E%90%EB%B0%94-Object-%ED%81%B4%EB%9E%98%EC%8A%A4-%EC%A0%95%EB%A6%AC-toString-equals-hashCode-clone">자바 Object 클래스 정리 - toString(), equals(), hashCode(), clone()</a>