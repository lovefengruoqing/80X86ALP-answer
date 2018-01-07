## 第二章习题

1. 分别指出下列指令中源操作数和目的操作数的寻址方式。  
```asm
(1)MOV SI, 10   
(2)MOV DI, [EAX]  
(3)ADD EAX, 4[BX]  
(4)SUB DX, 5[BX +DI]  
(5)MOV [EDI*4 +2], AX   
(6)MOV BH, DS:[10]  
(7)ADD [SI], SI  
(8)ADD CX, -7[BP +DI]  
```  
*[【解析】](./answer.md#answer1 "点击前往")*

2. 已知下列各条指令(实方式下)在执行前有:     
```asm
(EAX) = 0AAH     (EBX) = 0BBH     (ECX) = 0CCH     (EDX) = 0DDH
(EBP) = 5000H    (ESI) = 4000H     (EDI) = 6000H    (DS) = 1000H
(ES) = 2000H     (SS) = 3000H
```
以下存储单元的内容均是按字类型说明的:
```asm
(35000H) = 100H  (100BBH) = 200H     (16000H) = 300H    (24000H) = 400H
(35500H) = 355H  (13D00H) = 0F13DH   (17000H) = 0F13DH  (100B3H) = 0F8BBH
(27000H) = 270H  (39000H) = 39H      (160BBH) = 0       (100AAH) = 2
(1A000H) = 1AH
```
试判断下列各指令是否正确, 对于正确的指令,请填写表2.1 中的后面各项。

<div align="center" style="font-weight: bold;">表2.1 指令分析表(前后指令间没有关系)</div>

| 指令 | 目的操作数地址 | 本指令执行前目的操作数的值 | 本指令执行后目的操作数的值 |
| :- | :-: | :-: | :-: |
| MOV AX, 3 |
| SUB WORD PTR [AX], 3 |
| ADD WORD PTR [EAX], 3 |
| ADD BH, 2 |
| SUB EBP, 2 |
| SUB [CX], DX |
| ADD DI, 2 |
| SUB SI, 10 |
| MOV [BX], BX |
| MOV [DI], DX |
| MOV ES: [DX], BX |
| MOV ES: [SI], DS |
| MOV DX, SS |
| MOV EAX, EIP |
| SUB 2[DX], AX |
| ADD 500H[BP], CX |
| SUB [SI -300H], AX |
| MOV [AX +2], BX |
| MOV [DI +1000H], SI |
| MOV [CX -100H], AX |
| MOV [DX +60], AX |
| MOV -8[BX], CX |
| MOV ES: 1000[DI], BP |
| MOV [BP +SI], DX |
| MOV [DI +SI], DX |
| MOV [EDI +ESI], DX |
| MOV [BX +DI], 10H |
| MOV [BX +DI], DX |

*[【解析】](./answer.md#answer1 "点击前往")*



3. 阅读下列程序, 并指出程序执行之后, 以BUF2、BUF3、BUF4 为首址的3 个字节存储区中存放的数据。
```assembly
.386
STACK   SEGMENT  USE16 STACK
        DB  200 DUP(0)
STACK   ENDS
DATA    SEGMENT USE16
BUF1    DB 0, 1, 2, 3, 4, 5, 6, 7, 8, 9
BUF2    DB 10 DUP(0)
BUF3    DB 10 DUP(0)
BUF4    DB 10 DUP(0)
DATA    ENDS
CODE    SEGMENT USE16
        ASSUME CS: CODE, DS: DATA, SS: STACK
START:  MOV AX, DATA
        MOV DS, AX
        MOV SI, OFFSET BUF1
        MOV DI, OFFSET BUF2
        MOV BX, OFFSET BUF3
        MOV BP, OFFSET BUF4
        MOV CX, 10
LOPA:   MOV AL, [SI]
        MOV [DI], AL
        INC AL
        MOV [BX], AL
        ADD AL, 3
        MOV DS:[BP], AL
        INC SI
        INC DI
        INC BP
        INC BX
        DEC CX
        JNZ LOPA
        MOV AH, 4CH
        INT 21H
CODE    ENDS
        END START
```

*[【解析】](./answer.md#answer1 "点击前往")*

4. 试修改上一题中的代码段，改用变址寻址方式访问BUF1、BUF2、BUF3、BUF4四个存储区中的存储单元。

*[【解析】](./answer.md#answer1 "点击前往")*
