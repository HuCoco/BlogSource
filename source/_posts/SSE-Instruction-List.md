---
title: SSE Instruction List
date: 2018-03-26 22:22:53
thumbnail: http://hucoco.com/img/SIMD/Example_SIMD.jpg
tags: 
- C++
- SIMD
- SSE
categories:
- SIMD
- SSE
---

## move instruction

|Instruction|Description|
|::|::|
|movaps|move 4 alignment single precision value to xmm register|
|movups|move 4 non-alignment single precision value to xmm register|
|movss|move 1 alignment single precision value to low 4 bytes of register|
|movlps|move 2 alignment single precision value to low 8 bytes of register|
|movhps|move 2 alignment single precision value to high 8 bytes of register|
|movlhps|move 2 alignment single precision value to high 8 bytes of register from low 8 bytes|
|movhlps|move 2 alignment single precision value to low 8 bytes of register from high 8 bytes|


## basic operation instruction

|Instruction|Description|
|::|::|
|addps|add operation|
|subps|sub operation|
|mulps|mul operation|
|divps|div operation|
|rcpps|rcp opeartion|
|sqrtps|sqrt operation|
|rsqrtps|rcp sqrt operation|
|maxps|get max operation|	
|minps|get min operation|
|andps|and operation|
|andnps|negation operation|
|orps|or operation|
|xorps|xor operation|

## compared instruction

|Instruction|Description|
|::|::|
|cmpps|compared operation|
|cmpss|compared operation|
|comiss|compared and set eflags register|
|ucomiss|compared and set eflags register|

those instruction will return a value:

|Return Value|Description|
|::|::|
|0|Equal to|
|1|Less-than|
|2|Less than or equal to|
|3|Disorder|
|4|Not equal to|
|5|Greater than|
|6|Greater than or equal to|
|7|Order|