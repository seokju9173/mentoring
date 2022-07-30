# 운영체제

- 컴퓨터 하드웨어를 효율적으로 관리해서 사용자 또는 응용 프로그램에게 서비스를 제공하는 것.

## 운영체제의 역할

- User Interface ( 편리성 )
    - CUI ( Character user interface ) - 과거
    - GUI ( Graphical User interface ) - 현재
    - EUCI ( End-User Comfortable interface ) - 특정 목적을 위해 제작
- Resource management - 리소스 관리 ( 효율성 )
    - HW resource ( processor, memory, I/O devices, Etc. )
    - SW resource ( file, application, message, signal, Etc. )
- Process and Thread management - 프로그램을 실행하는 주체와 가벼운 프로세스를 쓰레드라고 함.
- System management ( 시스템 보호 )

## 운영체제의 구분

1. 동시 사용자 수
    - Single-user system - 단일 사용자
        - 한 명의 사용자만 시스템 사용 가능
            - 한 명의 사용자가 모든 시스템 자원 독점
            - 자원관리 및 시스템 보호 방식이 간단 함
        - 개인용 장비 ( PC, mobile ) 등에 사용
            - Window 7/10, android, MS-DOS 등
    - Multi-user system - 다중 사용자
        - 동시에 여러 사용자들이 시스템을 사용
            - 각종 시스템 자원( 파일 등 )들에 대한 소유 권한 관리 필요
            - 기본적으로 Multi-tasking 기능 필요 - 여러가지 프로그램을 가동하기 위함.
            - os의 기능 및 구조가 복잡
        - 서버, 클러스터 장비 등에 사용
            - Unix, Linux, Windows server 등

1. 동시 실행 프로세스 수
    - 단일작업 ( Single-tasking system )
        - 시스템 내에 하나의 작업( 프로세스 ) 만 존재
        - 운영체제의 구조가 간단
    - 다중작업 ( Multi-tasking system )
        - 동시에 여러 작업( 프로세스 )의 수행 가능
        - 운영체제의 기능 및 구조가 복잡

1. 작업 수행 방식
    - Batch processing system - 일괄 처리 시스템
        1. 모든 시스템을 중앙에서 관리 및 운영
        2. 사용자의 요청 작업을 일정시간 모아두었다가 한번에 처리
        3. 시스템 지향적 ( 처리 효율 향상, 긴 응답시간 )
    - Time-sharing system - 시분할 시스템
        1. 여러 사용자가 자원을 동시에 사용 ( os가 파일 시스템 및 가상 메모리 관리 )
        2. 사용자 지향적( 대화형 시스템, 단말기 사용 )
    - Distributed processing system - 분산 처리 시스템
        - 네트워크를 기반으로 구축된 병렬처리 시스템
    - Real-time system - 실시간 시스템
        - 작업 처리에 제한 시간을 갖는 시스템
            1. 제한 시간 내에 서비스를 제공하는 것이 자원 활용 효율보다 중요함.
    
    ## 운영체제의 구조
    
    ### 커널( Kernel )
    
    - OS의 핵심부분으로 가장 빈번하게 사용되는 기능들을 담당한다. ex) 시스템 관리( processeor, memory, Etc. ) 등
    - 핵( neucleus ), 관리자( supervisor ) 프로그램 등으로 불리기도 한다.
    
    ### 유틸리티 ( Utility )
    
    - 비상주 프로그램
    - UI등 서비스 프로그램
    - 자주 사용하지 않고, 가끔씩 사용하며 필요시에 메모리에 올려서 실행하게 된다.

## 운영체제의 기능

## Process Management

### 프로세스

- 커널에 등록된 실행 단위로, 사용자 요청/프로그램의 수행 주체( entity )이다.

### OS의 프로세스 관리 기능

- 생성/삭제, 상태관리
- 자원 할당
- 프로세스 간 통신 및 동기화
- 교착상태( deadlock ) 해결

### 프로세스 정보 관리

- PCB( Process Control Bloc )

## Processor Management

### 중앙 처리 장치 ( CPU )

- 프로그램을 실행하는 핵심 자원

### 프로세스 스케줄링 ( Scheduling )

- 시스템 내의 프로세스 처리 순서를 결정

### 프로세서 할당 관리

- 프로세스들에 대한 프로세서를 할당하는데, 한 번에 하나의 프로세스만 사용 가능

## Memory Management

### 주기억장치

- 작업을 위한 프로그램 및 데이터를 올려 놓는 공간

### Multi-user, Multi-tasking 시스템

- 프로세스에 대한 메모리 할당 및 회수
- 메모리 여유 공간 관리
- 각 프로세스의 할당 메모리 영역 접근 보호

### 메모리 할당 방법 ( scheme )

- 전체 적재 : 구현이 간단하나 공간이 제한적이다.
- 일부 적재 : 프로그램 및 데이터의 일부만 적재하며, 메모리의 효율적 활용이 가능하나 보조기억 장치 접근이 필요하다.

## File Management

- 파일 : 논리적 데이터 저장 단위

### 사용자 및 시스템의 파일 관리

### 디렉토리 구조 지원

### 파일 관리 기능

- 파일 및 디렉토리 생성/삭제
- 파일 접근 및 조작
- 파일을 물리적 저장 관으로 사상( mapping )
- 백업 등

## I/O Management

### 입출력( I/O ) 과정

- 입출력은 키에서 바로 실행중인 프로세스에 입력되는 것이 아니라, 운영체제를 거쳐서 입력되게 된다.
