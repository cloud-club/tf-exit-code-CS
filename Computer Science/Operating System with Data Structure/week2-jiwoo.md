# Week 02 - OS Memory / Concurrency + Core Data Structure

---

## 제출 기준

- 필수 답변: COMMON-041 ~ COMMON-060
- 선택 답변: COMMON-061 ~ COMMON-080

---

## 필수 질문

## [COMMON-041] 프로세스 주소 공간이 어떻게 구성되는지 설명해 주세요.

답변:

프로세스 주소 공간은 Code, Data, Heap, Stack 네 가지 영역으로 구성됩니다. Code는 실행 코드, Data는 전역/정적 변수, Heap은 동적 할당 공간, Stack은 함수 호출 정보와 지역 변수를 저장합니다. Heap은 낮은 주소에서 높은 주소로, Stack은 높은 주소에서 낮은 주소로 자라나며 두 영역이 충돌하면 메모리 오류가 발생합니다.

추가 설명)

```
높은 주소
┌─────────────┐
│    Stack    │  ↓ 성장 - 지역변수, 매개변수, 반환 주소
├─────────────┤
│      ↕      │  미사용 공간
├─────────────┤
│    Heap     │  ↑ 성장 - 동적 할당
├─────────────┤
│    Data     │  전역변수, 정적변수
├─────────────┤
│    Code     │  실행 코드, 기계어
└─────────────┘
낮은 주소
```

참고 자료:
- https://velog.io/@klm03025/%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9C-%ED%94%84%EB%A1%9C%EC%84%B8%EC%8A%A4-%EC%A3%BC%EC%86%8C-%EA%B3%B5%EA%B0%84
- https://velog.io/@heeco/CS-%ED%94%84%EB%A1%9C%EC%84%B8%EC%8A%A4-%EC%A3%BC%EC%86%8C%EA%B3%B5%EA%B0%84

---

## [COMMON-042] Code, Data, Heap, Stack 영역의 역할을 설명해 주세요.

답변:

Code 영역은 기계어 명령어를 저장하는 읽기 전용 공간이고, Data 영역은 전역/정적 변수를 저장합니다. Heap 영역은 런타임에 프로그래머가 직접 할당/해제하는 동적 공간이고, Stack 영역은 함수 호출마다 지역 변수와 매개변수를 담은 Frame이 자동으로 쌓이고 제거되는 LIFO 구조입니다.

추가 설명)

| 영역 | 저장 내용 | 생명주기 | 크기 결정 시점 |
|------|----------|---------|--------------|
| Code | 기계어 명령어 | 프로그램 전체 | 컴파일 시 |
| Data | 전역/정적 변수 | 프로그램 전체 | 컴파일 시 |
| Heap | 동적 할당 변수 | 수동 할당/해제 | 런타임 |
| Stack | 지역 변수, 매개변수 | 함수 호출~리턴 | 런타임 |

참고 자료:
- https://velog.io/@newjin46/OS-%EB%A9%94%EB%AA%A8%EB%A6%AC%EA%B5%AC%EC%A1%B0-code-data-stack-heap
- https://velog.io/@xxhaileypark/Memory-%EA%B5%AC%EC%A1%B0-code-data-stack-heap

---

## [COMMON-043] Stack과 Heap의 차이에 대해 설명해 주세요.

답변:

Stack은 OS가 자동으로 관리하며 할당/해제가 빠르지만 크기가 제한되어 있고, Heap은 프로그래머가 직접 관리하며 유연하지만 속도가 상대적으로 느립니다. Stack은 컴파일 타임에 크기가 결정되는 지역 변수를, Heap은 런타임에 크기가 결정되는 동적 데이터를 저장합니다.

추가 설명)

| 구분 | Stack | Heap |
|------|-------|------|
| 관리 주체 | OS 자동 | 프로그래머 수동 |
| 할당 속도 | 매우 빠름 | 상대적으로 느림 |
| 크기 | 제한적 | 가상 메모리 한도까지 |
| 구조 | LIFO, 연속적 | 불연속 가능 |
| 오류 위험 | Stack Overflow | Memory Leak, Dangling Pointer |

