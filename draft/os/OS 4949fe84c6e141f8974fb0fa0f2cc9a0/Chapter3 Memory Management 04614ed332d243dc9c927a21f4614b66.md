# Chapter3 Memory Management

# No Memory Abstraction

![Chapter3%20Memory%20Management%2004614ed332d243dc9c927a21f4614b66/Untitled.png](Chapter3%20Memory%20Management%2004614ed332d243dc9c927a21f4614b66/Untitled.png)

(a) 메모리 0번지에는 인터럽트 벡터 테이블이 있어야 한다. 

(b) 한번에 하나씩 실행할 수 밖에 없음

(c) interrupt vector table이 메모리에 0번지에 존재해야함 

user program ⇒ interrupt vector table ⇒ OS in ROM

# Address Spaces

- Pyhsical address space : dram
- Logical address space: a process's view of its own memory

# Address Generation

![Chapter3%20Memory%20Management%2004614ed332d243dc9c927a21f4614b66/Untitled%201.png](Chapter3%20Memory%20Management%2004614ed332d243dc9c927a21f4614b66/Untitled%201.png)

- 컴파일러는 메모리에 적재되는 주소를 모른다. 그래서 0 번지를 기준으로 만든다.
- 프로그램은 자기만의 메모리 주소공간이 있다.
- 적재된 메모리 주소에 따라 프로그램 상에 명시된 주소를 다시 바꾸어야 한다.

# Multiple Programs	Without Memory

![Chapter3%20Memory%20Management%2004614ed332d243dc9c927a21f4614b66/Untitled%202.png](Chapter3%20Memory%20Management%2004614ed332d243dc9c927a21f4614b66/Untitled%202.png)

(a)는 제대로 된 위치에서 시작하고, (0번에서 시작했으니)

(c)는 다른 위치에서 시작하면 엉뚱한 주소로 jump하게 된다. 

- 처음 명령은 보통 점프로 시작한다.

어떻게 해결하지?

- 컴파일할때 주소를 미리 알던가
- 메모리에 적재 될 떄 주소를 모두 바꾼다.

# Partitioning

![Chapter3%20Memory%20Management%2004614ed332d243dc9c927a21f4614b66/Untitled%203.png](Chapter3%20Memory%20Management%2004614ed332d243dc9c927a21f4614b66/Untitled%203.png)

- Equal-size partitioning
    - Because all partitions are of equal size, it does not matter which partition is used.
    - If dynamic relocation is impossible, target partition is decided at compile time. If possible, target partition is decided at loading time
- Unequal-size partitioning
    - Can assign each process to the smallest partition within which it will fit
    - Queue for each partition
    - If dynamic relocation is possible, processes are assigned in such a way as to minimize wasted memory within a partition

Example of fixed partitioning 

파티션을 왜 미리 해야하지? 

미리 파티셔닝을 하면 어디 갈지 정해놓고 미리 코딩을 할 수 있다. 리로케이션 할 필요도 없고 

# Placement with Partitioning

![Chapter3%20Memory%20Management%2004614ed332d243dc9c927a21f4614b66/Untitled%204.png](Chapter3%20Memory%20Management%2004614ed332d243dc9c927a21f4614b66/Untitled%204.png)

- Physical memory is divided into a number of fixed-sized partitions
- 컴파일 할때 어느 위치에 적재할지 결정하고 주소를 결정 (절대 주소)하는 방법이 있다.
    - 정해진 파티션에서만 프로그램들이 대기해야하며, 비효율 적이다.

Problem 

- Internal fragmentation
- inefficient

# Dynamic Program Relocation

![Chapter3%20Memory%20Management%2004614ed332d243dc9c927a21f4614b66/Untitled%205.png](Chapter3%20Memory%20Management%2004614ed332d243dc9c927a21f4614b66/Untitled%205.png)

- program is dynamically "relocated" at execution time.
- Base (also called relocation) Register + Limit Register
- Limit register : size of program
    - 500 보다 크면 os는 스스로 인터럽트를 발생시켜 핸들러를 호출 시킴
- Base Register: 프로그램이 적재된 기준 주소
    - 프로그램 상의 모든 주소값에 base register 의 값을 더함
- 이 기능을 하드웨어적으로는 아주 쉽게, 빠르게 구현할 수 있다.
- 이런 구조라면 partitioning 도 미리 할 필요가 있는가?

![Chapter3%20Memory%20Management%2004614ed332d243dc9c927a21f4614b66/Untitled%206.png](Chapter3%20Memory%20Management%2004614ed332d243dc9c927a21f4614b66/Untitled%206.png)

