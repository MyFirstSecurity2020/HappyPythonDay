# python開發環境
```
python有許多開發環境與IDE
本課程將使用底下兩種
[1]linux python程式開發
[2]使用Goolge Colab 平台開發python程式

其他請參閱Google或書籍
   如Windows python程式開發
```
# linux python程式開發(1)互動式模式
### python3[持續發展中, 學習python的重心]
```
打開terminal ==> 輸入python3
     ===> 開始python3互動式 開發
     
root@kali:~# python3 <==輸入python3
Python 3.7.3rc1 (default, Mar 13 2019, 11:01:15) 
[GCC 8.3.0] on linux
Type "help", "copyright", "credits" or "license" for more information.


>>> print("Hello, World!")   <== 輸入簡單的程式
Hello, World!

>>> exit() <== 離開互動式模式
```
### python2[已經不再更新, 但仍有些套件使用]
```
打開terminal ==> 輸入python
     ===> 開始python2互動式 開發

oot@kali:~# python  <==輸入python
Python 2.7.16 (default, Apr  6 2019, 01:42:57) 
[GCC 8.3.0] on linux2
Type "help", "copyright", "credits" or "license" for more information.

>>> print("Hello, World!")    <== 輸入簡單的程式
Hello, World!

>>> exit()   <== 離開互動式模式
```
# linux python程式開發(2)標準模式
```
使用最簡單的gedit 撰寫程式 ==> gedit XXX.py
執行程式 ==> python3 XXX.py
```
### 範例

使用最簡單的gedit 撰寫程式 ==> gedit test.py
```
# 先不用解釋程式 後面會教
i = 1

while i < 6:
  print(i)
  i += 1
```
執行程式 ==> python3 XXX.py
```
python3 test.py

#若是要執行python2程式 則執行python test.py
```
