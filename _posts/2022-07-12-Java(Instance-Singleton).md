---
title : "[Java] 인스턴스" 
excerpt : "클래스 / 객체 / 인스턴스 / 싱글톤 / 추상화"
categories :
    - Java
tags : 
    - [Java]
    - [Study]

post-order : 1 

toc : "Java"
toc_sticky : true
toc_icon : true
toc_label : "Java"

date : 2022-07-17
last_modified_at : 2022-07-17

sidebar:
  nav: "docs"
---

# 클래스 / 객체 / 인스턴스 / 싱글톤 / 추상화

### 클래스
- 객체를 만드는 틀
- 객체를 만들기 위한 설계도

### 객체 Object
- 객체는 인스턴스를 포함하는 일반적인 의미
- 구현할 대상

### 인스턴스
- 생성된 객체, 어떤 클래스에 속하는 각각의 객체
- 객체를 생성하여 JVM이 관리하는 메모리에 적재된 것을 의미
- 어떤 클래스로부터 만들어진 객체를 그 클래스의 인스턴스라고 함
- 클래스로부터 객체를 만드는 과정을 인스턴스화 라고 한다 == 클래스로부터 인스턴스를 생성
- 실체화된 인스턴스는 메모리에 할당된다
- new 연산자가 생성자를 실행시키고, 생성자는 클래스를 뼈대로 힙 영역에 인스턴스를 생성하고 그 인스턴스의 주소를 리턴하는 역할을 하기 때문에 인스턴스 생성을 위해서는 반드시 생성자를 호출해야만 한다
- 메모리 공간을 할당받는 과정을 인스턴스화 한다고 한다
- 할당된 클래스를 인스턴스 또는 객체라고 한다

### getInstance()
- `getInstance()` : 인스턴스를 만드는 것
- 최초에 할당된 하나의 메모리를 계속 쓰는 방식

- 인스턴스 생성 방법 1 : new 연산자를 이용해서 클래스를 새로운 메모리에 할당하는 방법
```java
public class A {
  // 전역 객체변수로 사용하기 위해 static 객체변수로 생성
  static A instance;
  
  // 생성자를 private로 만들어 접근을 막는다
  private A(){}
  
  // getInstance 메소드를 통해 한번만 생성된 객체를 가져온다
  public static A getInstance(){
    if(instance == null) { // 최초 한번만 new 연산자를 통하여 메모리에 할당한다
      instance = new A();
    }
    return insance;
  }
}
```
- 인스턴스 생성 방법 2 : `getInstance()`를 통해 최초에 할당된 하나의 메모리를 쓰는 방식
```java
public class B{
  public B(){}
}
```
- 메인 
```java
public class Main{
  public static void main(String[] args){
    // getInstance() 이용
    A a1 = A.getInstance();
    A a2 = A.getInstance();
    A a3 = A.getInstance();
    
    syso(a1); 
    syso(a2);
    syso(a3);
    // 주소값이 a1, a2, a3 모두 같음
    
    // new 생성자를 이용
    B b1 = new B();
    B b2 = new B();
    B b3 = new B();
    
    syso(b1);
    syso(b2);
    syso(b3);
    // 주소값이 b1, b2, b3 모두 다름
  }
}
```
## 싱글톤 Singleton
- 정의
  - 소프트웨어 디자인 패턴에서 싱글턴 패턴(Singleton pattern)을 따르는 클래스는, 생성자가 여러 차례 호출되더라도 실제로 생성되는 객체는 하나이고 최초 생성 이후에 호출된 생성자는 최초의 생성자가 생성한 객체를 리턴한다. 이와 같은 디자인 유형을 싱글턴 패턴이라고 한다. 주로 공통된 객체를 여러개 생성해서 사용하는 DBCP(DataBase Connection Pool)와 같은 상황에서 많이 사용된다

