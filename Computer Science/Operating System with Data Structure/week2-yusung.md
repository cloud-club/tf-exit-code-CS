# Week 02 - OS Memory / Concurrency + Core Data Structure

---

## 제출 기준

- 필수 답변: COMMON-041 ~ COMMON-060
- 선택 답변: COMMON-061 ~ COMMON-080

---

## 필수 질문

## [COMMON-041] 프로세스 주소 공간이 어떻게 구성되는지 설명해 주세요.

답변:

- 프로세스 주소 공간 (Process Address Space)
  - 하나의 프로세스가 사용할 수 있는 가상 메모리 전체 범위
  - OS가 프로세스마다 독립적으로 할당하여, 다른 프로세스와 격리
  - 각 프로세스는 Code, Data, Heap, Stack 영역을 독립적으로 보유
- 주요 구성 영역 (낮은 주소 → 높은 주소)
  - Code (Text): 실행 가능한 기계어 명령어 저장, 읽기 전용
  - Data: 전역 변수, static 변수 등 프로그램 수명 동안 유지되는 데이터
  - Heap: 동적 할당(malloc, new) 영역, 런타임에 크기가 변함
  - Stack: 함수 호출, 지역 변수, 매개변수, 반환 주소 저장
- 가상 주소와 물리 주소
  - 프로세스는 가상 주소로 접근하고, MMU가 페이지 테이블을 통해 물리 주소로 변환
  - 각 프로세스는 동일한 가상 주소를 사용할 수 있으나, 실제 물리 메모리는 다름
- 스레드와의 관계
  - 같은 프로세스 내 스레드들은 Code, Data, Heap을 공유
  - 각 스레드는 독립적인 Stack을 가짐

참고 자료:

---

## [COMMON-042] Code, Data, Heap, Stack 영역의 역할을 설명해 주세요.

답변:

- Code (Text) 영역
  - 컴파일된 실행 코드(기계어)가 저장되는 공간
  - 읽기 전용(Read-Only) → 실행 중 코드 변조 방지
  - 여러 프로세스가 같은 프로그램을 실행할 때 Code 영역 공유 가능
- Data 영역
  - 전역 변수(global), static 변수 등이 저장
  - 프로그램 시작 시 할당되고, 종료 시까지 유지
  - 초기화된 데이터(.data)와 초기화되지 않은 데이터(.bss)로 구분
- Heap 영역
  - 프로그래머가 직접 동적 할당/해제하는 메모리 공간
  - malloc, calloc, new 등으로 런타임에 크기 조절
  - OS 또는 메모리 할당자(allocator)가 관리, 해제하지 않으면 Memory Leak 발생
- Stack 영역
  - 함수 호출 시 지역 변수, 매개변수, 반환 주소, 프레임 포인터 저장
  - LIFO(후입선출) 구조, 함수 호출·종료에 따라 자동 할당·해제
  - 스레드마다 독립적인 Stack 보유

참고 자료:

---

## [COMMON-043] Stack과 Heap의 차이에 대해 설명해 주세요.

답변:

- Stack
  - 함수 호출 시 자동으로 할당·해제 (컴파일러/OS가 관리)
  - LIFO 구조, SP(Stack Pointer)로 크기 추적
  - 크기가 컴파일 시점 또는 스레드 생성 시 정해짐 → 제한적
  - 접근 속도가 빠름 (캐시 친화적, 연속된 메모리)
  - 지역 변수, 함수 매개변수, 반환 주소 저장
- Heap
  - 프로그래머가 명시적으로 할당·해제 (malloc/free, new/delete)
  - 크기와 수명이 런타임에 결정 → 유연하지만 관리 부담
  - 물리적으로 연속적이지 않을 수 있음
  - 할당·해제 비용이 Stack보다 큼
  - 동적 자료구조, 가변 크기 데이터에 적합
- 차이
  - 관리 주체: Stack은 OS/컴파일러, Heap은 프로그래머 + 할당자
  - 수명: Stack은 함수 스코프, Heap은 명시적 해제 전까지
  - 크기: Stack은 고정·제한, Heap은 상대적으로 큼
  - 오류: Stack Overflow vs Memory Leak

