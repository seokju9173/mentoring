## 자바의 예외 처리
**예외처리(Exception handling)** 란, 프로그래머가 예기치 못한 예외의 발생에 미리 대처하는 코드를 작성하는 것이다.
-	예외처리의 목적은 실행중인 프로그램의 비정상적인 종료를 막고, 상태를 정상상태로 유지하는 것이다.

## 자바에서 예외 처리 방법 (try, catch, throw, throws, finally)
## try, catch, throw, throws, finally
**`try`**, **`catch`**, **`finally`** 블록으로 예외가 발생한 메소드 내에서 예외 처리를 수행할 수 있다.
```java
try {
    // 예외 발생 가능성이 있는 코드
} catch(예외타입1 매개변수명) {
    // 예외타입1의 예외가 발생할 경우 처리 코드
} catch(예외타입2 매개변수명) {
    // 예외타입2의 예외가 발생할 경우 처리 코드
} finally {
    // 항상 처리할 필요가 있는 코드
}
```

### try ~ catch
```java
// 예외가 발생 가능한 부분을 감싼다.
try {
    System.out.println("나누기 시작");
    System.out.println("나눗셈 결과의 몫 : " + (num1 / num2));    // (1) 예외 발생!
    System.out.println("나눗셈 결과의 나머지 : " + (num1 % num2));  // (2)
}

// 예외가 발생하면 처리되는 코드
catch(ArithmeticException e) {
    System.out.println("ArithmeticException 발생! 0이 입력되었습니다.");
}

// 예외처리가 끝나고 나서 처리
    System.out.println("프로그램을 종료합니다.");

// 실행결과
    나누기 시작
    ArithmeticException 발생! 0이 입력되었습니다.
    프로그램을 종료합니다.
```
+ **`try`** 는 예외 발생의 감지 대상을 감싸는 목적으로 사용된다.
  + 예외가 발생할 가능성이 있는 범위를 지정하는 블록이다.
  + try문 코드를 살펴보면, (1)예외가 발생하면, (2)는 실행되지 않는다.
+ catch는 발생한 예외 상황의 처리를 위한 목적으로 사용된다.
  + 예외가 발생하지 않으면 catch문은 실행되지 않는다.
  + 반드시 `java.lang.Throwable` 클래스의 하위 클래스 타입으로 선언되어야 한다.
  + 여러 개의 catch문을 작성할 수 있다. (예외 클래스의 계층 구조를 정확히 알고 사용해야 한다. -> 범위가 작은 순으로 catch문을 작성해야 한다.)
+ Multi Catch 문
  + 자바 7 이후의 버전에서는 여러 개의 catch문 중 예외 처리가 동일하다면 catch문을 묶어서 처리할 수 있다. 여러 개의 Exception을 하나의 catch문에 **`|`** 으로 합칠 수 있다.
```java
try {
    // code
} catch(IllegalStateException | IllegalArugmentException e) {
    // catch
}
```
-	Multi catch문으로 묶는 Exception이 상속관계라면 불가능하다.

### finally
```java
try{
    System.out.println(1 / 0); // 예외 발생!
}

catch (ArithmeticException e){
    System.out.println("예외를 처리하는 코드");
}

finally{
    System.out.println("이 문장은 마지막에 꼭! 출력됩니다.");
}

// 실행 결과
    예외를 처리하는 코드
    이 문장은 마지막에 꼭! 출력됩니다.
```
-	try ~ catch가 실행되고 난 뒤, 항상 실행된다.
-	예기치 못한 예외가 발생해도, 예외가 발생하지 않아도 항상 실행된다.

### throws
예외가 발생한 메소드를 호출한 곳으로 예외 객체를 넘기는 방법이다.

### throw
-	인위적으로 예외를 발생시킬 때 사용할 수 있는 예약어이다.
-	개발자가 임의로 예외를 발생시킬 때 사용되며 특정 예외를 만났을 때 더욱 구체적인 예외로 처리하고자 할 때에도 사용된다.
```java
public static void main(String[] args) {
    try {
        // do something
    } catch(Exception e) {
        throw new IllegalStateException("...");
    } finally {
        // do something
    }
}
```