# Base and Limit Registers

# Placement Algorithm with and without

partitioning을 해두면 파티션마다 큐가 필요하다. 

dynamic relocation을 하면 큐를 쓰더라도 하나만 있으면 된다.

partitioning을 할 필요가 없다. 

# Dynamic Partitioning

# Example Of Dynamic Partitioning

![Chapter3%20Memory%20Management%2004614ed332d243dc9c927a21f4614b66/Untitled%207.png](Chapter3%20Memory%20Management%2004614ed332d243dc9c927a21f4614b66/Untitled%207.png)

- swapping
- memory fragmentation 을 어떻게 해결할 수 있을까?
- 이거 심각한 문제임

# Dynamic Partitioning Placement Algorithm

- Operating system decide which free block to allocate to a process
    - Best-fit algorithm
    - First-fit algorithm
    - Next-fit
    - Worst-fit

# Example of various placement algorithm

![Chapter3%20Memory%20Management%2004614ed332d243dc9c927a21f4614b66/Untitled%208.png](Chapter3%20Memory%20Management%2004614ed332d243dc9c927a21f4614b66/Untitled%208.png)

- first fit: 위에서 부터 찾는다
    - 아래가 비어있으니 큰 프로그램은 아래서 찾으면 잘 찾을 수 있다.
- next fit: 마지막에 할당된 블럭에서 부터 찾는다.
    - 골고루 써서 큰 프로그램은 공간을 찾기가 어렵다.
- best fit: 모든 빈 공간을 보고 가장 최적의 공간을 찾는다.
    - x 보다 크거나 같으면서 가장 작은 것
    - 이 친구가 쓰고 남은 건 엄청 작게 되서 재활용 될 가능성이 없다.
    - 구현하기는 조금 어려움
- worst fit: 모든 빈 공간을 보고 가장 널널한 공간을 찾느다.
    - x보다 큰 것중에 가장 큰 곳
    - 미리 빈 공간을 크기 순으로 정렬을 해두는 식으로 구현함 (binary heap)
    - 할당한 뒤에도 큰 공간이 남아 있어 이 곳은 재활용 될 가능성이 높다.

뭐가 좋다라고 말할 수 없음 케바케임 

어쨋거나 fragmentation 은 심각한 문제라 해결해야함 

다 옮기는건 굉장히 비싼 작업임 

first 와 next는 빠르다. 

best worst 는 다 찾아봐야 해서 오래걸린다. 

지금은 이런거 안쓰고 가상 메모리를 쓴다! 

# Solutions for Fragmentation ' 
1) Coalescing

![Chapter3%20Memory%20Management%2004614ed332d243dc9c927a21f4614b66/Untitled%209.png](Chapter3%20Memory%20Management%2004614ed332d243dc9c927a21f4614b66/Untitled%209.png)

- 빈공간은 합치자.
- 당연한데 컴퓨터는 이렇게 프로그래밍을 해야 동작할 거 아님

# Solution for Fragmentation
2) Compaction

![Chapter3%20Memory%20Management%2004614ed332d243dc9c927a21f4614b66/Untitled%2010.png](Chapter3%20Memory%20Management%2004614ed332d243dc9c927a21f4614b66/Untitled%2010.png)

- Dynamic relocation 기능이 있을 때 가능한 작업 '
- 프로그램을 옮겨야 하는데 엄청난 오버헤드를 가짐
- 프로그램 자체도 옮기는 동안 컴퓨터가 멈춰있어야 할 것
- 컴팩션 알고리즘이 생각보다 어려움

![Chapter3%20Memory%20Management%2004614ed332d243dc9c927a21f4614b66/Untitled%2011.png](Chapter3%20Memory%20Management%2004614ed332d243dc9c927a21f4614b66/Untitled%2011.png)

- 가장 조금만 옮기는 방법을 찾는 것이 어려움

## Problem of Compaction

- Hard to find optimal compaction algorithm
- System activity halts during compaction

## When to execute Compaction

- When memory utilization decreased under lower bound.
- When memory allocation fails Periodically

컴팩션을 수행하지 않고 스왑인 스왑 아웃을 하는 방법도 있다.

컴팩션은 가능하면 시행하지 않으려고 한다. 

# Swapping'

![Chapter3%20Memory%20Management%2004614ed332d243dc9c927a21f4614b66/Untitled%2012.png](Chapter3%20Memory%20Management%2004614ed332d243dc9c927a21f4614b66/Untitled%2012.png)