참고 자료:

---

## [COMMON-044] Stack Overflow가 발생하는 이유를 설명해 주세요.

답변:

- Stack Overflow (스택 오버플로)
  - Stack 영역의 할당 가능 공간을 초과하여 더 이상 데이터를 쌓을 수 없을 때 발생
  - OS 또는 런타임이 Stack 크기 한계를 감지하고 예외/시그널(SIGSEGV) 발생
- 주요 발생 원인
  - 무한 재귀 호출: 종료 조건 없이 함수가 자기 자신을 반복 호출 → Stack Frame이 계속 쌓임
  - 과도하게 큰 지역 변수: Stack에 큰 배열·구조체를 선언 → 한 번에 많은 공간 소비
  - 깊은 재귀 또는 깊은 함수 호출 체인: 정상 재귀이라도 깊이가 Stack 한계를 넘으면 발생
  - Stack 크기 설정 부족: 스레드 생성 시 Stack 크기를 너무 작게 지정
- 결과
  - 프로그램 비정상 종료 (Segmentation Fault 등)
  - 인접 메모리 영역 침범 가능 → 보안 취약점(버퍼 오버플로)으로 이어질 수 있음

참고 자료:

---

## [COMMON-045] Memory Leak이 무엇이고, 어떤 상황에서 발생할 수 있는지 설명해 주세요.

답변:

- Memory Leak (메모리 누수)
  - 동적으로 할당한 Heap 메모리를 해제하지 않아, 사용하지 않는 메모리가 계속 점유되는 현상
  - 프로세스가 살아 있는 동안 사용 가능한 메모리가 점점 줄어듦
- 발생 상황
  - malloc/new로 할당 후 free/delete를 호출하지 않음
  - 포인터를 덮어쓰거나 잃어버려 해제할 주소를 잃음
  - 예외 발생 시 해제 코드가 실행되지 않음 (RAII, try-finally 미사용)
  - 순환 참조: GC 언어에서도 참조가 남아 있으면 수집되지 않음
  - 종료되지 않는 컬렉션에 객체를 계속 추가만 하고 제거하지 않음
- 영향
  - 장시간 실행 서버, 임베디드 시스템에서 OOM(Out of Memory) 유발
  - 성능 저하 (스왑 증가, GC 부담 증가)
- Stack과의 차이
  - Stack은 함수 종료 시 자동 해제 → Leak 개념 없음
  - Heap은 명시적 해제 필요 → Leak 발생 가능

참고 자료:

---

## [COMMON-046] 가상 메모리가 무엇이고, 왜 필요한지 설명해 주세요.

답변:

- 가상 메모리 (Virtual Memory)
  - 각 프로세스에게 실제 물리 메모리보다 큰 독립적인 주소 공간을 제공하는 기법
  - 프로세스는 가상 주소(Virtual Address)로 접근하고, MMU + OS가 물리 주소(Physical Address)로 변환
  - 필요한 페이지만 물리 메모리에 적재하고, 나머지는 디스크(스왑)에 보관
- 필요한 이유
  - 메모리 보호: 프로세스 간 주소 공간 격리 → 다른 프로세스 메모리 접근 차단
  - 메모리 효율: 물리 RAM보다 큰 프로그램 실행 가능 (Demand Paging)
  - 단순한 프로그래밍: 연속된 큰 메모리 블록을 가정하고 코딩 가능
  - 공유: 같은 Code 영역, 공유 라이브러리를 여러 프로세스가 공유
  - 스왑(Swapping): 사용하지 않는 페이지를 디스크로 내려 RAM 확보

참고 자료:

---

## [COMMON-047] 페이징이 무엇이고, 페이지와 프레임의 차이를 설명해 주세요.

답변:

- 페이징 (Paging)
  - 가상 메모리와 물리 메모리를 고정 크기 블록 단위로 나누어 관리하는 메모리 기법
  - 외부 단편화를 줄이고, 비연속적인 물리 프레임에 가상 페이지를 매핑 가능
  - 페이지 테이블(Page Table)로 가상 페이지 → 물리 프레임 변환
