# Week 02 - OS Memory / Concurrency + Core Data Structure

---

## 제출 기준

- 필수 답변: COMMON-041 ~ COMMON-060
- 선택 답변: COMMON-061 ~ COMMON-080

---

## 필수 질문

## [COMMON-041] 프로세스 주소 공간이 어떻게 구성되는지 설명해 주세요.

답변: 프록세스 주소 공간은 프로세스가 사용할 수 있게 운영체제가 제공하는 독립적인 가상 메모리 공간입니다. 일반적으로 실행 코드가 저장되는 Code 영역, 전역 변수와 정적 변수가 저장되는 Data 영역, 동적 할당에 사용되는 Heap 영역, 함수 호출 정보와 지역 변수가 저장되는 Stack 영역으로 구성됩니다.

각 프로세스는 독립적인 주소 공간을 가지기 때문에 다른 프로세스의 메모리에 직접 접근할 수 없고 없고, 이를 통해 OS는 프로세스 간 격리와 메모리 보호를 제공합니다.

참고 자료: [Microsoft Learn - Virtual Address Spaces](https://learn.microsoft.com/en-us/windows-hardware/drivers/gettingstarted/virtual-address-spaces)

[Linux man-pages - /proc/pid/maps](https://man7.org/linux/man-pages/man5/proc_pid_maps.5.html)

---

## [COMMON-042] Code, Data, Heap, Stack 영역의 역할을 설명해 주세요.

답변: 

- Code 영역은 CPU가 실행할 명령어가 저장되는 영역입니다.

- Data 영역은 전역 변수와 정적 변수처럼 프로그램 실행 동안 유지되는 데이터가 저장되는 영역이고, 초기화된 전역 변수는 Data 영역에, 초기화되지 않은 전역 변수는 BSS 영역에 저장됩니다.

- Heap 영역은 실행 중 동적으로 할당되는 메모리 영역입니다. 
- Stack 영역은 함수 호출 시 생성되는 지역 변수, 매개변수, 리턴 주소 등을 저장하는 영역이며, 함수 호출이 끝나면 해당 Stack Frame이 제거됩니다.

참고 자료: [Linux man-pages - /proc/pid/maps](https://man7.org/linux/man-pages/man5/proc_pid_maps.5.html)
[Linux man-pages - pthreads(7)](https://man7.org/linux/man-pages/man7/pthreads.7.html)
---

## [COMMON-043] Stack과 Heap의 차이에 대해 설명해 주세요.

답변: Stack은 함수 호출과 관련된 메모리 영역입니다. 지역 변수, 매개변수, 리턴 주소 등이 저장되고, 함수 호출과 반환에 따라 자동으로 할당되고 해제됩니다. 관리가 단순하고 빠르지만 크기가 제한적이빈다.

Heap은 실행 중 필요한 메모리를 동적으로 할당하는 영역입니다. Stack보다 유연하게 사용할 수 있자만, 할당과 해제 비용이 더 크고 잘못 관리하면 메모리 누수가 발생할 수 있습니다.

참고 자료: 

---

## [COMMON-044] Stack Overflow가 발생하는 이유를 설명해 주세요.

답변: Stack Overflow는 스레드에 할당된 Stack 영역을 모두 사용했을 때 발생합니다. 특히, 재귀 호출이 깊어질 때 주로 발생하는 데요,

함수가 호출될 때마다 Stack Frame이 쌓이는데, 종료 조건이 없거나 재귀 깊이가 깊으면 Stack Frame이 계속 쌓여서 제한된 Stack 크기를 초과하게 됩니다.

참고 자료: [Microsoft Learn - Debugging a Stack Overflow](https://learn.microsoft.com/en-us/windows-hardware/drivers/debugger/debugging-a-stack-overflow)

---

## [COMMON-045] Memory Leak이 무엇이고, 어떤 상황에서 발생할 수 있는지 설명해 주세요.

답변: Memory Leak은 더 이상 사용하지 않는 메모리가 해제되지 않고 계속 남아 있는 상황을 말합니다. 메모리 누수가 누적되면 사용 가능한 메모리가 줄어들고, 장기적으로 성능 저하나 Out of Memory로 이어질 수 있습니다.

C/C++처럼 직접 메모리를 관리하는 언어에서는 할당한 메모리를 해제 하지 않을 때 발생할 수 있고, GC가 있는 언어에서도 객체에 대한 참조가 계속 남이 있으면 GC 대상이 되지 않아 메모리 누수가 발생할 수 있습니다.



참고 자료: [Microsoft Learn - Find memory leaks with the CRT library](https://learn.microsoft.com/en-us/cpp/c-runtime-library/find-memory-leaks-using-the-crt-library)
[IBM - Memory Leak Diagnosis on AIX](https://www.ibm.com/support/pages/memory-leak-diagnosis-aix)

---

## [COMMON-046] 가상 메모리가 무엇이고, 왜 필요한지 설명해 주세요.

답변: 가상 메모리는 프로세스가 실제 물리 메모리를 직접 사용하는 것이 아니라, 독립적인 가상 주소 공간을 사용하는 것처럼 보이게 하는 메모리 관리 기법입니다.

가상 메모리를 사용하면 각 프로세스에 독립적인 주소 공간을 제공할 수 있어 메모리 보호와 프로세스 격리가 가능해집니다. 또한 실제 물리 메모리보다 큰 주소 공간을 제공할 수 있고, 필요한 페이지를 필요할 때만 메모리에 올리는 방식도 사용할 수 있습니다.

참고 자료:

---

## [COMMON-047] 페이징이 무엇이고, 페이지와 프레임의 차이를 설명해 주세요.

답변: 페이징은 가상 메모리와 물리 메모리를 고정된 크기의 블록으로 나누어 관리하는 방식입니다.

페이지의 경우 가상 주소 공간, 프레임의 경우 물리 메모리의 블록입니다.

프로세스가 사용하는 가상 주소는 페이지 번호와 오프셋으로 나뉘고, 페이지 테이블을 통해 해당 페이지가 물리 메모리의 어떤 프레임에 매핑되는지 확인하는 과정으로 흘러가게 됩니다.

여기서 페이징을 사용하면 연속된 물리 메모리 공간이 없어도 프로세스의 가상 주소 공간을 관리할 수 있고, 외부 단편화 문제를 줄일 수 있습니다.

참고 자료: [Linux Kernel Docs - Page Tables](https://docs.kernel.org/mm/page_tables.html)
[Microsoft Learn - Virtual Address Spaces](https://learn.microsoft.com/en-us/windows-hardware/drivers/gettingstarted/virtual-address-spaces)

---

## [COMMON-048] Page Fault가 발생했을 때 처리 과정을 설명해 주세요.

답변: Page Fault는 프로세스가 접근하려는 페이지가 현재 물리 메모리에 없거나 접근할 수 없는 상태일 때 발생합니다.

처리 과정에 대해서는 우선 Page Fault가 발생하면 CPU는 예외를 발생시키고 운영체제의 Page Fault Handler로 제어를 넘깁니다. 여기서 운영체제는 해당 접근이 유효한지 확인해서, 유효하지 않은 접근이면 오류를 발생시키고, 유효한 접근이면 디스크나 backing store에서 필요한 페이지를 물리 메모리로 가져옵니다.

이후에 페이지 테이블을 갱신하고, 중단된 명령어를 다시 실핼합니다.

참고 자료: [Linux Kernel Docs - Page Tables](https://docs.kernel.org/mm/page_tables.html)
[Microsoft Learn - Working Set](https://learn.microsoft.com/en-us/windows/win32/memory/working-set)

---

## [COMMON-049] TLB가 무엇이고, TLB를 사용하면 왜 주소 변환이 빨라지는지 설명해 주세요.

답변: TLB는 Translation Lookaside Buffer의 약자로, 가상 주소를 물리 주소로 변환한 결과를 저장하는 캐시입니다.

페이징 환경에서는 가상 주소를 물리 주소로 변환하기 위해 페이지 테이블을 조회해야 합니다. 다만, 매번 메모리에 있는 페이지 테이블을 조회하면 비용이 크니까, 최근 사용한 주소 변환 정보를 TLB에 저장하는 것으로 알고 있습니다.

TLB를 통해 원하는 변환 정보가 있을 때 빠르게 물리 주소를 얻을 수 있어서 주소 변환 비용을 줄여 메모리 접근 성능을 높입니다.

참고 자료: [Linux Kernel Docs - Page Tables](https://docs.kernel.org/mm/page_tables.html)
[Linux Kernel Labs - Memory Management](https://linux-kernel-labs.github.io/refs/pull/345/merge/lectures/memory-management.html)

---

## [COMMON-050] 내부 단편화와 외부 단편화의 차이에 대해 설명해 주세요.

답변: 내부 단편화는 할당된 메모리 블록 내부에서 사용되지 않는 공간이 생기는 현상입니다. 4KB 페이지를 할당했는데 실제로 3KB만 사용한다면 나머지 1KB는 내부 단편화가 됩니다.

반면에 외부 단편화는 전체 여유 메모리 총량은 충분한데, 연속된 큰 공간이 없어서 메모리 할당에 실패하는 현상입니다. 페이징은 고정 크기 단위로 메모리를 관리하니까 외부 단편화를 줄일 수 있지만, 마지막 페이지에서 내부 단편화가 발생할 수 있습니다. 또 반대로 세그먼테이션처럼 가변 크기 단위로 관리하면 외부 단편화가 발생할 수 있습니다.

참고 자료: [Linux Kernel Docs - Page Tables](https://docs.kernel.org/mm/page_tables.html)
[UNSW OS Lecture - Virtual Memory Paging](https://cgi.cse.unsw.edu.au/~cs3231/07s1/lectures/lect10x6.pdf)

---

## [COMMON-051] 동시성과 병렬성의 차이에 대해 설명해 주세요.

답변: 동시성은 여러 작업이 동시에 진행되는 상황입니다. 하나의 CPU 코어에서도 여러 작업을 빠르게 전환하면 동시에 실행되는 것처럼 보일 수 있습니다.

병렬성은 여러 작업이 실제로 동시에 실행되는 것을 의미합니다. 일반적으로 여러 CPU 코어나 여러 프로세서가 있을 때 가능합니다.


참고 자료: [Oracle Java Tutorials - Concurrency](https://docs.oracle.com/javase/tutorial/essential/concurrency/)
[Microsoft Learn - Parallel Programming in .NET](https://learn.microsoft.com/en-us/dotnet/standard/parallel-programming/)

---

## [COMMON-052] Race Condition이 무엇이고, 어떤 상황에서 발생하는지 설명해 주세요.

답변: Race Condition은 여러 프로세서가 공유 자원에 동시에 접근하면서, 실행 순서에 따라 결과가  달리지는 상황입니다.

예를 들어 재고가 1개 남은 상품에 대해 두 명 이상의 사용자가 동시에 주문 API를 호출하면, 여러 요청이 모두 재고를 1로 읽고 각각 주문을 생성할 수 있습니다. 이 경우 실제 재고보다 많은 주문이 생성되는 문제가 생깁니다.

그래서 이런 상황이 우려되는 설계에서는 낙관적 락, 비관적 락을 적용하는 것으로 알고 있습니다.




참고 자료:

---

## [COMMON-053] Critical Section이 무엇이고, 임계 구역 문제를 해결하기 위한 조건을 설명해 주세요.

답변: Critical Section은 여러 스레드나 프로세스가 동시에 접근하면 문제가 생길 수 있는 공유 자원 접근 코드 영역입니다.

이런 임계 구역 문제를 해결하기 위해서는 
- 한번에 하나의 실행 흐름만 임계 구역에 들어갈 수 있는 Mutual Exclusion(상호 배제)
- 임계 구역이 비어 있고 들어가려는 대상이 있을 때 진입 결정이 미뤄지지 않도록 하는 Progress(진행)
- 특정 스레드나 프로세스가 무한정 기다리지 않도록 대기 시간이 제한되는 Bounded Waiting(한정 대기)

이 적용됩니다.

참고 자료:

---

## [COMMON-054] Mutex와 Semaphore의 차이에 대해 설명해 주세요.

답변: Mutex는 Mutual Exclusion의 약자로, 한 번에 하나의 스레드만 공유 자원에 접근하도록 제한하는 동기화 도구입니다. 그래서 Lock을 갖고 있던 스레드가 작업 끝나면 해제해주어야 합니다.

Semaphore는 내부적으로 카운터를 가지고 잇어서, 정해진 개수만큼의 스레드가 동시에 자원에 접근할 수 있도록 제어합니다. 카운터가 1인 Binary Semaphore는 Mutex와 비슷하게 사용할 수 있지만, 개념적으로 Semaphore는 소유자 개념이 약하고 지원 개수를 제어하는 데 일반적입니다.

참고 자료:

---

## [COMMON-055] Deadlock이 무엇이고, 발생 조건 4가지를 설명해 주세요.

답변: 데드락은 여러 프로세스나 스레드가 서로가 가진 자원을 기다리면서 더 이상 작업을 진행하지 못하는 상태입니다.

- Mutual Exclusion: 자원을 한 번에 하나의 프로세스만 사용할 수 있어야 합니다.
- Hold and Wait: 이미 자원을 가진 상태에서 다른 자원을 기다립니다.
- No Preemption: 다른 프로세스가 가진 자원을 강제로 빼앗을 수 없습니다.
- Circular Wait: 프로세스들이 순환적으로 서로의 자원을 기다립니다.

참고 자료:

---

## [COMMON-056] Deadlock을 예방하거나 회피하는 방법에 대해 설명해 주세요.

답변: 데드락을 예방하는 방법은 데드락 조건 4가지 중 하나를 사전에 깨뜨리면 됩니다. 예를 들어 자원을 한 번에 요청해서 점유 대기를 막거나, 락 획득 순서를 정해놓고 순환 대기를 막을 수 있습니다.

데드락 회피는 데드락 가능성을 보고 안전 상태에만 자원을 할당하는 은행원 알고리즘 같은 방식으로 해결한다고 알고 있습니다.

참고 자료:

---

## [COMMON-057] Thread Safe하다는 것은 어떤 의미인지 설명해 주세요.

답변: Thread Safe하다는 건 여러 스레드가 동시에 접근해도 의도한 결과가 유지되는 것입니다.

이걸 보장하려면, 공유 자원 접근을 동기화하거나, 원자적 연산, 불변 객체 사용 등 스레드마다 독립적인 상태를 사용해야 합니다.

참고 자료: [Oracle Java Tutorials - Synchronized Methods](https://docs.oracle.com/javase/tutorial/essential/concurrency/syncmeth.html)
[Oracle Java Tutorials - Memory Consistency Errors](https://docs.oracle.com/javase/tutorial/essential/concurrency/memconsist.html)

---

## [COMMON-058] Hash Table이 무엇이고, 해시 충돌을 해결하는 방법을 설명해 주세요.

답변: 해시 테이블은 키를 해시 함수에 넣어 해시 값을 기반으로 데이터를 저장하고 조회하는 자료구조입니다.

- 삽입, 삭제, 조회 O(1)
- 서로 다른 키가 같은 위치에 매핑되는 해시 충돌 가능
- 체이닝과 오픈 어드레싱으로 해결 가능
- 체이닝 -> 같은 버킷에 여러 데이터를 연결 리스트나 트리 등으로 저장하는 방식
- 오픈 어드레싱 -> 충돌이 발생한 경우 다른 빈 위치를 찾아 저장하는 방식


참고 자료: [Oracle Java Docs - HashMap](https://docs.oracle.com/javase/8/docs/api/java/util/HashMap.html)

---

## [COMMON-059] Stack, Queue, Deque의 차이와 사용 사례를 설명해 주세요.

답변:

- Stack: LIFO 구조, 함수 호출, 괄호 검사, DFS 등에 주로 사용
- Queue: FIFO 구조, 작업 대기열, BFS, 메시지 처리에 사용
- Deque: Double-Ended Queue로 양쪽 삽입 삭제 가능.

참고 자료:

---

## [COMMON-060] Tree, Binary Tree, Binary Search Tree의 차이에 대해 설명해 주세요.

답변:
- 트리: 계층적인 부모-자식 관계를 표현하는 비선형 자료구조. 루트에서 -> 여러 노드로 확장
- 바이너리 트리: 각 노드가 최대 두 개의 자식 노드를 가짐.
- 바이너리 서치 트리: 바이너리 트리의 한 종류로 왼쪽 서브트리에는 현재 노드보다 작은 값, 오른쪽 서브트리에는 큰 값을 배치하는 규칙이 있음

이 규칙을 통해서 탐색, 삽입, 삭제를 O(log N)에 수행할 수 있다. 
but, 한 쪽으로 솔리면 O(N)

참고 자료:

---

## 선택 질문

## [COMMON-061] 세그멘테이션과 페이징의 차이에 대해 설명해 주세요.

답변: 세그먼테이션은 메모리를 Code, Data, Stack처럼 논리적인 의미 단위로 나누는 방식이며, 각 세그먼트의 크기를 달리 할 수 있습니다.

반면에 페이징은 메모리를 고정된 크기의 페이지와 프레임으로 나누는 방식이고, 논리적 의미와 관계없이 일정한 크기로 나눠서 관리가 단순하고 외부 단편화를 줄일 수 있습니다.

세그먼테이션은 프로그램 구조를 반영하기 좋긴 하지만, 외부 단편화가 발생할 수 있고, 페이징은 외부 단편화를 줄이지만 내부 단편화가 발생할 수 있습니다.

참고 자료:

---

## [COMMON-062] 페이지 크기가 커지거나 작아질 때 발생하는 trade-off를 설명해 주세요.

답변: 페이지 크기가 작아지면 내부 단편화가 줄어들지만, 페이지 개수가 많아져서 테이블 크기가 커지고 주소 변환 관리 비용이 증가합니다.

페이지 크기가 커지면 페이지 테이블 크기가 줄어들 수 있고, 한 번에 더 많은 데이터를 가져와 공간 지역성을 활용하기 쉬워지지만, 내부 단편화와 메모리 낭비가 커질 수 있습니다.

참고 자료: [Linux Kernel Docs - Page Tables](https://docs.kernel.org/mm/page_tables.html)
[Microsoft Learn - Virtual Address Spaces](https://learn.microsoft.com/en-us/windows-hardware/drivers/gettingstarted/virtual-address-spaces)

---

## [COMMON-063] Thrashing이 무엇이고, 어떻게 완화할 수 있는지 설명해 주세요.

답변: Thrashing은 메모리가 부족해 Page Fault와 페이지 교체가 과도하게 발생하고, CPU가 실제 작업보다 펭지ㅣ를 가져오고 내보내는 데 대부분의 시간을 쓰는 상황입니다.

그래서 프로세스가 필요로 하는 작업 집합이 물리 메모리에 비해 너무 크면 Page Fault가 반복적으로 발생할 수 있고, 시스템 전체 성능을 떨어지게 할 수 있습니다.

해결 방법
- 실행 중인 프로세스 줄이기
- 물리 메모리 늘리기
- 프로세스에 프레임 더 할당
- Working Set을 고려해 필요한 페이지들이 메모리에 유지되도록 하기

참고 자료: [Microsoft Learn - Working Set](https://learn.microsoft.com/en-us/windows/win32/memory/working-set)
[Operating Systems: Three Easy Pieces](https://pages.cs.wisc.edu/~remzi/OSTEP/)

---

## [COMMON-064] 페이지 교체 알고리즘의 종류와 LRU의 동작 방식을 설명해 주세요.

답변: 페이지 교체 알고리즘은 물리 메모리에 빈 프레임이 없을 때 어떤 페이지를 내보낼지 결정하는 알고리즘입니다.

FIFO, Optimal, LRU, Clock 등이 있고, LRU(Least Recently Used)의 경우 말 그대로 가장 오랫동안 사용되지 않은 페이지를 교체하는데,
최근에 사용된 페이지가 가까운 미래에도 다시 사용될 가능성이 높다는 시간 지역성을 기반으로 하는 알고리즘입니다.

- Optimal: 앞으로 가장 오랫동안 사용되지 않을 페이지 교체하는 이상적 방식

참고 자료:

---

## [COMMON-065] LRU Cache를 구현한다면 어떤 자료구조를 사용할 수 있을까요?

답변: LRU Cache는 특정 Key의 값을 빠르게 찾고, 가장 오랫동안 사용되지 않은 데이터를 제거할 수 있어야 합니다.

이걸 구현하기 위해서는 Key로 노드를 O(1)에 찾을 수 있는 해시맵과 사용 순서 관리 용도로 Doubly Linked List를 사용할 수 있을 것 같습니다.

그래서 데이터 조회나 추가가 발생하면 해당 노드를 리스트의 앞쪽으로 이동시키고, 캐시 용량 초과나면 리스트의 뒤쪽에 있는 가장 오래 사용되지 않은 노드를 제거합니다.

참고 자료:

---

## [COMMON-066] 캐시 지역성이 무엇인지 시간 지역성과 공간 지역성으로 나누어 설명해 주세요.

답변: 캐시 지역성은 프로그램이 특정 데이터에 접근했을 때, 가까운 미래에 같은 데이터나 인접한 데이터에 다시 접근할 가능성이 높다는 성질입니다.

시간 지역성은 최근에 접근한 데이터가 다시 접근될 가능성이 높다는 거고, 공간 지역성은 특정 데이터에 접근하면 그 주변 주소 데이터도 접근될 가능성이 높다는 의미로 알고 있습니다.

참고 자료:

---

## [COMMON-067] 2차원 배열을 행 우선으로 탐색할 때와 열 우선으로 탐색할 때 성능 차이가 날 수 있는 이유를 설명해 주세요.

답변: 메모리 배치와 캐시 지역성에서 차이가 날 것 같습니다.

배열은 보통 메모리에 연속적으로 배치되는데, 행 우선은 저장되는 경우 원소들이 인접한 메모리 주소에 위치해서 공간 지역성을 잘 활용할 수 있습니다.

반대로 열 우선 접근 시에는 먼 위치를 반복해서 접근할 수 있어서 캐시 미스가 발생할 수 있습니다.

참고 자료:

---

## [COMMON-068] Spin Lock의 장점과 단점에 대해 설명해 주세요.

답변: Spin Lock은 Lock을 얻을 때까지 스레드가 잠들지 않고 반복적으로 Lock 상태를 확인하는 방식입니다.

- Lock 대기 시간이 짧을수록 스레드 컨텍스트 스위치 비용 감소
- Lock 대기 시간이 긴 경우, CPU를 계속 사용하면서 대기하기 때문에 자원 낭비. 

> 임계 구역이 짧고 대기 시간이 짧을 것으로 예상되는 상황에 Spin Lock 적합

참고 자료: [Linux Kernel Docs - Locking lessons](https://docs.kernel.org/locking/spinlocks.html)
[Linux Kernel Docs - Lock types and their rules](https://docs.kernel.org/locking/locktypes.html)

---

## [COMMON-069] Optimistic Lock과 Pessimistic Lock의 차이를 설명해 주세요.

답변: 비관적 락(Pessimistic Lock)은 충돌 발생 가능성이 높다고 보고, 데이터를 읽거나 수정할 때 미리 Lock을 거는 방식입니다. 안정성이 높지만, 다른 트랜잭션이나 스레드가 Lock 해제를 기다려야 해서 속도가 느릴 수 있습니다.

반면에 낙관적 락(Optimistic Lock)은 충돌이 자주 발생하지 않는다고 보고, 작업을 진행하고 나서 수정 시점에 충돌 여부를 확인합니다. 보통 버전 값 같은 걸로 변경 여부를 확인해서 업데이트를 합니다.

그래서 비관락이 충돌이 잦게 발생하면 정합성 면에서 유리하고, 낙관 락이 충돌이 적을 땐 성능상 유리해지는 걸로 알고 있습니다. 

추가로 서버 인스턴스가 여러 개인 분산 환경에선 애플리케이션 내부 락만으론 한 서버에서 락을 잡아도 다른 서버의 요청까지 막기가 힘들어서 동시성 문제 해결이 어렵고, 레디스 같은 걸 활용해서 모든 서버가 확인할 수 있는 기준을 두고 동시성을 제어해야 하는 것으로 알고 있습니다. 

참고 자료:

---

## [COMMON-070] Lock-free와 Wait-free의 차이에 대해 설명해 주세요.

답변: 

- Lock free와 Wait free는 전통적 Lock 없이 동시성을 제어하는 비차단 동기화 방식
- Lock free는 여러 스레드 중 적어도 하나의 스레드는 계속 진행할 수 있음을 보장함. -> 특정 스레드는 반복적으로 실패해 오래 기다릴 수 있음
- Wait free는 모든 스레드가 유한한 단계 안에 작업을 완료할 수 있음을 보장함. -> Wait free가 Lock free 보다 더 강한 보장 조건

참고 자료: [cppreference - std::atomic<T>::is_lock_free](https://en.cppreference.com/w/cpp/atomic/atomic/is_lock_free)
[cppreference - compare_exchange](https://en.cppreference.com/w/cpp/atomic/atomic/compare_exchange)

---

## [COMMON-071] Atomic 연산이 무엇이고, 동시성 문제 해결에 어떻게 사용되는지 설명해 주세요.

답변: Atomic 연산은 더 이상 쪼갤 수 없는 단위로 실행되는 연산입니다. 여러 스레드가 동시에 접근해도 중간 상태가 보이지 않고, 연산이 완료된 상태만 관찰됩니다.

상태 변경이나, Compare-And-Swap 같은 작업에서 락 없이 동시성 문제를 줄이는 데 사용됩니다.

참고 자료:

---

## [COMMON-072] volatile 키워드가 어떤 의미를 가지는지 설명해 주세요.

답변: volatile은 여러 스레드 간 변수 값의 가시성을 보장하는 키워드로 알고 있습니다.

일반 변수의 경우에는 스레드가 캐시된 값을 사용하게 되면, 한 스레드가 변경한 값을 다른 스레드가 바로 보지 못할 수 있는데, volatile을 사용하면 변수의 읽기, 쓰기에 대한 메모리 가시성을 보장합니다.

> 복합 연산의 경우에는 원자성 보장 못함 ex) count++

참고 자료: [Java Language Specification - Chapter 17. Threads and Locks](https://docs.oracle.com/javase/specs/jls/se8/html/jls-17.html)
[Oracle Java Tutorials - Memory Consistency Errors](https://docs.oracle.com/javase/tutorial/essential/concurrency/memconsist.html)

---

## [COMMON-073] Thread Pool이 무엇이고, 스레드 수는 어떤 기준으로 정할 수 있는지 설명해 주세요.

답변: Thread Pool은 스레드를 미리 만들어두고 재사용하는 방식입니다. 스레드를 요청마다 새로 만들면 생성 비용과 컨텍스트 스위칭 비용이 커지기 때문에, 풀을 통해 스레드 수를 제한하고 안정적으로 작업을 처리합니다.

스레드 수는 CPU-bound인지 I/O-bound인지에 따라 다르게 봅니다. CPU-bound 작업은 코어 수와 비슷하게 잡는 경우가 많고, I/O-bound 작업은 대기 시간이 많기 때문에 코어 수보다 더 많이 둘 수 있습니다.

또 리틀의 법칙을 이용해 `처리량 × 평균 응답 시간`으로 동시에 처리 중인 작업 수를 추정할 수 있습니다. 다만 실제로는 DB 커넥션 풀이나 외부 API 제한 같은 다른 병목도 함께 고려해야 하는 것으로 알고 있습니다.

참고 자료: [Oracle Java Docs - ThreadPoolExecutor](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/ThreadPoolExecutor.html)
[Microsoft Learn - Thread Pools](https://learn.microsoft.com/en-us/windows/win32/procthread/thread-pools)

---

## [COMMON-074] Heap 자료구조가 무엇이고, Priority Queue와 어떤 관계가 있는지 설명해 주세요.

답변: Heap은 완전 이진 트리 형태를 가지면서 부모와 자식 사이에 우선순위 규칙을 만족하는 자료구조입니다.

Heap은 최솟값이나 최댓값을 빠르게 꺼낼 수 있기 때문에 Priority Queue를 구현하는 데 자주 사용됩니다. 

- 삽입 삭제 O(log N)
- 최상위 원소 조회 O(1)

참고 자료:

---

## [COMMON-075] Red-Black Tree가 무엇이고, 왜 균형 이진 탐색 트리가 필요한지 설명해 주세요.

답변: Red-Black Tree는 스스로 균형을 유지하는 이진 탐색 트리입니다. 각 노드에 Red 또는 Black 색을 부여하고, 특정 규칙을 유지하도록 회전과 색 변경을 수행합니다.

일반 Binary Search Tree는 데이터가 정렬된 순서로 삽입되면 한쪽으로 치우칠 수 있어서 이런 경우에 삽입, 삭제가 O(N)까지 갈 수 있는데
이진 탐색 트리는 트리 높이를 O(log N) 수준으로 유지해 탐색, 삽입, 삭제 성능을 안정적으로 보장하기 위해 사용합니다.

참고 자료:

---

## [COMMON-076] Graph를 인접 행렬과 인접 리스트로 표현할 때의 장단점을 설명해 주세요.

답변: 인접 행렬은 정점 간 연결을 2차원 배열로 표시하는데, 두 정점 간 연결을 O(1)로 확인할 수 있습니다. 
다만, 정점이 많으면 간선이 적어도 V * V 크기의 메모리가 필요해서 메모리 낭비가 발생할 수 있습니다.

인접 리스트의 경우에는 연결된 정점 목록을 저장하는 방식이라, 실제 존재하는 간선만 저장해서 희소 그래프에서 메모리 효율이 좋지만, 특정 두 정점의 연결 여부를 확인하려면 리스트 탐색 비용이 들 수 있습니다.


참고 자료:

---

## [COMMON-077] BFS와 DFS의 차이와 각각 적합한 상황을 설명해 주세요.

답변: 너비 우선 탐색(BFS)는 가까운 정점부터 탐색하는 방식으로, Queue를 쓰고, 가중치 없는 그래프의 최단 거리 구할 때 사용합니다.
깊이 우선 탐색(DFS)는 한 경로로 끝까지 탐색하고 돌아오는 방식으로, 재귀나 Stack로 보통 구현하고, 백트래킹, 사이클 관련에서 자주 사용됩니다. 

참고 자료:

---

## [COMMON-078] 정렬 알고리즘 중 Quick Sort와 Merge Sort를 비교해 주세요.

답변: 퀵 솔트는 피벗을 기준으로 작은 값과 큰 값을 나누어 정렬하는 분할 정복 알고리즘입니다.
보통은 O(N log N)이지만, 피벗 선택에 따라 최악의 경우 O(N 제곱)까지 갈 수 있습니다.

머지 소트는 배열을 반으로 나누고 정렬한 뒤에 병합하는 분할 정복 알고리즘입니다. O(N log N)으로 안정적이지만, 병합 과정에서 추가 메모리가 필요합니다.

참고 자료:

---

## [COMMON-079] Stable Sort가 무엇이고, 어떤 상황에서 중요한지 설명해 주세요.

답변: 안정 정렬은 정렬 기준이 같은 원소들의 기존 상대적 순서를 유지하는 정렬입니다.

예를 들어 학생 데이터를 이름순으로 정렬하고 다시 반 번호로 정렬할 때 Stable Sort를 사용하면 같은 반 안에서 기존 이름 순 정렬 결과가 유지될 수 있습니다.


참고 자료: [Python Docs - Sorting HOWTO](https://docs.python.org/3/howto/sorting.html)

---

## [COMMON-080] 대용량 데이터를 메모리에 모두 올릴 수 없을 때 정렬하는 방법을 설명해 주세요.

답변: 
```
전체 데이터 -> 작은 청크로 나누고 메모리에 올림 -> 정렬 -> 임시 파일로 저장 -> 여러 개 정렬된 임시 파일 병합 
```

디스크 I/O를 활용해서, 메모리보다 큰 데이터를 정렬할 수 있다.

참고 자료:
