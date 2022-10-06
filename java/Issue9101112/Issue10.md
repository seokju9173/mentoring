## 자바의 멀티쓰레드 프로그래밍
### 멀티 스레드
한 프로세스가 여러 스레드로 동시에 여러 작업을 수행하는 것
### 멀티 스레딩의 장점
- 프로그램의 일부분이 중단되거나 긴 작업을 수행하더라도 프로그램의 수행이 계속 되어 사용자에 대한 응답성이 증가한다.
- 멀티 프로세스보다 적은 메모리 공간을 차지하고 캐시 메모리를 비울 필요가 없기 때문에 context switching이 빠르다.
- 스레드 간 통신에 별도의 자원을 이용하지 않고도, 전역변수 공간이나 힙 영역을 통해 데이터를 주고받을 수 있다.
- 스택을 제외한 모든 영역이 메모리를 공유하므로 통신 부담이 적다.

### 멀티 스레딩의 문제점
- Context switching, 동기화 등의 이유 때문에 싱글 코어 멀티 스레딩은 스레드 생성 시간이 오히려 오버헤드로 작용해 단일 스레드보다 느리다.
- 공유하는 자원에 동시에 접근하는 경우, 프로세스와 달리 스레드는 데이터와 힙 영역을 공유하기 때문에 어떤 스레드가 다른 스레드에서 사용 중인 변수나 자료구조에 접근하여 엉뚱한 값을 읽어오거나 수정할 수 있다. 따라서 동기화가 필요하다.
- 운영체제의 지원이 필요하다.
- 프로그래밍 난이도가 높다. 또한, 스레드 수만큼 자원을 많이 사용한다.

## Thread 클래스와 Runnable 인터페이스
자바에서 스레드를 생성하는 방법은 크게 두 가지로 나눌 수 있다.
-	Thread 클래스를 사용
-	Runnable 인터페이스를 사용
### Thread 클래스를 상속받는 방법
-	클래스를 Thread의 자식 클래스로 선언하는 것이다. 자식 클래스는 실행 메소드(run 메소드)를 재정의하여 인스턴스를 할당하고 실행할 수 있다.
-	Thread 클래스에는 start() 메소드가 있어서 호출하면 run() 메소드가 실행된다.
```java
public class ThreadTest {
    public static void main(String[] args) {
        SubThread thread = new SubThread();
        thread.start();
    }
}

class SubThread extends Thread {
    @Override
    public void run() {
        for (int i = 0; i < 5; i++) {
            System.out.println("Thread 클래스를 상속받은 클래스.");
        }
    }
}

> Task: ThreadTest.main()
> Thread 클래스를 상속받은 클래스.
> Thread 클래스를 상속받은 클래스.
> Thread 클래스를 상속받은 클래스.
> Thread 클래스를 상속받은 클래스.
> Thread 클래스를 상속받은 클래스.
```

### Runnable 인터페이스를 구현하는 방법
-	Runnable 인터페이스를 구현하는 클래스를 만들어 사용하는 방법으로 해당 클래스는 run() 메소드를 구현한다. run() 메소드를 구현했다면 클래스의 인스턴스를 할당하고 Thread를 만들 때 인수로 전달하고 시작할 수 있다.
-	Runnable 인터페이스에는 run() 메소드만 구현되어 있다.
```java
public class ThreadTest {
    public static void main(String[] args) {
        Runnable myThread = new MyThread2();
        Thread thread = new Thread(myThread);
        thread.start();
    }
}

class MyThread implements Runnable {
    @Override
    public void run() {
        for (int i = 0; i < 5; i++) {
            System.out.println("Runnable을 구현해서 만든 쓰레드");
        }
    }
}

> Task: ThreadThest.main()
> "Runnable을 구현해서 만든 쓰레드"
> "Runnable을 구현해서 만든 쓰레드"
> "Runnable을 구현해서 만든 쓰레드"
> "Runnable을 구현해서 만든 쓰레드"
> "Runnable을 구현해서 만든 쓰레드"
```