하드디스크는 시퀀셜 메모리 엑세스는 빠르다.

더 많은 프로세스를 실행할 수 있게 한다. 

컴퓨터 시스템의 활용도를 높이기 위해 

이것도 성능면에서 그래도 만족 스럽지 못하다. 

프로그램이 동적으로 더 많은 메모리를 요구 할 경우 어떻게 할까? 

![Chapter3%20Memory%20Management%2004614ed332d243dc9c927a21f4614b66/Untitled%2013.png](Chapter3%20Memory%20Management%2004614ed332d243dc9c927a21f4614b66/Untitled%2013.png)

프로그램의 크기를 키울려면 스택 영역이 위로 올라가야할 것

code base, data base stack base extra segment base 등 CPU는 segment 단위로 리로케이션 할 수 있도록 CPU가 레지스터를 가지고 있다. 

지금은 그냥 가상메모리면 충분하다. 

# Overlay

![Chapter3%20Memory%20Management%2004614ed332d243dc9c927a21f4614b66/Untitled%2014.png](Chapter3%20Memory%20Management%2004614ed332d243dc9c927a21f4614b66/Untitled%2014.png)

위의 모든 작업은 dram 보다 큰 프로그램은 사용할 수 없었다. 

메모리보다 큰 프로그램을 사용하기 위한 방법

함수를 호출하면 overlay 매니저를 호출 

함수가 메모리에 없다면 가져와서 적재

메모리에 없다면? 없다는 걸 어떻게 아니? 

원래는 프로그램을 모두 , 순차적으로 적재했어야 했다. 

이게 큰 제약이었음 이거 떄문에 fragmentation 이 생김

여전히 큰 문제가 있는데' 

# Memory Management with Bitmaps

![Chapter3%20Memory%20Management%2004614ed332d243dc9c927a21f4614b66/Untitled%2015.png](Chapter3%20Memory%20Management%2004614ed332d243dc9c927a21f4614b66/Untitled%2015.png)

(b) bitmap으로 어디를 쓰고 있는지 관리를 함 

(c) linked list로 관리를 한다. 

bitmap이 빠를 수도 linked list가 빠를 수도 있다. 

bitmap이 효율적인 자료구조임 

연속적인 빈 공간을 찾을 때도 효율적임 

## Memory Management with Linked List

![Chapter3%20Memory%20Management%2004614ed332d243dc9c927a21f4614b66/Untitled%2016.png](Chapter3%20Memory%20Management%2004614ed332d243dc9c927a21f4614b66/Untitled%2016.png)

# Introduction to Virtual Memory

![Chapter3%20Memory%20Management%2004614ed332d243dc9c927a21f4614b66/Untitled%2017.png](Chapter3%20Memory%20Management%2004614ed332d243dc9c927a21f4614b66/Untitled%2017.png)

- 정확히 무한대는 아님 (32bit 라면 2^32)

# Virtual Memory Concept

- Hide all physical aspect of memory from user
    - Memory is a logically unbounded
    - Only physical memory at any one time.
- 운영체제 코드 데이터도 나의 데이터, 언제던지 호출할 수 있다.
    - 마음대로 할 수 있는건 아니고, 반드시 인터럽트의 형태로 호출 할 수 있다.

![Chapter3%20Memory%20Management%2004614ed332d243dc9c927a21f4614b66/Untitled%2018.png](Chapter3%20Memory%20Management%2004614ed332d243dc9c927a21f4614b66/Untitled%2018.png)

# Paging

- A process's virtual address space is divided into equal sized pages
- A virtual address is a pair (p, o)

![Chapter3%20Memory%20Management%2004614ed332d243dc9c927a21f4614b66/Untitled%2019.png](Chapter3%20Memory%20Management%2004614ed332d243dc9c927a21f4614b66/Untitled%2019.png)

![Chapter3%20Memory%20Management%2004614ed332d243dc9c927a21f4614b66/Untitled%2020.png](Chapter3%20Memory%20Management%2004614ed332d243dc9c927a21f4614b66/Untitled%2020.png)

## Frame

- Physical memory is divided into equal sized frames.
    - size of page = size of frame
- Physical memory address is a pair *(f, o).*

    ![Chapter3%20Memory%20Management%2004614ed332d243dc9c927a21f4614b66/Untitled%2021.png](Chapter3%20Memory%20Management%2004614ed332d243dc9c927a21f4614b66/Untitled%2021.png)

