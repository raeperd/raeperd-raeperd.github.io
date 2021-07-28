# Chapter02. Processes and Threads

# The Process Model

- 하나의 CPU는 한순간에 하나만을 실행한다.

# Scheduling

스케쥴링 시점

## Type of Scheduling

Long term scheduling

- 사용자의 요구를 모두 받지는 않는다.
- 이미 시스템에 너무 많은 프로세스가 있다면 시스템이 다운 될 수 있어 실행을 거부

Medium-term-scheduling 

- 프로세스의 이미지를 하드의 스왑 영역에 복사하고 메모리에서 뺀다.
- 시스템이 다운되는 것을 방지하기 위해서

Short-term scheduling

- 1초에 여러번 실행됨

I/O 스케쥴 

- later

![Chapter02%20Processes%20and%20Threads%2095e727ea093f4130852d9257dc62fda9/Untitled.png](Chapter02%20Processes%20and%20Threads%2095e727ea093f4130852d9257dc62fda9/Untitled.png)

## Categories of Scheduling Algorithms

- Batch
- Interactive
- Real time

## Scheduling Algorithm Goals

![Chapter02%20Processes%20and%20Threads%2095e727ea093f4130852d9257dc62fda9/Untitled%201.png](Chapter02%20Processes%20and%20Threads%2095e727ea093f4130852d9257dc62fda9/Untitled%201.png)

- 스케쥴링 알고리즘마다 목표는 다르다.
- 아마 가장 중요한건 Fairness

## Scheduling in Batch System

- First-come first served
- Shortest job first
- Shortest remaining time next

## First-come first-served

![Chapter02%20Processes%20and%20Threads%2095e727ea093f4130852d9257dc62fda9/Untitled%202.png](Chapter02%20Processes%20and%20Threads%2095e727ea093f4130852d9257dc62fda9/Untitled%202.png)

![Chapter02%20Processes%20and%20Threads%2095e727ea093f4130852d9257dc62fda9/Untitled%203.png](Chapter02%20Processes%20and%20Threads%2095e727ea093f4130852d9257dc62fda9/Untitled%203.png)

- B 실행 중 CDE가 도착하지만 실행한 프로세스는 끝낸다.

## Shortest Job First

![Chapter02%20Processes%20and%20Threads%2095e727ea093f4130852d9257dc62fda9/Untitled%204.png](Chapter02%20Processes%20and%20Threads%2095e727ea093f4130852d9257dc62fda9/Untitled%204.png)

![Chapter02%20Processes%20and%20Threads%2095e727ea093f4130852d9257dc62fda9/Untitled%205.png](Chapter02%20Processes%20and%20Threads%2095e727ea093f4130852d9257dc62fda9/Untitled%205.png)

![Chapter02%20Processes%20and%20Threads%2095e727ea093f4130852d9257dc62fda9/Untitled%206.png](Chapter02%20Processes%20and%20Threads%2095e727ea093f4130852d9257dc62fda9/Untitled%206.png)

## Shortest Remaining Time Next

Preemptive version of Shortest Job First

![Chapter02%20Processes%20and%20Threads%2095e727ea093f4130852d9257dc62fda9/Untitled%207.png](Chapter02%20Processes%20and%20Threads%2095e727ea093f4130852d9257dc62fda9/Untitled%207.png)

- Minimize turnaround time
- 시간이 긴 작업에게는 두 스케쥴링은 불공평하다. (Starvation)

## Highest Response Ratio Next(HRRN)

![Chapter02%20Processes%20and%20Threads%2095e727ea093f4130852d9257dc62fda9/Untitled%208.png](Chapter02%20Processes%20and%20Threads%2095e727ea093f4130852d9257dc62fda9/Untitled%208.png)

- Choose next process with the greatest ratio
- (time spent waiting + expected service time) / expected service time
- Starvation을 처리하기 위함

## Scheduling in Interactive Systems

- Round-robin scheduling
- Priority scheduling
- Multiple queues
- Shortest process next
- Guaranteed Scheduling
- Lottery scheduling
- Fair-share scheduling

## Round-Robin Scheduling

![Chapter02%20Processes%20and%20Threads%2095e727ea093f4130852d9257dc62fda9/Untitled%209.png](Chapter02%20Processes%20and%20Threads%2095e727ea093f4130852d9257dc62fda9/Untitled%209.png)

## Determining Preeption Time Quantum

![Chapter02%20Processes%20and%20Threads%2095e727ea093f4130852d9257dc62fda9/Untitled%2010.png](Chapter02%20Processes%20and%20Threads%2095e727ea093f4130852d9257dc62fda9/Untitled%2010.png)

- 우선순위마다 타임 퀀텀이 다르다.
- 타임 퀀텀이 지나기전에 CPU 테스크가 끝나면 바람직
- 한번 실행하다가 다음 타임 퀀텀에서 조금 실행하다 멈추면 별로임
- 타임퀀텀은 프로세스가 CPU 연산이 끝나고 스스로 입출력을 요구하는 시간보다 조금 더 큰게 바람직

## Virtual Round-Robin

![Chapter02%20Processes%20and%20Threads%2095e727ea093f4130852d9257dc62fda9/Untitled%2011.png](Chapter02%20Processes%20and%20Threads%2095e727ea093f4130852d9257dc62fda9/Untitled%2011.png)

- Auxi que 가 ready q 보다 우선순위가 높다.
- 이게 필요한 이유

## Priority Scheduling

![Chapter02%20Processes%20and%20Threads%2095e727ea093f4130852d9257dc62fda9/Untitled%2012.png](Chapter02%20Processes%20and%20Threads%2095e727ea093f4130852d9257dc62fda9/Untitled%2012.png)

- ready que is not only one
- 대부분의 운영체제가 이 방식을 따른다.
- IO bound 와 CPU bound 사이의 우선순위는 IO 바운드가 높다.

    그런데 어떻게 ?

## Multilevel Feedback Queue **

- 입출력을 끝나면 RQ0로 돌아간다.

    ⇒ IO bound 프로세스의 우선순위를 높게 할 수 있다.

- RQ1 프로세스가 Starvation을 겪을 수 있다.

    ⇒ Aging mechanism must be supplemented

## Short Process Next

- Process with shortest expected processing time is selected next
- Short process jumps ahead of longer processes
- Exponential Averaging