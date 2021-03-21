---
title: "FarCall-vs-NearCall"
date: 2019-10-22T00:18:35+09:00
tags: ["security"]
---

# INTRO

어셈블리 레벨에서 프로그램을 분석할 때, `CALL` 명령은 매우 중요한 역할을 한다. 
실행 중인 프로그램의 실행 순서는 `CALL` 이나 JMP 와 같은 흐름을 제어하는 명령을 만나지 않는다면 기본적으로 순차적으로 진행된다. `CALL` 은 이런 프로그램의 진행 흐름을 변경하는 명령문으로 프로그램의 흐름을 따라가려면 당연하게도 `CALL` 명령어의 흐름을 따라갈 수 있어야한다.

printf 함수를 어셈블리 레벨에서 볼 일이 있었는데, 그 과정에서 알게된 사실을 기록해둔다. 

<br/>

# CONTENTS

## prtinf 함수의 본모습

`printf` 함수는 어셈블리 레벨에서 아래와 같은 코드로 구성되어 있다.

```
_printf:
  00401050: 55                   push        ebp
  00401051: 8B EC                mov         ebp,esp
  00401053: 83 EC 08             sub         esp,8
  00401056: 8D 45 0C             lea         eax,[ebp+0Ch]
  00401059: 89 45 FC             mov         dword ptr [ebp-4],eax
  0040105C: 8B 4D FC             mov         ecx,dword ptr [ebp-4]
  0040105F: 51                   push        ecx
  00401060: 6A 00                push        0
  00401062: 8B 55 08             mov         edx,dword ptr [ebp+8]
  00401065: 52                   push        edx
  00401066: 6A 01                push        1
  00401068: FF 15 AC 20 40 00    CALL        dword ptr [__imp____acrt_iob_func]
  0040106E: 83 C4 04             add         esp,4
  00401071: 50                   push        eax
  00401072: E8 A9 FF FF FF       CALL        __vfprintf_l
  00401077: 83 C4 10             add         esp,10h
  0040107A: 89 45 F8             mov         dword ptr [ebp-8],eax
  0040107D: C7 45 FC 00 00 00 00 mov         dword ptr [ebp-4],0
  00401084: 8B 45 F8             mov         eax,dword ptr [ebp-8]
  00401087: 8B E5                mov         esp,ebp
  00401089: 5D                   pop         ebp
  0040108A: C3                   ret
  0040108B: CC CC CC CC CC
```

두번의 `CALL` 명령을 각각 *00401068*, *00401072* 주소에서 확인 할 수 있다.  
  
자세히 보아야 확인할 수 있는데 두 `CALL` 명령어는 다른 기계어를 가지고 있다. 첫번째 `CALL` 명령어는 FF15 ~ 로 시작하는 반면 두번째 `CALL` 명령어는 E8~ 로 시작하는 것을 알 수 있다. 원래 `CALL` 명령은 당연히 E8 명령이라고 생각했는데 다른 기계어를 같은 어셈블리로 해석하는 것처럼 보여 머리에 쥐가 나기 시작했다. 그래서 이것저것 공부해본 것을 정리해 본다.

## Near Call and Far Call

프로그램이 함수를 호출할때, 현재의 주소를 스택에 저장하고 IP(Instruction Pointer)를 다음 함수의 호출 주소로 변경하면서 실행흐름을 변경한다. 함수 호출간의 인자를 어떻게 전달하고, 복귀 주소를 어떻게 저장하는지도 중요한 주제지만, 이번 글에서는 IP 가 어떻게 변경될 수 있는지에 대해 더 자세하게 알아보려 한다. 

CALL 명령의 인자로 전달 될 수 있는 값은 상수, 레지스터, 메모리 주소가 있다. 함수 호출에는 크게 다음과 같이 4가지 종류가 있다.

- Near Call 
  - 현재 code segment 내의 주소로의 CALL (CS register가 가리키고 있는 segment)
- Far Call
  - 다른 code segement 주소로의 CALL
- Inter-privilege-level far call 
  - 다른 권한을 가지고 있는 code segment 주소로의 Far Call
- Task switch
  - 다른 task에 존재하는 주소로의 CALL

 가장 아래의 두 CALL 은 protected mode 에서만 호출 될 수 있는 CALL 이다. 오늘의 관심사는 첫 두 콜에 대한 얘기가 될 것.

### Near Call

Near Call은 일반적으로 우리가 알고있는 함수 호출 과정이 이루어 진다. 프로세서는 IP를  스택에 push 하고, CALL 명령의 operand의 주소로 실행 흐름을 변경한다. 이때 operand 로 올 수 있는 값은 **1. code segment로 부터의 offset (absolute)** 과 **2. 현재 IP의 값으로 부터의 offset (relative)**, 두가지를 가질 수 있다. 

#### 1. operand가 code segment 로 부터의 offset 인 경우 (absolute)

이 경우 offset 은 메모리주소나 레지스터등을 통해 간접적으로 주어진다. 몇 bit 프로세서냐에 따라 operand의 크기는 달라지는데, 만약 64-bit 프로세서인 경우 offset 은 강제적으로 64 bit 에 맞춰진다. 곧 absolute offset 은 EIP(RIP) 레지스터에 바로 load 된다. 

#### 2. operand가 IP 로부터의 offset 인 경우 (relative)