- 객체의 인스턴스가 오직 1개만 생성되는 패턴을 의미
- 객체를 미리 생성해두고 가져오는 방법
```java
public class Singleton {

  private static Singleton instance = new Singleton();
  
  private Singleton() {
    // 생성자는 외부에서 호출 불가하도록 private로 지정해야 한다
  }
  
  public static Singleton getInstance() {
    return instance;
  }
}
```
- 싱글톤 패턴의 사용 이유 
  - 최초 한번의 new 연산자를 통해서 고정된 메모리 영역을 사용하기 때문에 추후 해당 객체에 접근할 때 메모리 낭비를 방지할 수 있다
  - 이미 생성된 인스턴스를 활용하니 속도에서의 이점
  - 데이터 공유가 쉽다 : 싱글톤 인스턴스가 전역으로 사용되는 인스턴스이기 때문에 다른 클래스의 인스턴스들이 접근하여 사용할 수 있다 하지만 여러 클래스의 인스턴스에서 싱글톤 인스턴스의 데이터에 동시에 접근하게 되면 동시성 문제가 발생가능
  - 도메인 관점에서 인스턴스가 한 개만 존재하는 것을 보증하고 싶은 경우 싱글톤 패턴을 사용하기도

- 싱글톤 패턴의 문제점
  - 싱글톤 패턴을 구현하는 코드 자체가 많이 필요
  - 테스트의 어려움 : 여러 자원을 공유하기 때문에 테스트가 결정적으로 격리된 환경에서 수행되려면 매번 인스턴스의 상태를 초기화시켜주어야 한다. 그렇지 않으면 어플리케이션 전역에서 상태를 공유하기 때문에 테스트가 온전하게 수행할 수 없음
  - 클라이언트가 구체 클래스에 의존하게 된다
  - private 생성자로 자식 클래스를 만들 수 없다
  - 내부 상태를 변경하기 어렵다
  - --> 유연성이 떨어지는 패턴

- 스프링의 싱글턴 패턴
  - 스프링 컨테이너는 싱글턴 패턴을 적용하지 않아도 객체 인스턴스를 싱글톤으로 관리한다

### 생성자 Constructor
- 생성자는 인스턴스가 생성될 때 호출하는 인스턴스 초기화 메서드 
- 인스턴스 변수의 초기화 작업에 주로 사용, 인스턴스 생성 시에 실행되어야 할 작업을 위해서도 사용
- 생성자의 이름은 클래스의 이름과 같아야 한다
- 생성자는 리턴 값이 없다
- 생성자도 오버로딩이 가능해서, 하나의 클래스에 여러 개의 생성자가 존재할 수도 있다
- 연산자 new를 통해서 인스턴스를 생성, 생성자는 인스턴스 생성시 초기화에 사용된다
- `Constructor c = new Constructor();`
- 연산자 new 에 의해서 메모리 힙 영역에 해당 클래스의 인스턴스가 생성된다

### 추상화 기법
- 분류 Classification
  - 객체 -> 클래스
  - 실재하는 객체들을 공통적인 속성을 공유하는 범주 또는 추상적인 개념으로 묶는 것
- 인스턴스화 Instantiation
  - 클래스 -> 인스턴스
  - 범주나 개념으로부터 실재하는 객체를 만드는 과정
  - 구체적으로 클래스 내의 객체에 특정한 변형을 정의하고, 이름을 붙인다음, 물리적인 어떤 장소에 위치시키는 등의 작업을 통해 인스턴스를 만드는 것을 말한다
  - 예시 Exemplification 라고도 부른다

## 출처
- [블로그 : 인스턴스] : <https://gmlwjd9405.github.io/2018/09/17/class-object-instance.html>
- [블로그 : getInstance] : <https://velog.io/@kyukim/%EC%BD%94%EB%93%9C%EC%8A%A4%EC%BF%BC%EB%93%9C-%EC%BD%94%EC%BD%94%EC%95%84-%EA%B3%BC%EC%A0%95%EC%9E%90%EB%B0%94-getInstance-%EB%A9%94%EC%86%8C%EB%93%9C>
- [블로그 : 싱글톤] : <https://tecoble.techcourse.co.kr/post/2020-11-07-singleton/>
