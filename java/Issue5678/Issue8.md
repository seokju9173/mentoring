## 자바의 인터페이스
-	자바는 하나의 상속만 가능하다는 특성이 있다. 이것은 객체 지향 프로그래밍에서 큰 제약이기 때문에 인터페이스라는 개념을 도입했다. 
-	인터페이스는 일종의 추상 클래스이다. 추상 클래스처럼 추상 메소드를 갖지만 추상 클래스보다 추상화 정도가 높아서 추상 클래스와 달리 일반 메소드 또는 멤버 변수를 가질 수 없다. 오직 추상 메소드와 상수만 멤버로 가질 수 있다. 하지만 자바 8부터 **`default`** 키워드를 통해 일반 메소드 구현이 가능해졌다.
### 인터페이스의 장점
+ 개발 시간을 단축시킬 수 있다.
  + 일단 인터페이스가 작성되면, 이를 사용해서 프로그램을 작성하는 것이 가능하다. 메소드를 호출하는 쪽에서는 메소드의 내용에 관계없이 선언부만 알면 되기 때문이다. 그리고 동시에 다른 한 쪽에서는 인터페이스를 구현하는 클래스를 작성하도록 하여, 인터페이스를 구현하는 클래스가 작성될 때까지 기다리지 않고도 양쪽에서 동시에 개발을 진행할 수 있다.
+ 표준화가 가능하다.
  + 프로젝트에 사용되는 기본 틀을 인터페이스로 작성한 다음, 개발자들에게 인터페이스를 구현하여 프로그램을 작성하도록 함으로써 보다 일관되고 정형화된 프로그램의 개발이 가능하다.
+ 서로 관계없는 클래스들에게 관계를 맺어줄 수 있다.
  + 서로 상속 관계에 있지도 않고, 같은 조상 클래스를 가지고 있지 않은 서로 아무런 관계도 없는 클래스들에게 하나의 인터페이스를 공통적으로 구현하도록 함으로써 관계를 맺어줄 수 있다.
+ 독립적인 프로그래밍이 가능하다.
  + 인터페이스를 이용하면 클래스의 선언과 구현을 분리시킬 수 있기 때문에 실제 구현에 독립적인 프로그램을 작성하는 것이 가능하다.
  + 클래스와 클래스 간의 직접적인 관계를 인터페이스를 이용해서 간접적인 관계로 변경하면 한 클래스의 변경이 관련된 다른 클래스에 영향을 미치지 않는 독립적인 프로그래밍이 가능하게 된다.

### 추상 클래스와 인터페이스의 차이점
|추상 클래스(abstract class)|인터페이스(interface)|
|----------|----------|
|일반 메소드 포함 가능|모든 메소드는 추상 메소드<br>자바 8 이후부터 default, static 메소드 추가 가능|
|다중 상속 불가능|다중 상속 가능|
|상수, 변수 필드 포함 가능|상수 필드만 포함 가능|
-	추상클래스 -> 개는 동물이다. -> IS – A
-	인터페이스 -> 개는 수영을 할 수 있다. -> HAS - A
-	개는 동물인 것이 확실하지만, 어떤 개는 수영을 할 수도, 못할 수도 있다.

## 인터페이스 정의하는 방법
-	인터페이스를 정의할 때는 interface 키워드를 사용한다.
-	모든 필드는 public final static 이어야 한다.
-	모든 메소드는 public abstract 이어야 한다.
-	이 부분은 모든 인터페이스에 공통으로 적용되는 부분이므로 이 제어자는 생략할 수 있다. 이렇게 생략된 제어자는 컴파일 시 자바 컴파일러가 자동으로 추가해준다.
```java
접근제어자 interface 인터페이스이름 {
	public static final 타입 상수이름 = 값;
	…
	public abstract 메소드이름(매개변수목록);
	…
}
```

## 인터페이스 구현하는 방법
인터페이스는 추상 클래스와 마찬가지로 자신이 직접 인스턴스를 생성할 수는 없다. 따라서 인터페이스가 포함하고 있는 추상 메소드를 구현해 줄 클래스를 작성해야만 한다.

자바에서 인터페이스는 다음과 같은 문법을 통해 구현한다.
```java
class 클래스이름 implements 인터페이스이름 { … }
```
만약 모든 추상 메소드를 구현하지 않는다면, abstract 키워드를 사용하여 추상 클래스로 선언해야 한다.

### 인터페이스 구현 예시
```java
interface Animal { public abstract void cry(); }

class Cat implements Animal {
    public void cry() {
        System.out.println("냐옹냐옹!");
    }
}

class Dog implements Animal {
    public void cry() {
        System.out.println("멍멍!");
    }
}

public class Polymorphism03 {
    public static void main(String[] args) {
        Cat c = new Cat();
        Dog d = new Dog();

        c.cry();
        d.cry();
    }
}
```
### 실행결과
```
냐옹냐옹!
멍멍!
```