참고 자료:
- https://velog.io/@cm961115/Memory-%EB%A9%94%EB%AA%A8%EB%A6%AC-%EA%B5%AC%EC%A1%B0-code-data-stack-heap

---

## [COMMON-044] Stack Overflow가 발생하는 이유를 설명해 주세요.

답변:

Stack은 OS가 프로세스에 할당한 고정 크기를 가지는데, 함수 호출마다 Stack Frame이 쌓이다가 이 한계를 초과하면 Stack Overflow가 발생합니다. 주요 원인은 종료 조건이 없는 무한 재귀 호출이며, 깊은 재귀나 함수 내 너무 큰 로컬 변수 선언도 원인이 됩니다.

추가 설명)

```
// 무한 재귀 예시
void func() {
    func(); // 종료 조건 없음 → Stack Frame 무한 축적 → Stack Overflow
}
```

참고 자료:
- https://velog.io/@enamu/%EC%8A%A4%ED%83%9D%EC%98%A4%EB%B2%84%ED%94%8C%EB%A1%9C%EC%9A%B0Stack-Overflow
- https://velog.io/@excellent/CSstack-overflow-out-of-memory

---

## [COMMON-045] Memory Leak이 무엇이고, 어떤 상황에서 발생할 수 있는지 설명해 주세요.

답변:

Memory Leak은 프로그램이 더 이상 사용하지 않는 Heap 메모리를 해제하지 않아 시스템 메모리가 점점 고갈되는 현상입니다. 주요 발생 상황은 C/C++에서 할당 후 해제를 빠뜨리거나, 객체 간 순환 참조, 파일/소켓/DB 커넥션을 닫지 않고 참조를 잃는 경우입니다. 누적되면 결국 OOM 오류로 프로그램이 종료됩니다.

추가 설명)

- **순환 참조**: 두 객체가 서로 참조해 Reference Counting 기반 GC에서 해제되지 않는 상황
- **전역 컬렉션 누수**: 리스트나 맵에 데이터를 계속 추가만 하고 제거하지 않는 경우
- **리소스 미반환**: 파일 핸들, 네트워크 소켓 등 OS 리소스를 닫지 않으면 OS 레벨 누수도 발생

참고 자료:
- https://velog.io/@ouk/%EB%A9%94%EB%AA%A8%EB%A6%AC-%EB%88%84%EC%88%98Memory-Leak%EB%8A%94-%EB%AC%B4%EC%97%87%EC%9D%B4%EB%A9%B0-%EC%96%B4%EB%96%BB%EA%B2%8C-%EB%B0%A9%EC%A7%80%ED%95%98%EB%82%98%EC%9A%94
- https://velog.io/@dhwltnoooh/Memory-leak

---

## [COMMON-046] 가상 메모리가 무엇이고, 왜 필요한지 설명해 주세요.

답변:

가상 메모리는 프로세스가 실제 물리 메모리 크기와 무관하게 독립적인 넓은 주소 공간을 사용할 수 있도록 OS가 제공하는 추상화 기법입니다. 물리 메모리 크기 제약 극복, 프로세스 간 격리 및 보호, 메모리 효율적 사용이 가능해지기 때문에 필요합니다.

추가 설명)

- 프로세스는 가상 주소를 사용하고, MMU가 페이지 테이블을 참조해 물리 주소로 변환합니다.
- 실제로 사용하는 페이지만 물리 메모리에 올리는 **Demand Paging** 기법으로 메모리 효율을 높입니다.
- 각 프로세스는 0번 주소부터 시작하는 독립된 가상 공간을 가지므로 서로의 메모리를 침범할 수 없습니다.

```
프로세스 A 가상 주소    프로세스 B 가상 주소
  0x0000 ~ 0xFFFF         0x0000 ~ 0xFFFF
          ↓  MMU 변환 ↓
        물리 메모리 - 실제 주소는 서로 다름
```

참고 자료:
- https://velog.io/@gndan4/OS-%EA%B0%80%EC%83%81-%EB%A9%94%EB%AA%A8%EB%A6%AC
- https://velog.io/@wejaan/%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9C-%EA%B0%80%EC%83%81-%EB%A9%94%EB%AA%A8%EB%A6%AC

