# 07 Multibedia Operating System

# General Real-Time Scheduling

![07%20Multibedia%20Operating%20System%2076253b3dfe434e4080dfad92de00c09a/Untitled.png](07%20Multibedia%20Operating%20System%2076253b3dfe434e4080dfad92de00c09a/Untitled.png)

- 데드라인을 만족시키기 ㅜ이해 정해진 시간안에 프로세스를 실행 시킬 필요가 있다.
- 운영체제의 모든 자료구조는 deterministic 하게 작동한다. 얼마안의 동작을 보장해야한다.
- Deadline 이전에 언제라도 한번만 실행하면 괜찮다.

## Rate Monotonic Scheduling

![07%20Multibedia%20Operating%20System%2076253b3dfe434e4080dfad92de00c09a/Untitled%201.png](07%20Multibedia%20Operating%20System%2076253b3dfe434e4080dfad92de00c09a/Untitled%201.png)

- 주기가 짧은, 더 자주 실행하는 프로세스의 우선순위를 높게 부여한다.

![07%20Multibedia%20Operating%20System%2076253b3dfe434e4080dfad92de00c09a/Untitled%202.png](07%20Multibedia%20Operating%20System%2076253b3dfe434e4080dfad92de00c09a/Untitled%202.png)

- utilization은 1보다 작아야한다.
- 2) 를 만족하면 rate monotonic scheduling 이 가능하다.
- 2) 를 만족하지 않으면, scheduling이 가능할 수도 있고 가능하지 않을 수도 있다. 직접 그림을 그려가며 스케쥴을 해볼 필요가 있다.

### RMS 가 가능하다는 증명에 필요한 가정

1. Each periodic process must complete within its period.
2. No process is dependent on any other process.
3. Each process needs same amount of CPU time on each burst.
4. Nonperiodic processes have no deadlines.
5. Process preemption occurs instantaneously and with no overhead.

- 현실적으로 불가능 하지만 수학적으로 증명할 때는 5가 필수적으로 필요하다.

### Example

![07%20Multibedia%20Operating%20System%2076253b3dfe434e4080dfad92de00c09a/Untitled%203.png](07%20Multibedia%20Operating%20System%2076253b3dfe434e4080dfad92de00c09a/Untitled%203.png)

- 우선순위 A>B>C
- RMS의 경우 LCM 까지 해보면 가능한지 불가능한지 알 수 있다.
- EDF는 수학적인 개념 없이 직접 해볼 수 밖에 없다.

## Earlisest Deadline First Scheduling

![07%20Multibedia%20Operating%20System%2076253b3dfe434e4080dfad92de00c09a/Untitled%204.png](07%20Multibedia%20Operating%20System%2076253b3dfe434e4080dfad92de00c09a/Untitled%204.png)

# Priority Inversion

- Can occur in any priority-based preemptive scheduling scheme
- Occurs when circumstances within the system force a higher priority task to wait for a lower priority task
- Ex) Unbounded Priority Inversion : Duration of a priority inversion depends on unpredictable actions of other unrelated tasks

![07%20Multibedia%20Operating%20System%2076253b3dfe434e4080dfad92de00c09a/Untitled%205.png](07%20Multibedia%20Operating%20System%2076253b3dfe434e4080dfad92de00c09a/Untitled%205.png)

- task들 간에 자원을 경쟁하면서 생기는 문제
- 우선순위 t1>t2>t3
    - t1이 t2를 기다리고 있는데, t2가 실행되면 우선순위가 역전된 것
    - t1이 t2를 기다리게 된다.