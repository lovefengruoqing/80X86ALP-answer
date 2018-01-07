## 第二章习题答案

### 第1题答案

*[点击查看题目](./homework.md '点击前往')*

```assembly
(1)MOV SI, 10           ;10为立即寻址，SI为寄存器寻址
(2)MOV DI, [EAX]        ;源操作数为寄存器间接寻址，目的操作数为寄存器寻址
(3)ADD EAX, 4[BX]       ;源操作数为变址寻址，目的操作数为寄存器寻址
(4)SUB DX, 5[BX +DI]    ;源操作数为基址加变址寻址，目的操作数为寄存器寻址
(5)MOV [EDI*4 +2], AX   ;源操作数为寄存器寻址，目的操作数为变址寻址
(6)MOV BH, DS:[10]      ;源操作数为直接寻址，目的操作数为直接寻址
(7)ADD [SI], SI         ;源操作数为寄存器寻址，目的操作数为寄存器间接寻址
(8)ADD CX, -7[BP +DI]   ;源操作数为基址加变址寻址，目的操作数为寄存器寻址
```

### 第2题答案

*[点击查看题目](./homework.md '点击前往')*

| 指令 | 目的操作数地址 | 执行前目的操作数的值 | 执行后目的操作数的值 | 指令正确与否 |
| :- | :-: | :-: | :-: | :-: |
| MOV AX, 3 | 寄存器AX | 0AAH | 3H | 是 |
| SUB WORD PTR [AX], 3 |  |  |  | 否|
| ADD WORD PTR [EAX], 3 | 100AAH | 2H | 3H | 是 |
| ADD BH, 2 | 寄存器BH | 0 | 2H | 是 |
| SUB EBP, 2 | 寄存器EBP | 5000H | 4FF8H | 是 |
| SUB [CX], DX |  |  |  | 否 |
| ADD DI, 2 | 寄存器DI | 6000H | 6002H | 是 |
| SUB SI, 10 | 寄存器SI | 4000H | 3FF6H | 是 |
| MOV [BX], BX | 100BBH | 200H | 2BBH | 是 |
| MOV [DI], DX | 16000H | 300H | 3DDH | 是 |
| MOV ES: [DX], BX | | | | 否 |
| MOV ES: [SI], DS | | |  | 否 |
| MOV DX, SS | | |  | 否 |
| MOV EAX, EIP |  | |  | 否 |
| SUB 2[DX], AX | | |  | 否 |
| ADD 500H[BP], CX | 35500H | 100H | 1CCH | 是 |
| SUB [SI -300H], AX | 13D00H | 0F13DH | 0F093H | 是 |
| MOV [AX +2], BX | | |  | 否 |
| MOV [DI +1000H], SI | 17000H | 0F13DH | 4000H | 是 |
| MOV [CX -100H], AX |  | |  | 否 |
| MOV [DX +60], AX | | |  | 否 |
| MOV -8[BX], CX | 100B3H | 0F8BBH | 0CCH | 是 |
| MOV ES: 1000[DI], BP | 27000H | 270H | 5000H | 是 |
| MOV [BP +SI], DX | 39000H | 39H | 0DDH | 是 |
| MOV [DI +SI], DX | | |  | 否 |
| MOV [EDI +ESI], DX | | | | 否 |
| MOV [BX +DI], 10H | 160BBH | 0H | 10H | 是 |
| MOV [BX +DI], DX | 160BBH | 0 | 0DDH | 是 |

### 第3题答案

*[点击查看题目](./homework.md '点击前往')*

代码的分析：
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
START:  MOV AX, DATA    ;这两句完成将数据段首址DATA置入数据段寄存器DS中
        MOV DS, AX
        MOV SI, OFFSET BUF1     ;OFFSET为取偏移地址算符
        MOV DI, OFFSET BUF2     ;此四句为取BUF1……BUF4的偏移地址送入寄存器之中
        MOV BX, OFFSET BUF3
        MOV BP, OFFSET BUF4
        MOV CX, 10      ;将立即数10送入CX寄存器中，作接下来作循环操作时的计数量，起到类似于高级语言（例如C语言）中for循环中i的作用
LOPA:   MOV AL, [SI]    ;此句用了寄存器间接寻址的方法，将SI中所存地址对应的内存中的数据送入寄存器AL之中
        MOV [DI], AL    ;此句将AL之中的数据，送入DI对应的的偏移地址的内存单元中
        INC AL          ;AL自增
        MOV [BX], AL    ;将自增后的AL中存储的数送入送入BX对应的的偏移地址的内存单元中
        ADD AL, 3       ;将AL中的数据再加上3，然后存储在AL中
        MOV DS:[BP], AL ;将AL中存储的数送入BP对应的的偏移地址的内存单元中
        INC SI          ;此四句将SI、DI、BP、CX分别自增，指向各自其后面的一个内存存储单元
        INC DI
        INC BP
        INC BX
        DEC CX          ;对CX自减
        JNZ LOPA        ;当CX不等于零时跳转到LOPA标号对应的地方继续执行指令
        MOV AH, 4CH     ;此两句为DOS功能调用指令，执行完该指令以后，计算机将结束本程序的运行，返回DOS状态
        INT 21H
CODE    ENDS
        END START
```
不难看出，BUF2指向的一片连续的地址空间区域存储的数据和BUF1指向的地址空间存储的数据一模一样，BUF3指向的地址空间存储的数据则为BUF1指向的地址空间存储的数据在原来的基础上"+1"后所得到的结果，而BUF4指向的地址空间存储的数据则为BUF1指向的地址空间存储的数据在原来的基础上"+4"后所得到的结果，因此以BUF2、BUF3、BUF4为首址i的三个字节存储区中存放的数据为：
```asm
BUF1    DB 0, 1, 2, 3, 4, 5, 6, 7, 8, 9
BUF2    DB 0, 1, 2, 3, 4, 5, 6, 7, 8, 9
BUF3    DB 1, 2, 3, 4, 5, 6, 7, 8, 9, 10
BUF4    DB 4, 5, 6, 7, 8, 9, 0AH, 0BH, 0CH, 0DH
```

### 第4题答案

*[点击查看题目](./homework.md '点击前往')*

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
        MOV SI, OFFSET BUF1     ;只取BUF1的偏移地址
        MOV CX, 10
LOPA:   MOV AL, [SI]
        MOV [SI + 10H], AL      ;通过变址寻址的方式定位内存单元
        INC AL
        MOV [SI + 14H], AL      ;通过变址寻址的方式定位内存单元
        ADD AL, 3
        MOV [SI + 1EH], AL      ;通过变址寻址的方式定位内存单元
        INC SI
        DEC CX
        JNZ LOPA
        MOV AH, 4CH
        INT 21H
CODE    ENDS
        END START
```
