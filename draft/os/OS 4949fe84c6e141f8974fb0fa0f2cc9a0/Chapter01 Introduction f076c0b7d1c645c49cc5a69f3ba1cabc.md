# Chapter01. Introduction

---

# What Is An Operating System(1)

A modern computer consists of:

- One or more processors
- Main memory
- Disks
- Printers
- Various input/output devices

Managing all these components requires a layer of software - the **operating system**

---

# What Is An Operating System(2)

[Chapter01%20Introduction%20f076c0b7d1c645c49cc5a69f3ba1cabc/untitled](Chapter01%20Introduction%20f076c0b7d1c645c49cc5a69f3ba1cabc/untitled)

- User interface program은 응용프로그램과 동급이다.

---

# The Operating System as an Extended Machine

[Chapter01%20Introduction%20f076c0b7d1c645c49cc5a69f3ba1cabc/untitled%201](Chapter01%20Introduction%20f076c0b7d1c645c49cc5a69f3ba1cabc/untitled%201)

---

# The Operating System as a Resource Manager

- Allow multiple programs to run at the same time
- Manage and protect memory, I/O devices, and other resources
- Includes multiplexing (sharing) resources in two different ways:
    - in time
    - in space

---

# History of Operating Systems

- (1945-55) Vacuum Tubes
- (1955-65) Transistors and Batch Systems
- (1965-80) ICs and Multiprogramming
- (1980-Present) Personal Computers

---

# ICs and Multiprogramming

[Chapter01%20Introduction%20f076c0b7d1c645c49cc5a69f3ba1cabc/untitled%202](Chapter01%20Introduction%20f076c0b7d1c645c49cc5a69f3ba1cabc/untitled%202)

- **Multiprogramming** : 계산은 빠른데 속도가 지연되는 부분은 IO 관리다. CPU를 놀게 하지 말고 메모리에 여럿을 올리자!

---

# Computer Hardware Review

[Chapter01%20Introduction%20f076c0b7d1c645c49cc5a69f3ba1cabc/untitled%203](Chapter01%20Introduction%20f076c0b7d1c645c49cc5a69f3ba1cabc/untitled%203)

- BUS: 데이터가 이동하는 회로
- CPU는 메모리만 상대를 한다.
    - 그래픽 카드의 그래픽 메모리
    - 각 I/O controller 안에 있는 register를 상대!

## Register에 주소를 주는 법

Main memory 와 충돌이 되지 않는 주소를 할당해주어야 한다. 

### Memory mapped I/O

- 실제 메모리(DRAM)가 물리적으로 가질 수 없는 주소를 가상으로 레지스터에 할당함
- AMD CPU가 이런 방법을 사용함

### I/O mapped I/O

- 메모리 주소와 I/O 주소를 따로 할당하는 방법
- I/O 주소를 port 번호로 칭하기도 했었다.
- Memory mapped와 같은 방식으로 주소를 할당해도 문제가 생기지는 않는다.

---

# Top-Level View

[Chapter01%20Introduction%20f076c0b7d1c645c49cc5a69f3ba1cabc/untitled%204](Chapter01%20Introduction%20f076c0b7d1c645c49cc5a69f3ba1cabc/untitled%204)

- Program Counter = Instruction Pointer

# Processor Registers

## User-Visible Registers

- Enable programmer to minimize main-memory references by optimizing register use. May be referenced by machine language
- Available to all programs - application and system progrmas
- Types of Registers
    - Data
    - Address
    - Index
    - Segment Pointer
    - Stack Pointer

## Special and/or User-Invisible Registers

- Used by processor to control operating of the processor
- Used by privileged operating-system routines to control the execution of programs
- Example: PC (Program Counter), IR (Instruction Register), PSW (Program Status Word Register)

---

# CPU Pipelining

[Chapter01%20Introduction%20f076c0b7d1c645c49cc5a69f3ba1cabc/untitled%205](Chapter01%20Introduction%20f076c0b7d1c645c49cc5a69f3ba1cabc/untitled%205)

