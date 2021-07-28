# 05 Input/Output

# Interrupts Revisited

![05%20Input%20Output%2042c716dc1c2a4b85b0aa87c1994d81dd/Untitled.png](05%20Input%20Output%2042c716dc1c2a4b85b0aa87c1994d81dd/Untitled.png)

- How an interrupt happens. The connections between the devices and the interrupt controller actually use interrupt lines on the bus rather than dedicated wires.

## Precise and Imprecise interrupts

### Properties of a precise interrupt

1. PC (Program Counter) is saved in a known place.
2. All instructions before the one pointed to by the PC have fully executed.
3. No instruction beyond the one pointed to by the PC has been executed.
4. Execution state of the instruction pointed to by the PC is known.

⇒ 인터럽트 순간의 명령어까지는 모두 수행된 상태

![05%20Input%20Output%2042c716dc1c2a4b85b0aa87c1994d81dd/Untitled%201.png](05%20Input%20Output%2042c716dc1c2a4b85b0aa87c1994d81dd/Untitled%201.png)

- (a) A precise interrupt
- (b) An imprecise interrupt

# Goal of I/O Software

### Device Independency

- Program can access any I/O devices with the same way
- Ex) HDD, CD-ROM, or USB stick
- Ex) Sort < input > output

### Uniform Naming

- Name of a file or device is a string or an integer
- Mount a file system of a device to root file system

### Error Handling

- Handle errors as close to the hardware as possible

## Synchronous vs asynchronous

- Blocked transfer vs. interrupt-driven

### Buffering

- Single Buffering
- Double Buffering

### Sharable vs. dedicated devices

- Disks are sharable
- Tape drivers would not be

![05%20Input%20Output%2042c716dc1c2a4b85b0aa87c1994d81dd/Untitled%202.png](05%20Input%20Output%2042c716dc1c2a4b85b0aa87c1994d81dd/Untitled%202.png)

# I/O Software Layers

![05%20Input%20Output%2042c716dc1c2a4b85b0aa87c1994d81dd/Untitled%203.png](05%20Input%20Output%2042c716dc1c2a4b85b0aa87c1994d81dd/Untitled%203.png)

### Interrupt handlers are best hidden

- Have driver starting an I/O operation block until interrupt notifies of completion

### Interrupt procedure does its task

- Then unblock driver that started it

### Steps must be performed in software after interrupt completed

- Save regs not already saved by interrupt hardware
- Set up context for interrupt service procedure

## Sequence of Interrupt Handlers

1. Save registers not already been saved by interrupt hardware.
2. Set up a context for the interrupt service procedure.
3. Set up a stack for the interrupt service procedure.
4. Acknowledge the interrupt controller. If there is no centralized interrupt controller, reenable interrupts.
5. Copy the registers from where they were saved to the process table.
6. Run the interrupt service procedure.
7. Choose which process to run next.
8. Set up the MMU context for the process to run next.
9. Load the new process’ registers, including its PSW.
10. Start running the new process.

- 7과 8사이에 문맥 교환이 발생함
- 기본 프로세스가 다시 실행되거나 새 프로세스가 생성되려면 7~8사이에 동작해야함

# Device Driver

![05%20Input%20Output%2042c716dc1c2a4b85b0aa87c1994d81dd/Untitled%204.png](05%20Input%20Output%2042c716dc1c2a4b85b0aa87c1994d81dd/Untitled%204.png)

## Functions of the device-independent I/O software

1. Uniform interfacing for device drivers 
2. Buffering 
3. Error reporting 
4. Allocating and releasing dedicated devices
5. Proving a device-independent block size

## Uniform Interfacing for Device Drivers

![05%20Input%20Output%2042c716dc1c2a4b85b0aa87c1994d81dd/Untitled%205.png](05%20Input%20Output%2042c716dc1c2a4b85b0aa87c1994d81dd/Untitled%205.png)

# Disk Arm Scheduling Algorithm

- Read/Write time factors
    1. Seek time
    2. Rotational delay
    3. Actual data transfer time 
- Seek time dominates
- Error checking is done by controllers

## FIFO

![05%20Input%20Output%2042c716dc1c2a4b85b0aa87c1994d81dd/Untitled%206.png](05%20Input%20Output%2042c716dc1c2a4b85b0aa87c1994d81dd/Untitled%206.png)

                                   Request order : 55 58 39 18 90 160 150 38 184

- Process request sequentially
- Fair to all processes
- Approaches random scheduling in performance if there are many processes

## Shortest Service Time First