---

## [COMMON-047] 페이징이 무엇이고, 페이지와 프레임의 차이를 설명해 주세요.

답변:

페이징은 가상 주소 공간과 물리 메모리를 동일한 고정 크기 블록으로 나눠 관리하는 기법입니다. 가상 주소 공간의 블록을 **페이지**, 물리 메모리의 블록을 **프레임**이라 하며, 크기는 동일합니다. OS는 페이지 테이블로 각 페이지가 어느 프레임에 올라가 있는지 추적합니다.

추가 설명)

| 구분 | 페이지 | 프레임 |
|------|--------|--------|
| 위치 | 가상 주소 공간 | 물리 메모리 |
| 크기 | 고정 | 페이지와 동일 |
| 관리 | 페이지 테이블 | OS 프레임 할당표 |

- **장점**: 외부 단편화 없음, 물리 메모리에 연속으로 올릴 필요 없음
- **단점**: 내부 단편화 발생 가능, 페이지 테이블 자체가 메모리를 차지

참고 자료:
- https://velog.io/@wejaan/%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9C-%EA%B0%80%EC%83%81-%EB%A9%94%EB%AA%A8%EB%A6%AC
- https://velog.io/@gndan4/OS-%EA%B0%80%EC%83%81-%EB%A9%94%EB%AA%A8%EB%A6%AC

---

## [COMMON-048] Page Fault가 발생했을 때 처리 과정을 설명해 주세요.

답변:

Page Fault는 접근하려는 페이지가 물리 메모리에 없을 때 발생하는 인터럽트입니다. OS가 디스크에서 해당 페이지를 물리 메모리로 로드하고 페이지 테이블을 갱신한 후 CPU가 중단된 명령어를 재실행합니다. 디스크 I/O를 수반하므로 성능에 큰 영향을 줍니다.

추가 설명)

```
CPU가 가상 주소 접근
   ↓
MMU가 페이지 테이블 확인 → Valid bit = 0
   ↓
Page Fault 인터럽트 → OS의 Page Fault Handler 실행
   ↓
빈 프레임 확인
  └ 없음 → 페이지 교체 알고리즘으로 희생 페이지 스왑아웃
   ↓
디스크에서 페이지 로드 → 페이지 테이블 갱신
   ↓
CPU가 명령어 재실행
```

참고 자료:
- https://velog.io/@gndan4/OS-%EA%B0%80%EC%83%81-%EB%A9%94%EB%AA%A8%EB%A6%AC
- https://imbf.github.io/interview/2020/11/26/NAVER-Interview-Preparation-1.html

---

## [COMMON-049] TLB가 무엇이고, TLB를 사용하면 왜 주소 변환이 빨라지는지 설명해 주세요.

답변:

TLB는 페이지 테이블의 최근 사용 항목을 캐싱하는 고속 하드웨어 캐시입니다. 페이지 테이블은 메모리에 있어 조회 시 메모리 접근이 한 번 더 필요한데, TLB에 해당 항목이 있으면 메모리 접근 없이 바로 물리 주소를 얻을 수 있어 주소 변환이 빨라집니다.

추가 설명)

- **TLB Hit**: 가상 주소 → TLB 조회 성공 → 물리 주소, 총 1회 메모리 접근
- **TLB Miss**: 가상 주소 → TLB 실패 → 페이지 테이블 조회 → TLB 갱신 → 물리 주소, 총 2회 메모리 접근
- TLB 적중률은 보통 99%에 가까워 실질적 오버헤드가 거의 없습니다.
- 컨텍스트 스위칭 시 TLB를 비워야 하므로 이것이 컨텍스트 스위칭 비용의 일부입니다.

```
가상 주소 요청 → TLB 조회
     Hit ↙       ↘ Miss
  물리 주소    페이지 테이블 → TLB 갱신 → 물리 주소
```

참고 자료:
- https://imbf.github.io/interview/2020/11/26/NAVER-Interview-Preparation-1.html
- https://velog.io/@wejaan/%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9C-%EA%B0%80%EC%83%81-%EB%A9%94%EB%AA%A8%EB%A6%AC

---

