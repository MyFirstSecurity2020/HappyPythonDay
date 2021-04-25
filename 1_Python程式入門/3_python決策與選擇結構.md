# python決策與選擇結構(SELECTION /DECISION)
```
程式語言都會有的決策與選擇結構(SELECTION /DECISION)
C C++ C# 
```
```
[1]if
[2]if ...elsif
[3]if ...else
[4]if ...elsif ...else
[5]各種判斷條件==> 善用 AND   OR
[6]範例
```
```
單向判斷式（if⋯）: 是非題｜對的才要做
雙向判斷式（if⋯else）: 二選一｜一定要選的
多向判斷式（if⋯elif⋯else）: 多選一｜一定要選的
```
```
PS: Python沒有switch
https://www.w3schools.com/python/python_conditions.asp
```
```
教學重點:教學要教最簡單的範例,要快速教完
作業要出很難的 磨出學生的程式力
```
# [1]if ==>底下程式輸出為何?
```
a = 33
b = 200

if b > a:
  print("b is greater than a")
```
```
a = 33
b = 20

if b > a:
  print("b is greater than a")
```
# [2]if ...elsif ==>底下程式輸出為何?

## 範例1
```
a = 32
b = 33

if b > a:
  print("b is greater than a")
elif a == b:
  print("a and b are equal")
```
## 範例2
```
a = 33
b = 33
if b > a:
  print("b is greater than a")
elif a == b:
  print("a and b are equal")
```
## 範例3
```
a = 35
b = 33

if b > a:
  print("b is greater than a")
elif a == b:
  print("a and b are equal")
```

# 雙向判斷式（if⋯else）: 二選一｜一定要選的

```
a = 200
b = 33

if b > a:
  print("b is greater than a")
else:
  print("b is not greater than a")
```
# [4]if ...elsif ...else  多選一｜一定要選的
```
a = 200
b = 33

if b > a:
  print("b is greater than a")
elif a == b:
  print("a and b are equal")
else:
  print("a is greater than b")
```
# [5]各種判斷條件 ==> 善用  AND  OR
```
判斷是否為閏年
```
```
year= eval(input("請輸入年"))

if ((year%400==0) or (year%4==0 and year%100!=0)):
  print("{0} 是閏年".format(year))
else:
  print("{0} 不是閏年".format(year))
```
### 作業
```
記得完成 PPC_Ez 3.calendar
```
# [6]歲月匆匆程式開發 ==>先講  運算思維  再講程式語法
```
問題簡述:
輸入:年月日
輸出:今年已經過了多少日
```
### 先測試看看輸入部分
```
year, month, day = eval(input("請輸入年月日"))
year, month, day
```
```
請輸入年月日2019,5,22
(2019, 5, 22)
```
### 再測試 閏年2月的時間
```
day_month = [31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31]

if year%400==0 or (year%4==0 and year%100!=0):	
  day_month[1] = 29
```
### 計算所有已經過去的日子
```
if month==1:
    print(day)
else:
    print(sum(day_month[:month-1])+day)
```

### 完整程式
```
year, month, day = eval(input("請輸入年月日"))
year, month, day

day_month = [31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31]

if year%400==0 or (year%4==0 and year%100!=0):	
  day_month[1] = 29

if month==1:
    print(day)
else:
    print(sum(day_month[:month-1])+day)
```
### 多向判斷式（if⋯elif⋯else）:收銀台程式開發[學生自己學習]
```
百貨公司週年慶活動血拼大打折，
吸引很多顧客上門，
公司決定再加碼回饋客戶，

只要客戶消費
金額在 100000 元以上就打八折，
金額在 50000 元以上就打八五折，
金額在 30000 元以上就打九折，
金額在 10000 元以上就打九五折，
金額在 10000 元以下就不打折

請幫該公司設計這個收銀台的程式，
輸入顧客購買金額後，計算顧客應付的金錢。

若輸入113234元顧客應付多少錢?
```
```
money = int(input("請輸入購物金額："))

if(money >= 10000):
    if(money >= 100000):
        print(money * 0.8, end=" 元\n")  #八折
    elif(money >= 50000):
        print(money * 0.85, end=" 元\n")  #八五折
    elif(money >= 30000):
        print(money * 0.9, end=" 元\n")  #九折
    else:
        print(money * 0.95, end=" 元\n")  #九五折
else:
    print(money, end=" 元\n")  #未打折
```