[Chapter01%20Introduction%20f076c0b7d1c645c49cc5a69f3ba1cabc/untitled%206](Chapter01%20Introduction%20f076c0b7d1c645c49cc5a69f3ba1cabc/untitled%206)

- 이렇게 Fetch unit 이 놀지 않게 할 수 있다.
- 너무 많은 pipelining은 전력소모를 크게 하게 되고, hazard가 커진다. (jmp 명령시 버릴게 많아진다)
- 단계를 쪼개고 한 단계에 실행시간을 줄이면 pipelining을 통해 얻는 이득이 커진다.
- 다음 단계로 CPU가 발전하기 시작한 것이 **Superscalar**
    - 여러개 가져온다고 해도 많은 경우 동시에 실행하지 못한다.
    - 이전 명령과 dependency가 있는경우 수행이 불가능하다.
    - 뒤에 명령이 앞에 명령보다 먼저 실행 될 수도 있다.
        - 프로그램은 순차로 실행되지 않을 수도 있다.
        - 특수한 명령을 넣어줘야 할 수도 있다.

⇒ CPU 설계는 굉장히 복잡하다. 

# Multithreaded and Multicore Chips

[Chapter01%20Introduction%20f076c0b7d1c645c49cc5a69f3ba1cabc/untitled%207](Chapter01%20Introduction%20f076c0b7d1c645c49cc5a69f3ba1cabc/untitled%207)

Figure 1-8. (a) A quad-core chip with a shared L2 cache. 
                  (b) A quad-core chip with separate L2 caches.

- Register → L1 cache → L2 cache → L3 cache → Main Memory
- L2 캐쉬 간의 coherencey 문제가 생길 수 있다.

---

# Interrupts

Suspend the normal sequence of execution

[Chapter01%20Introduction%20f076c0b7d1c645c49cc5a69f3ba1cabc/untitled%208](Chapter01%20Introduction%20f076c0b7d1c645c49cc5a69f3ba1cabc/untitled%208)

i번 instruction을 실행할때 interrupt가 걸리면, 

- CPU 레지스터에 interrupt vector table의 주소가 적힌 레지스터가 있다.
- Memory의 interrupt vector table의 intruupt handler 주소를 보고 이동
- interrupt handler 로 점프하고
- interrupt return 명령으로 i+1번 instruction으로 돌아온다.

ARM 의 경우

- Memory의 0번지부터 interrupt vector table이 존재한다.

---

# Change of Registers for Interrupt

[Chapter01%20Introduction%20f076c0b7d1c645c49cc5a69f3ba1cabc/untitled%209](Chapter01%20Introduction%20f076c0b7d1c645c49cc5a69f3ba1cabc/untitled%209)

- 사용자 프로그램도 interrput를 실행할 수 있다.
- 곧 운영체제를 실행할 수 있다.
- N번째 인스트럭션을 실행하다가 스스로 인터럽트를 걸면 복귀주소는 N
- 다른 경우는 N+1 이며 CPU레지스터의 값과 복귀 주소를 스택에 저장

---

# Memory  Hierachy

[Chapter01%20Introduction%20f076c0b7d1c645c49cc5a69f3ba1cabc/untitled%2010](Chapter01%20Introduction%20f076c0b7d1c645c49cc5a69f3ba1cabc/untitled%2010)

- 위로 갈수록 접근 속도는 빠르고 가격은 비싸짐

---

# Cache Memory

## Questions when dealing with cache:

- Cache Size & Block Size
- When to put a new item into the cache
- Which cache line to put the new item in (Mapping Function)
- Which item to remove from the cache when a slot is needed (Replacement Policy)
- Write Policy

[Chapter01%20Introduction%20f076c0b7d1c645c49cc5a69f3ba1cabc/untitled%2011](Chapter01%20Introduction%20f076c0b7d1c645c49cc5a69f3ba1cabc/untitled%2011)

(Right) Cache (Left) Main memory

