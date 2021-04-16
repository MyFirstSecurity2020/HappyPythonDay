# Python實戰 CTF::使用Python編碼與解碼
```
學習目標1:基礎必教篇 ==Python 內建函數 與 標準函式庫的示範
[1]使用Python程式與內建函數進行ASCII的編碼與解碼
[2]使用Python標準函式庫(Base 64模組)進行BASE64的編碼與解碼

[3](可不教)補充教材:數字系統( number system)的轉換

學習目標2:進階競賽
[4]Internetwache CTF 2016 : The hidden message
[5]需要神的靈光閃爍==SECCON CTF 2014: Easy Cipher 

其他參考資料
```
# 編碼與解碼101

# [1]使用Python程式與內建函數進行ASCII的編碼與解碼

### Python 內建函數(Built in Functions):chr()與ord()
```
https://docs.python.org/3/library/functions.html

chr(97)

chr(66)

#ord('a')
#print(bin(ord('a')))
#oct(ord('a'))
print(hex(ord('a')))
```
### 編碼與解碼 101---ASCII編碼解題

### 字串的分割
```
a="66 114".split("")
a

a="66 114".split("  ")
a

a="66 114".split(" ")
a
type(a)
```
### 把每一個分割後的字串==轉成整數後===再編碼出ASCII的字
```
a="66 114".split(" ")

result=''

for x in a:
  y=chr(int(x))
  result += y
# result = result + y 

result
```
### 最後的解答
```
#!/usr/bin/python

c = '66 114 101 97 107 65 76 76 67 84 70 123 65 109 118 48 117 68 121 101 114 118 80 116 109 86 114 57 83 83 83 75 125'

flag = ""

for _ in c.split(' '):
  flag += chr(int(_))

print(flag)
```
#### 
```
#!/usr/bin/python

c = '66 114 101 97 107 65 76 76 67 84 70 123 65 109 118 48 117 68 121 101 114 118 80 116 109 86 114 57 83 83 83 75 125'

flag = ""

for x in c.split(' '):
  flag += chr(int(x))

print(flag)
```

# [2]使用Python標準函式庫進行BASE64的編碼與解碼

###  Python 標準函式庫 (Standard Library)
```
C++ 有強大的標準模板庫(Standard Template Library，STL）

Python也有強大的標準函式庫 (Standard Library)

本課程示範幾個範例,帶你認識Python 標準函式庫

推薦書籍與資源

[1]官方說明
英文 https://docs.python.org/3/library/
中文 https://docs.python.org/zh-tw/3/library/index.html

[2]厚達上千頁的範例示範說明書

The Python 3 Standard Library By Example
Doug Hellmann

https://pymotw.com/3/
https://bitbucket.org/dhellmann/pymotw-3/src/master/
```

## Python 標準函式庫的Base 64 模組
```
What does the 'b' character do in front of a string literal?

https://stackoverflow.com/questions/6269765/what-does-the-b-character-do-in-front-of-a-string-literal
```
### 使用Base 64 模組編碼:b64encode()
```
import base64

data =b'BreakAllCTF{HappyPythonDay}'
encoded_data = base64.b64encode(data)
print('Original Data :', data)
print('Encoded :', encoded_data)
```
### 使用Base 64 模組解碼:b64decode()
```
import base64

encoded_data = b'QnJlYWtBTExDVEZ7NTN1c1pRM2hXVzI1ZGNoWjdkWGV9'
decoded_data = base64.b64decode(encoded_data)
print('Encoded :', encoded_data)
print('Decoded :', decoded_data)
```
### 使用Base 64 模組也可以進行Base 32編碼與解碼
```
import base64

original_data = b'BreakAllCTF{HappyPythonDay}'
print('Original data:', original_data)

encoded_data = base64.b32encode(original_data)
print('Encoded :', encoded_data)

decoded_data = base64.b32decode(encoded_data)
print('Decoded :', decoded_data)
```
### [作業] 使用Pyhon程式解Crytpo 101: Base64及Base32哪兩題

# 編碼與解碼102

# [3]angstromCTF 2016 : what-the-hex 20
```
Decode using hex and see what you get...
6236343a20615735305a584a755a58526659323975646d567963326c76626c3930623239736331397962324e72
```

#### [可以不必教]先試看看[底下程式要在Python 2 才會正常執行]