이 경우 일반적으로 어셈블리 코드에서는 label 을 확인할 수 있다. (위 예의 경우 원래 명령어는 `E8 A9 FF FF FF` 이지만  어셈블리에서는 `CALL __vfprintf_l` 와 같이 함수 이름을 확인할 수 있다.) 하지만 기계어 레벨에서는 부호가 있는 16 혹은 32-bit 값으로 해석된다. 64-bit 프로세서의 경우 operand의 크기는 32-bit 크기로 고정이고, 이 값은 RIP 와 연산되기 이전에 64-bit 로 확장되어 연산된다. 



### Far Call

Far Call의 경우 포로세서는 IP와 함께 CS 레지스터의 값도 스택에 push 한다. 이후 다른 code segment의 주소로 실행흐름을 변경해야하기 때문에 Near Call 보다 전달 받아야하는 인자가 많다. 이때 operand는 pointer 를 이용한 절대적인 주소일 수도 있고 double pointer를 통한 간접적인 주소일 수도 있다. 

pointer를 사용하는 방법의 경우 4바이트(16-bit)나 6바이트(32-bit)의 주소를 operand로 전달받는다. 

Far Call 의 경우 더 많은 케이스가 있는데 더 자세한 케이스는 [여기]( https://c9x.me/x86/html/file_module_x86_id_26.html ) 에서 확인할 수 있다. 

## 그래서 왜 FF15 도 CALL이죠?

### FF 의 비밀

결국 CALL 도 내가 알고 있던것 보다 더 많은 종류가 필요했고, 당연히 프로세서가 알아먹을려면 다른 명령어로 명령을 전달해야한다. 그런 이유에서 E8 외에도 다양한 명령이 필요한데, 가능한 명령은 아래와 같다. 


| Opcode  | Mnemonic        | Description                                                  |
| ------- | --------------- | ------------------------------------------------------------ |
| `E8 cw` | `CALL rel16`    | Call near, relative, displacement relative to next instruction |
| `E8 cd` | `CALL rel32`    | Call near, relative, displacement relative to next instruction |
| `FF /2` | `CALL r/m16`    | Call near, absolute indirect, address given in r/m16         |
| `FF /2` | `CALL r/m32`    | Call near, absolute indirect, address given in r/m32         |
| `9A cd` | `CALL ptr16:16` | Call far, absolute, address given in operand                 |
| `9A cp` | `CALL ptr16:32` | Call far, absolute, address given in operand                 |
| `FF /3` | `CALL m16:16`   | Call far, absolute indirect, address given in m16:16         |
| `FF /3` | `CALL m16:32`   | Call far, absolute indirect, address given in m16:32         |


FF 명령이 있는걸 확인할 수 있다 ! E8 과 구체적으로는 다른 명령어지만 비슷한 역할을 하기 때문에 dumpbin 은 그냥 같은 CALL 로 해석을 한 것 같다. 하지만 아직 FF 뒤의 한 바이트 0x15 는 이해하지 못했다.

### 0x15 바이트의 비밀

 FF로 시작하는 명령어는 `INC`, `DEC`, `CALLN`, `CALLF`, `JMPN`, `JMPF`, `PUSH` 가 있다. FF 뒤의 한 바이트는 특정한 [규칙](http://wiki.osdev.org/X86-64_Instruction_Encoding#ModR.2FM)을 따르는데, 0x15를 통해 예를 보자. 

 0x15는 바이너리로 표현하면 `0001 0101` 이 된다. 5번째 비트 부터 `010` 인데 이는 10진수로 2다. 이는 명령어 집합 {`INC`, `DEC`, `CALLN`, `CALLF`, `JMPN`, `JMPF`, `PUSH`} 중 3번째 원소를 택해야한다는 것을 의미한다. (인덱스가 0번부터 시작해서 2번 인덱스가 3번째 원소가 된다. 배열처럼!) 

 결국 `CALLN` 을 얻게 되는데, 이때의 N은 Near를 의미한다! 결국 `FF15` 는 간접호출 이면서, Near Call 을 의미하는 명령어 였던 것이다 ! 


# 결론


 다하고 나니까 이걸 왜했나 싶기도 한데 재밌었다! 역시 컴퓨터를 만든사람은 어지간히 똑똑한게 아니구나 싶었고 이런 고민도 결국 누군가가 이미 했구나는 생각이 든다. 본 글의 대부분도 StackOverflow 나 다른 블로그에서 본 글을 정리한것에 지나지 않는다. 

  

 아주 완벽하게 모든걸 이해한 것은 아니지만 (Far Call의 다른 호출이 어떤식으로 이루어지는지, 64비트와 32비트 프로세서가 어떻게 다르게 동작하는지 등) 이정도 이해하고 만족하고 넘어가려 한다. 나중에 더 궁금해지면 그때 다시 알아보면 된다. 


# Reference

[Why call instruction opcode is represented as FF15?](https://stackoverflow.com/questions/29837363/why-call-instruction-opcode-is-represented-as-ff15)  
[How can I tell if jump is absolute or relative?](https://stackoverflow.com/questions/31544052/how-can-i-tell-if-jump-is-absolute-or-relative)  
[X86-64 Instruction Encoding](https://wiki.osdev.org/X86-64_sInstruction_Encoding#ModR.2FM )