- Tag: address Block: data
- Main memory의 데이터를 cache에 넣을때는 block 단위(64바이트, 128바이트) 로 넣는다.
- 태그에서 주소를 매핑하는 방법이 두가지 있다.

## Fully Associative Mapping

[Chapter01%20Introduction%20f076c0b7d1c645c49cc5a69f3ba1cabc/untitled%2012](Chapter01%20Introduction%20f076c0b7d1c645c49cc5a69f3ba1cabc/untitled%2012)

- 블럭이 64바이트 단위면 주소의 하위 6비트는 태그에서 필요가 없음
- 캐시 주소 검색 → 없으면 메모리 검색
    - 캐시 주소 검색을 순차적으로 하면 오래걸림
    - 비교회로가 있어서 한번에 해당 주소가 어디 있는지 알 수 있음
- Hit rate가 높다.

- 대신 비싸다
    - 태그마다 비교회로가 들어가야함
    - 이걸 줄여보려고 했는데 그 결과가 Direct Mapping

## Direct Mapping

[Chapter01%20Introduction%20f076c0b7d1c645c49cc5a69f3ba1cabc/untitled%2013](Chapter01%20Introduction%20f076c0b7d1c645c49cc5a69f3ba1cabc/untitled%2013)

- 00 으로 끝나는 주소는 00 index에만 저장
- 주소에 따라 캐시에 들어갈 수 있는 위치가 지정되어 있음
- 이제 비교회로가 하나면 충분하다 ⇒ 싸다
- 태그 사이즈도 줄였다

- hit rate 가 Fully Associative Mapping 보다 낮다.
    - 충돌이 자주 일어남

### Replace policy

- Cache의 내용을 변경하는 정책
- 이전 기록을 보고 안쓰일 법한 데이터를 삭제하는 것이 중요하다.

### Write policy

- Cache 에 데이터를 먼저 저장하고 Memory에 데이터를 저장
- **Write Through**: Cache와 Memory를 동시에 변경
- **Write Back**: Cache만 먼저 변경하고 나중에 Memory를 변경
    - 성능상으론 분명 이득을 보지만 나중에 memory를 변경해 문제가 생길 수 있음
    - Coherency 문제가 생길 수 있다.

## Set Associative Mapping

[Chapter01%20Introduction%20f076c0b7d1c645c49cc5a69f3ba1cabc/untitled%2014](Chapter01%20Introduction%20f076c0b7d1c645c49cc5a69f3ba1cabc/untitled%2014)

- 위 둘 매핑의 중간 버전
- way 만큼의 비교회로만이 필요함
- way 를 늘리면 hit rate 도 올릴 수 있다.
- 팬티엄에서 쓰는 방식 (12ways)

---

# Performance analysis of cache memory

- Cache Access Time: 0.1us
- Main Memory Access Time: 1 us
- Cache hit rate: 0.95
- Average Acces Time
    - (0.95)(0.1us) + (0.05)(0.1us + 1us) = 0.095 + 0.055 = 0.15us
- Generally Hit rage is high because of Locality of Reference
- Locality
    - Temporal Locality
    - Spatial Locality
        - Tables, Arrays
    - Working set (최근에 참고한 친구들)

### Main memory를 사용하지 않고 100% Cache만을 사용하면 어떨까?

- 가능하다.
- 성능도 좋아질 것이지만 들인 돈만큼의 효과가 있을까?
    - Cache only access time: 0.1us vs Average access time: 0.15us

⇒ 실제로 큰 이득은 없다. 

### Hit rate가 높은 이유

- **Locality** (프로그램의 참고 지역성)
    - 한번 참고된 데이터는 다시 참고될 확률이 높다.

---

# Disks

[Chapter01%20Introduction%20f076c0b7d1c645c49cc5a69f3ba1cabc/untitled%2015](Chapter01%20Introduction%20f076c0b7d1c645c49cc5a69f3ba1cabc/untitled%2015)

- 헤드번호 → 트랙번호 →섹터 번호의 순서로 참조
- 기계적인 동작 많음
    - 수명으로 인한 고장
    - 충격에 약함
