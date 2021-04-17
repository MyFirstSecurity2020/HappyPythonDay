# pwntools PPC-CTF實戰
```
底下以範例說明PPC-CTF的解題
1.熟悉連線指令  6.hello world
2.PPC_Ez/3rd 分析與解題
```
```
講師請照時間挑選其他題講解

3.PPC_Ez/beautify 分析與解題
4.PPC_Ez/count 分析與解題
5.PPC_Hard/1.calculator
```
## 1.熟悉連線指令  6.hello world
```
題目敘述
你連的到伺服器嗎?

nc 120.114.62.216 2405
[每一次上課用的IP 與 Port 原則上都會改變, 請仔細讀題]
```

#### 解答
```
nc 120.114.62.216 2405

===== Welcome to CTF =====
You successfully reach this problem
Congratulation!!!
Wait for a few second, let me get you the flag

Here you go : CTF{XXXXXXXXXXXXXXXXXX}  <====解答在此

```
## 2.PPC_Ez/ 3rd 分析與解題
```
可以幫我找出第三大的數字嗎?

nc 120.114.62.216 2400
```
### 先連線看看 nc 120.114.62.216 2400
```
===== Welcome to 3rd Game =====
Can you help me find the 3rd largest number?
All numbers are unique
----- Example -----
numbers : 1 9 7 3 6 2
answer : 6
----- Now You Turn -----
numbers : 26101 59484 71067 3659 60676 16911 90992 84588 36095 6072 85067 44556 28648 30530 3723 85728 17658 71651 26494 82346 49468 40305 83547 66994 38093 23009 10713 11613 57481 66864 78220 16603 78326 93108 60024 90354 72992 78097 14177 36505 42824 35583 84293 94194 86569 17291 60452 95279 62714 29826 62555 50916 64029 29753 598 22380 22623 84748 38691 85756 25071 59893 32306 43447 65107 89621 67137 21846 40460 30377 94665 5229 41864 5549 47196 84516 98601 14389 59604 76420 60875 87412 67073 57658 50835 56111 27423 67654 93585 16019 39156 41773 82041 39784 38452 7709 24484 64275 16990 30925
answer :
```
### 運算思維與分析
### 前7行都不要
```
===== Welcome to 3rd Game =====
Can you help me find the 3rd largest number?
All numbers are unique
----- Example -----
numbers : 1 9 7 3 6 2
answer : 6
----- Now You Turn -----
```
### 運算思維
```
前7行都不要
第8行 ==> 要數字 ==> 然後排序(由小排到大)==> 再取倒數第3個 就是答案
把數字轉成字串
送出答案
```
### 程式實作
```
#!/usr/bin/env python3
from pwn import *

r = remote(‘題目的IP',題目的PORT)  <==

r.recvlines(7)

r.recvuntil('numbers : ')
numbers = map(int, r.recvline().strip().split())
ans = sorted(numbers)[-3]
r.sendlineafter('answer : ', str(ans))

r.interactive()
```
### 最後解答
```
#!/usr/bin/env python3
from pwn import *

r = remote(‘題目的IP',題目的PORT)

r.recvlines(7)

r.recvuntil('numbers : ')
numbers = map(int, r.recvline().strip().split())
ans = sorted(numbers)[-3]
r.sendlineafter('answer : ', str(ans))

r.interactive()
```
### 執行畫面
```
python3 test.py 


[+] Opening connection to 120.114.62.216 on port 2400: Done
[*] Switching to interactive mode
CTF{>>>>>>>解答在這裡>>>>>>>>>}  <====解答在此
[*] Got EOF while reading in interactive
```