![Chapter3%20Memory%20Management%2004614ed332d243dc9c927a21f4614b66/Untitled%2022.png](Chapter3%20Memory%20Management%2004614ed332d243dc9c927a21f4614b66/Untitled%2022.png)

- Pages are mapped to frames.
- Pages are contiguous in a virtual address space but
    - the corresponding frames are arbitrarily located in physical memory, and
    - not all pages are mapped to

일부는 디스크에, 일부는 프레임에 적재 되어있음

프로그램의 페이지 단위로 빈 프로그램에 적재해서 실행한다. 

## Virtual address translation'

![Chapter3%20Memory%20Management%2004614ed332d243dc9c927a21f4614b66/Untitled%2023.png](Chapter3%20Memory%20Management%2004614ed332d243dc9c927a21f4614b66/Untitled%2023.png)

- (p,o)에 접근하려 하면 실제로는 (f,o)에 접근해야 한다.
- CPU가 페이지 테이블을 가지고 있다.
- 이런 변환을 하드웨어가 굉장히 잘함
- 이런 회로를 MMU 회로라고함

이전의 CPU는 가상메모리가 없었음 

프로그램이 생각하는 virtual address space는 실제 dram 보다 더 크다. 

# of page table entry = # of page = 2^20 * (4byte) = 4mb

프로세스마다 페이지 테이블이 하나씩 필요하다. 이게 CPU에 있어야한다. 

 문맥교환을 할때마다 변경을 해야함 

그러니까 DRAM 에 넣어놓자.

![Chapter3%20Memory%20Management%2004614ed332d243dc9c927a21f4614b66/Untitled%2024.png](Chapter3%20Memory%20Management%2004614ed332d243dc9c927a21f4614b66/Untitled%2024.png)

- One table for each process - part of process’s
- contents
    - Flags: valid/invalid (also called resident) bit, dirty bit, reference (also called clock or used) bit
    - page frame number

page 테이블의 밸리드 플래그가 1이 아니면 디스크의 스왑 영역에 있음 

메모리 접근을 두번 해야한다. 

페이지 테이블을 메모리에 넣었기 때문에 그렇다. 

해결해야함

# Example

![Chapter3%20Memory%20Management%2004614ed332d243dc9c927a21f4614b66/Untitled%2025.png](Chapter3%20Memory%20Management%2004614ed332d243dc9c927a21f4614b66/Untitled%2025.png)

# Paging: Translation Lookaside Buffer**

![Chapter3%20Memory%20Management%2004614ed332d243dc9c927a21f4614b66/Untitled%2026.png](Chapter3%20Memory%20Management%2004614ed332d243dc9c927a21f4614b66/Untitled%2026.png)

- Problem - Each (virtual) memory reference requires two memory references!
- Solution - Translation Lookaside Buffer

CPU안에 page table을 변환했던 기록을 넣어두자.

TLB는 CPU안에 하나만 있고 문맥변환을 할때 초기화 된다. 

90% 이상의 주소변환이 TLB를 통해 이루어진다. 

# Paging: TLB

![Chapter3%20Memory%20Management%2004614ed332d243dc9c927a21f4614b66/Untitled%2027.png](Chapter3%20Memory%20Management%2004614ed332d243dc9c927a21f4614b66/Untitled%2027.png)

- valid 가 0이면 swap 영역의 block 번호가 저장됨 (MIPS만)
- valid 가 0이면 쓰면 안됨 (INTEL)

# TLB: Mips R2000

![Chapter3%20Memory%20Management%2004614ed332d243dc9c927a21f4614b66/Untitled%2028.png](Chapter3%20Memory%20Management%2004614ed332d243dc9c927a21f4614b66/Untitled%2028.png)

- tag에 p가 있느냐는 한번에 검사 Fully associatied (연관 메모리)
- 바로 물리 주소로 변환됨
- 없다면 page table 로 가서 가져오고 TLB에도 기록한다. (교체정책에 의해 교체)
- p를 얻으면 캐시를 먼저 본다.
- 없으면 메인메모리에 접근

# Paging: Protection and Sharing

- Protection
    - Protection is specified per page
- Sharing
    - Sharing is done by pages in different processes mapped to the same

![Chapter3%20Memory%20Management%2004614ed332d243dc9c927a21f4614b66/Untitled%2029.png](Chapter3%20Memory%20Management%2004614ed332d243dc9c927a21f4614b66/Untitled%2029.png)

프로그램 하나를 여러번 실행하면 별도의 프로세스