![05%20Input%20Output%2042c716dc1c2a4b85b0aa87c1994d81dd/Untitled%207.png](05%20Input%20Output%2042c716dc1c2a4b85b0aa87c1994d81dd/Untitled%207.png)

                                  Request order : 55 58 39 18 90 160 150 38 184

- Select the disk I/O request that requires the least movement of the disk arm from its current position
- Always choose the minimum Seek time

- 헤드가 움직이는 거리가 짧아지고 성능이 좋아진다.
- 공평하지 않고, starvation이 생길 수 있다.
- 엘리베이터는 한번 올라가면 끝까지 올라가고 한번 내려가면 끝까지 내려간다.

## SCAN or elevator algorithm

![05%20Input%20Output%2042c716dc1c2a4b85b0aa87c1994d81dd/Untitled%208.png](05%20Input%20Output%2042c716dc1c2a4b85b0aa87c1994d81dd/Untitled%208.png)

                                  Request order : 55 58 39 18 90 160 150 38 184

- Arm moves in one direction only, satisfying all outstanding requests until it reaches the last track in that direction
- Direction is reversed
- 엘레베이터 문제, 선풍기가 있으면 가운데 있을때 좀더 일정하게 온다. 서비스 타임의 편차가 생긴다.

## C-SCAN

![05%20Input%20Output%2042c716dc1c2a4b85b0aa87c1994d81dd/Untitled%209.png](05%20Input%20Output%2042c716dc1c2a4b85b0aa87c1994d81dd/Untitled%209.png)

                                  Request order : 55 58 39 18 90 160 150 38 184

- Restricts scanning to one direction only
- When the last track has been visited in one direction, the arm is returned to the opposite end of the disk and the scan begins again
- 서비스의 편차를 없애려면 한 방향으로만 헤드를 움직인다.

## N-step-SCAN

- Segments the disk request queue into subqueues of length N
- Subqueues are processed one at a time, using SCAN
- New requests added to other queue when queue is processed

## FSCAN

- Two queues
- One queue is empty for new requests

- N개의 큐를 둔다.
- 하나의 큐를 Scan이나 C-Scan을 이용한다.
- 내 앞뒤 N개 이내에 나는 처리된다.
- FSCAN은 단지 큐를 두개만 쓴다.

## Comparison of disk arm scheduling

![05%20Input%20Output%2042c716dc1c2a4b85b0aa87c1994d81dd/Untitled%2010.png](05%20Input%20Output%2042c716dc1c2a4b85b0aa87c1994d81dd/Untitled%2010.png)

# Error Handling

- 항상 bad sector가 생긴다.
- 이를 교체하기 위한 여분의 sector 가 필요하다.

### Operation for stable storage using identical disks:

1. Stable writes
2. Stable reads
3. Crash recovery

# Clock Hardware

- 클럭 인터럽트가 일어나야 스케쥴링이 일어난다.

## Typical duties of a clock driver

1. Maintaining the time of day.
2. Preventing processes from running longer than they are allowed to.
3. Accounting for CPU usage.
4. Handling alarm system call made by user processes.
5. Providing watchdog timers for parts of the system itself.
6. Doing profiling, monitoring, statistics gathering.

- 운영체제는 처음에 마더보드의 시계를 한번만 본다.
- 클럭 인터럽트가 일어날때 마다 시간을 확인한다.
- 타임 서버에 접근해서 시간을 가져오기도 한다.
- 시간을 한번에 바꾸지는 않고 서서히 시간을 바꾸기 위한 알고리즘이 존재한다.
    - 시간을 한번에 바꾸면 알람이 지나간다 던지 하는 문제가 발생할 수 있다.
- 프로파일링을 켜면 타임 인터럽트가 걸릴 때 마다 프로그램의 어떤 루틴을 시행하고 있는지 기록한다.

![05%20Input%20Output%2042c716dc1c2a4b85b0aa87c1994d81dd/Untitled%2011.png](05%20Input%20Output%2042c716dc1c2a4b85b0aa87c1994d81dd/Untitled%2011.png)

- 64비트면 시간을 표현하는데는 충분하다.
- 그런데 이런 필드는 크게 잡는것이 좋다.

## Soft Timers

- Main system timer is used by OS
- Second clock may be used by app to generate timer int
    - No problem if int frequency is low
- Soft timers avoid int

- 클럭 인터럽트가 초당 100번 정도 걸리게 한다.
- 10ms 단위로 명령을 실행할 수 있다.
- 일반 시스템 콜이 일어날 경우에도 타임 서비스를 제공함으로써 타임 인터럽트 ㅏㄷㄴ위보다 더 자주 타임 인터럽트 서비스를 실행할 수 있다.