步驟一:先將十六進位的數字轉成文字
```
'6236343a20615735305a584a755a58526659323975646d567963326c76626c3930623239736331397962324e72'.decode("hex")
```
```
'b64: aW50ZXJuZXRfY29udmVyc2lvbl90b29sc19yb2Nr'
```
步驟二:將獲得的文字再使用base 64解碼
```
import base64
base64.b64decode('aW50ZXJuZXRfY29udmVyc2lvbl90b29sc19yb2Nr')
```
### Python 3的寫法[使用 標準函式庫的binascii模組]
```
binascii模組包含很多用來方法來轉換二進位制和各種ASCII編碼的二進位制表示法
更多說明請參閱:
https://docs.python.org/zh-cn/3/library/binascii.html
```

步驟一:先將十六進位的數字轉成文字
```
import binascii
binascii.unhexlify('6236343a20615735305a584a755a58526659323975646d567963326c76626c3930623239736331397962324e72')
```
步驟二:將獲得的文字再使用base 64解碼
```
import base64
base64.b64decode('aW50ZXJuZXRfY29udmVyc2lvbl90b29sc19yb2Nr')
```

# [3]補充教材:數字系統( number system)的轉換
```
10進位(Decimal)  二進位(binary)  八進位(Octal)  十六進位(Hexadecimal)

1011(二進位) = 13(八進位) = B(十六進位) = 11(十進位)

Python表示法:
0b1011(二進位:0b開頭)  0o13(八進位:0o開頭)   0xb(十六進位:0x開頭)
```

###  使用Python 內建函數(Built in Functions)解決  數字系統的轉換問題

```
https://www.w3schools.com/python/python_ref_functions.asp

bin()
oct()
int()
hex()
```
```
# 數字系統( number system)的轉換
# 使用Python程式將10進位(Decimal)數字轉換成二進位(binary), 八進位(Octal) 及 十六進位(Hexadecimal) 
# https://www.programiz.com/python-programming/examples/conversion-binary-octal-hexadecimal

bin_n = 0b1011

print("上述二進位數字可被轉換成:")
print(bin_n,"十進位(Decimal).")
print(oct(bin_n),"八進位(octal).")
print(hex(bin_n),"十六進位(hexadecimal).")
```
### 給你十進位的 344, 二進位(binary)|八進位(octal)|十六進位(hexadecimal)是多少?
```
dec = 344

print("10進位數字",dec,"可被轉換成:")
print(bin(dec),"二進位(binary).")
print(oct(dec),"八進位(octal).")
print(hex(dec),"十六進位(hexadecimal).")
```
## int[]內建函數
```
功能:將一個字串或數位轉換為整數型。

語法: int(x, base=10)

參數說明:
x -- 字串或數位。
base -- 進制數，預設是十進位。

返回值:會返回一個整數型資料。

```
### 底下程式執行結果為何?
```
# [參考資料]From https://www.programiz.com/python-programming/methods/built-in/int

# binary 0b or 0B
print("For 1010, int is:", int('1010', 2))
print("For 0b1010, int is:", int('0b1010', 2))

# octal 0o or 0O
print("For 12, int is:", int('12', 8))
print("For 0o12, int is:", int('0o12', 8))

# hexadecimal
print("For A, int is:", int('A', 16))
print("For 0xA, int is:", int('0xA', 16))
```

# [4]Internetwache CTF 2016 : The hidden message
```
My friend really can’t remember passwords. So he uses some kind of obfuscation. Can you restore the plaintext?

Attachment: misc50.zip
```
把misc50.zip解壓縮後可以得到:
```
0000000 126 062 126 163 142 103 102 153 142 062 065 154 111 121 157 113
0000020 122 155 170 150 132 172 157 147 123 126 144 067 124 152 102 146
0000040 115 107 065 154 130 062 116 150 142 154 071 172 144 104 102 167
0000060 130 063 153 167 144 130 060 113 012
0000071
```

```
參考解答(writeups)
https://0x90r00t.com/2016/02/22/internetwache-ctf-2016-misc-50-the-hidden-message-write-up/
```
## 第一種解法:使用線上工具解

步驟一：線上工具
```
http://www.unit-conversion.info/texttools/octal/

V2VsbCBkb25lIQoKRmxhZzogSVd7TjBfMG5lX2Nhbl9zdDBwX3kwdX0K
```
步驟二：
```
import base64

encoded_data = b'V2VsbCBkb25lIQoKRmxhZzogSVd7TjBfMG5lX2Nhbl9zdDBwX3kwdX0K'
decoded_data = base64.b64decode(encoded_data)
print('Encoded :', encoded_data)
print('Decoded :', decoded_data)
```
## 第二種解法

