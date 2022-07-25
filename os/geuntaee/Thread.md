Thread를 공부하기 앞서 process를 다시 살펴보면 <span style="color:lightblue">프로세스는 단순히 실행 중인 프로그램</span>이다.
이러한 프로세스는 <span style="color:indianred">프로그램에 사용되는 데이터와 메모리 등의 자원 그리고 스레드</span>로 구성된다.

## Thread
그러면 Thread는 무엇이냐하면, 프로세스 내에서 실제로 작업을 수행하는 주체를 의미한다.![](https://velog.velcdn.com/images/dymnam/post/3d6ac19a-568d-4a60-ae84-0761cc8ef9a5/image.png)
위 그림과 같이 [프로세스]의 큰 틀 안에 Code, Data, Heap, Thread가 존재하며, Thread 안에는 Stack이 존재하고 있다.

## Thread 와 Process의 차이점
Thread와 Process의 주요 차이점은 동일한 프로세스 내에서 Thread는 <span style="color:indianred">공유 메모리 공간에서</span>, Process는 <span style="color:indianred">별도의 메모리 공간에서</span> 실행된다는 차이점이 있다.

## Stack을 Thread마다 독립적으로 할당하는 이유
Stack은 함수 호출 시 전달되는 인자, 선언하는 변수 등을 저장하기 위해 사용되는 메모리 공간이다.
Stack 메모리 공간이 독립적이라는 의미는 독립적인 함수 호출이 가능하다는 의미이며, 독립적인 실행 흐름이 추가된다는 것이다.
<span style="color:indianred">Thread의 정의에 의거하면 독립적인 실행 흐름을 추가하기 위해 최소한의 조건으로 독립된 Stack 영역을 할당하는 것</span>이다.

## PC register를 Thread마다 독립적으로 할당하는 이유
PC 값은 Thread가 명령어를 어디까지 수행했는지 나타나게된다.
<span style="color:indianred">Thread는 CPU를 할당받았다가 Scheduler에 의해 다시 선점 당하는데, 때문에 명령어가 연속적으로 수행되지 못하고 어느 부분까지 수행했는지 기억할 필요가 있다.</span>
결과적으로 PC register를 독립적으로 할당하는 이유가 된다.

## 스레드의 생성과 실행
```java
class ThreadEx extends Thread{
    public void run() {
        for (int i=0; i<5; i++) {
            System.out.println(getName()); // 현재 실행중인 스레드의 이름을 반환.
            try{
                // Thread.sleep() 코드만 사용하면 checkedException 인
                // InterruptedException 이 발생한다.
                // 그러므로 try.. catch 를 이용하여 오류를 해결한다.
                Thread.sleep(10);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}

class ThreadWithRunnable implements Runnable {
    public void run() {
        for (int i=0; i<5; i++) {
            // 현재 실행중인 스레드의 이름을 반환.
            System.out.println(Thread.currentThread().getName());
            try {
                Thread.sleep(10);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}

public class Thread0101 {
    public static void main(String[] args) {
        ThreadEx threadEx = new ThreadEx(); // Thread 클래스를 상속받는 방법
        Thread threadEx2 = new Thread(new ThreadWithRunnable()); // Runnable 인터페이스 구현 방법

        threadEx.start();
        threadEx2.start();
    }
}
```
위와 같이 작성한 뒤 프로그램을 실행하면 결과는 다음과 같다.
![](https://velog.velcdn.com/images/dymnam/post/6af3f727-3ee9-45a6-849b-474811083e51/image.png)
결과를 확인해보면서 알 수 있듯이, 생성된 스레드가 서로 번갈아가며 실행되고 있는 것을 확인 할 수 있다.
Thread 클래스를 상속 받으면 다른 클래스를 상속 받을 수 없으므로, 일반적으로 Runnable 인터페이스를 구현하는 방법으로 스레드를 생성한다.

~~Runnable 인터페이스는 몸체가 없는 메소드인 run() 메소드 단 하나만을 가지는 인터페이스임.~~

#### 용어 정리
- PC : Program Counter - 다음에 실행할 명령어의 주소 저장, 제어 레지스터

- SP : Stack Pointer - 현재 스택 영역에서 가장 최근에 저장된 부분을 가르킴, 데이터 레지스터
