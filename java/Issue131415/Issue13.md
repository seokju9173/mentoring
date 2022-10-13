## 자바의 제네릭
-	제네릭(Generic)은 ‘일반적인’ 이라는 뜻이다. 자바에서 제네릭이란 데이터의 타입을 일반화한다는 의미이다.
-	다양한 타입의 객체들을 다루는 메소드나 컬렉션 클래스에 컴파일 시의 타입 체크(compile-time type check)를 해주는 기능이다. 객체의 타입을 컴파일 시에 체크하기 때문에 객체의 타입 안정성을 높이고 형변환의 번거로움이 줄어든다.
-	타입 안정성을 높인다는 것은 의도하지 않은 타입의 객체가 저장되는 것을 막고, 저장된 객체를 꺼내 올 때 원래의 타입과 다른 타입으로 잘못 형변환되어 발생할 수 있는 오류를 줄여준다는 것이다.
-	타입 안정성을 제공하고, 타입체크와 형변환을 생략할 수 있어 코드가 간결해 진다는 장점이 있다.

## 1. 제네릭 사용법
### 제네릭 타입의 형변환
제네릭 타입과 원시 타입(row type) 간의 형변환은 항상 가능하지만 경고가 발생한다. 반면에, 대입된 타입이 다른 제네릭 타입 간의 형변환은 대입된 타입이 Object일지라도 불가능하다.
```java
// Gen<Object> objGen = (Gen<Object>)new Gen<String>();  
Gen<Object> objGen = new Gen<String>(); // Error. 형변환 불가능
```
**`Gen<String>`** 이 **`Gen<Object>`** 로 형변환될 수 없다. 한편, **`Gen<String>`** 이 **`Gen<? extends Object>`** 로 형변환이 된다.
```java
GenEx<? extends Num> gen = null; // OK. 미확인 타입으로 형변환 경고  
GenEx<One> oneGen = (GenEx<One>)gen;
```
반대로의 형변환도 성립하지만, 확인되지 않은 형변환이라는 경고가 발생한다. **`GenEx<? extends Num>`** 에 대입될 수 있는 타입이 여러 개인데, **`GenEx<One>`** 를 제외한 다른 타입은 **`GenEx<One>`** 으로 형변환될 수 없기 때문이다.

### 제네릭 타입의 제거
컴파일러는 제네릭 타입을 이용해서 소스 파일을 체크하고, 필요한 곳에 형변환을 넣어준다. 그리고 제네릭 타입을 제거한다. 따라서 컴파일된 파일(*.class)에는 제네릭 타입에 대한 정보가 없다.
#### 제네릭 타입의 기본적인 제거 과정
1.	제네릭 타입의 경계(bound)를 제거한다. 
2.	제거한 후 타입이 일치하지 않으면, 형변환을 추가한다.

와일드 카드가 포함되어 있는 경우에는 적절한 타입으로의 형변환이 추가된다.

## 2. 제네릭 주요 개념 (바운디드 타입, 와일드 카드)
### 바운디드 타입
타입 파라미터들은 바운드(bound)될 수 있다. 바운드된다는 것은 제한된다는 의미인데 메소드가 받을 수 있는 타입을 제한할 수 있다는 것이다.

예를 들어, 어떤 타입과 그 타입의 모든 서브 클래스들을 허용하거나 어떤 타입과 그 타입의 모든 부모 클래스들을 허용하도록 메소드를 작성할 수 있다.
```java
public <T extends Number> List<T> fromArrayToList(T[] a) {
    ...
}
```
위 코드에서 extends 키워드는 클래스의 경우 타입 T가 상위클래스를 상속받은 타입만 허용한다는 의미이며, 인터페이스의 경우에는 상위 인터페이스를 구현하는 타입을 허용한다는 의미이다.

또한 다중 바운드(Multiple Bounds)로 하나의 타입이 상위의 여러 타입들 중 상속받은 타입만 허용하도록 제한할 수도 있다.
```java
<T extends Number & Comparable>
```
타입 T가 상속받은 타입이 클래스인 경우 클래스 타입을 먼저 표기해야 한다. 이 순서가 바뀔 경우 컴파일 에러가 발생한다.