### Thread와 Runnable 중 어떤 것을 사용해야 할까?
-	run() 메소드 이외에 다른 메소드를 오버라이딩해야 한다면 Thread 클래스
-	run() 메소드만 오버라이딩한다면 Runnable 인터페이스를 구현하는 것이 일반적이다.

## 쓰레드의 상태
 ![image](https://user-images.githubusercontent.com/85390517/194315068-d713e304-f318-4c57-ba65-546c16a7e72a.png)

스레드의 생명주기이다.

|상태|설명|
|-----|----------|
|new<br>(생성)|스레드가 생성되고 아직 start()가 호출되지 않은 상태이다.|
|Runnable<br>(준비 상태)|스레드가 실행되기 위한 준비 단계이다.<br>CPU를 점유하고 있지 않으며 실행(Running)을 하기 위해 대기하고 있는 상태이다.<br>코딩 상에서 start() 메소드를 호출하면 run() 메소드에 설정된 스레드가 Runnable 상태로 진입한다.|
|Running<br>(실행 상태)|CPU를 점유하여 실행하고 있는 상태이며 run() 메소드는 JVM만이 호출이 가능하다.<br>Runnable(준비 상태)에 있는 여러 스레드 중 우선 순위를 가진 스레드가 결정되면 JVM이 자동으로 run() 메소드를 호출하여 스레드가 Running 상태로 진입한다.|
|Terminated<br>(종료 상태)|스레드가 종료된 상태이다.|
|Blocked<br>(지연 상태)|CPU 점유권을 상실한 상태이다. 후에 특정 메소드를 실행시켜 Runnable(준비 상태)로 전환한다.<br>wait() 메소드에 의해 Blocked 상태가 된 스레드는 notify() 메소드가 호출되면 Runnable 상태로 간다.<br>sleep(시간) 메소드에 의해 Blocked 상태가 된 스레드는 지정된 시간이 지나면 Runnable 상태로 간다.|

### Thread 메소드
-	Thread.sleep(long millis) / Thread.sleep(long millis, int nanos)
-	지정한 시간(1/1000초) 동안 스레드를 일시 정지 시킨다.
-	지정한 시간이 지나고 나면 다시 실행 대기 상태가 된다.

그 외에 다른 Thread 메소드
|Thread 메소드|설명|
|-----|----------|
|join()|스레드 자신이 하던 작업을 멈추고 다른 스레드가 지정된 시간동안 작업을 수행하도록 한다. 시간을 지정하지 않으면 해당 스레드가 작업을 모두 마칠 때까지 기다린다.|
|interrupt()|현재 실행중인 스레드를 중지시킨다. (inturrputedException 예외 발생)|
|yield()|자신의 작업 시간을 다른 스레드에게 양보하고 자신은 대기 상태가 된다.|
|suspend()|sleep()처럼 스레드를 일시정지한다.|
|resume()|suspend()에 의해 일시정지 상태에 있는 스레드를 실행 대기 상태로 만든다.|
|stop()|호출되는 즉시 스레드가 종료된다.|

## 쓰레드의 우선순위
-	자바에서 각 스레드는 우선순위(Priority)에 관한 자신만의 필드를 가지고 있다. 이러한 우선순위에 따라 특정 스레드가 더 많은 시간동안 작업을 할 수 있도록 설정한다.
-	스레드가 수행하는 작업의 중요도에 따라 스레드의 우선순위를 다르게 지정하여 특정 스레드가 더 많은 작업 시간을 갖도록 할 수 있다.
|필드|설명|
|----------|---------------|
|static int MAX_PRIORITY|스레드가 가질 수 있는 최대 우선순위를 명시함|
|static int MIN_PRIORITY|스레드가 가질 수 있는 최소 우선순위를 명시함|
|static int NORM_PRIORITY|스레드가 생성될 때 가지는 기본 우선순위를 명시함|

-	**`getPriority()`** 와 **`setPriority()`** 메소드를 통해 스레드의 우선순위를 반환하거나 변경할 수 있다.
-	스레드의 우선순위가 가질 수 있는 범위는 1부터 10까지이며, 숫자가 높을수록 우선순위가 높아진다.
-	스레드의 우선순위는 상대적인 값일 뿐이다. 우선순위가 10인 스레드가 우선순위가 1인 스레드보다 10배 빨리 수행되는 것이 아닌, 우선순위 10이 1보다 좀 더 많이 실행 큐에 포함되어 좀 더 많은 작업 시간을 할당 받을 뿐이다.

## Main 쓰레드
-	자바는 실행 환경인 JVM(Java Virtual Machine)에서 돌아가게 된다. 이것이 하나의 프로세스이고 Java를 실행하기 위해 우리가 실행하는 main() 메소드가 메인 스레드이다.
-	main() 메소드는 메인 스레드의 시작점을 선언하는 것이다.
```java
public class MainMethod {
    public static void main(String[] args) {
        // ...
    }
}
```
따로 스레드를 실행하지 않고 **main() 메소드만 실행하는 것을 ‘싱글 스레드 애플리케이션’** 이라고 한다.

### Daemon Thread
-	데몬 스레드는 다른 일반 스레드(데몬스레드 가 아닌)의 작업을 돕는 보조적인 역할을 수행하는 스레드이다.
-	데몬 스레드는 일반 스레드의 보조 역할을 수행하므로 일반 스레드가 모두 종료되고 나면 데몬 스레드의 존재 의미가 없어지기 때문에 **일반 스레드가 모두 종료되면 데몬 스레드는 강제적으로 자동 종료된다.**
+ isDaemon()
  + 스레드가 데몬인지 확인한다. 데몬 스레드라면 true, 아니면 false
+ setDaemon()
  + 스레드를 데몬 스레드로 또는 사용자 스레드로 변경한다. true면 데몬 스레드
```java
public class ThreadExample {
    public static void main(String[] args) {
        Thread main = Thread.currentThread();
        MyThread1 th1 = new MyThread1();

        th1.setDaemon(true);

        System.out.println("main.isDaemon() : " + main.isDaemon());
        System.out.println("th1.isDaemon() : " + th1.isDaemon());
    }
}

class MyThread1 extends Thread {
    @Override
    public void run() {
        super.run();
    }
}


> Task: ThreadExample.main()
> main.isDaemon() : false
> th1.isDaemon() : true
```
```java
// long의 최대값 만큼 대기
public class DaemonThread extends Thread {
    public void run() {
        try {
            Thread.sleep(Long.MAX_VALUE);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
    public void runCommonThread() {
        DaemonThread thread = new DaemonThread();
        thread.start();
    }

    // 프로그램이 대기하지 않고 그냥 끝나버린다.
// 데몬 쓰레드는 해당 쓰레드가 종료되지 않아도 다른 실행중인 일반 쓰레드가 없다면 멈춰버린다.
    public void runDaemonThread() {
        DaemonThread thread = new DaemonThread();
        thread.setDaemon(true);
        thread.start();
    }
```

## 동기화
싱글 스레드 프로세스의 경우 프로세스 내에서 단 하나의 스레드만 작업하기 때문에 프로세스의 자원을 가지고 작업하는데 별 문제가 없다. 멀티 스레드 프로세스의 경우 여러 스레드가 같은 프로세스 내의 자원을 공유해서 작업하기 때문에 서로의 작업에 영향을 주게 된다.

공유 자원 접근 순서에 따라 실행 결과가 달라지는 프로그램의 영역을 임계 영역(critical section)이라고 한다.

### 임계 영역 (Critical Section)
-	한 번에 하나의 프로세스만 액세스 할 수 있는 코드영역을 의미한다.
-	둘 이상의 프로세스가 동시에 접근할 수 없다.
-	만약 여러 프로세스가 동시에 임계영역에 접근하려고 한다면 Race Condition이 발생한다.

### RaceCondition
두 개 이상의 프로세스가 공유 자원을 병행적으로 읽거나 쓰는 상황을 말하며, 공유 자원 접근 순서에 따라 실행 결과가 달라지는 상황을 말한다.

### 자바에서 동기화하는 3가지 방법
-	Synchronized 키워드
-	Atomic 클래스
-	Volatile 키워드

### Synchronized 키워드
-	Java 예약어 중 하나로 변수명이나 클래스명으로 사용이 불가능하다.
#### Synchronized 사용 방법
-	메소드 자체를 synchronized로 선언하는 방법 (synchronized methods)
```java
public synchronized void calcSum() {
    // ,,,
}
```
-	메소드 내의 특정 문장만 synchronized로 감싸는 방법 (synchronized statements)
```java
synchronized (객체의 참조변수) {
    // ...
}
```

### Atomic
-	Atomicity(원자성)의 개념은 ‘쪼갤 수 없는 가장 작은 단위’를 뜻한다.
-	자바의 Atomic Type은 Wrapping 클래스의 일종으로, 참조 타입과 원시 타입 두 종류의 변수에 모두 적용이 가능하다. 사용시 내부적으로 CAS(Compare-And-Swap) 알고리즘을 사용해 lock 없이 동기화 처리를 할 수 있다.
-	CAS는 특정 메모리 위치와 주어진 위치의 value를 비교하여 다르면 대체하지 않는다.
-	변수를 선언할 때 타입을 Atomic Type으로 선언해주면 된다. 예) AtomicLong
#### Compare-And-Swap란?
-	메모리 위치의 내용을 주어진 값과 비교하고 동일한 경우에만 해당 메모리 위치의 내용을 새로 주어진 값으로 수정한다.
-	현재 주어진 값(현재 스레드에서의 데이터)과 실제 데이터와 저장된 데이터를 비교해서 일치할 때만 값을 업데이트한다.