### 先試看看是不是如你所想的一般.......
```
#!/usr/bin/python

c = '126 062 126 163 142 103 102 153 142 062 065 154 111 121 157 113 122 155 170 150 132 172 157 147 123 126 144 067 124 152 102 146 115 107 065 154 130 062 116 150 142 154 071 172 144 104 102 167 130 063 153 167 144 130 060 113 012'

flag = ""

for _ in c.split(' '):
  flag += chr(int(_,8))

print(flag)
```
## 接著就可以完成大業......
```
#!/usr/bin/python
import base64

c = '126 062 126 163 142 103 102 153 142 062 065 154 111 121 157 113 122 155 170 150 132 172 157 147 123 126 144 067 124 152 102 146 115 107 065 154 130 062 116 150 142 154 071 172 144 104 102 167 130 063 153 167 144 130 060 113 012'

flag = ""

for _ in c.split(' '):
  flag += chr(int(_,8))
  

solution = base64.b64decode(flag)
print(solution)
```
# [5]SECCON CTF 2014: Easy Cipher 
```
87 101 108 1100011 0157 6d 0145 040 116 0157 100000 0164 104 1100101 32 0123 69 67 0103 1001111 1001110 040 062 060 49 064 100000 0157 110 6c 0151 1101110 101 040 0103 1010100 70 101110 0124 1101000 101 100000 1010011 1000101 67 0103 4f 4e 100000 105 1110011 040 116 1101000 0145 040 1100010 0151 103 103 0145 1110011 0164 100000 1101000 0141 99 6b 1100101 0162 32 0143 111 1101110 1110100 101 0163 0164 040 0151 0156 040 74 0141 1110000 1100001 0156 056 4f 0157 0160 115 44 040 0171 1101111 117 100000 1110111 0141 0156 1110100 32 0164 6f 32 6b 1101110 1101111 1110111 100000 0164 1101000 0145 040 0146 6c 97 1100111 2c 100000 0144 111 110 100111 116 100000 1111001 6f 117 63 0110 1100101 0162 0145 100000 1111001 111 117 100000 97 114 0145 46 1010011 0105 0103 67 79 1001110 123 87 110011 110001 67 110000 1001101 32 55 060 100000 110111 0110 110011 32 53 51 0103 0103 060 0116 040 5a 0117 73 0101 7d 1001000 0141 1110110 1100101 100000 102 0165 0156 33
```
```
[參考解答]
http://4ngelboy.blogspot.com/2014/12/span-display-block-overflow-hidden.html


很明顯的這段文字是由四種不同進位的數字所組成，必須判斷出他是屬於哪個進位在轉換成 ASCII code 印出，
不過起初在解的時候沒有發現有特別的規則，導致剛開始一直判別不出來，仔細觀察過後可發現每個進位的數字有不同的特徵：
2 進位：字串長度 >= 6
8 進位：開頭一定是 0
16 進位：必有英文字 6d
10 進位：上述之外的

https://github.com/S42X/CTF/blob/master/SECCON/EasyCipher.md
```
```

ord('a')

oct(10)

hex(10)

```
```
https://www.quora.com/How-do-I-convert-hex-into-a-string-using-Python

https://stackoverflow.com/questions/18806772/most-pythonic-way-to-convert-a-string-to-a-octal-number

http://mini-stable.blogspot.com/2015/03/python-int-hex-char-string.html
```
### Python - int, hex, char, string的轉換
```
Int to Hex:   hex(97)  # '0x61'
Int to Char:   chr(97)  # 'a'
Int to String:  str(97)  # '97'
Hex to int:  int('0x61', 16)  # 97
Hex to Char:   chr(int('0x61', 16)) # 'a'
Hex to String:
string = '61626364'
''.join(chr(int(string[i:i+2], 16)) for i in range(0, len(string), 2))  # 'abcd'
```
```
Char to Int: ord('a')  # 97
Char to Hex: hex(ord('a'))  # '0x61'
String to Int: int('97')  # 97
String to Hex:

string = 'abcd'
''.join([hex(ord(x))[2:] for x in string])  # '61626364'
```