- 헤드가 원하는 트랙으로 움직이는 seek 타임이 오래 걸린다.

---

# I/O Devices

[Chapter01%20Introduction%20f076c0b7d1c645c49cc5a69f3ba1cabc/untitled%2016](Chapter01%20Introduction%20f076c0b7d1c645c49cc5a69f3ba1cabc/untitled%2016)

(a) The steps in starting an I/O device and getting an interrupt.
(b) 인터럽트를 받고 인터럽트 핸들러르 ㄹ실행하고 사용자 프로그램으로 돌아가는 것을 보이는 인터럽트 처리과정

- Controller chip이 모든 I/O 의 interrupt 신호를 받는다.
- Interrupt Controller chip이 CPU에게 다시 신호를 전달
- Interrupt도 IO마다 우선순위가 있다.
    - Network > Keyboard

### 왜 이런 Interrupt 방식으로 동작해야할까?

그렇지 않으면 CPU가 기다려야 한다. 

Programmed io 는 cpu가 노는 방식. 오래 걸린다. 

---

# Input and Output

## Programmed I/O and Interrupt-driven I/O

[Chapter01%20Introduction%20f076c0b7d1c645c49cc5a69f3ba1cabc/untitled%2017](Chapter01%20Introduction%20f076c0b7d1c645c49cc5a69f3ba1cabc/untitled%2017)

### Programmed I/O

- Interrupt가 없는 경우에, WRITE 작업에 소요되는 시간이 길다.
- I/O 처리가 느리기 때문에 CPU가 놀게된다.

### Interrupt-driven I/O

- WRITE를 구동시키고 돌아와 자기일을 한다.
- 실제로는 WRITE 과정이 더 오래걸리기 때문에 이렇게 이쁘게 돌아갈 수는 없다.
- READ는 I/O가 끝나야 뒤에 작업을 해야함
- 한 프로그램으로는 CPU를 놀지 않게 하는 것이 힘들다. 
⇒ context switching

---

# Dual Mode of Processors

[Chapter01%20Introduction%20f076c0b7d1c645c49cc5a69f3ba1cabc/untitled%2018](Chapter01%20Introduction%20f076c0b7d1c645c49cc5a69f3ba1cabc/untitled%2018)

## User mode

- Less-privileged mode
- User programs typically execute in this mode
- Cannot privileged instructions
- Interrupt or trap can switch user mode to kernel mode

## System mode, control mode, or kernel mode

- More-privileged mode
- Kernel of the operating system
- Can execute privileged instructoins

## Privileged instructions

- I/O instructions
- Set processor status
- Access privileged memory area

Interrupt → kenel mode → Interrupt return → user mode

### Dual mode를 사용하지 않으면?

- MS-DOS 에서는 사용자 프로그램이 직접 I/O 연산을 수행하기도 했음
- 두 개의 프로그램이 같은 I/O에 접근하면 컴터가 죽음

[Chapter01%20Introduction%20f076c0b7d1c645c49cc5a69f3ba1cabc/untitled%2019](Chapter01%20Introduction%20f076c0b7d1c645c49cc5a69f3ba1cabc/untitled%2019)

1. 프로그램 A 수행 
2. Timer Interrput (주기적으로 Interrupt)
3. Context Switching
4. System Call 
5. Interrupt return

⇒ OS is collection of Interrupt service routine

[Chapter01%20Introduction%20f076c0b7d1c645c49cc5a69f3ba1cabc/untitled%2020](Chapter01%20Introduction%20f076c0b7d1c645c49cc5a69f3ba1cabc/untitled%2020)

---

# Buses

[Chapter01%20Introduction%20f076c0b7d1c645c49cc5a69f3ba1cabc/untitled%2021](Chapter01%20Introduction%20f076c0b7d1c645c49cc5a69f3ba1cabc/untitled%2021)

- ISA 버스는 옛날 꺼임 PCI 버스가 좋은거
- **Bridge:** 버스와 버스를 이어주는 회로  
****