### Volatile
-	volatile 키워드는 Java 변수를 Main Memory에 저장하겠다라는 것을 명시하는 것이다.
-	매번 변수의 값을 Read할 때마다 CPU Cache에 저장된 값이 아닌 Main Memory에서 읽는 것이고, 변수의 값을 Write할 때마다 Main Memory에 작성하는 것이다.

## 데드락
-	데드락(Deadlock, 교착 상태)
-	프로세스가 자원을 얻지 못해 다음 처리를 하지 못하는 상태이다.
-	둘 이상의 프로세스가 각자 공유 자원을 할당 받은 후 다음 처리를 위해 다른 공유 자원을 할당 받아야할 때, 각자 필요한 자원이 상대방에게 할당 되어있는 경우 프로세스들이 다음 처리를 위한 공유 자원을 획득하기 위해 서로 무한정 대기하는 상태이다.

### 데드락 발생 조건
+ 상호배제 (Mutual Excusion)
  + 한 번에 하나만 해당 자원을 사용할 수 있다.
  + 사용 중인 자원을 다른 프로세스가 사용하려면 요청한 자원이 해제될 때까지 기다려야 한다.
+ 점유 대기 (Hold and Wait)
  + 자원을 최소한 하나만 보유하고, 다른 프로세스에 할당된 자원을 점유하기 위해 대기하는 프로세스가 존재해야 한다.