## [COMMON-050] 내부 단편화와 외부 단편화의 차이에 대해 설명해 주세요.

답변:

내부 단편화는 할당된 블록 내부에 사용되지 않는 낭비 공간이 생기는 현상이고, 외부 단편화는 가용 메모리 합계는 충분하지만 흩어져 있어 연속 공간이 필요한 요청을 수용하지 못하는 현상입니다. 페이징은 외부 단편화를 해결하지만 내부 단편화를 유발하고, 세그멘테이션은 그 반대입니다.

추가 설명)

| 구분 | 내부 단편화 | 외부 단편화 |
|------|-----------|-----------|
| 낭비 위치 | 할당 블록 내부 | 할당 블록 사이 |
| 원인 | 고정 크기 블록 할당 | 반복 할당/해제 |
| 주로 발생 | 페이징 | 세그멘테이션, 연속 할당 |
| 해결 방법 | 페이지 크기 최적화 | 압축, 페이징 적용 |

참고 자료:
- https://velog.io/@kangdev/%EA%B8%B0%EC%88%A0%EB%A9%B4%EC%A0%91%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9C-%EB%A9%94%EB%AA%A8%EB%A6%AC-%EB%8B%A8%ED%8E%B8%ED%99%94-Memory-Fragmentation
- https://junghyun100.github.io/%EB%A9%94%EB%AA%A8%EB%A6%AC%EB%8B%A8%ED%8E%B8%ED%99%94/

---

## [COMMON-051] 동시성과 병렬성의 차이에 대해 설명해 주세요.

답변:

동시성은 여러 작업이 빠르게 번갈아 실행되어 동시에 실행되는 것처럼 보이는 논리적 개념으로 단일 코어에서도 가능하고, 병렬성은 여러 작업이 실제로 같은 시간에 물리적으로 동시에 실행되는 것으로 멀티 코어가 필요합니다. 병렬성은 동시성에 포함되는 개념입니다.

추가 설명)

```
동시성 - 단일 코어
Time: 1  2  3  4  5  6  7  8
Core: A  A  B  B  A  A  B  B   ← 실제론 순차이지만 동시처럼 보임

병렬성 - 멀티 코어
Time:  1  2  3  4
Core1: A  A  A  A
Core2: B  B  B  B              ← 진짜로 동시에 실행
```

| 구분 | 동시성 | 병렬성 |
|------|--------|--------|
| 실제 동시 실행 | X | O |
| 필요 코어 수 | 1개 이상 | 2개 이상 |
| 구현 방법 | Context Switching | 멀티코어 |

참고 자료:
- https://velog.io/@wlsrhkd4023/%EB%8F%99%EC%8B%9C%EC%84%B1Concurrency%EA%B3%BC-%EB%B3%91%EB%A0%AC%EC%84%B1Parallelism-%EC%B0%A8%EC%9D%B4
- https://vagabond95.me/posts/concurrency_vs_parallelism/

---

## [COMMON-052] Race Condition이 무엇이고, 어떤 상황에서 발생하는지 설명해 주세요.

답변:

Race Condition은 두 개 이상의 스레드가 공유 자원에 동시에 접근하여 읽기/쓰기를 수행할 때, 실행 순서에 따라 결과가 달라지는 상황입니다. 동기화가 없는 상태에서 최소 하나의 스레드가 쓰기 작업을 수행할 때 발생하며, Mutex나 Semaphore로 임계 구역을 보호해 해결합니다.

추가 설명)

```
초기 잔액 = 1000원

스레드 A: Read 1000 → 1000 - 500 = 500 → Write 500
스레드 B: Read 1000 → 1000 - 300 = 700 → Write 700  ← A의 결과를 덮어씀

최종 잔액 = 700원  (올바른 결과: 1000 - 500 - 300 = 200원)
```

참고 자료:
- https://www.geeksforgeeks.org/operating-systems/race-condition-in-operating-systems/
- https://velog.io/@mooh2jj/%EA%B5%90%EC%B0%A9%EC%83%81%ED%83%9CDeadlock%EC%9D%B4%EB%9E%80

---

## [COMMON-053] Critical Section이 무엇이고, 임계 구역 문제를 해결하기 위한 조건을 설명해 주세요.

