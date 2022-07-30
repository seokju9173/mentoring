시작하기에 앞서 다중 프로그래밍이란, 동시에 메모리에 존재할 수 있는 프로그램의 수를 의미한다.

## CPU Scheduler 정의
CPU Scheduling은 프로세스가 생성되어 실행될 때 <span style=”color:indianred”>필요한 시스템의 여러 자원을 해당 프로세스에게 할당하는 작업</span>을 의미한다.

## CPU Scheduler 목적
- 공정한 스케줄링
- 처리량의 극대화
- 응답시간 최소화
- 균형있는 자원 사용
- 반환시간 예측 가능

## CPU Scheduling 유형
![](https://velog.velcdn.com/images/dymnam/post/1cc92052-1ce3-4c0c-aacf-84afab5d0f56/image.png)
#### 선점( Preemptive )
선점 스케줄링에서 작업은 대부분 우선 순위와 함께 할당된다.
장점으로는 빠른 응답과 시분할 시스템에 적합하다는 것이다.
단점으로는 Context Switching의 오버헤드가 발생할 수 있다.

#### 비선점( Non-Preemptive )
비선점 스케줄링에서 작업은 CPU가 특정 프로세스에 할당된다. 만약, CPU를 바쁘게 유지하는 프로세스는 Context Switching하거나 종료하여 CPU를 해제하게 된다.
다양한 하드웨어 플랫폼에서 사용할 수 있는 유일한 방법으로 선점형 스케줄링의 타이머 등이 필요하지 않다.

## CPU Scheduler 유형
CPU Scheduling 알고리즘의 종류에는 6가지 유형이 있다.
- First Come First Serve( FCFS ) - 선착순
- Shortest - Job - First( SJF ) - 최단 작업 우선
- Shortest Remaining Time - 최단 남은 시간
- Priority Scheduling - 우선순위
- Round Robin Scheduling -  라운드 로빈
- MultiLevel Queue Scheduling - 다단계 대기열

![](https://velog.velcdn.com/images/dymnam/post/3fe396fc-9098-4853-bac8-ad84d0c6a989/image.png)

## First Come First Serve( FCFS ) - 선착순
First Come First Serve, 줄여서 FCFS는 완전한 형태로 이루어져 있으며, 가장 쉽고 간단한 CPU Scheduling 알고리즘이다.
<span style=”color:indianred”>FCFS의 알고리즘에서는 CPU를 요청하는 프로세스가 먼저 CPU를 할당받는 식</span>이다.
FCFS Scheduling은 FIFO 대기열로 관리할 수 있다.
#### FCFS 방법의 특성
- 비선점 및 선점 스케줄링 알고리즘을 제공한다.
- 작업은 항상 선착순으로 실행된다.
- 구현 및 사용이 쉽다.
- 그러나 이 방법은 성능이 좋지 않고, 일반적으로 대기 시간이 상당히 길다.

## Shortest - Job - First( SJF ) - 최단 작업 우선
SJF는 실행 시간이 가장 짧은 프로세스를 다음에 실행하도록 선택해야하는 스케줄링 방법이다.
이 스케줄링 방법은 선점형 또는 비선점형일 수 있으며, 실행을 기다리는 다른 프로세스의 평균 대기 시간을 크게 줄인다.
#### SJF 방법의 특성
- 완료하는 시간 단위로 각 작업과 연결된다.
- SJF는 CPU가 가용한 경우 완료 시간이 가장 짧은 다음 프로세스 또는 작업을 먼저 실행한다.
- 먼저 실행해야 하는 더 짧은 작업을 제공하여 작업 출력을 개선한다. SJF로 실행하면 대부분 처리 시간이 더 짧은 특징이 있다.

## Shortest Remaining Time( SRT ) - 최단 남은 시간
SRT는 SJF의 선점형 스케줄링이라고도 한다.
이 방법에서 프로세스는 완료에 가장 가까운 작업에 할당되는 특징을 가지며, 새로운 준비 상태의 프로세스가 이전 프로세스의 완료를 보류하는 것을 방지한다.
#### SRT 방법의 특성
- SRT는 주로 짧은 작업이 우선되어야 하는 배치 환경에 적용된다.
- 이것은 필요한 CPU 시간을 알 수 없는 공유 시스템에서 구현하는 이상적인 방법이 아니다.
- 다음 CPU 버스트의 길이로 각 프로세스와 연결되게 되는데, 이러한 남은 시간 기반을 사용하여 가능한 최단 시간에 프로세스를 예약하는데 도움이 된다.

## Priority Scheduling - 우선순위
Priority Scheduling은 우선순위에 따라 프로세스를 스케줄링하는 방법으로, 이 방법에서 스케줄러는 우선순위에 따라 작업할 작업을 선택하게 된다.
#### Priority Scheduling 방법의 특성
- Priority Scheduling은 OS가 우선순위 할당을 포함하는데 도움이 된다.
- 제일 먼저 우선순위가 높은 작업이 수행되어야하고, 만약 우선순위가 같은 작업은 Round Robin 또는 FCFS 기반으로 수행된다.
- 메모리 요구 사항, 시간 요구 사항 등에 따라 우선 순위를 결정할 수 있다.

## Round Robin Scheduling -  라운드 로빈
Round Robin Scheduling은 가장 오래되고 간단한 스케줄링 방법이다.
주로 멀티태스킹 스케줄링 알고리즘에 사용되며, 프로세스의 오류 없는 실행에 도움이 된다.
#### Round Robin Scheduling 방법의 특성
- 라운드 로빈은 시계 구동 방식의 하이브리드 모델이다.
- 타임 슬라이스는 처리할 특정 작업에 할당되는 최소값이어야하며, 프로세스마다 다를 수 있다.
- 특정 시간 내에서 이벤트에 응답하는 실시간 시스템이다.

## MultiLevel Queue Scheduling - 다단계 대기열
MultiLevel Queue Scheduling에서는 프로세스 우선 순위, 메모리 크기 등과 같은 프로세스의 특정 속석을 기반으로 프로세스를 대기열에 할당한다.
그러나, 이것은 작업을 예약하기 위해 다른 유형의 알고리즘을 사용해야하기 때문에 독립적인 스케줄링 OS 알고리즘이 아니다.
#### MultiLevel Queue Scheduling 방법의 특성
- 몇 가지 특성이 있는 프로세스에 대해 여러 대기열을 유지해야한다.
- 모든 대기열에는 별도의 일정 알고리즘이 있을 수 있다.
- 각 대기열에 우선 순위가 부여된다.

## The Purpose of a Scheduling algorithm
- CPU는 효율성을 향상시키기 위해 스케줄링을 사용한다.
- 경쟁 프로세스 간에 리소스를 할당하는데 도움이 된다.
- CPU의 최대 활용은 다중 프로그래밍으로 얻을 수 있다.