## 예외를 처리하는 일반적인 3가지 방법
-	예외가 발생하면 다른 작업 흐름으로 유도하는 **예외 복구**
-	처리하지 않고 호출한 쪽으로 예외를 던져버리는 **예외처리 회피**
-	호출한 쪽으로 던질 때 명확한 의미를 전달하기 위해 다른 예외로 전환하여 던지는 **예외 전환**
### 예외 복구
```java
public void exceptionExam() {
        int num = 10;
        while(num-- > 0) {
            try {
                // 예외가 발생할 가능성이 있는 시도
                // 작업 성공 시 리턴
                return;
            } catch(Exception e) {
                // 로그 출력, 정해진 시간만큼 대기 등등
            } finally {
                // 리소스 반납 및 정리 작업
            }
        }
        throw new FailedException(); 직접 예외 발생
}
```
-	예외 복구의 핵심은 예외가 발생해도 애플리케이션은 정상 흐름으로 진행된다는 것이다. 위 예제는 예외가 발생하면 그 예외를 잡아서 일정 시간만큼 대기하고 다시 재시도를 반복한다. 그리고 최대 재시도 횟수를 넘기면 예외를 발생시킨다.
-	재시도를 통해 정상적인 흐름을 타게 한다거나, 예외가 발생하면 이를 미리 예측하여 다른 흐름으로 유도하도록 구현하면 예외가 발생하여도 정상적으로 작업을 종료할 수 있다.

### 예외처리 회피
```java
public void test() throws SQLException {
        // do someting..
}
```
-	예외가 발생하면 `throws`를 통해 호출한 쪽으로 예외를 던지고 그 처리를 회피하는 것이다. 하지만 무책임하게 예외를 던지는 것은 위험하다.
-	호출한 쪽에서 다시 예외를 받아 처리하도록 하거나, 해당 메소드에서 이 예외를 던지는 것이 최선의 방법이라는 확신이 있을 때만 사용해야 한다.

### 예외 전환
```java
try {
    // do something
} catch (SQLException e) {
    throw FailedException();
}
```
-	위의 예제 코드와 같이 예외를 잡아서(catch) 다른 예외를 던지는 것이다.
-	호출한 쪽에서 예외를 받아서 처리할 때 조금 더 명확히 인지할 수 있도록 돕기 위한 방법이다.

### try – with – resource
-	자바 7 이후부터, 예외 발생 시 자동으로 close()를 해준다.
-	사용하는 객체는 반드시 AutoClosable을 구현한 객체이어야 한다.

#### try – with – resource 사용 전
```java
FileInputStream fileInputStream = null;
try {
    fileInputStream = new FileInputStream("존재하지 않는 파일.txt");
    // 실행 코드
} catch (IOException e) {
    // 예외 처리
} finally {
    if (fileInputStream == null) {
        try {
            fileInputStream.close();
        } catch (IOExeption e) {
            // 예외 처리
        }
    }
}
```

#### try – with – resource 사용 후
```java
try (FileInputStream fis = new FileInputStream("존재하지 않는 파일.txt")){
    // 실행 코드
} catch (IOException e) {
    // 예외 처리
}
```

## 자바가 제공하는 예외 계층 구조
 
-	자바 최상위 객체인 Object를 필두로 에러 최상위 객체인 Java.lang.Throwable을 상속받는 에러(Error) 객체와 예외(Exception) 객체가 있다.
-	Exception 하위 예외 클래스 중 RuntimeException과 그 하위 예외를 선택적 예외로 개발자가 상황에 맞춰 대응해야 하는 예외이고, 그 외 나머지 예외 클래스와 그 하위 객체들을 필수(checked) 예외라 하여 반드시 체크해줘야 하는 예외이다.
-	RuntimeException 클래스들은 주로 프로그래머의 실수에 의해 발생될 수 있는 예외들이다. (배열의 범위를 벗어나거나, 값이 null인 참조 변수의 멤버를 호출하려 하는 경우 등)
-	Exception 클래스들은 주로 외부의 영향으로 발생할 수 있는 것들로, 대표적으로 I/O 입출력에 의해 발생하는 경우가 많다. (클래스의 이름을 잘못 적거나, 데이터 형식이 잘못되었거나, 사용자가 존재하지 않는 파일명을 입력한 경우 등)

