## Scheduler 정의
Process Scheduling 은 다중 프로그래밍 운영 체제의 필수적인 부분이다.
이러한 OS에서는 한 번에 둘 이상의 프로세스를 실행 가능 메모리에 로드할 수 있으며, 로드된 프로세스는 시간 다중화를 사용하여 CPU를 공유한다.

## Process Scheduler Queues
![](https://velog.velcdn.com/images/dymnam/post/e382559a-8396-4ab3-9846-6196195c7fff/image.png)
프로세서 안에는 수많은 Process/Thread가 있고, 이를 처리할 CPU Core/Thread는 한정되어 있어서 우리는 Process/Thread를 어떻게 처리할지 Scheduling 해야한다.
이를 위해서 3가지의 종류의 Queue가 존재한다.

- job Queue :
시스템의 모든 프로세스를 유지한다.

- Ready Queue :
주 메모리에 있는 모든 프로세스 집합을 실행 준비 상태로 유지한다. 또한, 새 프로세스는 항상 이 Queue에 들어간다.

- Device Queue :
I/O 장치를 사용할 수 없어 차단된 프로세스가 이 Queue를 구성하게 된다.

- 프로세스의 상태 :
ready -> running -> waiting -> ready

## Scheduler
Scheduler는 다양한 방식으로 Process Scheduler를 처리하는 특수 시스템 소프트웨어이다.
Scheduler의 주요한 일은 시스템에 제출할 작업을 선택하고 실행할 프로세스를 결정하는 것이다. Scheduler의 유형은 다음과 같이 3가지이다.
- Long-term Scheduler - 장기 스케줄러
- Short-term Scheduler - 단기 스케줄러
- Midium-term Scheduler - 중기 스케줄러

![](https://velog.velcdn.com/images/dymnam/post/c4ac63c8-57b4-4a62-9b95-f9ca67a77420/image.png)


## Long - term Scheduler
메모리는 한정적이기에 많은 프로세스들이 많은 프로세스가 한번에 적재될 수 없다.
만약, 많은 프로세스가 한번에 올라오게 된다면 대용량 메모리( 디스크 등 )에 임시로 저장하게 되는데, 이 저장소의 pool에 저장되어있는 프로세스 중 어떤 프로세스에 메모리를 할당하여 Ready Queue 로 보낼지 결정하는 역할을 한다.
<span style=”color:indianred”>다중 프로그래밍의 정도 안정적이려면 프로세스의 생성의 평균 비율이 시스템을 종료하는 프로세스의 평균 이탈 비율과 같아야 한다.</span>

## Short - term Scheduler
CPU Scheduler 라고도 말하는데, 주요 목표로 시스템 성능을 높이는 것을 선택하고 있는만큼 CPU와 메모리 사이의 Scheduling을 담당한다.
또, Ready Queue 에 존재하는 프로세스 중 어떤 프로세스를 running 시킬 것인지 결정하고,  프로세스에 CPU를 할당하는데 그것은 dispatch라고 하는 Short-term Scheduler이다.

## Medium - term Scheduler
Medium-term Scheduler는 메모리에서 프로세서를 제거하는 역할을 하며, 여유공간을 만들어내기 위해 프로세스를 통째로 메모리에서 디스크로 쫓아내는( Swapping ) 역할도 한다.
시스템에서 메모리에 너무 많은 프로그램이 동시에 올라가는 것을 조절해주는 역할도 이 친구가 한다.

## Comparison among Scheduler
**Long - term Scheduler**
- 속도는 단기 스케줄러보다 낮다.
- 다중 프로그래밍의 정도를 제어한다.
- 시분할 시스템에서 거의 없거나 미미하다.
- pool에서 프로세스를 선택하고 실행을 위해 메모리에 로드한다.

**Short - term Scheduler**
- 속도는 가장 빠르다
- 다중 프로그래밍 정도에 대한 제어력이 부족하다.
- 시분할 시스템에서도 최소화
- 실행할 준비가 된 프로세스를 선택한다.

**Medium - term Scheduler**
- 속도는 중기와 단기 사이이다.
- 다중 프로그래밍의 정도를 감소시킨다.
- 시간 공유 시스템의 일부이다.
- 프로세스를 메모리에 다시 도입하고 실행을 계속할 수 있다.
