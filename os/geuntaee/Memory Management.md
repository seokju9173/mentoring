## Memory Management( 메모리 관리 )
Memory Management는 기본 메모리를 처리하거나 관리하고 실행 중에 주 메모리와 디스크 간에 프로세스를 앞뒤로 이동하는 운영체제의 기능이다.
각각의 프로세스는 독립된 메모리 공간을 갖고, 운영체제 혹은 다른 프로세스의 메모리 공간에 접근할 수 없는 제한이 걸려있다.
예외로, 운영체제는 운영체제 메모리 영역과 사용자 메모리 영역의 접근에 제약을 받지 않는다.

## Process Address Space( 프로세스 주소 공간 )
Process Address Space는 프로세스가 코드에 참조하는 **논리적 주소 집합**이다.
ex - 32비트를 사용 중, 이론적인 크기가 2GB인 경우 2^31개의 숫자를 사용 가능하다.

OS는 프로그램에 메모리를 할당할 때 논리적 주소를 물리적 주소로 매핑하는 작업을 처리한다. 메모리가 할당되기 전과 후에 프로그램에서 사용되는 세 가지 유형의 주소가 있다.![](https://velog.velcdn.com/images/dymnam/post/4afc39ab-bcd4-44db-b3fd-6765b9278ae9/image.png)
프로그램에 의해 생성된 모든 논리 주소 집합을 **논리 주소 공간**이라고 한다. 이러한 논리 주소에 해당하는 모든 물리 주소 집합을 **물리 주소 공간**이라고 한다.

## Swapping( 스와핑 )
Swapping은 중기 스케줄러에서 나왔던 것으로 프로세스가 주 메모리에서 임시로 이동하여 보조 저장소( 디스크 )로 이동하고 해당 메모리를 다른 프로세스에서 사용할 수 있도록 하는 매커니즘이다.
성능은 일반적으로 Swapping 프로세스의 영향을 받지만 여러 개의 큰 프로세스를 병렬로 실행하는 데 도움이 되므로 Swapping은 <span style=”color:indianred”>메모리 압축 기술</span>로도 알려져 있다.
![](https://velog.velcdn.com/images/dymnam/post/fd20d216-93cb-44f1-a855-25ee3471bcce/image.png)
- Swap In : 보조 저장소( 디스크 )로 이동하는 것.
- Swap Out : 주 메모리( RAM )로 이동하는 것.

Swap에는 큰 디스크 전송시간이 필요하기 때문에 현재에는 메모리 공간이 부족할 때, Swapping이 시작된다.

## Memory Alloctaion( 메모리 할당 )
주 메모리( RAM )에는 두 개의 분할이 있다.
- Low Memory
운영체제가 이 메모리에 존재한다.
- High Memory
사용자 프로세스는 높은 메모리에 보관된다.

OS는 다음과 같은 메모리 할당 매커니즘을 사용한다.
![](https://velog.velcdn.com/images/dymnam/post/2e9c3e12-9354-438d-912a-3c3d10fc3491/image.png)

## Fragmentation( 단편화 )
프로세스가 로드되고 메모리에서 제거되면 여유 메모리 공간이 작은 조각으로 나뉘게 된다.
프로세스의 크기가 작고 메모리 블록이 사용되지 않은 상태로 남아 있기 때문에 프로세스를 메모리 블록에 할당할 수 없는 경우가 있는데, 이것을 Fragmentation( 단편화 )라고 한다.

Fragmentation에는 두 가지 유형이 있다.
- 외부 단편화
총 공간을 계산했을 때, 프로세스가 들어갈 수 있는 메모리가 있음에도 불구하고 공간들이 연속적이지 않아 사용할 수 없는 경우를 말한다.

- 내부 단편화
프로세스가 사용하는 메모리 공간보다 분할된 공간이 더 커서 메모리가 남는 경우를 말한다.

다음 그림은 단편화가 메모리 낭비를 유발할 수 있는 방법과 압축 기술을 사용하여 단편화된 메모리에서 더 많은 여유 메모리를 생성할 수 있는 방법을 보여준다.
![](https://velog.velcdn.com/images/dymnam/post/5c951621-57f3-4fc8-ad83-6b727cb70be7/image.png)
압축 메모리를 통해 외부 단편화를 줄여 사용 가능한 모든 메모리를 하나의 큰 블록에 함께 배치할 수 있다. 하지만, 압축이 가능하도록 하려면 재배치가 동적이어야 한다.
내부 단편화는 가장 작은 파티션을 효율적으로 할당하지만 프로세스에 충분히 큰 파티션을 줄일 수 있다.

## Paging( 페이징 )
Paging은 외부 단편화의 압축 작업의 비효율성을 해결하기 위한 방법으로 메모리는 Frame( 프레임 ), 프로세스는 Page( 페이지 )라 불리는 고정 크기의 블록( Block )으로 분리된다.

한 프로세스가 사용하는 공간은 여러 Page로 나뉘어 관리되고, 각각의 Page는 순서와 관계 없이 메모리의 Frame에 Mapping 되어 저장된다.
![](https://velog.velcdn.com/images/dymnam/post/c9eba79f-a4dd-4427-8ded-0ff568737eac/image.png)
프로세스가 순서대로 메모리에 저장되지 않기 때문에 프로세스를 실행하기 위해서는 Page가 어느 Frame에 들어있는지를 알아야 한다.
이에 대한 정보가 Page Table에 저장되어 있고, 이를 사용하여 논리적 주소를 물리적 주소로 변환한다.

![](https://velog.velcdn.com/images/dymnam/post/4e1521f8-1f58-44e8-ba9e-7636dceafdd1/image.png)
시스템이 임의의 페이지에 프레임을 할당하면 이 **논리 주소를 물리적 주소로 변환**하고 프로그램 실행 전반에 걸쳐 사용할 페이지 테이블에 항목을 생성한다.

#### 페이징의 순서
1. 프로그램 실행 시 OS는 RAM 가상 메모리의 페이지와 실제 메모리의 페이지를 연결시켜주기 위한 맵핑 테이블인 Page Table을 생성하고, 실행에 필요할 것 같은 페이지만 우선 프레임에 맵핑한다.

2. RAM에서의 주소값( 물리 주소 )과 일치하는 VAS( 가상 주소 공간 )에서의 주소값( 논리 주소 )을 Page Table에 저장한다.

3. MMU( Memory Management Unit )가 VAS 주소값을 RAM 주소값으로 변환하여 MAR( Memory Address Register )로 전달한다.

#### 페이징의 장점
- 페이징은 외부 단편화를 줄여준다.
- 구현하기 쉽고 효율적인 메모리 관리 기술이다.
- 페이지와 프레임의 크기가 동일하여 교체가 매우 쉽다.

#### 페이징의 단점
- 여전히 내부 단편화를 겪고 있다.
- 페이지 테이블에는 추가 메모리 공간이 필요하여 주 메모리( RAM )이 작은 시스템에는 적합하지 않다.

## Segmentation( 분할 )
Segmentation은 의미 단위로 하나의 프로세스를 나누는 것을 말한다.
작게는 프로그램을 구성하는 함수 하나하나를, 크게는 프로그램 전체를 하나의 Segment로 정의할 수 있다.

일반적으로 Code, Data, Stack 부분이 하나의 세그먼트로 정의된다.
운영 체제는 모든 프로세스에 대한 Segment Map Table과 Segment Num, 크기 및 주 메모리의 해당 메모리 위치와 함께 사용 가능한 메모리 블록 목록을 유지 관리한다.
![](https://velog.velcdn.com/images/dymnam/post/bc3914a7-8632-4f6d-ab77-0b0e12bd7749/image.png)

