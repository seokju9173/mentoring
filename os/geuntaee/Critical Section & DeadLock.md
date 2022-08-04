동시성 문제에 대해서 알아보려고 한다. 목차는 다음과 같다.
- Critical Section( 임계영역 )
- RaceCondition
- Deadlock( 교착상태 )
- Deadlock 발생 조건
- Deadlock 해결 방법

## Critical Section( 임계영역 )
Critical Section( 임계영역 ) : 둘 이상의 프로세스가 동일한 코드 세그먼트에 엑세스하는 경우 해당 세그먼트를 Critical Section이라고 한다.
Critical Section에는 데이터 변수의 일관성을 유지하기 위해 동기화해야하는 공유 변수 또는 리소스가 포함된다.
간단하게 말해서, 공유 자원 접근 순서에 따라 실행 결과가 달라지는 프로그램의 코드 영역을 뜻한다.
또한, <span style=”color:lightblue”>동시에 여러 프로세스가 임계구역 안에서 실행될 수 없는데, 이와 같은 임계구역 문제를</span><span style=”color:indianred”>race condition( 경쟁 상황 )</span>이라고 한다.

## Race Condition( 경쟁 상황 )
race condition( 경쟁 상황 ) : 2개 이상의 프로세스가 공유 자원을 병행적으로 읽거나 쓰는 상황을 말하며, 공유 자원 접근 순서에 따라 실행 결과가 달라지는 상황을 말한다.

Critial Section에서 발생하는 Race Condition을 해결하려면 3가지 요구 조건을 충족해야 한다.

- Mutual Exclusion( 상호배제 ) : 특정한 프로세스가 임계구역에서 실행되는 동안, 다른 프로세스가 접근할 수 없다.

- Progress( 진행 ) : Critial Section을 사용하지 않고 있다면, 다른 프로세스가 접근할 수 있도록 한다.

- Bounded Waiting( 한정된 대기 ) : Critial Section 진입 횟수에 한계가 있어서 같은 프로세스가 계속 독점해서 사용하지 못하게 한다.

위 3가지 모두를 만족시켜야 고유한 알고리즘이 완성된다.

전형적으로 프로세스는 크게 4가지의 구역으로 나눠진다.
- entry section
- critical section
- exit section
- remainder seciton

![](https://velog.velcdn.com/images/dymnam/post/b3904815-76d3-4fbf-9853-ef0ddeb3bb56/image.png)
```c
do{
    flag=1;
    while(flag); // (entry section)
        // critical section
    if (!flag)
        // remainder section
} while(true);
```

## Deadlock( 교착상태 )
Deadlock( 교착상태 ) : 각 프로세스가 자원을 보유하고 다른 프로세스가 획득한 다른 자원을 기다리기 때문에 일련의 프로세스가 차단되는 상황이다.
만약, 두 대의 열차가 같은 선로에서 서로를 향해 오고 있는데 선로가 하나뿐이라면 굉장히 난감할 것이다.
일부 자원을 보유하고 다른 프로세스가 보유하고 있는 자원을 기다리는 둘 이상의 프로세스가 있는 OS에서도 유사한 상황이 발생한다.
![](https://velog.velcdn.com/images/dymnam/post/ad77b72e-453a-4103-8c9f-fd5e9767fee3/image.png)

## Deadlock 발생 조건
DeadLock은 다음과 같은 4가지 조건이 동시에 성립하는 경우 발생할 수 있다.

- Mutual Exclusion( 상호 배제 )
둘 이상의 자원을 공유할 수 없다. 즉, 하나의 프로세스만이 자원을 사용할 수 있다.

- Hold and Wait( 보류 및 대기 )
프로세스가 최소 하나의 자원을 보유하고 대기중인 자원.

- No preemption( 비선점 )
프로세스가 자원을 해제하지 않는 한 프로세스에서 리소스를 가져올 수 없다.
즉, 프로세스는 OS에 의해 강제로 자원을 빼앗기지 않는다.

- Circular Wait( 순환 대기 )
일련의 프로세스가 순환 형태로 서로를 기다리는 상태로 즉, 자원을 기다리는 프로세스 간 사이클이 형성되어야 한다.

## Deadlock 해결 방법
DeadLock의 해결 방법은 다음과 같은 3가지 방법이 있으며, 4가지의 단어로 알아보자.
-> 예방, 탐색, 회피, 무시

- DeadLock Prevention or avoidance( 교착 상태 방지 또는 회피 )
시스템이 DeadLock 상태가 되지 않도록 하는 것으로, 위에서 언급한 DeadLock 발생 조건 중 하나를 무효화하여 예방한다. 하지만, 무효화하여 예방하는 방법은 자원 낭비가 심하다.

avoidance( 회피 )는 본질적으로 미래 지향적이다.
"회피"전략을 사용하여 가정해야하며, 프로세스를 실행하기 전에 프로세스에 필요한 자원에 대한 모든 정보를 알고 있는지 확인해야 한다. 하지만, 오버헤드가 많이 발생한다.

- DeadLock detection and recovery( 교착 상태 감지 및 복구 )
DeadLock이 발생하도록 한 다음 발생하면 선점하여 처리한다. 하지만, 자원을 요청할 때마다 탐지 알고리즘이 실행되면 오버헤드가 발생한다.

- lgnore the problem altogether( 문제 무시 )
교착 상태가 매우 드물다면 교착 상태가 발생하도록 하고 시스템을 재부팅한다. 이 방법은 Window와 UNIX 모두에서 사용하는 접근 방식이다.

