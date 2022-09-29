## 자바의 상속이란
+ 상속은 상위 클래스의 필드와 메소드를 하위 클래스가 물려받는 것을 말한다.
+ 상속을 사용하면 코드의 재사용성이 증가하고 클래스 간 계층관계를 구분하고 관리하는 것이 편해진다.
+ 상속해주는 클래스
  + 상위 클래스, 슈퍼 클래스, 부모 클래스, 기반 클래스(Base Class)라고도 부른다.
+ 상속을 받는 클래스
  + 하위 클래스, 서브 클래스, 자식 클래스, 파생 클래스(Derived Class)라고도 부른다.
+ 상속을 한다고 해서 부모 클래스의 모든 필드와 메소드를 상속받는 것은 아니다. `private` 접근지정자를 가지고 있는 필드와 메소드는 상속에서 제외되며 다른 패키지일 경우 `default` 접근지정자 또한 제외된다.

 ![image](https://user-images.githubusercontent.com/85390517/193027101-d7a97039-550b-4915-ae7d-c05534ef0a23.png)


### extends 키워드
자바에서는 상속을 위해서 `extends`라는 키워드를 사용한다. 부모 클래스의 메소드를 상속받아 **재정의(override)** 할 수 있기 때문에 **확장(extends)**이라고 한다.

## 자바 상속의 특징
-	자바에서는 단일 상속만 가능하다. (다중 상속 불가능)
-	부모 클래스의 생성자는 상속되지 않는다. (생성자는 멤버가 아니다.)
-	부모 클래스의 초기화 블록 `{}`은 상속되지 않는다.
-	`static` 메소드 혹은 변수도 상속된다.
-	동일한 이름의 변수가 부모 클래스와 자식 클래스에 모두 존재할 경우 부모 클래스의 변수는 가려진다.
-	자바에서 최상위 클래스는 `Object` 클래스이며 모든 클래스는 `Object` 클래스의 자식 클래스이다.
-	접근 지정자에 따라 필드와 메소드가 상속되지 않을 수 있다.
-	다중 상속은 불가능하지만 상속에 대한 횟수는 제한이 없다.
-	동일한 변수가 부모 클래스와 자식 클래스에 모두 존재할 경우 부모 클래스의 변수는 가려진다. 이때 부모 클래스의 변수에 접근하고자 할 때 `super` 키워드를 사용한다.

## super 키워드
-	`super` 키워드는 자식 클래스가 부모 클래스의 필드와 메소드를 참조하는데 사용하는 참조 변수이다.
-	인스턴스 변수와 매개 변수, 지역 변수의 이름이 같을 경우 `this` 키워드로 인스턴스 변수를 가리킬 수 있듯이 `super` 키워드로 부모 클래스를 가리킬 수 있다.
-	부모 클래스의 변수 접근 : `super.변수`
-	부모 클래스의 메소드 접근 : `super.메소드(파라미터)`
-	부모 클래스의 생성자 호출 : `super(파라미터)`
```java
import java.lang.invoke.SwitchPoint;

public class SuperTest {
    public int num = 730;
}

class SubTest extends SuperTest {
    public int num = 906;

    public static void main(String[] args) {
        SubTest subTest = new SubTest();
        SuperTest superTest = new SubTest();

        subTest.test();

        System.out.println(subTest.num); // 906
        System.out.println(superTest.num); // 730
    }
    
    public void test() {
        System.out.println(num); // 906
        System.out.println(super.num); // 730
    }
}
```
-	자바에서는 자식 객체를 생성하면 부모 객체가 먼저 생성되고 자식 객체가 그 다음에 생성된다.

## 메소드 오버라이딩
+ 메소드 오버로딩 (method overloading)
  + 메소드의 이름과 반환 타입은 같지만 매개변수가 다른 메소드를 의미한다.
```java
public void test() {
        System.out.println("test1");
        }

public void test(String str) {
        System.out.println(str);
        }

public void test(int a) {
        System.out.println(a);
        }
```
+ 메소드 오버라이딩 (method overriding)
  + 부모 클래스와 자식 클래스가 상속 관계일 때 자식 클래스는 부모 클래스의 메소드를 상속받아 사용할 수 있다.
  + 부모 클래스의 메소드를 그대로 사용할 수도 있지만 부모 클래스의 메소드를 재정의하여 사용할 수도 있는데 이것을 오버라이딩이라고 한다.
```java
public class Super {
    public void test() {
        System.out.println("Super");
    }
}

public class Sub extends Super{
    // 메소드 오버라이딩 
    // 부모 클래스의 test 메서드를 상속 받아 자식 클래스에서 구현부를 재정의 하였다.
    @Override
    public void test() {
        System.out.println("Sub");
    }
}
```

### 오버라이딩의 조건
-	오버라이딩이란 메소드의 동작만을 재정의하는 것이므로, 메소드의 선언부는 기존 메소드와 완전히 같아야 한다. 하지만 반환 타입은 부모 클래스의 반환 타입으로 타입 변환할 수 있는 타입이라면 변경할 수 있다.
-	부모 클래스의 메소드보다 접근 제어자를 더 좁은 범위로 변경할 수 없다.
-	부모 클래스의 메소드보다 더 큰 범위의 예외를 선언할 수 없다.

## 다이나믹 메소드 디스패치 (Dynamic Method Dispatch)
### 디스패치 (Dispatch)
+ 정적 메소드 디스패치 (Static Method Dispatch)
  + 컴파일 시점에서 컴파일러가 어떤 메소드를 호출할지 명확하게 알고 있는 경우
```java
public class A {
    public void print() {
        System.out.println("A");
    }
}

public class B extends A {

    @Override
    public void print() {
        Systen,out.println("B");
    }
}

public class Test {
    public static void main(String[] args) {
        B b = new B();
        b.print();        // B 출력
    }
}
```
+ 동적 메소드 디스패치 (Dynamic Method Dispatch)
  + 정적 메소드 디스패치와 다르게 컴파일러가 어떤 메소드를 호출해야 하는지 모르는 경우
```java
class A {
    private BInterface bInter;

    A(BInterface bInter) {
        this.bInter = bInter;
    }

    void print() {
        bInter.print();
    }
}

class B1 implements BInterface{
    public void print() {
        System.out.println("B1");
    }
}

class B2 implements BInterface {
    public void print() {
        System.out.println("B2");
    }
}

interface BInterface {
    void print();
}
```
+ 더블 디스패치 (Double Dispatch)
  + Dynamic Dispatch가 두 번 발생하는 것을 말한다.
  + 더블 디스패치와 오버로딩의 차이점은 오버로딩의 경우 객체의 타입은 정해져 있지만 매개변수의 타입만 다른 것이고, 더블 디스패치는 호출하는 객체의 타입과 매개변수의 타입 모두 런타임 시점에 결정되는 것을 의미한다.
```java
interface Post {
    void postOn(SNS sns);
}

class Text implements Post {
    public void postOn(SNS sns) {
        sns.post(this)
    }
}

class Picture implements Post {
    public void postOn(SNS sns) {
        sns.post(this);
    }
}
```
```java
interface SNS {
    void post(Text text);
    void post(Picture picture);
}

class Facebook implements SNS {
    public void post(Text text) {
        // text -> Facebook
    }

    public void post(Picture picture) {
        // picture -> Facebook
    }
}

class Twitter implements SNS {
    public void post(Text text) {
        // text -> Twitter
    }

    public void post(Picture picture) {
        // picture -> Twitter
    }
}

    public static void main(String[] args) {
        List<Post> posts = Arrays.asList(new Text(), new Picture());
        List<SNS> sns = Arrays.asList(new Facebook(), new Twitter());

        posts.forEach(p -> sns.forEach(s -> p.postOn(s)));
    }
```
위의 예제는 총 두 번의 Dynamic Dispatch가 이루어진다.
1.	Post에서 어떤 구현체의 postOn을 사용할지
2.	postOn에서 SNS의 어떤 구현체의 post 함수를 사용할지
자바에서는 현재 Double Dispatch를 지원하고 있지 않지만 방문자 패턴을 통해 유사한 행동을 구현할 수 있다.

### 방문자 패턴 (Visitor Pattern)
일반적으로 OOP는 객체가 스스로 행위를 수행하게 하지만 경우에 따라서는 객체의 행위 수행을 회부 클래스에 위임할 수 있다. 이때 사용하는 디자인 패턴의 종류는 전략 패턴, 커맨드 패턴, 방문자 패턴이 있다.
+ 전략 패턴 (Strategy Pattern)
  + 하나의 객체가 여러 동작을 하게 하는 패턴 (1:N)
+ 커맨드 패턴 (Command Pattern)
  + 하나의 객체가 하나의 동작(+보조동작)을 하게 하는 패턴 (1:1)
+ 방문자 패턴 (Visitor Pattern)
  + 여러 객체에 대해 각 객체의 동작들을 지정하는 패턴 (N:N)

## 추상 클래스
-	추상이란 실체 간에 공통적인 특성을 추출한 것을 말한다.
-	추상 클래스는 객체를 직접 생성할 수 있는 실체 클래스의 공통적인 특성(필드, 메소드)를 추출하여 선언한 클래스이다. 클래스를 만들어내기 위한 일종의 설계도라고 볼 수 있다.
-	추상 클래스는 인스턴스를 생성할 수 없다.
-	추상 클래스를 사용하기 위해서는 반드시 자식 클래스에서 상속받아 추상 클래스를 모두 구현해야 한다.
### 추상 클래스의 목적
-	실체 클래스의 공통 필드와 메소드 이름을 통일하기 위함
-	실체 클래스의 작성 시간을 절약
### 추상 클래스 선언
`abstract` 키워드를 이용하여 추가한다.
```java
public abstract class 클래스 이름 {
	// 필드
	// 생성자
	// 메소드
}
```
-	추상 클래스를 상속받은 자식 클래스의 객체가 생성될 때 내부에서 `super` 키워드가 실행되어 추상 클래스 객체를 생성하기 때문에 생성자가 필요하다.
-	추상 클래스는 반드시 하나 이상의 추상 메소드를 포함하고 있어야 하며 생성자와 멤버변수, 일반 메소드 모두 가질 수 있다.

## final 키워드
`final` 키워드는 엔티티를 한 번만 할당하겠다는 의미로 자바에서는 총 3가지의 의미로 사용된다.
+ final 변수
  + 우리가 흔히 알고 있는 상수를 의미한다. 생성자나 대입연산자를 초기화하고 이후엔 값을 변경할 수 없는 변수이다.
```java
public final int number = 10;
```
+ final 메소드
  + 오버라이드하거나 숨길 수 없음을 의미한다.
```java
public final String message(String msg) { ... }
```
+ final 클래스
  + 해당 클래스를 상속할 수 없다는 의미. 상속할 수 없기 때문에 상속 계층에서 마지막 클래스라는 의미로 쓰인다.
```java
public final class 클래스명 { ... }
```

## Object 클래스
java.lang.Object 클래스는 모든 클래스의 최상위 클래스이다. 클래스 선언을 할 때 다른 클래스를 상속받지 않으면 암시적으로 `java.lang.Object` 클래스를 상속받는다. 다형성으로 인해 모든 객체는 `Object` 타입으로 변환할 수 있다.


|메소드|설명|
|---|----------|
|protected Object clone()|해당 객체의 복제본을 생성하여 반환함|
|Boolean equals(Object obj)|해당 객체와 전달받은 객체가 같은지 여부를 반환함|
|protected void finalize()|해당 객체를 더는 참조하지 않아 가비지 컬렉터가 객체의 리소스를 정리하기 위해 호출함|
|Class<T> getClass()|해당 객체의 클래스 타입을 반환함|
|int hashCode()|해당 객체의 해시 코드 값을 반환함|
|void notify()|해당 객체의 대기(wait)하고 있는 하나의 스레드를 다시 실행할 때 호출함 |
|void notifyAll()|해당 객체의 대기(wait)하고 있는 모든 스레드를 다시 실행할 때 호출함|
|String toString()|해당 객체의 정보를 문자열로 반환함|
|void wait()|해당 객체의 다른 스레드가 notify()나 notifyAll() 메소드를 실행할 때까지 현재 스레드를 일시적으로 대기(wait)시킬 때 호출함|
|void wait(long timeout)|해당 객체의 다른 스레드가 notify()나 notifyAll() 메소드를 실행하거나 전달받은 시간이 지날 때까지 현재 스레드를 일시적으로 대기(wait)시킬 때 호출함|
|void wait(long timeout, int nanos|해당 객체의 다른 스레드가 notify()나 notifyAll() 메소드를 실행하거나 전달받은 시간이 지나거나 다른 스레드가 현재 스레드를 인터럽트(interrupt)할 때까지 현재 스레드를 일시적으로 대기(wait)시킬 때 호출함|
