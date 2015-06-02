---
layout: post
title: C++筆記--指針
---

資料來源: [Cplusplus.com](http://www.cplusplus.com/doc/tutorial/pointers/)

### 定义
存儲另一個變量地址的變量被稱為指針。

### 運算符

 + &: address-of operator, 中文譯為“取地址運算符”.
  
  `foo = &myvar;` myvar的地址就通過'&'+'myvar'，被指針foo得到。存儲器中變量的實際地址在執行之前是未知的。但是，我們假設，myvar的地址在程序執行期間為1776.
  
 ```
 myvar = 25;
 foo = &myvar;
 bar = myvar;
 ```
 在這一段程序執行之後，變量的存儲位置如圖所示：
 ![](http://www.cplusplus.com/doc/tutorial/pointers/reference_operator.png)
 
1. 首先，將數值25賦給myvar。
2. 第二个聲明將myvar的地址賦給foo，即為1776（是我們假設的地址）
3. 最後一個聲明，將myvar中的值發給了bar。
 

+ \*: Dereference operator, 中文譯為“間接運算符”.
  指針一個很有意思的性質就是可以被用來直接讀取它們所指的變量。這樣就是通過採用'pointer name'+'*'的形式。

 `baz = *foo;`,可以被解讀為“baz同foo所指向的值相等”
 
 ![](http://www.cplusplus.com/doc/tutorial/pointers/dereference_operator.png)
 
 
###指針聲明
 
 指針在指向不同類型數值的時候擁有不同的屬性。在指針聲明的時候，類型一定要聲明:`type * name`。同時要注意：聲明中的'*'只是表明是指針，和我們前面所提到的簡潔運算符'\*'是不同的。它們雖然標誌是一樣的，但是所代表的意義卻是不同的。
 
```C++
int firstvalue, secondvalue;
  int * mypointer;

  mypointer = &firstvalue;
  *mypointer = 10;
  mypointer = &secondvalue;
  *mypointer = 20;
  cout << "firstvalue is " << firstvalue << '\n';
  cout << "secondvalue is " << secondvalue << '\n';
  return 0;
```

這裡面的 ' int * mypointer' 和' *mypointer = 10'中的'\*'所代表的意義是不同的。第一個'\*'只是表明聲明的為pointer，而第二個則代表mypointer所指向的地址。

下面是一個更加詳細的說明：

```C++
#include <iostream>
using namespace std;

int main ()
{
  int firstvalue = 5, secondvalue = 15;
  int * p1, * p2;

  p1 = &firstvalue;  // p1 = address of firstvalue
  p2 = &secondvalue; // p2 = address of secondvalue
  *p1 = 10;          // value pointed to by p1 = 10
  *p2 = *p1;         // value pointed to by p2 = value pointed by p1
  p1 = p2;           // p1 = p2 (value of pointer is copied)
  *p1 = 20;          // value pointed by p1 = 20
  
  cout << "firstvalue is " << firstvalue << '\n';
  cout << "secondvalue is " << secondvalue << '\n';
  return 0;
}
```

以上兩個程序的輸出均為：

```
firstvalue is 10
secondvalue is 20
```

還有一點要注意，同時聲明兩個指針的時候，需要`int *p1, *p2`而不能`int *p1, p2`。後面的聲明是將p2聲明成了整形變量，而非指針。

###指針與數組
數據的概念與指針相關。實際上，數據的功能與指向數組的第一個變量的指針非常相像。

```C++
int myarray [20] ;
int * mypointer ;
mypointer = myarray;
```
執行之後，mypointer和array都會相同，並且有相似的性質。主要的區別是，mypointer可以被賦給其它的地址，但是myarray不能被賦給任何東西，並且總是代表著擁有20個int型的塊（block)。因此`myarray = mypointer; `是不允許的。

指針與數組的例子如下：

```C++
#include <iostream>
using namespace std;

int main ()
{
  int numbers[5];
  int * p;
  p = numbers;  *p = 10;
  p++;  *p = 20;
  p = &numbers[2];  *p = 30;
  p = numbers + 3;  *p = 40;
  p = numbers;  *(p+4) = 50;
  for (int n=0; n<5; n++)
    cout << numbers[n] << ", ";
  return 0;
}
```
輸出為：`10, 20, 30, 40, 50,`.

並且數據的名字可以用當成指向其第一個數據的指針來使用。

```
a[5] = 0;       // a [offset of 5] = 0
*(a+5) = 0;     // pointed by (a+5) = 0  
```
###指針的初始化
指針可以初始化為指向一個特定地址的指針。

```C++
int myvar;
int * myptr = &myvar;
```
```C++
int myvar;
int * myptr;
myptr = &myvar;
```
指針初始化的時候，初始化的是指針所指向的地址(myptr)，而不是所指向的值(\*myptr），因此下面的聲明是**錯誤**的：

```C++
int myvar;
int * myptr;
*myptr = &myvar;
```
同時，指針可以被初始化為變量地址，也可以初始化為另一個指針或是數組的值。

```C++
int myvar;
int *foo = &myvar;
int *bar = foo;
```
###指針計算
指針計算同數值計算有些不同。只有加法和減法的運算被允許。而其他運算在指針的世界并沒有多大的意義。但是由於指針所屬的類型不同，指針的加減法也有一些差異。

首先定義三個指針：

```C++
char *mychar;
short *myshort;
long *mylong;
```
並且（假設）我們知道它們所指的存儲器的位置分別為1000，2000，3000.因此，當我們執行：

```
++mychar;
++myshort;
++mylong;
```

之後，指針所知的位置的實際增加卻是不一樣的。因此sizeof(char)為1，sizeof(short)為2，sizeof(long)為8，因此結果如圖所示：

![](http://www.cplusplus.com/doc/tutorial/pointers/pointer_arithmetics.png)

**注意: mylong應該指向3008**

指針運算一些例子如下：

```C++
*p++   // same as *(p++): increment pointer, and dereference unincremented address
*++p   // same as *(++p): increment pointer, and dereference incremented address
++*p   // same as ++(*p): dereference pointer, and increment the value it points to
(*p)++ // dereference pointer, and post-increment the value it points to 
```
注意'++'比'*'擁有更高的運算等級。

###指針與常量(const)
指針可以通過地址來讀取一個變量，並且讀取可以包括改變所指的數值。同時，我們也可以聲明只可以讀取所指數值但不能改變數值的指針。這裡，就需要是使用const來幫助：

```C++
int x;
int y = 10;
const int * p = &y;
x = *p;          // ok: reading p
*p = x;          // error: modifying p, which is const-qualified 
```
可以通過指針p來讀取y的值，但是卻不能改變所指向的值。

```C++
// pointers as arguments:
#include <iostream>
using namespace std;

void increment_all (int* start, int* stop)
{
  int * current = start;
  while (current != stop) {
    ++(*current);  // increment value pointed
    ++current;     // increment pointer
  }
}

void print_all (const int* start, const int* stop)
{
  const int * current = start;
  while (current != stop) {
    cout << *current << '\n';
    ++current;     // increment pointer
  }
}

int main ()
{
  int numbers[] = {10,20,30};
  increment_all (numbers,numbers+3);
  print_all (numbers,numbers+3);
  return 0;
}
```
輸出為：

```
11
21
31
```
注意到，指針指向的內容不能被改變，但是指針本身確實可以改變的，`++ current`。

還有其他關於常量指針的應用：

```C++
int x;
      int *       p1 = &x;  // non-const pointer to non-const int
const int *       p2 = &x;  // non-const pointer to const int
      int * const p3 = &x;  // const pointer to non-const int
const int * const p4 = &x;  // const pointer to const int 
const int * p2a = &x;  //      non-const pointer to const int
int const * p2b = &x;  // also non-const pointer to const int 
```
在具體使用中，可以更加深入體會常量指針的用處。

###指針和字符串
不含空格終止的字符序列所組成的數組成為字符串。字符串也可以直接被指針所讀取。字符串的聲明為: `const char * foo = "hello"; `（作為字符串，他們不能被修改）
![](http://www.cplusplus.com/doc/tutorial/pointers/pointer_assignment.png)

要注意到: foo 是一個指針，並且包含地址值 1702，不是'h'，也不是'hello'，儘管1702實際是'h'和'hello'的地址。

```
*(foo+4)
foo[4]
```
這兩個聲明均為 'o'。

###指向指針的指針
C++允許指向指針的指針的使用。

```C++
char a;
char * b;
char ** c;
a = 'z';
b = &a;
c = &b;
```
變量的地址值均為假設值：

![](http://www.cplusplus.com/doc/tutorial/pointers/pointer_to_pointer.png)

+ c is of type char** and a value of 8092
+ \*c is of type char* and a value of 7230
+ **c is of type char and a value of 'z'

###空指針(void pointer)
void型是指針的一個特殊類型。在C++中，代表此指針類型的空缺。因此，void型指針是指向沒有類型的數值的指針。

雖然void指針有很大的彈性，可以指向任何數據類型。但同時，也有一些限制，它們所指向的數據不能被直接dereference。因此，void指針的地址需要被轉換到它所指向數據的類型，才能夠被dereference.

```C++
#include <iostream>
using namespace std;

void increase (void* data, int psize)
{
  if ( psize == sizeof(char) )
  { char* pchar; pchar=(char*)data; ++(*pchar); }
  else if (psize == sizeof(int) )
  { int* pint; pint=(int*)data; ++(*pint); }
}

int main ()
{
  char a = 'x';
  int b = 1602;
  increase (&a,sizeof(a));
  increase (&b,sizeof(b));
  cout << a << ", " << b << '\n';
  return 0;
}
```
輸出為`y, 1063`。

###無效指針與空指針（Invalid pointers and null pointers）
指針可以指向任何地址，包括沒有任何有效元素的地址。

```C++
int * p;               // uninitialized pointer (local variable)

int myarray[10];
int * q = myarray+20;  // element out of bounds 
```
指針P為指向任何地址，指針q指向不存在元素的地址。但是上面的聲明均沒有出現錯誤。在C++中，指針可被存在賦予任何地址值，不論這個地址上是否存在元素。但是，讓dereference這樣一個指針（實際上市他們所指的值），程序就會出現錯誤。讀取這樣一個指針會引用異常行為。

同時，有些時候，指針需要表明不指向任何地址，或一個無效的地址。

```C++
int * p = 0;
int * q = nullptr;
```
p和q均為空指針，實際上，它們是相等的。早期的代碼也這樣定義`int * r = NULL;`。

不要將**null pointer**與**void pointer**搞混。

+ A null pointer is a value that any pointer can take to represent that it is pointing to "nowhere". It refers to the value stored in the pointer.
+ A void pointer is a type of pointer that can point to somewhere without a specific type. It refers to the type of data it points to.
 
###指向函數的指針

```C++
#include <iostream>
using namespace std;

int addition (int a, int b)
{ return (a+b); }

int subtraction (int a, int b)
{ return (a-b); }

int operation (int x, int y, int (*functocall)(int,int))
{
  int g;
  g = (*functocall)(x,y);
  return (g);
}

int main ()
{
  int m,n;
  int (*minus)(int,int) = subtraction;

  m = operation (7, 5, addition);
  n = operation (20, m, minus);
  cout <<n;
  return 0;
}
```
輸出為: `8`.

其中，minus是指向函數的指針，這個函數擁有兩個int型的變量。

初始化：`int (* minus)(int,int) = subtraction;`








