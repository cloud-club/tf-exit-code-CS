# Week 02 - OS Memory / Concurrency + Core Data Structure

---

## 제출 기준

- 필수 답변: COMMON-041 ~ COMMON-060
- 선택 답변: COMMON-061 ~ COMMON-080

---

## 필수 질문

## [COMMON-041] 프로세스 주소 공간이 어떻게 구성되는지 설명해 주세요.

답변:

프로세스 주소 공간은 일반적으로 Code, Data, Heap, Stack 영역으로 구성된다. 멀티스레드 환경에서는 Code, Data, Heap은 공유하지만 Stack은 스레드마다 별도로 가진다.

참고 자료:
https://man7.org/linux/man-pages/man5/proc_pid_maps.5.html

---

## [COMMON-042] Code, Data, Heap, Stack 영역의 역할을 설명해 주세요.

답변:

Code 영역에는 실행할 명령어, Data 영역에는 전역 변수와 정적 변수, Heap 영역에는 동적으로 할당한 데이터, Stack 영역에는 함수의 지역 변수, 매개변수, 반환 주소 등이 저장된다.

참고 자료:
https://www.geeksforgeeks.org/c/memory-layout-of-c-program/

---

## [COMMON-043] Stack과 Heap의 차이에 대해 설명해 주세요.

답변:

Stack은 함수 호출에 따라 자동으로 할당되고 해제되며 속도가 빠르지만 크기가 제한적이다. Heap은 실행 중 필요한 메모리를 동적으로 할당하는 영역으로 크기가 비교적 자유롭지만 할당과 해제 비용이 크고 메모리 누수가 발생할 수 있다.

참고 자료:
https://www.geeksforgeeks.org/dsa/stack-vs-heap-memory-allocation/

---

## [COMMON-044] Stack Overflow가 발생하는 이유를 설명해 주세요.

답변:

Stack Overflow는 스레드에 할당된 Stack 공간을 모두 사용했을 때 발생한다. 주로 종료 조건이 없는 재귀 호출, 지나치게 깊은 함수 호출, 큰 지역 변수 선언으로 발생한다.

참고 자료:
https://docs.oracle.com/javase/8/docs/api/java/lang/StackOverflowError.html

---

## [COMMON-045] Memory Leak이 무엇이고, 어떤 상황에서 발생할 수 있는지 설명해 주세요.

답변:

Memory Leak은 더 이상 사용하지 않는 메모리가 해제되지 않고 계속 점유되는 현상이다. 동적 메모리를 반환하지 않거나 사용하지 않는 객체의 참조를 계속 유지할 때 발생할 수 있다.

참고 자료:
https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Memory_management

---

## [COMMON-046] 가상 메모리가 무엇이고, 왜 필요한지 설명해 주세요.

답변:

가상 메모리는 각 프로세스에 독립적인 가상 주소 공간을 제공하고 이를 물리 메모리에 매핑하는 방식이다. 프로세스 간 메모리를 격리하고, 필요한 페이지만 물리 메모리에 올려 메모리를 효율적으로 사용할 수 있게 한다.

참고 자료:
https://learn.microsoft.com/en-us/windows/win32/memory/virtual-memory

---

## [COMMON-047] 페이징이 무엇이고, 페이지와 프레임의 차이를 설명해 주세요.

답변:

페이징은 가상 메모리와 물리 메모리를 같은 크기의 블록으로 나누어 관리하는 방식이다. 페이지는 가상 주소 공간의 단위이고, 프레임은 물리 메모리의 단위이다.

참고 자료:
https://docs.kernel.org/mm/page_tables.html

---

## [COMMON-048] Page Fault가 발생했을 때 처리 과정을 설명해 주세요.

답변:

Page Fault가 발생하면 운영체제는 접근 주소가 유효한지 확인한다. 유효하면 필요한 페이지를 물리 메모리에 적재하고 페이지 테이블을 갱신한 뒤 중단된 명령을 다시 실행한다. 빈 프레임이 없다면 기존 페이지를 교체한다.

참고 자료:
https://docs.kernel.org/mm/page_tables.html

---

## [COMMON-049] TLB가 무엇이고, TLB를 사용하면 왜 주소 변환이 빨라지는지 설명해 주세요.

답변:

TLB는 최근 사용한 가상 페이지와 물리 프레임의 매핑을 저장하는 캐시이다. TLB에 주소 정보가 있으면 메모리의 페이지 테이블을 조회하지 않고 바로 물리 주소를 찾을 수 있어 주소 변환이 빨라진다.

참고 자료:
https://www.geeksforgeeks.org/operating-systems/translation-lookaside-buffer-tlb-in-paging/

---

## [COMMON-050] 내부 단편화와 외부 단편화의 차이에 대해 설명해 주세요.

답변:

내부 단편화는 할당된 메모리 영역 안에 사용하지 않는 공간이 생기는 현상이다. 외부 단편화는 빈 공간의 총합은 충분하지만 작은 공간으로 흩어져 연속된 메모리를 할당하지 못하는 현상이다.

참고 자료:
https://www.geeksforgeeks.org/operating-systems/difference-between-internal-and-external-fragmentation/