### 와일드 카드
-	제네릭 클래스가 아닌 클래스에 static 메소드의 매개변수로 특정 타입을 지정해줬을 때, 제네릭 타입을 고정해 놓으면 다른 타입의 객체가 메소드의 매개변수가 될 수 없기 때문에 여러 가지 타입의 매개변수를 갖는 메소드를 만들어야 한다.
-	그러나 이와 같이 오버로딩하면 컴파일 에러가 발생한다. 제네릭 타입이 다른 것만으로는 오버로딩이 성립하지 않기 때문이다. 제네릭 타입은 컴파일러가 컴파일 할 때만 사용하고 제거해버린다. 따라서 위 설명과 같은 경우에 메소드들은 오버로딩이 아니라 ‘메소드 중복 정의’가 된다.
-	이럴 때 사용하기 위해 고안된 것이 **와일드 카드** 이다. 와일드 카드는 기호 **`?`** 로 표현하며, 어떠한 타입도 될 수 있다.
+ `?` 만으로는 Object 타입과 다를 게 없으므로, 다음과 같이 상한(upper bound)과 하한(lower bound)을 제한할 수 있다.
+ **<? extends T>**
  + 와일드 카드의 상한 제한. T와 그 자손들만 가능
+ **<? super T>**
  + 와일드 카드의 하한 제한. T와 그 조상들만 가능
+ **<?>**
  + 제한 없음. 모든 타입이 가능. <? extends Object> 와 동일(raw type)

> 제네릭 클래스와 달리 와일드 카드에는 `&` 를 사용할 수 없다.

## 3. 제네릭 메소드 만들기
### 제네릭 클래스 선언
제네릭 타입은 클래스와 메소드에 선언할 수 있다.
```java
class Box {
    Object item;
    void setItem(Object item) {
        this.item = item;
    }    
    Object getItem() { 
        return item; 
    }  
}
```
위와 같은 클래스가 있을 때 제네릭 클래스로 변경하려면 클래스 옆에 **`<T>`** 를 붙이면 된다. 그리고 **`Object`** 를 모두 **`T`** 로 바꾼다.
```java
class Box<T> {
    T item;    
    
    void setItem(T item) { 
        this.item = item; 
    }    
    T getItem() { 
        return item; 
    }  
}
```
-	클래스 옆의 **`T`** 를 ‘타입 변수(type variable)’라고 하며, ‘Type’의 첫 글자에서 따온 것이다. 
-	타입 변수는 T가 아닌 다른 것을 사용해도 된다. ArrayList<E>의 경우, ‘Element(요소)’의 첫 글자를 따서 사용했다.
-	타입 변수가 여러 개인 경우 콤마 **`,`** 를 구분자로 나열하면 된다.
-	글자만 다를 뿐 모두 ‘임의의 참조형 타입’을 의미한다.

제네릭 클래스가 된 클래스의 객체를 생성할 때는 참조변수와 생성자에 타입 T 대신 사용될 실제 타입을 지정해주어야 한다.
```java
Box<String> b = new Box<String>(); // 타입 T 대신, 실제 타입을 지정
b.setItem(new Object()); // Error. String이외의 타입 지정 불가  
b.setItem("ABC"); // OK. String타입이므로 가능  
String item = b.getItem(); // 형변환 필요없음
```
타입 T 대신 String 타입을 지정해 줬으므로 제네릭 클래스 Box는 다음과 같다.
```java
class Box {
    String item;

    void setItem(String item) { this.item = item; }
    String getItem() { return item; }
}
```
-	제네릭 클래스여도 예전 방식으로 객체를 생성할 수 있다. 하지만 제네릭 타입을 지정하지 않아 안전하지 않다는 경고가 발생한다.
-	타입 변수 T에 Object 타입으로 지정하면, 타입을 지정하지 않은 것이 아닌 알고 적은 것이므로 경고는 발생하지 않는다.