+ 비선점 (Non Preemptive)
  + 이미 할당된 자원을 강제로 빼앗을 수 없다.
+ 순환 대기 (Circular wait)
  + 대기 프로세스의 집합이 순환 형태로 자원을 대기하고 있어야 한다.

### 데드락 해결 방법
+ 교착 상태 예방
  + 교착 상태가 발생하기 전에 미리 조치를 취하는 방식으로, 교착 상태 발생 조건 중 하나를 제거함으로써 해결한다.
  + 자원의 상호배제 조건 방지
    + 모든 자원을 공유 허용
  + 점유와 대기 조건 방지
    + 모든 자원에 대해 선점 허용
  + 비선점 조건 방지
    + 필요 자원을 한 번에 모두 할당하기
  + 순환 대기 조건 방지
    + 자원에게 순서 부여를 통해 프로세스 순서의 증가 방향으로만 자원 요청

+ 교착 상태 회피
  + 프로세스가 자원을 요구할 때, 시스템은 자원을 할당한 후에도 안정 상태로 남아있는가를 확인하여 교착 상태를 회피하는 방법

+ 교착 상태 탐지
  + Deadlock이 발생하면 빠르게 발견하고 문제를 해결하는 것

+ 교착 상태 회복
  + 교착 상태를 일으킨 프로세스를 종료하거나 할당된 자원을 해제하면서 회복