---

# The Operating System Zoo

- Mainframe operating systems
- Server operating systems
- Multiprocessor operating systems
- Personal computer operating systems
- Handheld operating systems
- Embedded operating systems
- Sensor node operating systems
- Real-time operating systems
- Smart card operating systems

---

# Operating System Conceps

- Processes
- Address spaces
- Files
- Input / Output
- Protection
- The shell
- Ontogency recapitulates phylogeny
    - Large memories
    - Protection hardware
    - Disks
    - Virtual memory

---

# Processse

[Chapter01%20Introduction%20f076c0b7d1c645c49cc5a69f3ba1cabc/untitled%2022](Chapter01%20Introduction%20f076c0b7d1c645c49cc5a69f3ba1cabc/untitled%2022)

- 실행중인 프로그램
- 프로세스는 부모 자식 관계를 가지고 있다.
- 프로세스를 실행하는 것은 사용자 인터페이스 프로그램이다.
- 리눅스의 경우 최초의 프로세스는 init 프로세스

---

# File

[Chapter01%20Introduction%20f076c0b7d1c645c49cc5a69f3ba1cabc/untitled%2023](Chapter01%20Introduction%20f076c0b7d1c645c49cc5a69f3ba1cabc/untitled%2023)

- Directory = folder

[Chapter01%20Introduction%20f076c0b7d1c645c49cc5a69f3ba1cabc/untitled%2024](Chapter01%20Introduction%20f076c0b7d1c645c49cc5a69f3ba1cabc/untitled%2024)

## pipe

- 두개의 프로그램이 데이터를 주고받는 통로
- 운영체제가 컨트롤 해줌
- 유닉스의 경우 디스크가 아닌 메모리상에만 존재하는 가상의 파일에 read write를 함으로써 데이터 공유

---

# System Calls

[Chapter01%20Introduction%20f076c0b7d1c645c49cc5a69f3ba1cabc/untitled%2025](Chapter01%20Introduction%20f076c0b7d1c645c49cc5a69f3ba1cabc/untitled%2025)

- Dispatch = Interrupt service routine
- 운영체제는 주체가 없다.
- 주체는 각각의 프로세스

## System Calls for Process Management

[Chapter01%20Introduction%20f076c0b7d1c645c49cc5a69f3ba1cabc/untitled%2026](Chapter01%20Introduction%20f076c0b7d1c645c49cc5a69f3ba1cabc/untitled%2026)

[Chapter01%20Introduction%20f076c0b7d1c645c49cc5a69f3ba1cabc/untitled%2027](Chapter01%20Introduction%20f076c0b7d1c645c49cc5a69f3ba1cabc/untitled%2027)

[Chapter01%20Introduction%20f076c0b7d1c645c49cc5a69f3ba1cabc/untitled%2028](Chapter01%20Introduction%20f076c0b7d1c645c49cc5a69f3ba1cabc/untitled%2028)

## A Simple Shell

```c
#define TRUE 1
while (TRUE) {
		type_prompt();
		read_command(command, parameters);

		if (frok()!=0) {
			// Paraent Code
			waitpid(-1, &status, 0);
		}	
		else {
			// Child Code
			execve(command, parameters, 0);
		}
	}
}
```

# Memory Layout

[Chapter01%20Introduction%20f076c0b7d1c645c49cc5a69f3ba1cabc/untitled%2029](Chapter01%20Introduction%20f076c0b7d1c645c49cc5a69f3ba1cabc/untitled%2029)

- Gap = Heap
- 가상메모리에서는 항상 0번부터 코드가 시작함

---

# Linking

[Chapter01%20Introduction%20f076c0b7d1c645c49cc5a69f3ba1cabc/untitled%2030](Chapter01%20Introduction%20f076c0b7d1c645c49cc5a69f3ba1cabc/untitled%2030)

(a) Two directories before linking /usr/jim/memo to ast's directory
(b) The same directories after linking