답변:

임계 구역은 공유 자원에 접근하는 코드 구간으로, 한 번에 하나의 스레드만 진입해야 합니다. 임계 구역 문제 해결을 위한 3가지 조건은 상호 배제, 진행, 한정 대기입니다.

추가 설명)

1. **상호 배제**: 한 스레드가 임계 구역에 있으면 다른 스레드는 진입 불가
2. **진행**: 임계 구역이 비어 있을 때 진입을 원하는 스레드가 있으면 반드시 진입 허용
3. **한정 대기**: 진입 요청 후 무한정 거부되지 않아야 함, 기다리는 횟수에 상한 존재

| 조건 | 위반 시 문제 |
|------|------------|
| 상호 배제 | Race Condition |
| 진행 | 무한 대기 |
| 한정 대기 | Starvation |

참고 자료:
- https://www.geeksforgeeks.org/operating-systems/critical-section-in-synchronization/

---

## [COMMON-054] Mutex와 Semaphore의 차이에 대해 설명해 주세요.

답변:

Mutex는 하나의 스레드만 자원에 접근할 수 있는 Lock으로, 획득한 스레드만 해제할 수 있는 소유권 개념이 있습니다. Semaphore는 카운터로 N개의 스레드까지 접근을 허용하며, 소유권이 없어 다른 스레드가 signal을 호출해 해제할 수 있습니다.

추가 설명)

| 구분 | Mutex | Semaphore |
|------|-------|-----------|
| 동시 접근 수 | 1 | 1 이상 |
| 소유권 | O, 획득한 스레드만 해제 | X, 누구나 signal 가능 |
| 용도 | 단일 자원 보호 | 자원 풀 관리, 실행 순서 제어 |
| 종류 | - | Binary, Counting |

- Binary Semaphore는 카운터 최대값이 1로 Mutex와 유사하게 동작하지만 소유권이 없어 다릅니다.
- Semaphore는 Mutex가 될 수 있지만, Mutex는 소유권이 있으므로 Semaphore가 될 수 없습니다.

참고 자료:
- https://velog.io/@heetaeheo/%EB%AE%A4%ED%85%8D%EC%8A%A4Mutex%EC%99%80-%EC%84%B8%EB%A7%88%ED%8F%AC%EC%96%B4Semaphore%EC%9D%98-%EC%B0%A8%EC%9D%B4
- https://junghyun100.github.io/Semaphore&Mutex/

---

## [COMMON-055] Deadlock이 무엇이고, 발생 조건 4가지를 설명해 주세요.

답변:

Deadlock은 두 개 이상의 프로세스가 서로가 점유한 자원을 기다리며 무한히 대기하는 상태입니다. 발생 조건 4가지는 상호 배제, 점유 대기, 비선점, 순환 대기이며, 이 4가지가 동시에 성립해야 발생합니다. 하나라도 깨면 예방할 수 있습니다.

추가 설명)

1. **상호 배제**: 자원은 한 번에 하나의 프로세스만 사용 가능
2. **점유 대기**: 자원을 점유한 채 다른 자원을 기다림
3. **비선점**: 다른 프로세스의 자원을 강제로 빼앗을 수 없음
4. **순환 대기**: A→B, B→C, C→A 형태로 자원을 원형으로 기다림

```
A 점유: 자원1, 대기: 자원2
B 점유: 자원2, 대기: 자원1
→ A와 B가 서로를 기다리며 영구 블로킹
```

참고 자료:
- https://velog.io/@yanghl98/%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9COS-Deadlock%EB%8D%B0%EB%93%9C%EB%9D%BD-%EC%A0%95%EC%9D%98-%EB%B0%9C%EC%83%9D-%EC%A1%B0%EA%B1%B4-%ED%95%B4%EA%B2%B0-%EB%B0%A9%EB%B2%95

---

## [COMMON-056] Deadlock을 예방하거나 회피하는 방법에 대해 설명해 주세요.

답변:

