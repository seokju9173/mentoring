MultiThread란 무엇인가 ? 생각하기 전에 Thread를 다시 한번 생각해보면 <span style="color:lightblue">Thread는 프로세스 내에서 실제로 작업을 하는 주체를 의미하며, 프로세스란 큰 틀 안에 Code, Data, Heap, Thread가 존재하고, Thread 안에는 Stack이 존재</span>하는 것이다.

## MultiThread
![](https://velog.velcdn.com/images/dymnam/post/1e49e742-5a59-4433-8cef-618a807e07b4/image.png)
멀티 스레드는 CPU의 최대 활용을 위해 프로그램을 두 개 이상 실행하는 기술이다.
![](https://velog.velcdn.com/images/dymnam/post/1bf9691a-8c6d-4d36-8dd8-c1be8302a3db/image.png)
위 그림을 보면 멀티스레드에 대해서 조금 더 이해하기 쉬울 것 같다.
이러한 작업은 Context Switching 이라는 것을 통해서 이루어 지는데 하나의 스레드의 일부를 진행하다가 또 다른 하나의 스레드의 일부를 진행하는 형식이다.
쉽게 말해서 Context Switching이 엄청 빠르게 일어나면서 사용자의 시선에서 프로그램들이 동시에 수행되는 것처럼 보이게 된다.

## MultiThread 장점
- 프로세서가 여러 개인 경우 :
프로세서의 MultiThread를 각각 다른 프로세서에서 병렬적으로 수행될 수 있게하여 Thread의 병렬성( Parallelism )을 높일 수 있다.

- 프로세서가 하나인 경우 :
프로세서가 하나라면 MultiThread를 통해 동시성( Concurrency )을 높일 수 있다. 
<span style="color:indianred">만약, 어떤 Thread가 blocked( wating ) 되어도 커널이 다른 Thread로 Switch 시켜 실행 할 수 있다.</span>

- 프로세서의 개수 이상의 프로세스를 사용하면 Context Swithing이 발생하면서 오버헤드가 발생함에 따라 Multi Thread에 비해 비교적 부정적이다.

## MultiThreading 장점
- 응답성 ( Responsiveness ) :
프로그램의 일부분( Thread 중 하나 )이 중단되거나 긴 작업을 수행하더라도 프로그램의 수행이 계속 되어 사용자에 대한 응답성이 증가한다.
-> MultiThread의 경우, 에러 발생시 새로운 Thread를 생성하여 극복한다.

- 자원 공유 ( Resource Sharing ) :
프로세스는 오직 공유 메모리나 메세지 패싱을 이용하여 자원을 공유할 수 있는데, Thread는 Thread 간 통신에 별도의 자원을 이용하지 않고도 전역변수 공간이나 heap 영역을 통해 데이터를 주고 받을 수 있다.

- 경제성 ( Economy ) :
프로세스 내 자원들과 메모리를 공유하기 때문에 메모리 공간과 시스템 자원 소모가 줄어들게 된다.
자원 공유에 이어서 스레드 간 통신이 필요한 경우에도 쉽게 데이터를 주고 받을 수 있으며, 프로세스의 Context Switching과는 다르게 Thread에서의 Context Switching은 캐시 메모리를 비울 필요가 없다는 장점이 있다.

- 확장성 ( Scalability ) :
Single Thread인 경우 한 프로세스는 오직 한 프로세서에서만 수행이 가능하다. 하지만, MultiThread의 경우에서는 한 프로세스를 여러 프로세서에서 수행 가능하므로 훨씬 효율적인 모습을 보인다.

## MultiThread의 단점
- Thread 간 자원을 공유하는 이유로 하나의 Thread에서 오류가 발생해 프로세스가 종료된다면, 전체 Thread가 종료될 수 있다.

- Single Thread에 비해 Multi Thread는 동일한 작업을 수행하기 위해 더 많은 코드를 작성해야 한다.

- Multi Thread 응용 프로그램은 작성, 이해, 디버그 및 유지 관리가 어렵다.

## Multi Thread VS Multi Process
<span style="color:indianred">Multi Thread와 Multi Process 모두 동시에 두 가지 이상의 루틴을 수행할 수 있는 역할을 하는 것을 동일</span>

- Multi Thread는 단일 프로세스의 Context 내에서 여러 Thread를 동시에 실행하고, 프로세스 내의 Thread들은 Code, Data, Heap 영역을 공유한다.

- Multi Process는 fork를 사용해 프로세스를 복사할 수는 있지만, 부모-자식 관계라 해도 환경변수와 프로세스 핸들 테이블이 상속 가능할 뿐 프로세스들은 자신만의 메모리 영역을 가지게 된다.
또한, 프로세스 간 통신을 하려면 IPC( Inter Process Communication )를 통해야 한다.