### 제네릭의 용어
+ **Box<T>**
  + 제네릭 클래스, ‘T의 Box’ 또는 ‘T Box’ 라고 읽는다.
+ **T**
  + 타입 변수 또는 타입 매개변수 (T는 타입 문자)
+ **Box**
  + 원시 타입 (raw type)

타입 매개변수에 타입을 지정하는 것을 **제네릭 타입 호출** 이라고 하고, 지정된 타입을 **매개변수화된 타입(parameterized type)** 혹은 **대입된 타입** 이라고 한다.

### 제네릭의 제한
-	제네릭 클래스의 객체를 생성할 때 객체별로 다른 타입을 지정하는 것은 괜찮지만, 모든 객체에 대해 동일하게 동작해야 하는 static 멤버에는 타입 변수 T를 사용할 수 없다. T는 인스턴스 변수로 간주되기 때문이다. static 멤버는 대입된 타입의 종류에 관계없이 동일한 것이어야 한다.
-	제네릭 타입의 배열을 생성하는 것도 허용되지 않는다. 제네릭 배열 타입의 참조변수를 선언하는 것은 가능하지만, **`new T[10]`** 과 같이 배열을 생성하는 것은 안된다.
-	생성할 수 없는 이유는 new 연산자 때문이다. 이 연산자는 컴파일 시점에 타입 T가 무엇인지 정확히 알아야 한다. instanceof 연산자도 같은 이유로 T를 피연산자로 사용할 수 없다.

### 제네릭 메소드
메소드의 선언부에 제네릭 타입이 선언된 메소드를 제네릭 메소드라고 한다. **`Collections.sort()`** 는 제네릭 메소드이며, 제네릭 타입의 선언 위치는 반환 타입 바로 앞이다.
```java
static <T> void sort(List<T> list, Comparator<? super T> c)
```
제네릭 클래스에 정의된 타입 매개변수와 제네릭 메소드에 정의된 타입 매개변수는 별개의 것이다. 같은 타입의 문자 T를 사용해도 같은 것이 아니다.
> 제네릭 메소드는 제네릭 클래스가 아닌 클래스에도 정의될 수 있다.
```java
class GenericClass<T> {
      ...
    static <T> void sort(List<T> list, Comparator<? super T> c) {
      ...
    }
}
```
-	위 코드에서 제네릭 클래스에 선언된 타입 매개변수 T와 제네릭 메소드 sort()에 선언된 타입 매개변수 T는 타입 문자만 같고 서로 다른 것이다. sort()가 static 메소드이므로 타입 매개변수를 사용할 수 없지만, 메소드에 제네릭 타입을 선언하고 사용하는 것은 가능하다.
-	메소드에 선언된 제네릭 타입은 지역 변수를 선언한 것과 같다고 생각하면 된다. 이 타입 매개변수는 메소드 내에서만 지역적으로 사용될 것이므로 메소드가 static이던 아니던 상관없다.

제네릭 메소드를 호출할 때는 타입 변수에 타입을 대입해야 한다.
```java
GenericClass<Exam> genericClass = new GenericClass<Exam>();
        ...
System.out.println(Example.<Exam>genericMethod(genericClass));
```
대부분의 경우 컴파일러가 타입을 추정할 수 있기 때문에 생략할 수 있다.
```java
System.out.println(Example.genericMethod(genericClass)); // 대입된 타입 생략
```
주의해야할 점은 제네릭 메소드를 호출할 때, 대입된 타입을 생략할 수 없는 경우에는 참조변수나 클래스 이름을 생략할 수 없다.
```java
System.out.println(<Exam>genericMethod(genericClass)); // Error. 클래스 이름 생략불가
System.out.println(this.<Exam>genericMethod(genericClass)); // OK
System.out.println(Example.<Exam>genericMethod(genericClass)); // OK
```
같은 클래스 내에 있는 멤버 간에는 참조변수나 클래스 이름을 생략하고 메소드 이름만으로 호출이 가능하지만, 대입된 타입이 있을 때는 반드시 써줘야 한다.

