## JVM이란 무엇인가
-	Java Virtual Machine, 자바 가상 머신
Java는 OS에 종속적이지 않다는 특징을 가지고 있다.
OS에 종속 받지 않고 CPU가 Java를 인식, 실행할 수 있게 하는 가상 컴퓨터가 JVM이다.

 ![image](https://user-images.githubusercontent.com/85390517/189087235-6c91b749-f168-4394-a209-73e661fafc9b.png)


-	Java 소스코드(**`*.java`** 파일)는 CPU가 인식하지 못하므로 기계어로 컴파일 해줘야 한다. 하지만 Java는 JVM이라는 가상머신을 거쳐서 OS에 도달하기 때문에 기계어로 바로 컴파일 되는 것이 아니라 JVM이 인식할 수 있는 Java Bytecode(**`*.class`** 파일)로 변환된다.
-	Java Compiler가 **`.java`** 파일을 **`.class`** 라는 Java Bytecode로 변환한다.
-	변환된 bytecode는 기계어가 아니기 때문에 OS에서 바로 실행되지 않고, JVM이 OS가 bytecode를 이해할 수 있도록 도와준다. 따라서 Bytecode는 JVM 위에서 OS에 상관없이 실행될 수 있는 것이다.

## 컴파일 하는 방법
-	컴파일은 Java Compiler에 의해 **`.java`** 파일을 **`.class`** 라는 java Bytecode로 만드는 과정이다.
-	Java Compiler는 JDK를 설치하면 **`javac.exe`** 라는 실행 파일 형태로 설치된다.
-	Java Compiler의 **`javac`** 라는 명령어를 사용하면 **`.class`** 파일을 생성할 수 있다.

### 예시
```java
public class seokju {
	public static void main(String[] args) {
		System.out.println(“Hello World”);
	}
}
```
“Hello World”를 출력하는 **`.java`** 파일을 생성하고 이를 **`.class`** 파일로 변환시켜본다.

 ![image](https://user-images.githubusercontent.com/85390517/189087575-f1f8feab-fd22-4d9d-a124-798d2d561041.png)


Windows 기준, cmd 창을 열고 해당 **`.java`** 파일이 있는 곳으로 이동한다.

 ![image](https://user-images.githubusercontent.com/85390517/189087582-537b2de4-4c55-4bb4-bc23-e21bb639c822.png)


해당 위치에서 **`javac`** 명령어로 컴파일을 진행한다.

 ![image](https://user-images.githubusercontent.com/85390517/189087592-550af873-389e-48dd-8cd8-5f6f2cc08ba0.png)


해당 위치에 **`.class`** 파일이 생성된 것을 확인할 수 있다.

## 실행하는 방법
### 자바 코드의 실행 과정
 ![image](https://user-images.githubusercontent.com/85390517/189087621-eab80d29-7f4d-4c30-a0dc-0e1127303e95.png)


1)	프로그램이 실행되면 JVM은 OS로부터 이 프로그램이 필요로 하는 메모리를 할당받는다. (JVM은 이 메모리를 용도에 따라 여러 영역으로 나누어 관리한다.)
2)	자바 파일(.java)이 자바 컴파일러에 의해 자바 바이트 코드(.class)로 변환된다.
3)	클래스 로더를 통해 자바 바이트 코드를 JVM으로 필요한 시점에 로딩한다.
4)	해석된 바이트 코드는 런타임 데이터 영역에 배치되어 실질적인 수행이 이루어지게 된다.
5)	실행과정 속에서 JVM은 필요에 따라 GC와 같은 관리 작업을 수행한다.

### 예시
**`java`** 명령어로 **`.class`** 파일을 실행시킬 수 있다.