```
string = '61626364'
''.join(chr(int(string[i:i+2], 16)) for i in range(0, len(string), 2))  # 'abcd'
```
### 使用python內建模組 binascii
```
https://docs.python.org/2/library/binascii.html
```
```
#coding:utf-8
import binascii
a = 'HappyCTF{Useful tools binascii}'
b = binascii.b2a_hex(a)
print b
print binascii.a2b_hex(b)
```
```
Python2 Online
https://paiza.io/en/languages/python
```

```
https://nandynarwhals.org/seccon2014-easycipher/
```
```
#!/usr/bin/python

vals = "87 101 108 1100011 0157 6d 0145 040 116 0157 100000 0164 104 1100101 32 0123 69 67 0103 1001111 1001110 040 062 060 49 064 100000 0157 110 6c 0151 1101110 101 040 0103 1010100 70 101110 0124 1101000 101 100000 1010011 1000101 67 0103 4f 4e 100000 105 1110011 040 116 1101000 0145 040 1100010 0151 103 103 0145 1110011 0164 100000 1101000 0141 99 6b 1100101 0162 32 0143 111 1101110 1110100 101 0163 0164 040 0151 0156 040 74 0141 1110000 1100001 0156 056 4f 0157 0160 115 44 040 0171 1101111 117 100000 1110111 0141 0156 1110100 32 0164 6f 32 6b 1101110 1101111 1110111 100000 0164 1101000 0145 040 0146 6c 97 1100111 2c 100000 0144 111 110 100111 116 100000 1111001 6f 117 63 0110 1100101 0162 0145 100000 1111001 111 117 100000 97 114 0145 46 1010011 0105 0103 67 79 1001110 123 87 110011 110001 67 110000 1001101 32 55 060 100000 110111 0110 110011 32 53 51 0103 0103 060 0116 040 5a 0117 73 0101 7d 1001000 0141 1110110 1100101 100000 102 0165 0156 33"
vals = vals.split()

def contains_hex(val):
    h = "abcdef"
    for i in val:
        if i in h:
            return True
    return False

def main():
    flag = []
    for i in vals:
        if len(i) > 5:
            flag.append(int(i, 2))
        elif i[0] == "0":
            flag.append(int(i, 8))
        elif contains_hex(i):
            flag.append(int(i, 16))
        else:
            flag.append(int(i))
    print("%s" % "".join(map(chr, flag)))

if __name__ == "__main__":
    main()
```
### 使用正規表達法regression expression
```
https://wiremask.eu/writeups/seccon-ctf-2014-crypto-100-easy-cipher/
```
```
#!/usr/bin/env python
import re
import sys

message = "87 101 108 1100011 0157 6d 0145 040 116 0157 100000 0164 104 1100101 32 0123 69 67 0103 1001111 1001110 040 062 060 49 064 100000 0157 110 6c 0151 1101110 101 040 0103 1010100 70 101110 0124 1101000 101 100000 1010011 1000101 67 0103 4f 4e 100000 105 1110011 040 116 1101000 0145 040 1100010 0151 103 103 0145 1110011 0164 100000 1101000 0141 99 6b 1100101 0162 32 0143 111 1101110 1110100 101 0163 0164 040 0151 0156 040 74 0141 1110000 1100001 0156 056 4f 0157 0160 115 44 040 0171 1101111 117 100000 1110111 0141 0156 1110100 32 0164 6f 32 6b 1101110 1101111 1110111 100000 0164 1101000 0145 040 0146 6c 97 1100111 2c 100000 0144 111 110 100111 116 100000 1111001 6f 117 63 0110 1100101 0162 0145 100000 1111001 111 117 100000 97 114 0145 46 1010011 0105 0103 67 79 1001110 123 87 110011 110001 67 110000 1001101 32 55 060 100000 110111 0110 110011 32 53 51 0103 0103 060 0116 040 5a 0117 73 0101 7d 1001000 0141 1110110 1100101 100000 102 0165 0156 33"
codes = message.split()

for i, code in enumerate(codes):
    if re.match("^[01]+$", code)  and code[0] == "1" and len(code) > 3:
        sys.stdout.write(chr(int(code, 2)))
    elif re.match("^[0-9]+$", code) and code[0] == "0":
        sys.stdout.write(chr(int(code, 8)))
    elif re.match("^[0-9]+$", code):
        sys.stdout.write(chr(int(code, 10)))
    else:
        sys.stdout.write(chr(int(code, 16)))

```