## 4. Erasure
-	제네릭 타입의 안정성을 보장하며 실행시간에 오버헤드가 발생하지 않도록 하기 위해 추가되었다.
-	컴파일러는 컴파일 시점에 제네릭에 대하여 **`type erasure(타입 이레이저)`** 라고 부르는 프로세스를 적용한다.
-	타입 이레이저는 모든 타입의 파라미터들을 제거하고 나서 그 자리를 제한하고 있는 타입으로 변경하거나 타입 파라미터의 제한 타입이 지정되지 않았을 경우에는 Object로 대체한다. 따라서 컴파일 후에 바이트 코드는 새로운 타입이 생기지 않도록 보장하는 일반 클래스들과 인터페이스, 메소드들만 포함한다. Object 타입도 컴파일 시점에 적절한 캐스팅이 적용된다.

```java
public <T> List<T> genericMethod(List<T> list) {
    return list.stream().collect(Collectors.toList());
}
```
타입 이레이저가 적용되면서 특정 타입으로 제한되지 않은 T는 Object로 대체된다.
```java
public List<Object> withErasure(List<Object> list) {
    return list.stream().collect(Collectors.toList());
}

public List withErasure(List list) {
    return list.stream().collect(Collectors.toList());
}
```
타입이 제한되어 있을 경우 그 타입은 컴파일 시점에 제한된 타입으로 교체된다.
```java
public <T extends Building> void genericMethod(T t) {
    ...
}
```
위 코드는 컴파일 후 다음과 같이 변경된다.
```java
public void genericMethod(Building t) {
    ...
}
```

## 5. 공변성, 반공변성, 무변성
### LSP (Liskov Substitution Principle)
A를 B로 교체해도 서비스에 아무 문제가 없도록 만드는 원칙을 **리스코프 치환 원칙(Liskov Substitution Principle)** 이라고 한다.
> B가 A의 서브타입일 때, A를 B로 대체해도 프로그램이 작동하는데 문제가 없어야 한다.

### 변성 (Variance)
**`Integer`** 는 **`Number`** 을 상속받아 만들어진 객체이다. 따라서 **`Integer`** 는 **`Number`** 의 하위 타입이라고 할 수 있기 때문에 다음과 같이 작성할 수 있다.
```java
public void test() {
    List<Number> list;
    list.add(Integer.valueOf(1));
}
```
하지만 **`List<Integer>`** 는 **`List<Number>`** 의 하위 타입이 될 수 없다. 이러한 상황에서 Java, Kotlin에서는 제네릭의 **type parameter에 타입 경계(bound)를 명시** 하여 subtype, supertype을 지정하도록 한다.

이러한 기능을 **변성(Variance)** 이라고 한다.

|Bound|Kotlin|Java|
|----------|-----|-----|
|상위 경계 (Upper bound)|Type<out T>|Type<? extends T>|
|하위 경계 (Lower bound)|Type<in T>|Type<? super T>|

### 공변성 (Covariance)
공변성은 타입생성자에게 리스코프 치환 원칙(LSP)을 허용하여 유연한 설계를 가능하게 한다. **자기 자신과 자식 객체만을 허용** 한다.

### 반공변성 (Contravariance)
공변성과 반대로 **자기 자신과 부모 객체만을 허용** 한다.

### 무공변성 (Invariance)
Java, Kotlin의 제네릭은 **기본적으로 무공변성** 으로 아무런 설정이 없는 기본 제네릭을 뜻한다.

### 정리
|변성|설명|
|-----|----------|
|공변성 (Covariance)|`T’`가 `T`의 서브타입이면, `C<T’>`는 `C`의 서브타입이다.|
|반공변성 (Contravariance)|`T’`가 `T`의 서브타입이면, `C`는 `C<T’>`의 서브타입이다.|
|무공변성 (Invariance)|`C`와 `C<T’>`는 아무 관계가 없다.|