Deadlock 처리 전략은 예방, 회피, 탐지/복구 세 가지입니다. 예방은 4가지 발생 조건 중 하나를 원천적으로 제거하고, 회피는 은행원 알고리즘으로 안전 상태를 유지하며 자원을 할당하고, 탐지/복구는 발생 후 사이클을 탐지해 프로세스를 종료합니다.

추가 설명)

- **예방 - 순환 대기 제거**: 자원에 번호를 부여하고 오름차순으로만 요청, 가장 실용적
- **회피 - 은행원 알고리즘**: 자원 요청 시 가상 할당 후 안전 순서가 존재하면 허용, 없으면 대기
- **탐지/복구**: 자원 할당 그래프의 사이클을 주기적으로 탐지, 발견 시 프로세스 종료 또는 자원 선점

| 전략 | 방법 | 오버헤드 |
|------|------|---------|
| 예방 | 4조건 중 하나 제거 | 자원 낭비 또는 처리량 감소 |
| 회피 | 은행원 알고리즘 | 매 요청마다 안전 상태 계산 |
| 탐지/복구 | 사이클 탐지 후 종료 | 탐지 주기 동안 낭비 |

참고 자료:
- https://velog.io/@chappi/OS%EB%8A%94-%ED%95%A0%EA%BB%80%EB%8D%B0-%ED%95%B5%EC%8B%AC%EB%A7%8C-%ED%95%A9%EB%8B%88%EB%8B%A4.-11%ED%8E%B8-Deadlock%EA%B5%90%EC%B0%A9%EC%83%81%ED%83%9C2-Deadlock-%ED%95%B4%EA%B2%B0-%EB%B0%A9%EB%B2%95-%EC%98%88%EB%B0%A9%EA%B3%BC-%EA%B2%80%EC%B6%9C-%ED%9A%8C%EB%B3%B5
- https://velog.io/@jerry92/OS-%EA%B5%90%EC%B0%A9%EC%83%81%ED%83%9Cdeadlock%EC%9D%98-%EC%A1%B0%EA%B1%B4%EA%B3%BC-%ED%95%B4%EA%B2%B0-%EB%B0%A9%EB%B2%95

---

## [COMMON-057] Thread Safe하다는 것은 어떤 의미인지 설명해 주세요.

답변:

Thread Safe하다는 것은 여러 스레드가 동시에 접근/호출해도 Race Condition 없이 항상 올바른 결과를 보장한다는 의미입니다. Mutex/Lock 사용, Atomic 연산, Immutable 객체, Thread-Local Storage 등으로 구현할 수 있습니다.

추가 설명)

```python
# Thread Safe하지 않은 예시
count = 0

def increment():
    global count
    count += 1  # read-modify-write가 원자적이지 않아 동시 실행 시 손실 발생

# Thread Safe하게 만들기: Lock 적용
import threading
lock = threading.Lock()

def safe_increment():
    with lock:
        count += 1
```

참고 자료:
- https://velog.io/@hoo00nn/Thread-safe%EC%99%80-%EB%8F%99%EA%B8%B0%ED%99%94-%EA%B0%9D%EC%B2%B4
- https://ko.wikipedia.org/wiki/%EC%8A%A4%EB%A0%88%EB%93%9C_%EC%95%88%EC%A0%84

---

## [COMMON-058] Hash Table이 무엇이고, 해시 충돌을 해결하는 방법을 설명해 주세요.

답변:

해시 테이블은 키를 해시 함수로 변환한 인덱스에 값을 저장하는 자료구조로, 평균 O(1) 시간에 삽입/삭제/탐색이 가능합니다. 해시 충돌은 서로 다른 키가 동일한 인덱스로 매핑되는 현상으로, 체이닝과 개방 주소법으로 해결합니다.

추가 설명)

- **체이닝**: 같은 인덱스의 값들을 연결 리스트나 트리로 연결, 구현 간단하지만 최악 O(n)
- **선형 탐사**: 충돌 시 다음 빈 슬롯을 고정 간격으로 탐색, 군집화 발생 가능
- **제곱 탐사**: 1², 2², 3² 간격으로 탐색, 군집화 감소
- **이중 해싱**: 두 번째 해시 함수로 탐사 간격 결정, 군집화 최소화