---

## [COMMON-051] 동시성과 병렬성의 차이에 대해 설명해 주세요.

답변:

동시성은 여러 작업이 번갈아 실행되며 동시에 진행되는 것처럼 보이는 개념이다. 병렬성은 여러 CPU 코어가 여러 작업을 실제로 같은 시점에 실행하는 개념이다.

참고 자료:
https://docs.oracle.com/javase/tutorial/essential/concurrency/

---

## [COMMON-052] Race Condition이 무엇이고, 어떤 상황에서 발생하는지 설명해 주세요.

답변:

Race Condition은 여러 스레드가 공유 자원에 동시에 접근할 때 실행 순서에 따라 결과가 달라지는 현상이다. 공유 데이터를 적절한 동기화 없이 읽고 수정할 때 발생한다.

참고 자료:
https://docs.oracle.com/javase/tutorial/essential/concurrency/interfere.html

---

## [COMMON-053] Critical Section이 무엇이고, 임계 구역 문제를 해결하기 위한 조건을 설명해 주세요.

답변:

Critical Section은 공유 자원에 접근하는 코드 영역이다. 임계 구역 문제를 해결하려면 한 번에 하나만 접근하는 상호 배제, 진입 결정을 무한히 미루지 않는 진행, 무한 대기를 방지하는 한정 대기 조건을 만족해야 한다.

참고 자료:
https://www.geeksforgeeks.org/operating-systems/g-fact-70/

---

## [COMMON-054] Mutex와 Semaphore의 차이에 대해 설명해 주세요.

답변:

Mutex는 한 번에 하나의 스레드만 공유 자원에 접근하도록 하며 Lock을 획득한 스레드가 해제하는 소유권 개념이 있다. Semaphore는 카운터를 이용해 정해진 개수의 스레드가 동시에 접근하도록 제한한다.

참고 자료:
https://www.baeldung.com/cs/mutex-vs-semaphore

---

## [COMMON-055] Deadlock이 무엇이고, 발생 조건 4가지를 설명해 주세요.

답변:

Deadlock은 여러 프로세스나 스레드가 서로의 자원을 기다리며 작업을 진행하지 못하는 상태이다. 상호 배제, 점유 대기, 비선점, 순환 대기 조건이 모두 만족될 때 발생할 수 있다.

참고 자료:
https://docs.oracle.com/javase/tutorial/essential/concurrency/deadlock.html

---

## [COMMON-056] Deadlock을 예방하거나 회피하는 방법에 대해 설명해 주세요.

답변:

Deadlock 예방은 발생 조건 중 하나가 성립하지 않도록 Lock 획득 순서를 통일하거나 필요한 자원을 한 번에 요청하는 방식이다. 회피는 자원 할당 후에도 안전 상태인지 확인하고 요청을 허용하며, 대표적으로 Banker’s Algorithm이 있다.

참고 자료:
https://www.geeksforgeeks.org/operating-systems/deadlock-prevention/

---

## [COMMON-057] Thread Safe하다는 것은 어떤 의미인지 설명해 주세요.

답변:

Thread Safe는 여러 스레드가 하나의 객체나 코드에 동시에 접근해도 데이터가 손상되지 않고 의도한 결과가 유지되는 것을 의미한다. Lock, Atomic 연산, 불변 객체 등을 통해 구현할 수 있다.

참고 자료:
https://docs.oracle.com/javase/tutorial/essential/concurrency/sync.html

---

## [COMMON-058] Hash Table이 무엇이고, 해시 충돌을 해결하는 방법을 설명해 주세요.

답변:

Hash Table은 Key를 해시 함수로 변환한 위치에 값을 저장하는 자료구조로, 평균적으로 삽입, 조회, 삭제가 O(1)이다. 충돌은 같은 위치에 여러 Key가 매핑되는 현상이며 Chaining이나 Open Addressing 방식으로 해결한다.

참고 자료:
https://www.geeksforgeeks.org/dsa/hashing-data-structure/

---

## [COMMON-059] Stack, Queue, Deque의 차이와 사용 사례를 설명해 주세요.

답변:

Stack은 LIFO 구조로 함수 호출, 괄호 검사, DFS 등에 사용된다. Queue는 FIFO 구조로 작업 스케줄링과 BFS 등에 사용된다. Deque는 양쪽 끝에서 삽입과 삭제가 가능하며 Stack과 Queue의 역할을 모두 수행할 수 있다.

참고 자료:
https://docs.python.org/3/library/collections.html#collections.deque

---

## [COMMON-060] Tree, Binary Tree, Binary Search Tree의 차이에 대해 설명해 주세요.

답변:

Tree는 부모와 자식의 계층 관계를 표현하는 자료구조이다. Binary Tree는 각 노드가 최대 두 개의 자식을 가지는 트리이며, Binary Search Tree는 왼쪽에 작은 값, 오른쪽에 큰 값을 저장하는 정렬 규칙을 가진 이진 트리이다.

참고 자료:
https://www.geeksforgeeks.org/dsa/binary-search-tree-data-structure/

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