- 페이지 (Page)
  - 가상 주소 공간을 나눈 고정 크기 블록 (논리적 단위)
  - 프로세스 관점에서의 메모리 조각
  - 일반적으로 4KB (시스템마다 다름)
- 프레임 (Frame)
  - 물리 메모리(RAM)를 나눈 고정 크기 블록 (물리적 단위)
  - 페이지와 동일한 크기
  - OS가 프레임을 할당·회수하며 관리
- 차이
  - 페이지: 가상(논리) 주소 공간의 단위
  - 프레임: 물리 주소 공간의 단위
  - 크기는 같지만, 페이지는 프로세스별, 프레임은 시스템 전체에서 관리

참고 자료:

---

## [COMMON-048] Page Fault가 발생했을 때 처리 과정을 설명해 주세요.

답변:

- Page Fault (페이지 폴트)
  - CPU가 가상 주소에 접근했으나, 해당 페이지가 물리 메모리에 없거나 접근 권한이 없을 때 발생하는 예외
  - 소프트웨어 인터럽트(Trap)로 OS 커널에 전달
- 처리 과정
  - 1. CPU가 페이지 테이블 조회 → Valid/Present 비트 확인, 접근 권한 검사
  - 2. Page Fault 발생 → 현재 CPU 상태(PC, 레지스터) 저장, 커널 모드로 전환
  - 3. OS가 Page Fault 원인 판별
    - 페이지가 디스크(스왑/실행 파일)에 있음 → Demand Paging으로 디스크에서 RAM으로 적재
    - 잘못된 주소 접근 → Segmentation Fault, 프로세스 종료
    - Copy-on-Write 등 특수 상황 → 해당 정책에 따라 처리
  - 4. 물리 메모리에 빈 프레임이 없으면 페이지 교체 알고리즘(LRU 등)으로 Victim 페이지 선정·스왑 아웃
  - 5. 디스크 I/O로 페이지를 프레임에 로드, 페이지 테이블 갱신
  - 6. TLB 갱신(또는 flush) 후, fault를 발생시킨 명령어부터 재실행
- 성능 영향
  - Major Fault(디스크 I/O 필요): 수 ms ~ 수십 ms 지연
  - Minor Fault(TLB miss 등): 상대적으로 가벼움

참고 자료:

---

## [COMMON-049] TLB가 무엇이고, TLB를 사용하면 왜 주소 변환이 빨라지는지 설명해 주세요.

답변:

- TLB (Translation Lookaside Buffer, 변환 탐색 버퍼)
  - MMU 내부의 고속 캐시, 최근 사용한 가상 페이지 → 물리 프레임 매핑 정보 저장
  - 페이지 테이블(메모리에 위치) 조회 없이 주소 변환을 가속
- 주소 변환 과정
  - CPU가 가상 주소 접근 → TLB에서 해당 항목 검색 (TLB Hit)
  - TLB Hit: 즉시 물리 주소 획득 → 메모리 접근 (1~2 사이클 수준)
  - TLB Miss: 메모리의 페이지 테이블을 조회(PTE 읽기) → TLB에 항목 추가 → 물리 주소 획득
- 빨라지는 이유
  - 페이지 테이블은 메모리에 있어 조회 시 추가 메모리 접근 필요 (2번의 메모리 접근)
  - TLB는 CPU 칩 내부 캐시 → 메모리 접근 없이 변환 가능
  - 프로그램의 시간·공간 지역성으로 TLB Hit Rate가 높음 → 대부분의 접근이 빠르게 처리
- 컨텍스트 스위칭 시
  - 프로세스마다 페이지 테이블이 다름 → TLB flush 필요
  - 이전 프로세스의 변환 정보가 남아 있으면 잘못된 물리 주소로 접근할 수 있음

참고 자료:

---

## [COMMON-050] 내부 단편화와 외부 단편화의 차이에 대해 설명해 주세요.

답변:

- 내부 단편화 (Internal Fragmentation)
  - 할당된 메모리 블록 내부에서 실제 사용하지 않는 공간이 남는 현상
  - 고정 크기 단위(페이지, 슬롯)로 할당할 때, 요청 크기보다 큰 블록을 할당하면 발생
  - 예: 4KB 페이지에 1KB 데이터만 사용 → 3KB 내부 단편화
  - 할당된 영역 안에 갇혀 있어 다른 프로세스가 사용 불가
