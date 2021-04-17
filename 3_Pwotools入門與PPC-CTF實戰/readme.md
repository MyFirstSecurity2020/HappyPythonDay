# pwntools
```
pwntools is a CTF framework and exploit development library. 

Written in Python, it is designed for rapid prototyping and development, 
and intended to make exploit writing as simple as possible.
```
# 
```
https://docs.pwntools.com/en/latest/   <==文件
https://github.com/Gallopsled/pwntools <== 
```
# 
```
from pwn import *
context(arch = 'i386', os = 'linux')

r = remote('exploitme.example.com', 31337)
# EXPLOIT CODE GOES HERE
r.send(asm(shellcraft.sh()))
r.interactive()
```
