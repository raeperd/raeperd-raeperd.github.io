# INTERPROCESS COMMUNICATION

- 프로세스들이 자원을 공유할 필요가 있음 근데 문제가 있음
    1. 어떻게 정보를 주지?
    2. 서로의 프로세스에게 영향을 안주면서, 
    3. 프로세스의 종속성이나 순서도 신경쓰면서 말이야
- 1번 빼고는 Mutlthread 환경에서도 적용되는 얘기

## Race Conditions
![](https://i.imgur.com/27gCaYl.png)
- 두개 이상의 프로세스가 하나의 자원에 접근하고자 할때 접근 시점에 따라 결과가 바뀌는 상황

### Critical Region
- In concurrent programming, concurrent accesses to shared resources can lead to unexpected or erroneous behavior, so parts of the program where the shared resource is accessed need to be protected in ways that avoid the concurrent access. This protected section is the critical section or critical region.
- A.K.A. [Critical section - Wikipedia](https://en.wikipedia.org/wiki/Critical_section)

### Race Condition을 피하기 위한 조건
1. **Mutual Exclusion**: 동시에 두 프로세스가 Critical Region에 접근하면 안된다.
2. **Progress**: Critical Region 밖의 프로세스가 다른 프로세스를 막으면 안된다.
3. **Bounded Waiting**: 모든 프로세스의 대기 시간은 유한해야 한다.
4. CPU의 속도나 숫자에 어떤 가정도 있어선 안된다. (Optional) 

## Mutual Exclusion with Busy Waiting
### Disabling Interrupts
- 가장 간단하고 직관적임. Critical Region에 접근하면 인터럽트를 막아버려서 완전히 실행됨을 보장함
- 일개 프로세스가 Interrupt 를 막는건  OP임
- 가끔은 유용한 방법이지만 이는 프로세스가 아닌 OS에 의해 행해져야함
- CPU가 2개 이상이면 그것도 안됨

### Lock Variable
- 0: no process in Critical region 1: otherwise
- 연산도중 Context Switching 을 당할 수 있다.

### Strict Alternation
![](https://i.imgur.com/mGvsvZE.png)
- turn 을 쓰면 프로세스마다 속도 차이가 있을때 불공평하다.
    ⇒ 하나의 프로세스가 두번 들어가지 못함 
    ⇒ Critical Region에 있지않은 프로세스가 다른 프로세스를 Block

### Dekker's Solution
![](https://i.imgur.com/61Xg0Wh.png)


### Peterson's Solution (Mutual Exclusion Fails)

![2%203%20INTERPROCESS%20COMMUNICATION%20a837c1d34485400189e598efb7d480b6/Untitled%203.png](2%203%20INTERPROCESS%20COMMUNICATION%20a837c1d34485400189e598efb7d480b6/Untitled%203.png)

1. i가 flag[j] = False 라서 들어왔다. 
2. Context Switching

![2%203%20INTERPROCESS%20COMMUNICATION%20a837c1d34485400189e598efb7d480b6/Untitled%204.png](2%203%20INTERPROCESS%20COMMUNICATION%20a837c1d34485400189e598efb7d480b6/Untitled%204.png)

- Bounded Waiting: i가 두번 갈동안 j는 한번 갈 수도 있다.
- 상대에게 양보하는게 더 좋은 구조일 수도 있다.

### Backery Algorithm
![2%203%20INTERPROCESS%20COMMUNICATION%20a837c1d34485400189e598efb7d480b6/Untitled%205.png](2%203%20INTERPROCESS%20COMMUNICATION%20a837c1d34485400189e598efb7d480b6/Untitled%205.png)

- (a,b) < (c,d) := `a < c or ( a == c and b < d)`

**Spin lock:**

Busy Waiting 을 쓰는 Lock

### The TSL Instruction

### Peterson's Solution (Textbook version)

![2%203%20INTERPROCESS%20COMMUNICATION%20a837c1d34485400189e598efb7d480b6/Untitled%206.png](2%203%20INTERPROCESS%20COMMUNICATION%20a837c1d34485400189e598efb7d480b6/Untitled%206.png)

![2%203%20INTERPROCESS%20COMMUNICATION%20a837c1d34485400189e598efb7d480b6/Untitled%207.png](2%203%20INTERPROCESS%20COMMUNICATION%20a837c1d34485400189e598efb7d480b6/Untitled%207.png)

Bus 를 Lock 해야지만 다른 프로세서의 작업으로부터 독립적일 수있다. (인터럽트를 막는게 아니라)

## Sleep and Wakeup

- 위의 과정들은 모두 자신의 차례가 올때 까지 기다리기만 한다. (Busy Waitting)
- 높은 우선순위인 프로세스가 waiting 하고 있고 낮은 우선순위가 Critical Region에 있으면 DeadLock 에 걸린다. ⇒ Priority inversion problem
- Sleep is system call and wakeup is also

### The Producer-Consumer Problem

```c
#define N 100 /* number of slots in the buffer */
int count = 0; /* number of items in the buffer */
void producer(void)
{
	int item;
	while (TRUE) { /* repeat forever */
		item = produce item( ); /* generate next item */
		if (count == N) sleep( ); /* if buffer is full, go to sleep */
		inser t item(item); /* put item in buffer */
		count = count + 1; /* increment count of items in buffer */
		if (count == 1) wakeup(consumer); /* was buffer empty? */
	}
}

void consumer(void)
{
	int item;
	while (TRUE) { /* repeat forever */
		if (count == 0) sleep( ); /* if buffer is empty, got to sleep */
		item = remove item( ); /* take item out of buffer */
		count = count − 1; /* decrement count of items in buffer */
		if (count == N − 1) wakeup(producer); /* was buffer full? */
		consume item(item); /* pr int item */
	}
}
```

- count 변수에 접근하는 상황이 race condition 이다.

1. consumer 가 count가 0 인 것을 확인
2. context switching to producer
3. count ++
4. producer wakeup consumer
5. wakeup signal is lost 
6. and cousmer sleep 

sleep 이 아닌 프로세스에게 wakeup을 하면 신호가 무시된다. 

## Semaphores
- 0: no wakeup saved 1: 1wakeup saved
- 값을 확인하는 것과 변경하는 것 혹은 확인 후 잠드는 것은 한번에 수행한다. (atomic op)

```c
struct semaphore {
int count;
queueType queue;
}

void down (semaphore s) {
	s.count--; 
	if (s.count < 0) {
		place this process in s.queue;
		block this process;
	}
}

void up (semaphore s) {
	s.count++;
	if (s.count<=0) {
		remove a process p from s.queue
		place process P on ready list
	}
}

```

### Solving the Producer-Consumer Problem

```c
#define N 100 /* number of slots in the buffer */
typedef int semaphore; /* semaphores are a special kind of int */
semaphore mutex = 1; /* controls access to critical region */
semaphore empty = N; /* counts empty buffer slots */
semaphore full = 0; /* counts full buffer slots */
void producer(void)
{
    int item;
    while (TRUE) { /* TRUE is the constant 1 */
        item = produce item( ); /* generate something to put in buffer */
        down(&empty); /* decrement empty count */
        down(&mutex); /* enter critical region */
        insert item(item); /* put new item in buffer */
        up(&mutex); /* leave critical region */
        up(&full); /* increment count of full slots */
    }
}
void consumer(void)
{
    int item;
    while (TRUE) { /* infinite loop */
        down(&full); /* decrement full count */
        down(&mutex); /* enter critical region */
        item = remove item( ); /* take item from buffer */
        up(&mutex); /* leave critical region */
        up(&empty); /* increment count of empty slots */
        consume item(item); /* do something with the item */
    }
}
```

- 1로 세팅된 세마포어는 한 프로세스만 접근 해야함을 보장한다.
- `mutex` 는 mutual exclusion 을 위해 사용됨
- `full` 과 `empty`는 synchronization을 위해 사용됨

## Mutexes
![2%203%20INTERPROCESS%20COMMUNICATION%20a837c1d34485400189e598efb7d480b6/Untitled%208.png](2%203%20INTERPROCESS%20COMMUNICATION%20a837c1d34485400189e598efb7d480b6/Untitled%208.png)
- mutex는 unlock 혹은 lock 되어있는 변수다.

### Mutexes in Pthreads
![2%203%20INTERPROCESS%20COMMUNICATION%20a837c1d34485400189e598efb7d480b6/Untitled%209.png](2%203%20INTERPROCESS%20COMMUNICATION%20a837c1d34485400189e598efb7d480b6/Untitled%209.png)
![2%203%20INTERPROCESS%20COMMUNICATION%20a837c1d34485400189e598efb7d480b6/Untitled%2010.png](2%203%20INTERPROCESS%20COMMUNICATION%20a837c1d34485400189e598efb7d480b6/Untitled%2010.png)
- condition 변수는 쓰레드가 특정 조건을 만족할때까지 block 되게 한다.
- condition 변수는 waiting과 blocking을 atomic하게 실행함을 보장한다.

## Monitor
- semaphore 는 프로그래머가 실수하기 너무 쉽다.
- 이를 프로그래밍 언어차원에서 지원하자. 컴파일러가 세부 구현을 실현한다.
- 자바에서는 `synchronized` 로 구현이 되어 있다.
- 깨어난 애가 먼저 Hoare                       : Better
- 깨워준 애가 먼저 Lampson & Redell's

## Lock-free Data Structure
[https://blog.naver.com/jjoommnn/130040068875](https://blog.naver.com/jjoommnn/130040068875)

## Message Passing
- 메시지가 유실될 수 있으니 수신확인 메시지가 필요하다
- 수신 확인 메시지가 유실 될 수 있으니 수신자는 이전 메일과 현재 메일을 비교할 수 있어야한다.

## Barriers
![2%203%20INTERPROCESS%20COMMUNICATION%20a837c1d34485400189e598efb7d480b6/Untitled%2011.png](2%203%20INTERPROCESS%20COMMUNICATION%20a837c1d34485400189e598efb7d480b6/Untitled%2011.png)