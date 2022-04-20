# Redis란 무엇일까?

Redis란 Remode Dictionary Server의 약자로, Key-Value 구조를 가진 비정형 데이터를 저장하기 위한 오픈 소스 기반의 바관계형 DBMS이다.

## 특징

이는 디스크에 데이터를 저장하지 않고, 메모리에 데이터를 저장하게 된다.
또, 데이터의 안전한 보관과 백업을 위해 다른 서버의 메모리에 실시간으로 사본을 남길 수 있고, 디스크에 저장하는 방법을 제공한다.

성능은 초당 100,000만회 명령을 실행한다. 이는 서버(CPU)에 따라 다르지만 일반적으로 50,000회 ~ 250,000회 실행한다.

기본적으로 Key-Value저장 방식 이지만, Lists, Sets, Sorted Sets, Hashes, Streams 같은 다양한 저장 방식을 제공한다.

Redis Server Instance는 1개의 프로세스만으로 수행이 되며, 평상 시 CPU는 1core만 사용한다. 가끔 AOF, RDB, Full Resync 시에 추가 1개 Core를 더 사용한다. 그러므로 서버 머신 또는 VM 하나에 여러 개의 Redis Server를 사용할 수 있다.

## 사용 분야

Message Queue, Shared Memory 의 용도로 사용을 한다.
특히, Remote Dictionary로서 RDBMS의 캐시 솔루션으로 사용 용도가 굉장히 높다고 할 수 있는데, RDBMS에서 SELECT 쿼리문을 날려 특정 데이터들을 FETCH했을 때, RDBMS의 구조상 DISK에서 데이터를 꺼내오는 데 Memory에서 읽어들이는 것 보다 천배 가량 느리기 때문.

### 캐싱 전략

캐시를 옆에 두고, 필요할 때만 데이터를 캐시에 로드하는 전략을 가질 수 있다.

1. 데이터 가져오는 요청
2. Redis에 먼저 요청 → 있으면 반환
3. 없으면 데이터베이스에 데이터 요청
4. 이 데이터를 Redis에 저장

### Write-through

Write-throuht구조는 데이터를 데이터베이스에 작성할 때마다 캐시에 데이터를 추가하거나 업데이트한다. 

하지만, 이로 인해 캐시데이터는 항상 최신 상태로 유지할 수 있지만, 쓰지않는 데이터도 캐시에 저장이 되기 때문에 안그래도 적은 메모리의 리소스 낭비가 일어날 수 있다.
또, 쓰기 지연 시간이 증가 된다.

이를 해결하기 위해서는 TTL(Time-to-live)을 꼭 사용하여 사용되지 않는 데이터를 삭제해야 한다.

# 정리

Redis는 Server와 DBMS 사이에 있는 에매한 위치라고 생각이 들었으나, 이렇게 정리를 하다보니 Redis의 위치와 성능을 이용하면 더욱 유연한 구현을 할 수 있다는것을 알게 되었다.

Cache 서버로 사용을 하거나, insert와 update가 자주 일어나 이를 바로 DBMS로 보내면 DBMS의 성능이 저하될 수 있는 상황을 예방해 중간다리 역할을 해줄 수 있다는 장점을 가지고 있어 추후에는 내부 구조와 실행방식에 대해 더 공부하여 직접 적용해보면 좋을것 같다.

---

### 출처

[http://redisgate.kr/redis/introduction/redis_intro.php](http://redisgate.kr/redis/introduction/redis_intro.php)