- 외부 단편화 (External Fragmentation)
  - 전체 여유 메모리는 충분하지만, 연속된 빈 공간이 없어 할당에 실패하는 현상
  - 가변 크기 할당(세그멘테이션, 연속 할당)에서 주로 발생
  - 메모리 블록 사이사이에 작은 빈틈(hole)이 분산
  - Compaction(압축)으로 해결 가능하나 비용 큼
- 차이
  - 위치: 내부 = 할당된 블록 안, 외부 = 블록 사이
  - 주요 원인: 내부 = 고정 크기 할당, 외부 = 가변 크기 할당
  - 페이징: 외부 단편화 거의 없음, 내부 단편화는 존재

참고 자료:

---

## [COMMON-051] 동시성과 병렬성의 차이에 대해 설명해 주세요.

답변:

- 동시성 (Concurrency)
  - 여러 작업이 시간적으로 겹쳐 진행되는 것 (논리적 동시 실행)
  - 단일 CPU 코어에서도 Time Slicing + 컨텍스트 스위칭으로 구현 가능
  - 작업 간 전환이 빠르면 동시에 실행되는 것처럼 보임
  - 목적: 응답성, 자원 활용, 구조적 분리 (I/O 대기 중 다른 작업 처리)
- 병렬성 (Parallelism)
  - 여러 작업이 물리적으로 동시에 실행되는 것
  - 멀티코어 CPU, 여러 CPU에서 각각 다른 작업을 동시 처리
  - 실제 연산 시간 단축 (throughput 향상)
- 차이
  - 동시성: "동시에 처리하는 것처럼 보이는 것" (1코어 가능)
  - 병렬성: "실제로 동시에 실행" (2코어 이상 필요)
  - 동시성은 설계·구조 문제, 병렬성은 실행·하드웨어 문제
  - 동시성 프로그램이 병렬로 실행될 수도, 번갈아 실행될 수도 있음

참고 자료:

---

## [COMMON-052] Race Condition이 무엇이고, 어떤 상황에서 발생하는지 설명해 주세요.

답변:

- Race Condition (경쟁 상태)
  - 여러 스레드/프로세스가 공유 자원에 동시에 접근하고, 실행 순서(타이밍)에 따라 결과가 달라지는 상황
  - 동기화 없이 read-modify-write 같은 비원자적 연산을 수행할 때 발생
- 발생 상황
  - 공유 변수에 대한 동시 읽기·쓰기 (예: counter++)
  - 공유 자료구조(리스트, 맵)를 동시에 수정
  - Check-Then-Act 패턴: 조건 확인과 실행 사이에 다른 스레드가 끼어듦
  - Lazy Initialization: 객체 생성 전에 다른 스레드가 접근
- 예시
  - counter가 0일 때, 두 스레드가 동시에 counter++ → 기대값 2, 실제값 1 가능
  - 은행 계좌 잔액을 동시에 출금 → 잔액 불일치
- 해결
  - Mutex, Semaphore, Atomic 연산 등으로 Critical Section 보호

참고 자료:

---

## [COMMON-053] Critical Section이 무엇이고, 임계 구역 문제를 해결하기 위한 조건을 설명해 주세요.

답변:

- Critical Section (임계 구역)
  - 공유 자원에 접근하는 코드 구간
  - 한 번에 하나의 프로세스/스레드만 진입해야 하는 영역
  - 진입(Entry) → 임계 구역(Critical Section) → 탈출(Exit) 구조
- 임계 구역 문제 해결 조건 (Dijkstra, 1965)
  - 1. 상호 배제 (Mutual Exclusion)
    - 한 번에 하나의 프로세스만 임계 구역에 진입
  - 2. 진행 (Progress)
    - 임계 구역 밖에 실행 가능한 프로세스가 있으면, 선택이 무한정 지연되지 않음
    - 아무도 임계 구역에 없을 때, 진입 대기 중인 프로세스 중 하나는 반드시 진입
  - 3. 한정적 대기 (Bounded Waiting)
    - 어떤 프로세스도 임계 구역 진입을 무한정 기다리지 않음
    - 선착순 또는 공정한 순서 보장
