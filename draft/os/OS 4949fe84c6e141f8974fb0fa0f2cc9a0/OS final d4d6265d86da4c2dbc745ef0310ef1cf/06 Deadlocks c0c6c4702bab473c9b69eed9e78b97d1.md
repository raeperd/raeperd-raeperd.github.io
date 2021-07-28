# 06 Deadlocks

# Deadlock

### Deadlock free code

```c
typedef int semaphore;
semaphore res_1;
semaphore res_2;

void precess_A(void) {
	down(&res_1);
	down(%res_2);
	use_both_res();
	up(&res_1);
	up(&res_2);
}

void process_B(void) {
	down(&res_1);
	down(%res_2);
	use_both_res();
	up(&res_1);
	up(&res_2);
}
```

### Code with potential deadlock

```c
#include <iostream>
using namespace std;
typedef int semaphore;

semaphore res_1;
semaphore res_2;

void process_A(void) {
	down(&res_1);
	down(&res_2);
	use_both_res();
	up(&res_1);
	up(&res_2);
}

void process_A(void) {
	down(&res_2);
	down(&res_1);
	use_both_res();
	up(&res_1);
	up(&res_2);
}
```

### Deadlock

- A set of processes is deadlocked if each process in the set is waiting for an event that only another process in the set can cause.

## Conditions for Resource Deadlocks

1. Mutual exclusion condition
2. Hold and wait condition
3. No preemption condition
4. Circular wait condition 

## Deadlock modeling

![06%20Deadlocks%20c0c6c4702bab473c9b69eed9e78b97d1/Untitled.png](06%20Deadlocks%20c0c6c4702bab473c9b69eed9e78b97d1/Untitled.png)

## Strategies for dealing with deadlocks:

1. Just ignore the problem.
2. Detection and recovery. Let deadlocks occur, detect them, take action.
    - Deadlock detection: cycle detection, banker’s algorithm
3. Dynamic avoidance by careful resource allocation.
    - Resource is allocated only when deadlock is not caused by the allocation
4. Prevention, by structurally negating one of the four required conditions.
    1. Prevention of mutual exclusion
    2. Prevention of hold and wait
    3. Prevention of no-preemption
    4. Prevention of circular wait

## Deadlock detection

![06%20Deadlocks%20c0c6c4702bab473c9b69eed9e78b97d1/Untitled%201.png](06%20Deadlocks%20c0c6c4702bab473c9b69eed9e78b97d1/Untitled%201.png)

- 시각화 했을 때, 사이클이 존재할 경우 데드락이 발생한 것으로 봄
- 리소스가 하나일때만 사용할 수 있는 방법

### Algorithm for detecting deadlock: (시험문제는 안나옴)

1. For each node, N in the graph, perform the following five steps with N as the starting node.
2. Initialize L to the empty list, designate all arcs as unmarked.
3. Add current node to end of L, check to see if node now appears in L two times. If it does, graph contains a cycle (listed in L), algorithm terminates.
4. From given node, see if any unmarked outgoing arcs. If so, go to step 5; if not, go to step 6.
5. Pick an unmarked outgoing arc at random and mark it. Then follow it to the new current node and go to step 3.
6. If this is initial node, graph does not contain any cycles, algorithm terminates. Otherwise, dead end. Remove it, go back to previous node, make that one current node, go to step 3.

### 여러 자원이 있는 경우의 deadlock detection

![06%20Deadlocks%20c0c6c4702bab473c9b69eed9e78b97d1/Untitled%202.png](06%20Deadlocks%20c0c6c4702bab473c9b69eed9e78b97d1/Untitled%202.png)

### Deadlock detection algorithm:

1. Look for an unmarked process, *Pi* , for which the i-th row of *R* is less than or equal to *A*.
2. If such a process is found, add the *i-th* row of *C*to *A*, mark the process, and go back to step 1.
3. If no such process exists, the algorithm terminates.

![06%20Deadlocks%20c0c6c4702bab473c9b69eed9e78b97d1/Untitled%203.png](06%20Deadlocks%20c0c6c4702bab473c9b69eed9e78b97d1/Untitled%203.png)

### Attacking the Circular Wait Condition

- 모든 자원에 번호를 메기고 작은 번호부터 자원을 할당받도록 한다.

=> 자원에 순서를 매기면 원형 구조가 생기지 않지만, 현실적으로 모든 자원에 이렇게 하기는 힘들다.

프로그래밍 할때 주석으로 이런 언급을 많이함

링크드리스트에 멀티쓰레딩을 적용하려면 맨 앞 head에 락을 걸고 접근을 해야한다.

## Attacking the No Preemption, Mutual Exclusion, Hold & Wait

### No Preemption

- Process must release resource and request again
- Operating system may preempt a process to require it releases its resources
- Possible for memory or CPU registers
- Impossible for printer or tape drive

### Mutual Exclusion

- Many resources must be used exclusively
- Must be supported by the operating system

### Hold and Wait

- Require a process request all of its required resources at one time
    - Low resource utilization
    - Starvation
    - Programmers don’t know all resources used in their program
    - Hard to program

## Integrated deadlock strategy

- Group resources into resource classes
- Use linear ordering strategy to prevent circular wait
- Within a resource class, use appropriate algorithm for that class

    (1) Swappable space : allocate or not all of the required resource, hold-and-wait prevention strategy.

    (2) Process resource : deadlock avoidance or prevention by resource ordering

    (3) Main memory : prevention by preemption

    (4) Internal resource : prevention by resource ordering