## 인터페이스 레퍼런스를 통해 구현체를 사용하는 방법
인터페이스는 다형성을 이용해 구현할 수 있다. 자식 클래스의 인스턴스를 부모 클래스의 참조 변수로 참조하는 것이 가능하다는 의미이다.
-	인터페이스는 객체의 타입으로만 사용된다. 이 말은 상속에서와 같이 변수 선언 시에 객체의 타입으로 인터페이스를 사용할 수 있다는 것을 의미한다.
-	인터페이스 타입이 있을 때는 항상 인터페이스 타입 기반으로 코딩해야 한다.
인터페이스로 객체 타입을 선언하게 된다면, 추후에 클래스를 교체할 일이 생겼을 때 클래스만 바꾸면 된다. 시스템이 유연해지게 만드는 것이다.

## 인터페이스 상속
-	인터페이스끼리 상속을 할 때에는 **`extends`** 키워드를 사용한다.
-	클래스와 다르게 다중 상속이 가능하다.
```java
public interface SubInterface extends SuperInterface1, SuperInterface2 { … }
```

## 인터페이스의 기본 메소드 (Default Method), 자바 8
-	**자바 8 이전** 에 인터페이스가 가질 수 있는 메소드는 추상 메소드뿐이었다. 해당 인터페이스를 구현한 클래스에서 메소드의 구현부를 완성해야 메소드를 사용할 수 있었다.
-	문제점은 구현 클래스들이 메소드의 구현부를 작성할 때, 그 내용이 일치하더라도 클래스마다 새로 구현부를 적어줘야 한다는 것이다.
-	이 문제를 해결하기 위해 **자바 8 이후** 에는 **`default 메소드`** 를 도입하게 되었다.
-	**`default 메소드`** 는 인터페이스 내부에 존재할 수 있는 구현 메소드이다. **`implements`** 한다면 따로 메소드를 구현하지 않아도 사용할 수 있다.
-	기본 메소드 (Default Method) 는 구현부 (body) 를 가지게 된다.
```java
interface TestInter {
    default void test() {
        System.out.println("디폴트 메소드 실행");
    }
}

class TestClass implements TestInter {
    public static void main (String[] args) {
        TestClass testClass = new TestClass();
        testClass.test();  // 디폴트 메소드 실행
    }
}
```
> 만약 interface에 동일한 default 메소드가 선언되어 있다면, 컴파일 에러가 발생하기 때문에 무조건 overriding을 해야 한다.

## 인터페이스의 static 메소드, 자바 8
-	인터페이스의 추상성을 해친다는 이유로 제약을 걸어왔지만, **자바 8 이후** 부터는 개발의 유연성을 위해 **`static 메소드`** 를 선언할 수 있게 되었다.
-	인터페이스의 **`static 메소드`** 는 인터페이스 내에서 이미 구현부를 구현한 메소드인 점은 default 메소드와 동일하다. 하지만 인터페이스를 구현한 클래스에서 오버라이딩하여 사용할 수 없다.
-	static 메소드는 오버라이딩이 불가하다.
-	default 나 abstract 메소드는 모두 instance 메소드처럼 처리된다.
-	default 메소드를 static 메소드로 덮으려 하면 안된다. -> 컴파일 에러가 발생한다.
```java
interface TestInter {
    static void test() {
        System.out.println("static 메소드 실행");
    }

    void hello(String msg);
}

public class TestClass implements TestInter {
    @Override
    public void hello(String msg) {
        System.out.println("추상 메소드 구현 : " + msg);
    }

    public static void main(String[] args) {
        TestClass testClass = new TestClass();

        TestInter.test();                    // static 메소드 실행
        testClass.hello("구현 메소드 실행")    // 추상 메소드 구현 : 구현 메소드 실행
    }
}
```

## 인터페이스의 private 메소드, 자바 9
**자바 9 이후** 부터는 인터페이스 내에 **`private 메소드`** 를 가질 수 있게 되었다.
```java
public interface Camera {
    default void print() {
        ..
    }

    private void info() {
        ..
    }
}
```

+ 인터페이스에서 사용 가능한 멤버
  + 상수
  + abstract 메소드
  + default 메소드
  + static 메소드
  + private 메소드
  + private static 메소드

private 메소드는 오직 해당 인터페이스 내에서만 접근이 가능하고, 인터페이스를 상속받은 클래스나 서브 인터페이스에서는 접근할 수 없다.