## Exception과 Error의 차이는?
### 에러 (Error)
-	시스템의 비정상적인 상황이 생겼을 때 발생한다. 시스템 레벨에서 발생하기 때문에 개발자가 미리 예측하여 처리할 수가 없다.
-	메모리 부족(OutOfMemoryError), 스택오버플로우(StackOverFlowError)처럼 JVM이나 하드웨어 등 기반 시스템의 문제로 발생하는 것이다. 발생을 대비하여 프로그래머가 할 수 있는 것이 없다. 발생하는 순간 프로그램은 비정상 종료가 되기 때문에 주의해야 한다.
+ 에러는 크게 컴파일 에러와 런타임 에러로 구분할 수 있다.
+ 컴파일 에러
  + 컴파일 과정에서 일어나는 에러로 기본적으로 자바 컴파일러가 문법 검사를 통해서 오류를 잡아준다.
+ 런타임 에러
  + 실행 과정에서 일어나는 에러로 컴파일이 문제없이 되더라도 실행 과정(Runtime)에서 오류가 발생할 수 있다. 
  + 이러한 런타임 에러를 방지하기 위해 프로그램 실행 도중 일어날 수 있는 모든 경우의 수를 고려해야 한다.
  + 자바에서는 런타임 에러를 예외(Exception)와 에러(Error) 두 가지로 구분하여 대응하고 있다.

### 예외 (Exception)
-	프로그램 내에서 복구할 수 있는 장애로, 예외가 발생하더라도 프로그램이 비정상적으로 종료되지 않도록 코드를 작성하여 핸들링 해줄 수 있다.
-	예외가 발생하면 런타임 시스템은 Call stack 내에서 이를 핸들링 할 수 있는 메소드를 찾기 시작한다. 만약 Call stack 내에서 해당 예외를 핸들링 할 수 있는 메소드를 찾지 못한다면 런타임 시스템은 종료된다.
### 콜 스택 (Call stack)
-	런타임 시스템은 예외를 제어할 수 있는 코드 블록을 가지고 있는 메소드를 찾기 위해 **`Call stack`** 에서 검색한다. 이 코드 블록을 **`Exception handler`** 라고 부른다.
-	검색은 오류가 발생한 메소드로부터 시작하여 메소드가 호출된 메소드를 역순으로 콜 스택을 통해서 진행된다. 적절한 핸들러가 발견되면 예외를 핸들러로 전달한다.
-	만약 런타임 시스템이 적절한 핸들러를 찾지 못하고 콜 스택의 모든 메소드를 검색하면, 런타임 시스템은 종료된다. 즉, 프로그램은 종료된다. 이는 비정상 종료라고 볼 수 있다.
### 예외를 사용하여 에러를 처리하였을 때 기존 처리 기술에 비한 장점
- 에러를 처리하는 코드와 일반 코드가 분리될 수 있다.
- 콜 스택을 따라 에러 전파가 가능해 실질적으로 처리가 될 수 있는 지점에서 처리를 해줄 수 있다.
- 오류를 그룹화할 수 있고 분류할 수 있다.

## RuntimeException과 RuntimeException가 아닌 것의 차이는?
자바의 예외 계층 구조에서 볼 수 있듯이 예외 클래스는 두 그룹으로 나눌 수 있다.
-	Exception 클래스와 그 자손들
-	RuntimeException 클래스와 그 자손들

