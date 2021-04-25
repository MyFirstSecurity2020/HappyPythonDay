# 4_進階PPC-CTF實戰示範(PPC_Hard)
# PPC_Hard 5.hurry-up
```
root@kali:~# nc 140.110.112.22 2401
===== Welcome to pretty shop =====
Can you help me beautify these sentences?
Rule 1 : change all ' -_' to ' '
Rule 2 : change all alphabet to lower case
----- Example -----
sentence : ThiS-iS_tEst tRY to BeautIfY_mE
answer : this is test try to beautify me
----- Now You Turn -----
sentence : MISs cOnTEmPT_NECk-dip dEFeAT-PAymEnt_innOCENT-pEn-cOnfRonTatION_AdMIraTIon_SChOOL-Trap-CIGarETte-owe hunTER
answer :
```

```
#!/usr/bin/env python3
from pwn import *

def shift(text, s):
    ans = ""
    for c in text:
        if c in string.ascii_letters:
            base = ord('a') if c >= 'a' else ord('A')
            c = ord(c) - base
            c = (c + s + 26) % 26
            c = chr(base + c)
        ans += c
    return ans

r = remote("127.0.0.1", 20000)

r.recvline()

for i in range(100):
    r.recvline()
    line = r.recvline().decode('utf-8')
    s, cipher = line.split(':')
    cipher = cipher.strip()
    s = int(s[35:].strip())
    plain = shift(cipher, s)
    r.sendline(plain)

r.interactive()
```
# PPC_Hard 6.lambda
```
#!/usr/bin/env python3
from pwn import *

F = [lambda x: 3 * (x ** 2) + x + 3,
     lambda x: 5 * (x ** 2) + 8,
     lambda x: 4 * (x ** 3) + 6 * x + 6,
     lambda x: 7 * (x ** 3) + 5 * (x ** 2),
     lambda x: x ** 2 + 4 * x + 3]

r = remote('127.0.0.1', '20000')

r.recvlines(12)

for _ in range(100):
    r.recvline()
    r.recvuntil("function : ")
    f = int(r.recvline().strip())
    r.recvuntil("x = ")
    x = int(r.recvline().strip())
    r.sendline(str(F[f](x)))

r.interactive()
```