- 구현 방법
  - Mutex, Semaphore, Monitor, Atomic 연산 등

참고 자료:

---

## [COMMON-054] Mutex와 Semaphore의 차이에 대해 설명해 주세요.

답변:

- Mutex (Mutual Exclusion, 상호 배제)
  - 임계 구역에 한 번에 하나의 스레드만 진입하도록 보장하는 동기화 primitive
  - Lock(잠금) / Unlock(해제) 두 가지 연산
  - 소유권(Ownership) 개념: Lock한 스레드만 Unlock 가능
  - Binary Semaphore(0 또는 1)와 유사하나, 소유권이 명확
- Semaphore (세마포어)
  - 정수형 카운터로 자원의 개수 또는 동시 접근 허용 수를 관리
  - wait(P) / signal(V) 연산
  - 카운터 > 0: 진입 허용, 카운터 감소
  - 카운터 = 0: 대기(Block)
  - 소유권 없음: wait한 스레드와 signal한 스레드가 달라도 됨
- 차이
  - 목적: Mutex는 상호 배제(1개), Semaphore는 자원 개수 관리(N개)
  - 소유권: Mutex는 Lock/Unlock 같은 스레드, Semaphore는 무관
  - 값: Mutex는 0/1, Semaphore는 0 이상의 정수
  - 용도: Mutex는 Critical Section 보호, Semaphore는 생산자-소비자, Connection Pool 등

참고 자료:

---

## [COMMON-055] Deadlock이 무엇이고, 발생 조건 4가지를 설명해 주세요.

답변:

- Deadlock (교착 상태)
  - 두 개 이상의 프로세스/스레드가 서로 상대방이 보유한 자원을 기다리며 무한정 대기하는 상태
  - 아무도 진행하지 못하고 영구적으로 Block
- 발생 조건 4가지 (Coffman Conditions, 네 가지 모두 충족 시 발생)
  - 1. 상호 배제 (Mutual Exclusion)
    - 자원을 한 번에 하나의 프로세스만 사용 가능
  - 2. 점유와 대기 (Hold and Wait)
    - 프로세스가 이미 자원을 보유한 채, 다른 자원을 추가로 기다림
  - 3. 비선점 (No Preemption)
    - 프로세스가 보유한 자원을 OS가 강제로 빼앗을 수 없음
  - 4. 순환 대기 (Circular Wait)
    - P1 → P2 → ... → Pn → P1 형태로 서로의 자원을 기다리는 순환 구조
- 예시
  - 스레드 A: Lock1 획득 → Lock2 대기
  - 스레드 B: Lock2 획득 → Lock1 대기 → Deadlock

참고 자료:

---

## [COMMON-056] Deadlock을 예방하거나 회피하는 방법에 대해 설명해 주세요.

답변:

- Deadlock 예방 (Prevention)
  - Coffman 4조건 중 하나 이상을 원천적으로 불가능하게 만듦
  - 상호 배제 제거: 공유 가능한 자원만 사용 (현실적으로 어려운 경우 많음)
  - 점유와 대기 제거: 실행 전 모든 자원을 한 번에 요청 (자원 낭비, 기아 가능)
  - 비선점 제거: 자원 획득 실패 시 보유 자원 전부 반납 후 재시도
  - 순환 대기 제거: 자원에 전역 순서 부여, 항상 같은 순서로 Lock 획득
- Deadlock 회피 (Avoidance)
  - 발생 가능성을 사전에 판단하고, 위험한 할당은 하지 않음
  - Banker's Algorithm: Safe State인지 검사 후 자원 할당
  - 자원 수와 프로세스 수가 고정된 환경에 적합
- Deadlock 탐지 및 복구 (Detection & Recovery)
  - Wait-for Graph 등으로 Deadlock 탐지
  - Victim 프로세스 선정 후 강제 종료 또는 자원 회수
- 실무적 접근
  - Lock 획득 순서 통일 (순환 대기 제거)
  - Lock timeout 설정, tryLock 사용
  - Deadlock-free 자료구조, Lock-free/Wait-free 알고리즘 사용

