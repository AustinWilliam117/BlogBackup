---
title: 实验14 访问CMOS RAM
date: 2021-08-02 19:35:02
tags: ["汇编"]
categories: ["计算机基础"]
---

编程，用加法和移位指令计算ax=ax*10
提示，ax*10=ax*2+ax*8

```asm
mov al,00000001b

mov bx,ax
shl ax,1
mov cl,3
shl bx,cx
add ax,bx
```

CMOS RAM中，存放着当前的时间：年、月、日、时、分、秒。这6个信息的长度都为1个字节，存放单元为：
秒：0 分：2 时：4 日：7 月：8 年：9，这些数据以BCD码的方式存放

<!--more-->

编程，在屏幕中间显示当前的月份

分析，这个程序主要做以下两部分工作
1.从CMOS RAM的8号单元读出当前月份的BCD码,应首先向地址端口70h写入要访问的单元的地址
mov al,8
out 70h,al
然后从数据端口71h中取得指定单元的数据
in al,71h

2.将BCD码表示的月份以十进制的形式显示到屏幕上
BCD码值 = 十进制数码值，则BCD码值+30h=十进制对应的ASCII码

从CMOS RAM的8号单元读出的一个字节中，包含了用两个BCD码表示的两位十进制数
高4位BCD码表示十位，低4位BCD码表示个位。

比如 00010100b表示 14

我们需要进行两步工作
将CMOS RAM的8号单元中读出一个字节，分为两个表示BCD码值的数据

mov ah,al   ;al中为从CMOS RAM的8号单元中读出数据
mov cl,4
shr ah,cl   ;ah中为月份的十位数码值
and al,00001111b ;al中为月份的个数数码值

显示ah+30h 和 al+30h 对应的ASCII码字符

```asm
assume cs:code

code segment

start:  mov al,8
        out 70h,al
        in al,71h
        
        mov ah,al
        mov cl,4
        shr ah,cl
        and al,00001111b
        
        add ah,30h
        add al,30h
        
        mov bx,0b800h
        mov es,bx
        mov byte ptr es:[160*12+40*2],ah  ;显示月份的十位数码
        mov byte ptr es:[160*12+40*2+2],al  ;显示月份的个位数码
        
        mov ax,4c00h
        int 21h
   
code ends
end start
```

编程，以“年/月/日 时:分:秒”的格式，显示当前的日期、时间

注意：CMOS RAM 中存储着系统的配置信息，除了保存时间信息的单元外，不要向其他单元写入内容，否则将引起一些系统错误

在CMOS RAM中，存放着当前的时间：年、月、日、时、分、秒。这6个信息的长度为1个字节，存放单元为：

秒：0      分：2       时：4       日：7       月：8       年：9
0000 0000   0000 0010  0000 0100   0000 0111   0000 1000   0001 0000

```asm
assume cs:code,ds:data,ss:stack

data segment
    db 9,8,7,4,2,0
data ends

stack segment
    db 128 dup (0)
stack ends

code segment

time_style  db  'YY/MM/DD HH:MM:SS',0

time_CMOS   db  9,8,7,4,2,0

start:      mov ax,stack
            mov ss,ax
            mov sp,128

            call init_reg
            call show_clock

            mov ax,4c00h
            int 21h   
            
;=============================================
show_clock: call showTimeStyle

showTime:   mov si,offset time_CMOS
            mov di,160*10+30*2
            mov cx,6

showDate:   mov al,ds:[si]
            out 70h,al
            in al,71h

            mov ah,al

            shr ah,1
            shr ah,1
            shr ah,1
            shr ah,1

            and al,00001111b

            add ah,30h
            add al,30h

            mov es:[di],ah
            mov es:[di+2],al

            inc si
            add di,6            ;di+6跳到YY/MM/DD HH:MM:SS中M位置
            loop showDate

            jmp showTime        ;无限循环

            ret

;=============================================
show_string:push dx
            push ds
            push es
            push si
            push di

string:     mov dl,ds:[si]
            cmp dl,0
            je showStringRet
            mov es:[di],dl
            add di,2
            inc si
            loop string

showStringRet:
            pop di
            pop si
            pop es
            pop ds
            pop dx

            ret

;=============================================
init_reg:   mov bx,0B800H
            mov es,bx

            mov bx,cs
            mov ds,bx
            ret

;=============================================
showTimeStyle:
            mov si,offset time_style
            mov di,160*10+30*2

            call show_string
            ret
            

code ends

end start

```