```
체이닝:  버킷[3] → [A] → [B] → NULL
선형탐사: 인덱스3 충돌 → 4 → 4도 충돌 → 5에 저장
```

참고 자료:
- https://velog.io/@cyranocoding/Hash-Hashing-Hash-Table%ED%95%B4%EC%8B%9C-%ED%95%B4%EC%8B%B1-%ED%95%B4%EC%8B%9C%ED%85%8C%EC%9D%B4%EB%B8%94-%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0%EC%9D%98-%EC%9D%B4%ED%95%B4-6ijyonph6o
- https://velog.io/@jaeyunn_15/DataStructure-%ED%95%B4%EC%8B%9C-%EC%B6%A9%EB%8F%8C%EA%B3%BC-%ED%95%B4%EA%B2%B0-%EB%B0%A9%EB%B2%95

---

## [COMMON-059] Stack, Queue, Deque의 차이와 사용 사례를 설명해 주세요.

답변:

Stack은 한쪽 끝에서만 삽입/삭제하는 LIFO 구조, Queue는 한쪽에서 삽입하고 반대쪽에서 삭제하는 FIFO 구조, Deque는 양쪽 끝 모두에서 삽입/삭제가 가능한 구조입니다. 삽입/삭제 모두 O(1)입니다.

추가 설명)

| 구분 | Stack | Queue | Deque |
|------|-------|-------|-------|
| 삽입/삭제 방향 | 한쪽 | 양쪽 각각 | 양쪽 모두 |
| 순서 원칙 | LIFO | FIFO | 자유 |
| 시간 복잡도 | O(1) | O(1) | O(1) |

- **Stack 사용 사례**: 함수 호출 스택, 브라우저 뒤로가기, 괄호 유효성 검사, Undo/Redo
- **Queue 사용 사례**: 프린터 대기열, CPU 프로세스 스케줄링, BFS, 메시지 큐
- **Deque 사용 사례**: 슬라이딩 윈도우 최솟값, 팰린드롬 검사, 양방향 BFS

참고 자료:
- https://velog.io/@nnnyeong/%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0-%EC%8A%A4%ED%83%9D-Stack-%ED%81%90-Queue-%EB%8D%B1-Deque
- https://choiiis.github.io/data-structure/basics-of-stack-queue-and-deque/

---

## [COMMON-060] Tree, Binary Tree, Binary Search Tree의 차이에 대해 설명해 주세요.

답변:

트리는 노드와 간선으로 구성된 계층적 자료구조이고, 이진 트리는 자식 노드가 최대 2개로 제한된 트리입니다. 이진 탐색 트리는 이진 트리에 왼쪽 자식 < 부모 < 오른쪽 자식의 순서 속성을 추가한 것으로, 평균 O(log n) 탐색이 가능합니다. 세 개념은 BST ⊂ 이진 트리 ⊂ 트리의 포함 관계입니다.

추가 설명)

```
트리 - 자식 무제한    이진 트리 - 자식 ≤ 2    이진 탐색 트리 - 왼<부<오
        A                    1                        8
      / | \                /   \                    /   \
     B  C  D              2     3                  3    10
     |                   / \                      / \   /
     E                  4   5                    1   6  9
```

| 구분 | 트리 | 이진 트리 | 이진 탐색 트리 |
|------|------|----------|--------------|
| 자식 노드 수 | 제한 없음 | 최대 2개 | 최대 2개 |
| 순서 규칙 | 없음 | 없음 | 왼<부모<오 |
| 탐색 복잡도 | O(n) | O(n) | 평균 O(log n), 최악 O(n) |
| 용도 | 파일 시스템, DOM | 힙, 표현식 트리 | 탐색/삽입/삭제 최적화 |

- 이진 탐색 트리는 편향되면 최악 O(n)이 되므로, 이를 해결하기 위해 AVL 트리, Red-Black 트리 같은 균형 이진 탐색 트리를 사용합니다.

참고 자료:
- https://velog.io/@rlwjd31/TreeStandard-Tree-Binary-Tree-Binary-Search-Tree
- https://velog.io/@hosickk/%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0-%EA%B8%B0%EC%B4%88-Binary-Tree-Binary-Search-Tree

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