참고 자료:

---

## [COMMON-057] Thread Safe하다는 것은 어떤 의미인지 설명해 주세요.

답변:

- Thread Safe (스레드 안전)
  - 여러 스레드가 동시에 접근해도 프로그램의 정확성(correctness)이 보장되는 것
  - Race Condition 없이, 어떤 실행 순서에서도 올바른 결과를 반환
- Thread Safe한 코드의 특징
  - 공유 가변 상태(mutable state)에 대한 접근이 동기화됨
  - 또는 공유 상태가 없음(immutable, stateless)
  - 원자적(atomic) 연산 또는 Mutex 등으로 Critical Section 보호
- Thread Safe하지 않은 예
  - static 변수, singleton lazy init without sync
  - non-thread-safe 컬렉션(ArrayList 등)을 여러 스레드가 동시 수정
- Thread Safe 보장 방법
  - synchronized, Mutex, Read-Write Lock
  - Immutable 객체 사용
  - Thread-safe 컬렉션 (ConcurrentHashMap 등)
  - 각 스레드에 독립적인 데이터 (ThreadLocal)
- 주의
  - Thread Safe ≠ Deadlock Free (동기화 순서 잘못하면 Deadlock 가능)

참고 자료:

---

## [COMMON-058] Hash Table이 무엇이고, 해시 충돌을 해결하는 방법을 설명해 주세요.

답변:

- Hash Table (해시 테이블)
  - Key-Value 쌍을 저장하는 자료구조
  - 해시 함수(Hash Function)로 Key를 배열 인덱스로 변환 → O(1) 평균 시간에 삽입·조회·삭제
  - Key는 고유하지 않을 수 있음 → 같은 인덱스로 매핑되면 해시 충돌(Collision) 발생
- 해시 충돌 해결 방법
  - 1. Chaining (체이닝)
    - 같은 버킷(bucket)에 Linked List(또는 Tree)로 여러 항목 저장
    - 충돌 시 리스트에 추가, 조회 시 리스트 순회
    - Load Factor(항목 수 / 버킷 수)가 높으면 리스트가 길어져 성능 저하
  - 2. Open Addressing (개방 주소법)
    - 충돌 시 다른 빈 슬롯을 탐색하여 저장
    - Linear Probing: 다음 슬롯 순차 탐색
    - Quadratic Probing: i² 간격으로 탐색
    - Double Hashing: 두 번째 해시 함수로 탐색 간격 결정
- 좋은 해시 함수
  - 균등한 분포, 계산이 빠름, 충돌 최소화

참고 자료:

---

## [COMMON-059] Stack, Queue, Deque의 차이와 사용 사례를 설명해 주세요.

답변:

- Stack (스택)
  - LIFO (Last In First Out, 후입선출)
  - push(삽입), pop(삭제) 모두 top에서 수행
  - 사용 사례: 함수 호출 스택, Undo/Redo, 괄호 검사, DFS, 역폴란드 표기법
- Queue (큐)
  - FIFO (First In First Out, 선입선출)
  - enqueue(rear에 삽입), dequeue(front에서 삭제)
  - 사용 사례: BFS, 작업 스케줄링, 메시지 큐, 프린터 대기열, 버퍼
- Deque (Double-Ended Queue, 덱)
  - 양쪽 끝(front, rear) 모두에서 삽입·삭제 가능
  - Stack과 Queue를 일반화한 구조
  - 사용 사례: Sliding Window Maximum, Palindrome 검사, 0-1 BFS, Work Stealing Deque
- 차이
  - 삽입·삭제 위치: Stack은 한쪽, Queue는 양쪽(고정), Deque는 양쪽(자유)
  - 순서: Stack은 역순, Queue는 순차, Deque는 양방향

참고 자료:

---

## [COMMON-060] Tree, Binary Tree, Binary Search Tree의 차이에 대해 설명해 주세요.

답변:

- Tree (트리)
  - 노드(Node)와 간선(Edge)으로 구성된 계층적 비선형 자료구조
  - 사이클 없음, 루트(Root) 1개, 리프(Leaf) 노드 존재
  - N개 노드 → N-1개 간선
  - 파일 시스템, 조직도, HTML DOM 등에 활용