- inode has all information about directory
- inode가 같은데 다른 이름이 여러군데 있을 수 있다. : hardlinking
- 파일은 하나 이름은 여러개
- 권장하지 않음
- 지울때 루트를 지우고 이름을 다 지워야 파일이 지워진다.
- 링크를 여러개 만드는건 별로임
- soft link symbolic link 바로가기를 만드는게 권장됨
- 서로 다른 별개의 파일이지만 오히려 부작용이 적다.
- 그래서 바로가기 파일을 더 권장한다.
- 파일이나 사용량을 확인하는데도 문제가 생긴다.

---

# Mounting

[Chapter01%20Introduction%20f076c0b7d1c645c49cc5a69f3ba1cabc/untitled%2031](Chapter01%20Introduction%20f076c0b7d1c645c49cc5a69f3ba1cabc/untitled%2031)

(a) File system before the mount
(b) File system after the mount

---

# Windows Win32 API

[Chapter01%20Introduction%20f076c0b7d1c645c49cc5a69f3ba1cabc/untitled%2032](Chapter01%20Introduction%20f076c0b7d1c645c49cc5a69f3ba1cabc/untitled%2032)

- Window와 UNIX는 비슷한 구조를 가진다.

---

# Operating Systems Structure

## Monolithic systems - basics structure:

[Chapter01%20Introduction%20f076c0b7d1c645c49cc5a69f3ba1cabc/untitled%2033](Chapter01%20Introduction%20f076c0b7d1c645c49cc5a69f3ba1cabc/untitled%2033)

- A main program that invokes the requested service procedure
- A set of service procedures that carry out the system calls
- A set of utility procedures that help the service procedure

It is what linux looks like

linux module programming 

## Layered System

[Chapter01%20Introduction%20f076c0b7d1c645c49cc5a69f3ba1cabc/untitled%2034](Chapter01%20Introduction%20f076c0b7d1c645c49cc5a69f3ba1cabc/untitled%2034)

- 네트워크의 OSI와 다르게 실패함
- 서로 간의 의존관계가 있는데 상하 관계로의 설계는 실패할 수 밖에 없지!

## Microkernels

[Chapter01%20Introduction%20f076c0b7d1c645c49cc5a69f3ba1cabc/untitled%2035](Chapter01%20Introduction%20f076c0b7d1c645c49cc5a69f3ba1cabc/untitled%2035)

- 핵심 기능만 Microkernel로 빼냄
- 다른 프로그램은 Microkernel 위에서 동작하는 서버 형태로 동작
- 윈도우가 이를 참고했고 성능 향상을 위해 다른 기능등을 Microkernel에 추가함

## Client-Server Model

[Chapter01%20Introduction%20f076c0b7d1c645c49cc5a69f3ba1cabc/untitled%2036](Chapter01%20Introduction%20f076c0b7d1c645c49cc5a69f3ba1cabc/untitled%2036)

---

# Virtual Machines

[Chapter01%20Introduction%20f076c0b7d1c645c49cc5a69f3ba1cabc/untitled%2037](Chapter01%20Introduction%20f076c0b7d1c645c49cc5a69f3ba1cabc/untitled%2037)

- 프로그램이 가상으로 PC를 Emulate
- CMS: 단일 사용자 운영체제

[Chapter01%20Introduction%20f076c0b7d1c645c49cc5a69f3ba1cabc/untitled%2038](Chapter01%20Introduction%20f076c0b7d1c645c49cc5a69f3ba1cabc/untitled%2038)

- Guest OS 는 입출력을 할때 , Hypervisor를 통해야 한다.

## Virtual Machine Support

### Pentium

- User mode
- Kernel mode (non-root mode) for OS
- Kernel mode (root mode) for Hypervisor

### ARM

- EL 0 for user program
- EL 1 for OS
- EL 2 for Hypervisor

이전의 Pentium은 user모드의 입출력을 무시했는데, 이는 VM 을 구현하는 걸림돌

입출력 코드 → 트랩 → 하이퍼바이저 

VM 지원을 위해 CPU가 개선될 필요가 있었다. 

---