이 두 프로세스는 코드영역을 공유한다. 

코드는 절대로 변경되지 않는다. 

# Summary of Paging

- Partition memory into small equal fixed-size chunks and divide each process into the same size chunks
- The chunks of a process are called pages and chunks of memory are called frames
- Operating system maintains a page table for each process
    - Contains the frame location for each page in the process
    - Memory address consist of a page number and offset within the page

없는 페이지에 접근하면 어떻게 할 것인가?

# Assginment of Process Pages to Free

![Chapter3%20Memory%20Management%2004614ed332d243dc9c927a21f4614b66/Untitled%2030.png](Chapter3%20Memory%20Management%2004614ed332d243dc9c927a21f4614b66/Untitled%2030.png)

![Chapter3%20Memory%20Management%2004614ed332d243dc9c927a21f4614b66/Untitled%2031.png](Chapter3%20Memory%20Management%2004614ed332d243dc9c927a21f4614b66/Untitled%2031.png)

- virtuual address space는 연속적이지만 프레임은 비연속적으로 메모리에 적재될 수 있다.

![Chapter3%20Memory%20Management%2004614ed332d243dc9c927a21f4614b66/Untitled%2032.png](Chapter3%20Memory%20Management%2004614ed332d243dc9c927a21f4614b66/Untitled%2032.png)

Figure 7.10 Data Structures for the Example of Figure 7.9 at the Time Epoch (f)

# Virtual Memory - Paging

![Chapter3%20Memory%20Management%2004614ed332d243dc9c927a21f4614b66/Untitled%2033.png](Chapter3%20Memory%20Management%2004614ed332d243dc9c927a21f4614b66/Untitled%2033.png)

![Chapter3%20Memory%20Management%2004614ed332d243dc9c927a21f4614b66/Untitled%2034.png](Chapter3%20Memory%20Management%2004614ed332d243dc9c927a21f4614b66/Untitled%2034.png)

- virtual address space 를 만든다 == page table을 만든다.
- page table의 상위는 운영체제의 코드 데이터를 가리키게 세팅된다.
- 안쓰는 주소공간을 파일에 매핑해서 사용할 수 있다.

![Chapter3%20Memory%20Management%2004614ed332d243dc9c927a21f4614b66/Untitled%2035.png](Chapter3%20Memory%20Management%2004614ed332d243dc9c927a21f4614b66/Untitled%2035.png)

# Structure of Page Table Entry

![Chapter3%20Memory%20Management%2004614ed332d243dc9c927a21f4614b66/Untitled%2036.png](Chapter3%20Memory%20Management%2004614ed332d243dc9c927a21f4614b66/Untitled%2036.png)

present == valid 

caching disabled = 이 페이지의 내용은 캐싱하지 마라

입출력 데이터를 저장하는 메모리 공간

라이트 백 캐쉬의 경우 메모리에 저장을 안할 수 있으니 출력할떄 엉뚱한 데이터를 출력할 것임

referenced bit : 이 페이지의 내용을 한번이라도 접근했다. == 물리 주소로 변환을 했다. 그려면 1로 세팅한다. TLB도 가지고 있다. 

page table에서 ref가 0인걸 교체하는 것이 좋으니까! 

CPU가 서비스하는 것임 이걸 os가 써먹음

modified: 이 페이지의 내용을 변경했다. 

명령어를 보고 메모리의 내용이 변환됬는지 확인을 하고 mod 비트를 변경한다. 

TLB에도 이것이 세팅됨 

1이라면 디스크에 적어야한다. 아니라면 그대로 두면됨

CPU가 서비스하는 것임 

Protection: 이 페이지의 내용이 read write execution 가능하다. 권한 설정 

운영체제가 미리 권한을 세팅해준다. 

문제가 있으면 바로 찾아낼 수 있도록 

# Speeding Up Paging

### Paging implementation issues:

- The mapping from virtual address to physical address must be
- If the virtual address space is large, the page table will be

# Translation Lookaside Buffers

![Chapter3%20Memory%20Management%2004614ed332d243dc9c927a21f4614b66/Untitled%2037.png](Chapter3%20Memory%20Management%2004614ed332d243dc9c927a21f4614b66/Untitled%2037.png)

- 한번 페이지에 접근하면 다시 접근할 확률이 높다. (Locality)
- 몇 개의 코드 데이터를 집중적으로 접근할 가능성이 높다.
- 엔트리는 몇 안되도 hit율은 매우 높다.

# Problem: Too Large Page Table