- Binary Tree (이진 트리)
  - Tree의 특수 형태, 각 노드가 최대 2개의 자식(Left, Right)을 가짐
  - 순회: Inorder, Preorder, Postorder
  - Full, Complete, Perfect 등 다양한 형태
  - BST 속성 없이도 유효한 이진 트리
- Binary Search Tree (BST, 이진 탐색 트리)
  - Binary Tree + 탐색 속성
  - Left 자식 < 부모 < Right 자식 (중위 순회 시 오름차순)
  - 평균 O(log n) 탐색·삽입·삭제, 최악 O(n) (한쪽으로 치우친 경우)
  - 검색, 정렬된 데이터 관리, 범위 쿼리에 적합
- 차이
  - Tree: 가장 일반적, 자식 수 제한 없음
  - Binary Tree: 자식 최대 2개, 정렬 속성 없음
  - BST: Binary Tree + 좌 < 부모 < 우 정렬 속성

참고 자료:

---

## 선택 질문

## [COMMON-061] 세그멘테이션과 페이징의 차이에 대해 설명해 주세요.

답변:

참고 자료:

---

## [COMMON-062] 페이지 크기가 커지거나 작아질 때 발생하는 trade-off를 설명해 주세요.

답변:

참고 자료:

---

## [COMMON-063] Thrashing이 무엇이고, 어떻게 완화할 수 있는지 설명해 주세요.

답변:

참고 자료:

---

## [COMMON-064] 페이지 교체 알고리즘의 종류와 LRU의 동작 방식을 설명해 주세요.

답변:

참고 자료:

---

## [COMMON-065] LRU Cache를 구현한다면 어떤 자료구조를 사용할 수 있을까요?

답변:

참고 자료:

---

## [COMMON-066] 캐시 지역성이 무엇인지 시간 지역성과 공간 지역성으로 나누어 설명해 주세요.

답변:

참고 자료:

---

## [COMMON-067] 2차원 배열을 행 우선으로 탐색할 때와 열 우선으로 탐색할 때 성능 차이가 날 수 있는 이유를 설명해 주세요.

답변:

참고 자료:

---

## [COMMON-068] Spin Lock의 장점과 단점에 대해 설명해 주세요.

답변:

참고 자료:

---

## [COMMON-069] Optimistic Lock과 Pessimistic Lock의 차이를 설명해 주세요.

답변:

참고 자료:

---

## [COMMON-070] Lock-free와 Wait-free의 차이에 대해 설명해 주세요.

답변:

참고 자료:

---

## [COMMON-071] Atomic 연산이 무엇이고, 동시성 문제 해결에 어떻게 사용되는지 설명해 주세요.

답변:

참고 자료:

---

## [COMMON-072] volatile 키워드가 어떤 의미를 가지는지 설명해 주세요.

답변:

참고 자료:

---

## [COMMON-073] Thread Pool이 무엇이고, 스레드 수는 어떤 기준으로 정할 수 있는지 설명해 주세요.

답변:

참고 자료:

---

## [COMMON-074] Heap 자료구조가 무엇이고, Priority Queue와 어떤 관계가 있는지 설명해 주세요.

답변:

참고 자료:

---

## [COMMON-075] Red-Black Tree가 무엇이고, 왜 균형 이진 탐색 트리가 필요한지 설명해 주세요.

답변:

참고 자료:

---

## [COMMON-076] Graph를 인접 행렬과 인접 리스트로 표현할 때의 장단점을 설명해 주세요.

답변:

참고 자료:

---

## [COMMON-077] BFS와 DFS의 차이와 각각 적합한 상황을 설명해 주세요.

답변:

참고 자료:

---

## [COMMON-078] 정렬 알고리즘 중 Quick Sort와 Merge Sort를 비교해 주세요.

답변:

참고 자료:

---

## [COMMON-079] Stable Sort가 무엇이고, 어떤 상황에서 중요한지 설명해 주세요.

답변:

참고 자료:

---

## [COMMON-080] 대용량 데이터를 메모리에 모두 올릴 수 없을 때 정렬하는 방법을 설명해 주세요.

답변:

참고 자료:
