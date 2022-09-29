## package 키워드
-	자바의 패키지는 컴퓨터의 파일들을 정리하는 폴더와 비슷한 개념이다.
-	패키지의 물리적인 형태는 파일 시스템의 폴더이다.
-	패키지는 클래스를 유일하게 만들어 주는 식별자 역할을 한다.
-	클래스의 완전한 이름은 **`패키지 + 클래스명`** 이며 **`FQCN(Fully Qualified Class Name)`** 이라고 한다.
-	패키지가 상, 하위로 구분되어 있다면 **`.`** 를 사용하여 표현한다.
-	패키지의 이름은 java로 시작하면 안된다.
-	파일 시스템의 디렉토리와 비슷한 역할이기 때문에 패키지명과 동일한 디렉토리 구조를 따라야한다.

### 패키지 이름 명명 규칙
|패키지 시작 이름|내용|
|---|-----|
|java|자바 기본 패키지 (Java vendor 개발)|
|javax|자바 확장 패키지 (Java vendor 개발)|
|org|일반적으로 비영리단체 (오픈소스) 패키지|
|com|일반적으로 영리단체(회사) 패키지|

-	패키지 이름은 소문자여야 한다.
-	자바의 예약어는 사용하면 안된다.
-	개발 패키지 표준은 정하는 것에 따라 지정하면 된다.

## import 키워드
같은 패키지에 있는 클래스는 조건 없이 서로 사용할 수 있지만, 다른 패키지에 있는 클래스를 사용하려면 2가지 방법 중 하나를 선택해서 사용해야 한다.
1.	패키지와 클래스 모두 명시, 즉 FQCN을 이용하는 방법
```java
package com.birthday

public class Present {
    com.cellphone.Iphone iphone = new com.cellphone.Iphone();

    // com.cellphone.Iphone : 데이터 타입
    // iphone : 필드명
    // new com.cellphone.Iphone() : 객체 생성
}
```
2.	import 문을 사용하는 방법
```java
package com.birthday

import com.cellphone.Iphone;
// import com.cellphone.*     // cellphone 하위의 모든 클래스 import

public class Present {
    Iphone iphone = new iphone();
}
```

-	import 문은 패키지 선언과 클래스 선언 사이에 선언된다.
-	패키지에 있는 여러 개의 클래스를 사용한다면 **`*`** 을 이용하면 패키지 하위에 있는 클래스들을 import한 것과 같다.
-	import 문의 개수 제한은 없다.

### 빌트-인 패키지 (Built-in Package)
-	자바는 개발자들이 사용할 수 있도록 여러 패키지 및 클래스를 제공한다.
-	가장 자주 쓰이는 패키지로는 **`java.lang`** 과 **`java.util`** 이 있다.
-	**`java.lang`** 은 자주 사용하는 패키지이긴 하나 import 하지 않아도 사용할 수 있다.
+ **`java.lang`**
  + language support 클래스들을 포함하는 패키지
  + primitive type이나 수학 연산의 정의가 되는 클래스들
  + 자동으로 import가 되기 때문에 바로 사용할 수 있다.
+ **`java.io`**
  + 입출력 지능을 지원하는 클래스들을 포함하는 패키지
+ **`java.util`**
  + 자료구조 구현을 위한 유틸리티 클래스들을 포함하는 패키지
+ **`java.applet`**
  + Applets을 생성하기 위한 클래스들을 포함하는 패키지
+ **`java.awt`**
  + GUI 컴포넌트를 구현하기 위한 클래스들을 포함하는 패키지
+ **`java.net`**
  + 네트워킹 기능을 지원하기 위한 클래스들을 포함하는 패키지

## 클래스패스
-	CLASSPATH란 JVM이나 Java 컴파일러에 사용자 정의 클래스와 패키지의 위치를 지정해주는 파라미터이다. 클래스를 찾기 위한 경로라고 보면 된다.
-	JVM은 CLASSPATH의 경로를 확인하여 라이브러리 클래스들의 위치를 참조하게 된다.
-	**`.class`** 파일을 찾을 때 CLASSPATH에 지정된 경로를 사용하며 기본적으로는 **`java`** 명령을 실행하는 위치를 의미한다.
-	CLASSPATH는 .class 파일이 포함된 디렉토리와 파일을 **`;`** 으로 구분한 목록이다.
+ CLASSPATH를 지정하기 위한 방법은 두가지가 있다.
  + CLASSPATH 환경변수 사용
  + java runtime에 -classpath 옵션 사용

## CLASSPATH 환경변수
-	컴퓨터 시스템 변수 설정을 통해 지정할 수 있다. JVM이 시작될 때 JVM의 클래스 로더는 이 환경 변수를 호출한다. 환경 변수에 설정되어 있는 디렉토리가 호출되면 그 디렉토리에 있는 클래스들을 먼저 JVM에 로드한다.
-	최근에는 운영체제 상의 환경변수로 클래스패스를 설정하는 것은 지양하고 IDE나 빌드 도구를 통해 클래스패스를 설정한다.

## -classpath 옵션
-	컴파일러가 컴파일하기 위해서 필요로 하는 참조할 클래스 파일들을 찾기 위해서 컴파일시 파일 경로를 지정해주는 옵션이다.
-	다른 클래스에 의존하는 클래스의 소스 파일을 컴파일하기 위해서 다른 클래스가 위치하는 경로를 잡아주는 것이다.
-	**`-classpath`** 대신 단축어인 **`-cp`** 를 사용해도 된다. classpath 옵션은 java와 javac 명령어에 모두 사용할 수 있다.
```java
// Windows - 세미콜론 사용
java -cp ".;lib" ClasspathTest

// Linux, OSX 유닉스 계열의 시스템 - 콜론 사용
java -cp ".:lib" ClasspathTest

// . : 현재 디렉토리에서 클래스를 찾는다는 의미
// ; : 경로와 경로를 구분해주는 구분자
// lib : 현재 디렉터리에 없다면 하위 디렉토리 중 lib에서 클래스를 찾는다는 의미
// 만약 .을 제외한다면 현재 디렉토리의 클래스를 찾지 못한다. 하지만 하위 lib 디렉토리에 있는 클래스는 찾을 수 있다.
```

## 접근지시자
-	접근지시자, 접근제어자
 ![image](https://user-images.githubusercontent.com/85390517/193028386-d48f661e-645d-482c-a4dc-84cfc32c3c98.png)


|-|public|protected|default|private|
|----------|---|---|---|---|
|같은 패키지, 같은 클래스|O|O|O|O|
|같은 패키지, 상속 관계|O|O|O|X|
|같은 패키지, 상속 관계 아님|O|O|O|X|
|다른 패키지, 상속 관계|O|O|X|X|
|다른 패키지, 상속 관계 아님|O|X|X|X|
> 허용 O, 불용 X
