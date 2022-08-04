목차

- FIFO (First In First Out)
- Optimal Page Replacement
- LRU (Least Recently Used)
- LFU (Least Frequently Used)
- MFU (Most Frequently Used)

## Page Replacement
Demand Paging에서 특정 프로세스의 Page를 메모리로 Swap-in하려는데 비어있는 Frame이 없을 수 있다. 이런 경우 Virtual Memory 시스템에서 당장 사용하지 않는 Frame 중 일부를 다시 Swap-out하는데, 이것을 Page Replacemnet라고 한다.

Page Replacement의 순서는 다음과 같다.

![](https://velog.velcdn.com/images/dymnam/post/6e127a57-1a8c-4b3c-baf2-2e2edb051914/image.png)

1. 비어있는 Frame이 없다면, 빈 Frame을 만들기 위해서 현재 올라와 있는 Frame 중 Swap-out 시킬 Frame을 선택한다.

2. Frame의 내용을 보조기억장치에 쓰고 해당 프로세스의 Page table 내용을 업데이트 후, 커널도 Frame table과 PCB의 내용을 업데이트한다.

3. Swap-in 하려는 페이지를 I/O 작업을 거쳐 빈 Frame에 쓴다. 그리고 다시 page table과 Frame table의 정보를 업데이트한다.

4. CPU 제어권을 다시 프로세스에게 넘겨준다.

## FIFO (First In First Out)
![](https://velog.velcdn.com/images/dymnam/post/3122eb3b-6275-4ddd-b7e4-dd8cd5f032e3/image.png)

FIFO 알고리즘은 페이지 교체 알고리즘 중 제일 단순하다.
현재 메모리에 있는 모든 페이지의 큐를 유지하며, 메모리에서 가장 오래 된 페이지는 큐의 앞쪽에 있고 가장 최근 페이지는 큐의 뒤쪽에 있다.

Fage Fault가 발생할 때마다 운영 체제는 새로 요청된 페이지로 교체될 페이지를 알기 위해 대기열의 처음 시작점을 확인한다. 또한, 새로 요청된 페이지를 뒤쪽에 추고하고 대기열의 앞쪽에서 가장 오래된 페이지를 제거한다.

#### 장점
- 이해하고 구현하기 쉽다.
- 많은 오버헤드가 발생하지 않는다.

#### 단점
- 성능이 저하된다.
- 마지막으로 사용한 시간의 빈도를 사용하지 않고 가장 오래된 페이지를 교체한다.
- Belady's Anomaly 현상이 발생할 수 있다.

#### 참고
![](https://velog.velcdn.com/images/dymnam/post/553bf9fc-149c-40dc-8468-663f523bedf2/image.png)

## Optimal Page Replacement( 최적 페이지 교체 )
![](https://velog.velcdn.com/images/dymnam/post/4f8bb1d3-eb77-4150-9afd-fe59ee8fe709/image.png)

Optimal Page Replacement는 가장 적은 수의 Page Fault를 발생시키는 알고리즘이다.
여기에서 Page는 앞으로 가장 오랫동안 사용되지 않을 페이지로 대체된다.

#### 장점
- 우수한 효율성
- 복잡성 감소
- 간단한 데이터 구조를 사용하여 구현 가능하다.
- 다른 알고리즘의 벤치마크로 사용한다.

#### 단점
- 많은 시간이 소요된다.
- 오류 처리가 어렵다.
- 매번 가능하지 않은 프로그램에 대한 향후 인식이 필요하다.


## LRU( Least Recently Used )
![](https://velog.velcdn.com/images/dymnam/post/40430f8b-5249-45f6-afa7-ab251dd57a72/image.png)

일정 기간 동안 페이지 사용을 추적하여 짧은 시간동안 동일한 메모리 위치 집합에 반복적으로 액세스하는 경향이 있다는 참조의 지역성 원칙을 기반으로 작동한다.
이 알고리즘에서는 Page Fault가 발생하면 가장 오랫동안 사용되지 않은 페이지를 새로 요청한 페이지로 교체한다.

#### 장점
- 최적 알고리즘과 비슷한 효과를 낼 수 있다.
- 성능이 좋은 편이다.
- 많은 운영체제가 채택하는 알고리즘이다.

#### 단점
- 구현하려면 추가 데이터 구조가 필요하다.
- 높은 하드웨어 지원이 필요하다.

## LFU( Least Frequently Used ) 
![](https://velog.velcdn.com/images/dymnam/post/49f7783f-aa97-4b44-8f20-fdf46803733f/image.png)

물리적 메모리 내에 존재하는 페이지 중 과거에 참조 횟수가 가장 적은 페이지를 교체 시킬 페이지로 결정하는 알고리즘이다. LFU 알고리즘은 크게 2가지이다.

Incache-LFU: 메모리에 적재될 때부터 페이지의 횟수를 카운트 하는 방식
Perfect-LFU: 메모리 적재 여부와 상관 없이 페이지의 과거 총 참조 횟수를 카운트 하는 방식

Perfect-LFU의 경우 정확하게 참조 횟수를 참조할 수 있지만 시간에 따른 참조의 변화를 반영하지 못하고, 구현이 복잡하다는 단점이 있다.

- LFU 알고리즘은 참조횟수가 가장 적은 페이지를 교체하는 알고리즘이다.
- 교체 대상이 여러 개라면 가장 오랫동안 사용하지 않은 페이지를 교체한다.
- 하지만, 최신 흐름을 잘 반영하지 못할 수 있다.

## MFU (Most Frequently Used)
![](https://velog.velcdn.com/images/dymnam/post/a988c73b-564d-4115-9013-e43e38100e49/image.png)

LFU 알고리즘과 반대로, 참조 횟수가 가장 많은 페이지를 교체하는 알고리즘이다.

- 구현에 상당한 비용이 든다.
- 최적 페이지 교체( Optimal )만큼 제대로 구현하지 못하기 때문에 잘 사용하지 않는다.