**`.class`** 파일이 위치한 곳으로 이동 후 **`java (.class 파일 이름)`** 을 입력해 실행시킨다.

 ![image](https://user-images.githubusercontent.com/85390517/189087767-40f8e52f-bfe2-4c25-84ad-8bd6ca1eb049.png)


“Hello World”가 출력되면서 **`.class`** 파일이 실행된 것을 확인할 수 있다.


## 바이트코드란 무엇인가
### 바이트 코드
바이트 코드(Bytecode, portable code, p-code)는 특정 하드웨어가 아닌 가상 컴퓨터에서 돌아가는 실행 프로그램을 위한 이진 표현법이다. 하드웨어가 아닌 소프트웨어에 의해 처리되기 때문에, 보통 기계어보다 더 추상적이다.

### 자바 바이트코드
-	자바 바이트코드(Java Bytecode)는 JVM이 실행하는 명령어의 형태이다. JVM이 이해할 수 있는 언어로 변환된 자바 소스 코드를 의미한다.
-	명령어의 크기가 1byte 라서 자바 바이트코드라고 불리고, 자바 코드를 배포하는 가장 작은 단위이다.
-	확장자는 **`.class`** 이다.


## JIT 컴파일러란 무엇이며 어떻게 동작하는지
-	JIT 컴파일(Just-In-Time compliation) 또는 동적 번역(dynamic translation)
-	JIT 컴파일러는 프로그램을 실제 실행하는 시점에 기계어로 번역하는 컴파일러이다.
-	자바를 실행할 때 인터프리터는 바이트코드를 한 줄씩 실행한다는 단점이 있고, 이 단점을 보완하기 위해 도입된 것이 JIT 컴파일이다. **인터프리터 방식으로 실행하다가 적절한 시점에 바이트코드 전체를 컴파일하여 기계어로 변경하고, 이후에는 더 이상 인터프리팅 하지 않고 기계어로 직접 실행하는 방식**
-	JIT 컴파일러가 컴파일하는 과정은 바이트코드를 인터프리팅하는 것보다 훨씬 오래 걸리기 때문에 한 번만 실행되는 코드라면 컴파일하지 않고 인터프리팅하는 것이 유리하다.
-	자바에서는 자바 컴파일러가 자바 프로그램 코드를 바이트코드로 변환한 다음, 실제 바이트코드를 실행하는 시점에서 자바 가상 머신(JVM, 정확히는 JRE)이 바이트 코드를 JIT 컴파일을 통해 기계어로 변환한다.


## JVM 구성 요소
 ![image](https://user-images.githubusercontent.com/85390517/189087816-2c689389-7ab0-4cf1-9eb3-2c01bac204e5.png)

JVM은 크게 아래와 같이 이루어져 있다.
+ 클래스 로더(Class Loader)
+ 실행 엔진(Execution Engine)
  + 인터프리터(Interpreter)
  + JIT 컴파일러(Just-In-Time)
  + 가비지 콜렉터(Garbage Collector)
+ 런타임 데이터 영역(Runtime Data Area)

### 클래스 로더
-	JVM 내로 클래스 파일(**`.class`**)을 로드하고, 링크를 통해 배치하는 작업을 수행하는 모듈
-	런 타임시 동적으로 클래스를 로드하고 **`jar`** 파일 내 저장된 클래스들을 JVM 위에 탑재한다.
-	즉, 클래스를 처음으로 참조할 때, 해당 클래스를 로드하고 링크하는 역할을 한다.

### 실행 엔진
-	클래스를 실행시키는 역할
-	클래스 로더가 JVM 내의 런타임 데이터 영역에 바이트코드를 배치시키고, 이것은 실행 엔진에 의해 실행된다.
-	자바 바이트코드(**`.class`**)는 기계가 바로 수행할 수 있는 언어보다는 비교적 인간이 보기 편한 형태로 기술된 것이다. 그래서 실행 엔진은 이와 같은 바이트코드를 실제로 JVM 내부에서 기계가 실행할 수 있는 형태로 변경한다.

#### 인터프리터
-	실행 엔진은 자바 바이트코드를 명령어 단위로 읽어서 실행한다.
-	한 줄씩 수행하기 때문에 느리다는 단점이 있다.

#### JIT(Just-In-Time)
-	인터프리터 방식으로 실행하다가 적절한 시점에 바이트코드 전체를 컴파일하여 기계어로 변경하고, 이후에는 더 이상 인터프리팅 하지 않고 기계어로 직접 실행하는 방식이다.

#### 가비지 콜렉터
-	더 이상 사용되지 않는 인스턴스를 찾아 메모리에서 삭제한다.

### 런타임 데이터 영역
 ![image](https://user-images.githubusercontent.com/85390517/189087958-8684efeb-a48d-4173-a9ce-ea722295de79.png)

프로그램을 수행하기 위해 OS에서 할당받은 메모리 공간

#### PC Register
-	Thread가 시작될 때마다 생성되며 생성될 때마다 생성되는 공간으로, 스레드마다 하나씩 존재한다.
-	Thread가 어떤 부분을 어떤 명령으로 실행해야 할 지에 대한 기록을 하는 부분으로 현재 수행중인 JVM 명령의 주소를 갖는다.

#### JVM 스택 영역
-	프로그램 실행과정에서 임시로 할당되었다가 메소드를 빠져나가면 바로 소멸되는 특성의 데이터를 저장하기 위한 영역이다.
-	각종 형태의 변수나 임시 데이터, 스레드나 메소드의 정보를 저장한다.
-	메소드 호출 시마다 각각의 스택 프레임(그 메소드 만을 위한 공간)이 생성된다. 메소드 수행이 끝나면 프레임 별로 삭제를 한다.
-	메소드 안에서 사용되는 값들을 저장한다. 또 호출된 메소드의 매개변수, 지역변수, 리턴 값 및 연산 시 일어나는 값들을 임시로 저장한다.

#### Native method stack
-	자바 프로그램이 컴파일되어 생성되는 바이트 코드가 아닌 실제 실행할 수 있는 기계어로 작성된 프로그램을 실행시키는 영역
-	JAVA가 아닌 다른 언어로 작성된 코드를 위한 공간
-	Java Native Interface를 통해 바이트코드로 전환하여 저장하게 된다.
-	일반 프로그램처럼 커널이 스택을 잡아 독자적으로 프로그램을 실행시키는 영역

#### Method Area (= Class Area = Static Area)
클래스 정보를 처음 메모리 공간에 올릴 때 초기화되는 대상을 저장하기 위한 메모리 공간
+ **Runtime Constant Pool**
  + 스태틱 영역에 존재하는 별도의 관리영역
  + 상수 자료형을 저장하여 참조하고 중복을 막는 역할을 수행한다.

#### Heap 영역
 ![image](https://user-images.githubusercontent.com/85390517/189088002-050e9b14-f8d4-4529-8046-563ff38c444e.png)

-	객체를 저장하는 가상 메모리 공간. New 연산자로 생성되는 객체와 배열을 저장한다.
-	Class Are(Static Area)에 올라온 클래스들만 객체로 생성할 수 있다.

#### Heap은 세 가지 부분으로 나뉘어진다.
+ Permanent Generation
  + 직역하면 영구적인 세대이다.
  + 생성된 객체들의 정보의 주소값이 저장된 공간이다.
  + 클래스 로더에 의해 load되는 Class, Method 등에 대한 Meta 정보가 저장되는 영역이고 JVM에 의해 사용된다.
  + Reflection을 사용하여 동적으로 클래스가 로딩되는 경우에 사용된다.
> Reflection
> 객체를 통해 클래스의 정보를 분석해 내는 프로그래밍 기법

+ New/Young 영역
  + 이 곳의 인스턴스들은 추후 가비지 콜렉터에 의해 사라진다.
  + 생명 주기가 짧은 ‘젊은 객체’를 GC 대상으로 하는 영역이다.
  + 여기서 일어나는 가비지 콜렉트를 Minor GC 라고 한다.
    + Eden : 객체들이 최초로 생성되는 공간
    + Survivor 0, 1 : Eden에서 참조되는 객체들이 저장되는 공간
> Eden 영역에 객체가 가득차게 되면 첫번째 가비지 콜렉트가 발생한다.
> Eden 영역에 있는 값들을 Survivor 1 영역에 복사하고 이 영역을 제외한 나머지 객체를 삭제한다.

+ Old 영역
  + 이 곳의 인스턴스들은 추후 가비지 콜렉터에 의해 사라진다.
  + 생명 주기가 긴 ‘오래된 객체’를 GC 대상으로 하는 영역이다.
  + 여기서 일어나는 가비지 콜렉트를 Major GC 라고 한다. Minor GC에 비해 속도가 느리다.
  + New/Young 영역에서 일정시간 참조되고 있는, 살아있는 객체들이 저장되는 공간이다.


## JDK와 JRE의 차이
### JDK
-	Java Development Kit (자바 개발 키트)
-	Java를 사용하기 위해 필요한 모든 기능을 갖춘 Java용 SDK(Software Development Kit)이다.
-	JDK는 JRE를 포함하고 있다.
-	JRE에 있는 모든 것뿐만 아니라 컴파일러(javac)와 jdb, javadoc과 같은 도구도 있다.
-	즉, JDK는 프로그램을 **생성, 실행, 컴파일**할 수 있다.

### JRE
-	Java Runtime Environment (자바 런타임 환경)
-	JVM + 자바 클래스 라이브러리(Java Class Library) 등으로 구성되어 있다.
-	컴파일 된 Java 프로그램을 **실행**하는데 필요한 패키지이다.

#### 요약
-	JDK는 자바 프로그램을 실행, 컴파일, 개발용 도구
-	JDK는 JRE, JVM을 모두 포함하는 키트
-	JRE는 자바 프로그램을 실행할 수 있게 하는 도구
-	JRE는 JVM을 포함하고 있다.