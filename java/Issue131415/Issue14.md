## 자바의 람다식
-	람다식(Lambda expression)은 **메소드를 하나의 ‘식(expression)’으로 표현한 것** 이다. 람다식은 함수를 간략하면서도 명확한 식으로 표현할 수 있게 해준다.
-	메소드를 람다식으로 표현하면 메소드의 이름과 반환값이 없어지므로, 람다식을 **익명 함수 (anonymous function)** 라고도 한다.

```java
int[] arr = new int[5];
Arrays.setAll(arr, (i) -> (int)(Math.random() * 5) + 1);
```
위 문장에서 **`() -> (int)(Math.random() * 5) + 1`** 이 바로 람다식이다. 이 람다식이 하는 일을 메소드로 표현하면 다음과 같다.
```java
int method() {
    return (int)(Math.random() * 5) + 1;
}
```
모든 메소드는 클래스에 포함되어야 하므로 클래스를 새로 만들어야 하고, 객체도 생성해야 메소드를 호출할 수 있다. 그러나 **람다식은 람다식 자체만으로도 메소드의 역할을 대신할 수 있다.**

## 1. 람다식 사용법
람다식은 **익명 함수** 답게 메소드에서 이름과 반환타입을 제거하고 매개변수 선언부와 몸통 **`{ }`** 사이에 **`->`** 를 추가한다.
```
반환타입 메서드이름(매개변수 선언) {
    문장들
}
    ⇩
(매개변수 선언) -> {
    문장들
}
```
반환값이 있는 메소드의 경우, return문 대신 **식(expression)** 으로 대신할 수 있다. 식의 연산결과가 자동적으로 반환값이 되고 이 때는 **문장(statement)** 이 아닌 식이기 때문에 끝에 **`;`** 을 붙이지 않는다.
```java
(int a, int b) -> { return a > b ? a : b; } // return문
(int a, int b) -> a > b ? a : b             // 식(expression)
```
람다식에 선언된 매개변수의 타입은 추론이 가능한 경우에 생략할 수 있다. 대부분의 경우 생략이 가능한데 람다식에 반환타입이 없는 이유도 항상 추론이 가능하기 때문이다. 한 가지 주의해야 할 점은 두 매개변수 중 어느 하나의 타입만 생략하는 것은 허용되지 않는다.

## 2. 함수형 인터페이스
-	함수형 인터페이스는 오직 하나의 추상 메소드만 존재하는 인터페이스이다.
-	애노테이션 **`@FunctionalInterface`** 를 사용하면 컴파일러가 올바르게 정의되어 있는지 확인해준다. 두 개 이상의 추상 메소드가 선언되거나 추상 메소드가 없을 때 컴파일 오류를 발생시킨다.
-	애노테이션 **`@FunctionalInterface`** 는 선택사항이며 없더라도 하나의 추상 메소드만 있다면 함수형 인터페이스이다. 그러나 실수로 두 개 이상의 추상 메소드를 선언하는 것을 방지하기 위해 붙여주는 것이 좋다.

```java
public interface Functional1 {
    boolean accept();
}

public interface Functional2 {
    boolean accept();
    default boolean reject() { return !accept(); }
}

@FunctionalInterface
public interface Functional3 {
    boolean accept();// 추상 메서드가 하나 더 있으면 컴파일러 에러 발생 !
}

public interface NotFunctional {
    boolean accept();
    boolean reject();
}
```

## 3. Variable Capture
멤버 메소드 내부에서 클래스의 객체를 생성해 사용할 경우 다음과 같은 문제가 있다.
-	익명 구현 객체를 포함해 객체를 생성한다는 것은 new 키워드를 사용하는데 이 키워드를 사용한다는 의미는 동적 메모리 할당 영역인 heap 영역에 객체를 생성한다는 것이다.
-	이렇게 생성된 객체는 자신을 감싸고 있는 멤버 메소드의 실행이 끝난 후에도 heap 영역에 존재하므로 사용할 수 있다. 하지만 이 멤버 메소드에 정의된 매개변수나 지역변수는 런타임 스택 영역에 할당되어 메소드 실행이 끝나면 사라져 더 이상 사용할 수 없게 된다.
-	따라서 멤버 메소드 내부에서 생성된 객체가 자신을 감싸고 있는 메소드의 매개변수나 지역변수를 사용하려 할 때 문제가 생길 수 있다.
### 정리
1.	클래스의 멤버 메소드의 매개변수와 이 메소드 실행 블록 내부의 지역변수는 JVM의 런타임 스택 영역에 생성되고 메소드의 실행이 끝나면 런타임 스택 영역에서 사라진다.
2.	new 연산자를 사용해 생성한 객체는 JVM의 heap 영역에 객체가 생성되고 GC에 의해 관리되며, 더 이상 사용하지 않는 객체에 대해 메모리에서 제거한다.

