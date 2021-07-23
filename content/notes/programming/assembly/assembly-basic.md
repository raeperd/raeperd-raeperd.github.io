---
title: Assembly Basic
date: 2018-03-25
tags: ["assembly"]
---

# 어셈블리 기초

## Assembly Basic

- 어셈블리는 무엇인가?
- 어셈블리를 잘하면 좋은점?
- Architecture 별 어셈블러 소개
- CPU 레지스터
- 시스템 콜

## 어셈블리는 무엇인가요?

- 프로그래밍 언어의 하나. 기계어에서 한 단계 위의 언어이며 기계어와 함께 단 둘 뿐인 로우레벨 언어에 속한다.
- 기계어 한 라인에 어셈블리 한라인에 매칭이 된다.
- 기계어는 CPU가 채택한 ISA에 따라 다 다르기 때문에 어셈블리어의 명령어 역시 통일된 규격이 없다.
- 또한 문법 아키텍처에 따라서도 다르고 어셈블러의 종류에 따라서도 문법/매크로 등이 제각각이다.

![./image/Untitled.png](https://i.ibb.co/NLYKJjq/Untitled.png)

## 실습 환경 구성

- Ubuntu 16.04.3
- NASM 설치 (INTEL ASM 문법)
    - sudo apt-get install nasm
- gcc (AT&T ASM 문법, 기본적으로 설치되어 있음)

![./image/Untitled1.png](https://i.ibb.co/9GLv77d/Untitled1.png)

## Hello World!

```clike
global _start

section .text
_start:
				mov rax, 1
				mov rdi, 1
				mov rsi, message
				mov rdx, 14
				syscall
				mov rax, 60
				xor rdi, rdi
				syscall

section .data
message: db "Hello, World!", 10
```

> nasm -felf64 hello.asm ; ld -o hello hello.o

> ./hello

text 영역 / data 영역 / 리소스 영역 / bss 영역

section .text : 다음 텍스트를 text영역에 넣어라

section .data : 다음을 데이터 영역에 넣어달라 (실행가능한 영역이 아님)

-tab 없이도 실행 가능함

## 어셈블리 & Hex code 확인

![./image/Untitled2.png](https://i.ibb.co/0mwZF8L/Untitled2.png)

> objdump -M intel -d hello

- M을 빼면 기본적으로 AT&T 문법으로 나옴
- 1을 eax를 넣고
1을 edi에 넣고
rsi에 변수를 넣고 (변수의 주소 0x)

**함수호출 규약**

- rax 에는 시스템 콜의 번호가 들어간다
- rdi 에는 시스템 콜의 첫번째 인자
- rsi 시스템 콜의 두번쨰 인자
rdx
rcx
r8r9 ~ 15

- rax를 eax 로 왜 바꿨지?
- eax가 더 작아 이거면 충분해
- 7번째 인자는 스택을 이용한다.
- 시스템 콜의 번호 1번이 무슨의미일까?
    - 1은 write, 60은 exit

## 레지스터

- 어셈블리에서 사용 되는 기본 변수 이름
    - 다른 언어와 달리, 하드웨어 적으로 구현됨
    - **이름 변경 불가능**
    - CPU 종류 별로 레지스터 이름이 다 다름
    - 레지스터 이름 별로, 정해진 용도 존재
- 그럼 message도 레지스터 인가?
    - NO : nasm이 정의한 symbol 임 (objdump 결과 다시 확인 ㄱ)

## x64 레지스터

![./image/Untitled3.png](https://i.ibb.co/LRVwRVF/Untitled3.png)

- RFLAGS
- EIP / RIP 레지스터: 다음에 실행될 주소

![./image/Untitled4.png](https://i.ibb.co/MsFVpKK/Untitled4.png)

- 64bit 를 쓸때는 RAX를 씀
32bit 를 쓸 떄는 EAX
16bit 를 쓸 때는 AX
8bit AH (high) / AL (low) 상위 / 하위를 따로 쓸 수 있다.
RBX RCX RDX도 마찬가지다
- RSI의 32,16,8 비트 표현은?
    - esi, si, sil
- sih는 있는가?
    - sih 레지스터는 없다
- R8?
    - r8d r8w r8b (word, byte)

- RAX : 함수 반환값에 사용한다
- RBX : 메모리 주소 지정, RSI,RDI와 함께
- RCX : loop counter로 사용
- RSI : 복사될 주소 strcpy 두번째 인자
- RDI : 복사할 주소 strcpy 첫번쨰 인자
- RBP : 현재 함수의 Base Pointer
- RSP : Stack Pointer, 현재 스택 최상위를 가리킴, Push/Pop에 의해 항상 바뀜
- RIP : 다음에 수행 될 코드 영역을 가리키고 있음

## MIPS Register

![./image/Untitled5.png](https://i.ibb.co/BzXc43W/Untitled5.png)

## ARM core registers in ARM state

![./image/Untitled6.png](https://i.ibb.co/7bgp2c9/Untitled6.png)

# x64 명령어

**mov**

- mov [reg|addr], [imm|reg|mem]
- 값 이동

**LEA**

- LEA [reg], [mem]
- 주소 이동

## MOV vs LEA

- MOV eax, [rbp-16] // rbp에 있는 주소에 16을 빼고 그 주소의 값을 가져와서 eax에 넣는다
    - eax = *(rbp-16)
- LEA eax, [rbp-16]
    - eax = (rbp-16)
    - mov eax, rbp ; sub rax, 16; 와 같음.
- LEA가 있음으로써, 2줄짜리 mov 명령어를 1개 instruction으로 만듬

**CMP**

- CMP [imm|reg|mem], [reg,mem]

**TEST**

- TEST [reg], [imm|reg]

**JMP**

- JMP 다음에 오는 LABEL, 프로시저로 이동

**CALL**

- CALL 다음에 있는 주소를 호출함
- CALL 다음 명령어 주소를 스택에 PUSH

## CALL vs JMP

JMP : 그냥 JMP 다음의 주소로 이동
CALL: CALL 다음에 있는 명령어 주소를 스택에 PUSH

**RET**

- 현재 RSP가 가리키고 있는 값으로 점프한다.
- ==⇒ 스택 최상위에 있는 값으로 JUMP한다.
- ==⇒ RSP를 8증가 시킨다.

**JZ**

- JZ JE JLE JGE JL JG JS JNS
- eflags에 영향을 가장 많이 받는 명령어들

**REP**

- RCX 레지스터를 1씩 감소 시키면서, 문자열 관련 명령어를 처리한다.
- memset, memcopy 구현 할 때 필요 하지 않을까?

**STOS**

- AX를 EDI가 가리키는 주소에 넣음
- rep stos byte ptr[EDI]

**SCAS**

- AX에 저장되 있는 값과 EDI가 가리키는 곳에 저장 되어 있는 값을 비교 (strcomp랑 비슷)

**PUSH[reg] / POP[reg]**

- PUSH : 스택 최상위에 값 넣음 ; RSP 레지스터 증가
- POP : 스택 최상위에서 값 빼옴 ; RSP 감소

**ADD/SUB/MUL/DIV**

- e(r)flags에 상당한 영향을 주는 명령어들임

## x64 명령어 모음

[https://www.felixcloutier.com/x86/](https://www.felixcloutier.com/x86/)

# **Stack**

- 함수호출
- 지역 변수 저장 및 사용 (전역변수는 스택을 사용하지 않는다)
- 함수 인자 전달 (7번째 인자부터)
- 운영체제에서 직접 사용하기 위해 만든 스택

## Stack 관련 레지스터

- RSP
- RBP
    - 현재 함수 암에서 변수들의 기준점이 됨
- Return Address, a.k.a. RET (not instruction)
    - 함수 종료 후, 돌아갈 주소를 가리키고 있음
- (STACK||Saved) Frame Pointer, a.k.a SFP
    - caller 함수의 RBP 값을 가지고 있음

![./image/Untitled7.png](https://i.ibb.co/GkNmjn0/Untitled7.png)

- 스택에 꺼내고서 초기화를 해주어야함! 

## 함수 호출과 스택

```clike
void func(int a, int b) {
 return a+b;
}
int main() {
	func1(1,2);
	return 0;
}
```

스택비초기화 취약점

## POSIX C 함수 구현 실습

- x64 Assembly로 자주 사용되는 POSIX C 함수 구현
- 구현 해 볼것
    - memcpy / memset / srtcpy / strlen

memcpy는 널이 있어도 복사 / strcpy는 널이 있으면 그만

memcpy가 최적화가 안되있음