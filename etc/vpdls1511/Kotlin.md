개발자라면 꼭 한번쯤 들어보았을 기술의 이름 Kotlin.
요새 HOT한 언어라고 하던데, 왜 HOT할까?

# 소개

Kotlin이란 JVM, Android, Browser를 위한 정적 타입의 프로그래밍 언어이다.

여기서 정적 타입이란 Java와 같이 동일하게 안정성이 좋은 프로그래밍 언어라는 뜻이다. 이로 인해서 자동완성 기능이 상당히 좋으며, 타입이 정적이다 보니 IDE가 자동완성을 도와줄 수 있다.

# 어느순간부터 뜨기 시작했다

 사실, 옛날부터 Kotlin이 뜰것이다. 좋다. 라는 말이 많았다. 실제로 요즘 Android 개발에서 Kotlin이 뜨고있고, Spring을 하면서도 Kotlin을 사용하는 추세이기도 하다.

 이 외에도 Kotlin을 제 1언어로 채택하여 앞으로 컴포넌트 혹은 다양한 라이브러리 들도 Kotlin으로 개발이 되고있다고 한다. 그렇다보니 Spring개발자 혹은 Android개발자, 더 넓게 나아가서 Brower에서도 사용된다는데 앞으로는 Kotlin을 이용해 모든 플랫폼을 사용할 수 있지않을까 라는 생각도 좀 들게된다.

그럼 도대체 무엇이 어떻게 다르기 때문에 다들 열광하고 제 1언어로 채택하기까지 하는것일까? 한번 차근차근 알아보도록 하자.

## 변수/상수

```java
//Java
String str = ""; // 변수
final String str = "" // 상수
```

```kotlin
// Kotlin
var str = "" // 변수
val str = "" // 상수
```

무엇이 다른지 모이지 않는가?

Java에서는 final을 붙히나 안붙히나로 변수와 상수가 나뉘게 되는 반면, Kotlin은 var val, 단어 하나 차이로 변수와 상수가 나뉘게 된다.

솔직히 여기서 보면 무엇이 더 편한 방법인지는 개인의 편차라고 생각이 되는데, JavaScript를 많이 했거나 Swift를 한 사람들이라면 Kotlin이 더 편할것이다.

## Null 안정성

```java
//Java
@Nullable String str = null;
@NonNull String str = "";
```

```kotlin
// Kotlin
var str : String? = null;
var str : Sring = "";

```

이 둘의 차이는 Null을 어떻게 처리하는지에대한 차이를 보여준다.

Java는 Annotation을 이용해 Null과 NonNull을 구분하였지만, Kotlin의 경우 ?(Optional)을 사용하여 Null과 NonNull을 구분한다.

Kotlin은 ?을 붙히지 않으면 Non Null이 Default로 되어있다.

그런데 변수 선언에서만 이렇게 할 수 있을까?

```java
//Java
if(str != null) str.split("/");
```

```kotlin
// Kotlin
str?.split("/");

```

Java의 경우 Null Check를 하지 않고 split을 하게 되면 NPE를 뿜어내 if를 사용하여 Null Check를 하게 된다. 하지만, Kotlin은 더 짧게 ?를 이용해 처리를 할 수 있다.

>?: 는 엘비스 연산자라고 한다.


## 데이터 클래스

```java
public class JavaData {
		private String s;
    private int i;
    private boolean b;

    JavaData(String s, int i, boolean b) {
        this.s = s;
        this.i = i;
        this.b = b;
    }    

    public String getS() { return s; }
    public int getI() { return i; }
    public boolean getB() { return b; }

    public void setS(String s) { this.s = s; }
    public void setI(int i) { this.i = i; }
    public void setB(boolean b) { this.b = b; }
}
```

```java
JavaData data1 = new JavaData("", 0, true); // 생성자로 초기화

JavaData data2 = new JavaData(); // 빈 생성자로 생성 후 set 함수로 초기화
data2.setS("");
data2.setI(0);
data2.setB(true);
```

```kotlin
data class KotlinData(var s: String?, var i:int, var b:Boolean)

val data1 = KotlinData("",0,true)
```

DTO를 설계할 때 확실히 코드가 간결해진것을 알 수 있다.

이러한 이유 때문에 Java에서 Kotlin으로 넘어간다고 생각된다.

이 외에도 함수선언, also block등 Kotlin만의 다양한 기능들 때문에 훨씬 좋은 개발능력을 기를 수 있을것이라고 한다.

---

### 출처

[https://dev-imaec.tistory.com/m/36](https://dev-imaec.tistory.com/m/36)