heap 영역에 생성된 객체가 스택 영역의 변수를 사용하려고 하는데, 해당 시점에 스택 영역에 더 이상 변수가 존재하지 않을 수 있고, 이 때문에 오류가 발생한다.
-	자바에서는 이 문제를 **Variable Capture** 라고 하는 값 복사를 사용해 해결한다.
-	컴파일 시점에 멤버 메소드의 매개변수나 지역변수를 멤버 메소드 내부에서 생성한 객체가 사용할 경우 객체 내부로 값을 복사해 사용한다. 하지만 모든 값을 복사해서 사용할 수 있는 것은 아니고, final 키워드로 작성되거나 final 성격을 가져야 한다는 제약이 있다.

### 로컬 변수 캡쳐 (Local Variable Capture)
-	Local Variable은 조건이 final 또는 effective final이어야 한다.
-	effective final은 Java 8에 추가된 syntactic sugar의 일종으로 초기화된 이후 값이 한번도 변경되지 않았다는 것을 말한다. effective final 변수는 final 키워드가 붙어있지 않지만 final 키워드를 붙인 것과 동일하게 컴파일에서 처리한다. -> 초기화하고 값이 변경되지 않은 것을 말한다.
-	Local Variable에 이런 조건이 붙어있는 이유는 JVM의 메모리 구조를 보면 알 수 있다.
-	지역변수는 스레드 간에 공유가 불가능하다. 인스턴스 변수는 JVM의 heap 영역에 생성되는데, 지역변수와 달리 스레드 간에 공유가 가능하다. 즉, 지역변수가 스택에 저장되기 때문에 람다식에서 값을 바로 참조하는데 제약이 있다. 복사된 값을 사용하는데 멀티 스레드 환경에서 변경이 되면 동시성에 대한 이슈를 대응하기 힘들기 때문이다.

## 4. 메소드, 생성자 레퍼런스 (메소드 참조)
### 메소드 레퍼런스
람다식을 더욱 간결하게 표현하는 방법이 있다. 람다식이 하나의 메소드만 호출하는 경우에 **`메소드 참조(method reference)`** 라는 방법으로 간략히 할 수 있다.
### 메소드 참조 정리
+ static 메소드 참조
  + 메소드 참조는 static 메소드를 직접적으로 가리킬 수 있다.

```java
클래스이름::메서드이름
(매개변수) -> Class.staticMethod(매개변수)

String::valueOf
str -> String.valueOf(str)
```

+ 인스턴스 메소드 참조

```java
클래스이름::메서드이름
(obj, 매개변수) -> obj.instanceMethod(매개변수)

String::length
(value) -> value.length();
```

+ 특정 객체 인스턴스 메소드 참조
  + 특정 인스턴스의 메소드를 참조할 수 있다. 클래스 이름이 아닌 인스턴스 명을 넣는다.

```java
obj::instanceMethod
(매개변수) -> obj.instanceMethod(매개변수)

object::toString
() -> object.toString()
```

### 생성자의 메소드 참조
생성자를 호출하는 람다식도 메소드 참조로 변환할 수 있다.
```java
Supplier<MyClass> s = () -> new MyClass();
Supplier<MyClass> s = MyClass::new;
```
매개변수가 필요한 생성자라면 매개변수의 개수에 따라 알맞은 함수형 인터페이스를 사용하면 된다. 필요에 따라 함수형 인터페이스를 새로 정의해야 한다.
```java
Function<Integer, MyClass> f = (i) -> new MyClass(i);
Function<Integer, MyClass> f2 = MyClass::new;
```
