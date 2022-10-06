## 자바의 열거형
### enum이란?
-	**`enum`** 이란 **`enumerated type`** 의 줄임말로 열거형이라고 부르기도 한다.
-	컴퓨터 프로그래밍에서 열거형(enumerated type, enumeration)은 요소, 멤버라 불리는 명명된 값의 집합을 이루는 자료형이다. 열거자 이름들은 일반적으로 해당 언어의 상수 역할을 하는 식별자이다.
-	서로 관련된 상수를 편리하게 선언하기 위한 것으로 여러 상수를 정의할 때 사용하면 유리하다.
-	일부 열거자 자료형은 언어에 기본적으로 포함되어 있을 수 있다.
-	예를 들면 Boolean 자료형은 true와 false 값이 미리 정의된 열거형이라고 볼 수 있다.
### enum의 장점
-	IDE의 지원을 받을 수 있다. 자동 완성, 오타 검증, 텍스트 리팩토리 등
-	허용 가능한 값들을 제한할 수 있다.
-	리팩토링 시 변경 범위가 최소화된다. -> 내용을 추가해도 enum 코드만 수정하면 된다.
-	확실한 부분과 불확실한 부분을 분리할 수 있다.
-	문맥(Context)을 담을 수 있다.

## enum 정의하는 방법
-	**`{}`** 안에 상수의 이름을 나열하면 된다.
-	기본적으로 상수명은 대문자로 작성하고, 여러 단어일 경우 **`_`** 로 이어주는 것이 관례이다.
```java
enum 열거형이름 {
    상수명1, 상수명2, 상수명n
}
```
-	enum에 정의된 상수를 사용하는 방법은 **`열거형이름.상수명`** 이다. 클래스의 static 변수를 참조하는 것과 같다.
-	상수에 괄호를 사용하면 생성자를 생성하여 변수를 추가할 수 있다.
```java
public enum Age {
    ONE(1), TWO(2);

    private int age;

    Age(int age) {
        this.age = age;
    }

    public int getAge() {
        return this.age;
    }
}
```


## enum이 제공하는 메소드 (value()와 valueOf())
-	Enum을 생성하면 Enum Class를 상속받는다.
+ valueOf()
  + Enum Class 내에 존재
  + 인자로 받은 이름과 같은 Enum 값으로 반환한다.
```java
Direction dirction = Direction.valueOf("WEST");
Direction.WEST == Direction.valueOf("WEST"); // true 반환
```
+ values()
  + 컴파일러가 추가
  + Enum 내 모든 상수를 배열에 담아 반환한다.
```java
Direction[] arr = Direction.values();
```


## java.lang.Enum
모든 열거 타입은 컴파일 시 java.lang.Enum 클래스를 상속받는다. 따라서 단일 상속만 허용되는 자바이기 때문에 enum은 별도의 상속을 받을 수 없다.

java.lang.Enum 클래스도 최고조상인 Object 클래스를 상속받는데, 대부분은 final이어서 Override 할 수 없지만 toString 메소드는 final이 아니어서 사용할 수 있다.
-	abstract 클래스로 인스턴스를 생성할 수 없다.
-	인스턴스의 생성을 막고자 추상클래스로 선언했기 때문에 abstract 메소드가 존재하지 않는다.
-	우리가 선언하는 enum 타입은 이 Enum 클래스를 자동으로 상속하여 Enum에서 제공하는 ordinal과 같은 변수 및 메소드를 사용할 수 있게 된다.

## EnumSet
EnumSet은 Set 인터페이스를 기반으로 열거형 타입으로 지정해 놓은 요소들을 보다 쉽고 빠르게 배열처럼 다룰 수 있는 기능을 제공한다.

EnumSet은 모든 메소드가 static 키워드를 사용하여 정의되어 있기 때문에 별도의 객체 생성 없이 사용할 수 있다고 하는데 사실은 객체를 생성할 수 없는 것이다. 이 클래스는 abstract 키워드를 사용한 추상 클래스이기 때문이다.

+ allOf(Class elementType)</b>
  + 지정한 Type의 모든 원소를 포함하는 EnumSet을 만든다.
+ clone()
  + 이 집합의 복사본을 반환한다.
+ complementOf(EnumSet s)</b>
  + 지정한 EnumSet에 포함되지 않은 원소만 갖는 동일한 Type의 EnumSet을 만든다.
+ copyOf(Collection c)</b>
  + 지정한 Collection에서 초기화된 EnumSet을 만든다.
+ copyOf(EnumSet s)</b>
  + 지정한 EnumSet과 동일한 Type을 가진 EnumSet을 만든다. 이때, 처음과 동일한 원소(원소가 있는 경우)를 포함한다.
+ noneOf(Class elementType)</b>
  + 지정한 Type을 가지는 빈 EnumSet을 만든다.
+ of(E e), of(E first, E… rest), of(E e1, E e2)
  + 지정한 원소(또는 원소들)를 포함하는 EnumSet을 만든다.
+ range(E from, E to)
  + 지정된 두 원소 사이에 있는 모든 원소를 포함하는 EnumSet을 만든다.