### Checked Exception
Checked Exception(RuntimeException 클래스들을 제외한 나머지 클래스들)은 주로 외부의 영향으로 발생하는 것들로, 프로그램 사용자들의 동작에 의해서 발생하는 경우가 많다.
-	존재하지 않는 파일의 이름을 입력한 경우 (FileNotFoundException)
-	실수로 클래스의 이름을 잘못 적은 경우 (ClassNotFoundException)
-	입력한 데이터의 형식이 잘못된 경우 (DataFormatException)
등의 경우에 발생한다.

이름에서 알 수 있듯이 반드시 예외 처리를 해야 하고, 컴파일되는 시점에서 확인한다.

### Unchecked Exception
Unchecked Exception(RuntimeException 클래스와 그 자손들)은 주로 프로그래머의 실수에 의해서 발생될 수 있는 예외들로 자바의 프로그래밍 요소들과 관계가 깊다.
-	배열의 범위를 벗어난 경우 (ArrayIndexOutOfBoundsException)
-	값이 null인 참조변수의 멤버를 호출하려 한 경우 (NullPointerException)
-	클래스 간의 형변환을 잘못한 경우 (ClassCastException)
-	정수를 0으로 나누려고 한 경우 (ArithmethicException)
등의 경우에 발생한다.

명시적인 처리를 강제하지 않고, 실행되는 시점(Runtime)에 확인한다.

### 구분하는 방법
-	두 가지의 가장 명확한 구분 기준은 **꼭 예외 처리를 해야하는지** 이다. Checked Exception이 발생할 가능성이 있는 메소드라면 반드시 **`try ~ catch`** 로 감싸거나 **`throw`** 로 던져서 처리해야 한다.
-	반면에 Unchecked Exception은 명시적인 예외 처리를 하지 않아도 된다. 대부분의 예외가 부주의로 발생하고, 예측하지 못했던 상황의 예외가 아니기 때문에 굳이 로직으로 처리할 필요가 없도록 만들어져 있다.
-	예외를 확인할 수 있는 시점으로도 구분할 수 있다. 일반적으로 컴파일 단계에서 명확하게 Exception 체크가 가능한 것을 Checked Exception이라 하며, 실행 과정 중 어떠한 특정 논리에 의해 발견되는 Exception을 Unchecked Exception이라 한다.


## 커스텀한 예외 만드는 방법
기존에 정의된 예외 클래스 외에 필요에 따라 프로그래머가 새로운 예외 클래스를 정의하여 사용할 수 있다. 보통 Exception 클래스로부터 상속받는 클래스를 만들지만, 필요에 따라서 알맞은 예외 클래스를 선택할 수 있다.
-	Checked Exception을 구현할 때는 Exception 을 확장
-	Unchecked Exception을 구현할 때는 RuntimeException을 확장
+ 커스템 예외를 만들 때 4가지 Best Practices를 따르는 것이 좋다.
  + Always Provide a Benefit
    + 자바 표준 예외들에는 포함되어 있는 다양한 장점을 가지는 기능들이 있다.
    + 커스텀 예외가 어떠한 장점을 제공하지 못한다면 오히려 표준 예외 중 하나를 사용하는 것이 낫다.
  + Follow the Naming Convention
    + JDK가 제공하는 예외 클래스들을 보면 클래스의 이름이 모두 Exception으로 끝난다.
    + 이러한 네이밍 규칙은 자바 생태계 전체에 사용되는 규칙이다.
    + 커스텀 예외도 이 네이밍 규칙을 따르는 것이 좋다.
  + Provide Javadoc Comments for your Exception Class
    + 기본적으로 API의 모든 클래스, 멤버변수, 생성자들에 대해서는 문서화하는 것이 일반적인 Best Practices이다.
    + API 메소드가 어떤 하나의 예외를 기술하고 있다면, 그 예외는 API의 한 부분이 되는 것이며 그 예외를 문서화해야 한다.
  + Provide a Constructor That Sets the Cause
    + 커스텀 예외를 던지기 전에 표준 예외를 Catch하는 케이스가 많다.
    + 보통 예외에는 제품에 발생한 오류를 분석하는데 필요한 정보가 포함되어 있다.
    + 예외의 Cause를 설정할 수 있는 생성자를 제공해야 한다.
