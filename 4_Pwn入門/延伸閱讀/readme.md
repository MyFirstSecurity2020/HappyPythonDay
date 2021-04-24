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

#### 測試
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
