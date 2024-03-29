# C与C++的区别

+ C++保留了C的内容，还包括容器、模板、智能指针、auto变量等一系列新特点。
+ C++时面向对象的编程语言，引入了新的数据类型——**类**，因此引申出了三大特性
  + 封装
  + 继承
  + 多态
+ C语言有一些不安全的语言特性，而C++对此增加了不少新特性来改善安全性。

> C语言：指针的潜在危险、强制转换的不确定性和内存泄漏等
>
> C++：const常量、引用、智能指针。。

+ C++可复用性高，引入了**模板**的概念，并且在此基础上，实现了方便开发的标准模板库STL(Standard Template Library)

# C++程序的执行过程

## 1.预编译

(1)将所以#define删除，并且展开所以宏定义

(2)处理所有的条件预编译指令，如#if,#ifdef

(3)处理所有#include预编译指令，将被包含的文件直接覆盖到指令位置

(4)过滤所以的注释

(5)添加行号与文件名表示

---

**头文件<>与""有什么区别？**

+ <>是系统文件，“”是自定义文件
+ 查找路径不同。
  + <>：编译器设置的头文件路径->头文件
  + “ ”：当前头文件目录->编译器设置的头文件路径->头文件

## 2.编译

(1) 词法分析：将源代码的字符序列分割成一系列的记号。

(2)语法分析：对记号进行语法分析，产生语法树。

(3)语义分析：判断表达式是否有意义。

(4)代码优化

(5)目标代码生成：**生成汇编代码**。

(6)目标代码优化：

## 3.汇编

将汇编代码转化成机器可以执行的指令

## 4.链接

将**不同的源文件**产生的目标文件进行链接，从而形成一个可以执行的程序。

链接又分**动态链接**和**静态链接**

+ 静态链接：在链接时把已经要调用的函数或者过程链接到了生成的可执行文件中。就算删除掉静态库也不会影响可执行程序的执行。
  + 静态库windows下以.lib为后缀，Linux下以.a为后缀
+ 动态链接：在链接时没有把调用的函数代码链接进去，而是在执行过程中，再去找要链接的函数，生成的可执行文件中没有函数代码，只包括函数的重定位信息，所以当你删除动态库时，可执行程序就不能运行
  + 动态库windows下以.dll为后缀，linux下以.so为后缀

![img](https://uploadfiles.nowcoder.com/images/20210329/675098158_1617014922398/602E8F042F463DC47EBFDF6A94ED5A6D)

# extern

+ extern用于跨文件引用全局变量，但**引用时不可初始化。**

+ extern C：为能够正确实现C++代码调用其他C语言。加上后会指示编译器按**C语言**进行编译。
+ extern在链接阶段作用。

## C和C++编译有什么区别

由于C++支持函数重载，因为编译器编译函数的过程中会将函数的**参数类型**也加到编译后的代码中，而**不仅是函数名**。

# 宏到底是是什么

宏是在预编译时进行文本替换的工具，**以#开头来区分，**有两种格式。

+ 不带参数的宏定义

  ```c
  #define PI 3.414
  ```

+ 带参数的宏定义

  ```c
  #define MAX(x,y) ((x)>(y)?(x):(y))
  ```

## 宏的安全性

由于在预编译阶段不会进行语法检查和语义分析，所以宏的使用很容易出问题，所以在宏定义时最好加上`()`

```c
#define N 9+3

int main()
{
	int result = N*N;
	cout<<result<<endl;
}
```

在宏进行暴力替换后，改成了

```c
int result = 9+3*9+3;
```

结果并不是我们想要的逻辑,如果加上`()`,就正确了

```
#define N (9+3)

int result = (9+3)*(9+3);
```

## 宏的安全性解决办法

+ 不带参数的宏命令常用`const`来替代

```
const int PI = 3.1415;
```

​	因为`const`语句会在编译阶段进行语法检查，会比宏更安全。

+ 带参数的宏命令有点类似函数的功能，在C++可以使用**内联函数或者模板**,用`inline`关键字定义函数，即声明该函数为内联函数。
+ 内联函数会与宏定义一样在原地展开，省去了函数调用开销，且能做类型检查。

# 内联函数

+ 通常是与类一起使用的。

+ 如果一个函数是内联的，那么编译器会把该函数的代码副本放在每个调用该函数的地方

## 为什么使用内联函数

+ 函数的调用是有开销的，包括：先保存寄存器，返回时再恢复，复制实参等。

+ 如果一个准备被调用的函数很简单，那么函数调用的开销就远大于函数体执行的开销。为了减少这种开销，就要用到内联函数

## 内联函数使用的条件

以下不能使用内联：

+ 函数本身代码比较长。
+ 函数体内出现循环

# 条件编译

+ 在允许的情况下才能被编译

```c
#ifdef 标识符
	程序段1
#else  标识符
	程序段2
#endif
```

+ 标识符一般是用#define定义

  ```c++
  #define Open_watchdog 1
  ```

# 字节对齐

## 什么是内存对齐

在C语言中，结构体是一种复合数据类型，其构成元素既可以是基本数据类型(int long float)等变量，也可以是一些复合数据类型(数组、结构体、联合等)等变量。

在结构体中，**编译器为结构体的每个成员按其自然边界(alignment)分配空间，第一个成员的地址为整个结构体的地址**

+ 各数据类型占用字节在win32还是win64都是一样
+ 指针类型占用字节大小在win32还是win64是**不一样**的

+ 单独说下，long在linux64位下是8字节

为了能够使CPU对变量进行快速的访问，变量的起始地址必须具有**对齐**的特性。

+ **自然对齐**：如果一个变量的内存地址正好位于它长度的整数倍。

## 为什么要字节对齐

+ 提高CPU访问数据的效率。

  > 如果整型(int)变量的地址不是自然对齐，问0x0000 0002,则CPU取它的值需要访问两次内存。
  >
  > + 第一次取从0x0000,0002-0x0000,0003的一个short
  > + 第二次取从0x0000,0002-0x0000,0003的一个short
  > + 组合起来就是需要的数据

## 计算字节大小

1. **题1：**

```C+
union example {  
    int a[5];  
    char b;  
    double c;  
};  
int result = sizeof(example);  
```

答案=28；8+4*4 = 24.

**占用的内存空间大小需要是结构体/联合体中占用最大内存空间的类型的整数倍。**

如果答案为20的话，内部double占8字节，这段内存的地址0x0000,0020不是double的整数倍，只有当最小为0x0000,0024时

+ 可以满足整除double(8byte)
+ 同时又可以满足容纳`a[5](20byte)`的大小

2. **题2：**

```
struct example {  
    int a[5];  
    char b;  
    double c;  
}test_struct;
int result = sizeof(test_struct);  
```

20+1=21，因为21不是8的整数倍，所以扩充到24，24+8=32.

3. 提3：

   ```c
   struct example {  
       char b;  
       double c;  
       int a;  
   }test_struct;  
   int result = sizeof(test_struct);  
   ```

   1；再扩充到8；8+8=16；16+4=20

又因为20不是8的整数倍，所以要扩到24，

**占用的内存空间大小需要是结构体/联合体中占用最大内存空间的类型的整数倍。**

## 强制转换

让结构体按固定的模式对齐

```c++
#include <stdio.h>  
#include <stdlib.h>  
#include <iostream>  
#pragma pack(4)  
using namespace std;  
struct example{  
    char a;  
    double b;  
    int c;  
}test_struct;  
int main() {  
    int result = sizeof(test_struct);  
    cout << result << endl;  
    system("Pause");  
    return 0;  
}  
```

`#pragma pack(n) `表示结构体成员所占用内存的起始地址需要是`n`的整数倍。

**对应字节数=min(成员起始地址应是n的倍数时填充的字节数，自然对齐时填充的字节数)**

![img](https://uploadfiles.nowcoder.com/images/20210329/675098158_1617015220525/9EB9CD58B9EA5E04C890326B5C1F471F)

结合上图分析。

+ 首先为`char a`分配空间，起始地址为`0x0000`,0可以整除4。`char a`大小为1个字节，分布至`0x0001`
+ 接着为`double b`分配空间，由于`0x0001`不能整除8，所以需要补字节
  + 若满足自然对齐，则需补`7`个字节
  + 若满足强制对齐，则最多可补`3`个字节
+ 按照上述规则，我们取最小值，即补3个字节。

+ 再加上`double b`占用8个字节，此时`int c`起始地址为`0x0012`
+ 接着为`int c`分配空间，起始地址满足4的倍数，`int c`占用4个字节，总共结构体满足`16`个字节。
+ 同时满足占用的内存空间大小需要是结构体中占用`最大内存空间的类型`的整数倍。

**题1：**

```c++
#pragma pack(4)  
using namespace std;  
struct example{  
    char a;  
    double b;  
    char c;  
}test_struct;  
```

0+1=1；

1+3=4；

4+8=12；

12+1=13；因为13不是8的整数倍，所以扩充到13+3=16；

题2：

```
#pragma pack(4)  
using namespace std;  
struct example{  
    int a;  
    char b;  
    short int c;  
    char d;  
}test_struct;  
```

0+4=4；

4+1=5；

5+1=6；补字节至2的整数倍

6+2=8；`short int c`占两个字节

8+1=9；

9+3=12；

题3：

```
#pragma pack(8)  
using namespace std;  
struct example{  
    int a;  
    char b;  
    short int c;  
    int d;  
}test_struct;  
```

0+4=4；

4+1=5；

5+1=6；

6+2=8；

8+4=12；

## 强制转换总结

+ 通过预编译命令`#pragma pack(n)`设置强制字节对齐系数,n为对齐系数。n=1，2，4，8，16
+ 对齐字节数 = min(`成员起始地址应是n的倍数时填充的字节数`，自然对齐时填充的字节数)
+ 结构体变量占用的内存空间大小需要是结构体占用最大内存空间类型的整数倍。

## 取消字节对齐

```c++
#include <stdio.h>  
#include <stdlib.h>  
#include <iostream>  
using namespace std;  
struct {  
    char b;  
    double c;  
    int a;  
}__attribute__((packed)) test_struct;  
int main() {  
    int result = sizeof(test_struct);  
    cout << result << endl;  
    return 0;  
} 
/*
result = 13
0+1=1;
1+8=9;
9+4=13;
*/
```

在结构体中加上`attribute((packed))`

# const

## 常量

常量是一种**标识符**，其值在运行期内恒定不变。

可以用`#define`、`const`来修饰常量

在C++中尽量使用`const`

> 在C++11标准中，'只读'使用const；'常量'使用constexpr

## const用法

`const`用来修饰内置类型变量、自定义对象、成员函数、返回值、函数参数等。

如果这个值在程序运行中是保持不变的，就可以明确使用`const`

## C和C++的const作用机制

+ C中，`const`修饰的变量依然被当作变量，内存中有存储的空间，而且可以通过**指针**间接的改变该内存空间的值
+ C++中，`const`修饰的常量会被编译器直接用常数替换掉。

## const修饰指针

在定义指针的时候，可以用`const`修饰，分为三种

1. 指向常量的指针==
2. 常指针
3. 指向常量的常指针

---

1. 指向常量的指针：防止指针指向的值被修改

   ```c++
   const int *p;
   ```

2. 常指针：防止指针指向改变

   ```c++
   int *const p;
   ```

3. 指向常量的常指针：常量指针指向常指针

   ```c++
   const int *const p;
   ```

**没有引用常量，因为引用本身就不能改变。**

## const修饰参数传递

对于const修饰函数参数可以分为三种情况

1. 值传递
2. 指针传递
3. 引用传递

---

 1. 值传递

    ```c++
    #include<iostream> 
    using namespace std;
    void douya(const int a){
        cout<<a;
        // ++a;  是错误的，a 不能被改变
    }
    
    int main(void) {
        douya(8);
        system("pause");
        return 0;
    }
    ```

    因为函参会复制实参的值，这种情况不会影响实参，一般不需要加`const`，但是加了也没关系。

 2. 指针传递

    ```c++
    #include<iostream> 
    using namespace std; 
    void douya(int *const a){ // 防止指针a被意外篡改
        cout<<*a<<" ";
        *a = 9;
    }
    
    int main(void){
        int a = 8;
        douya(&a);
        cout<<a; // a 为 9
        system("pause");
        return 0;
    }
    ```

    `const`参数为指针时，可以防止指针被意外篡改

 3. 引用传递

    ```c++
    #include<iostream>
    using namespace std;
    class Test{
    public:
        Test(){}
        Test(int _m):_cm(_m){}
        int get_cm()const{
           return _cm;
        }
    
    private:
        int _cm;
    };
    
    void douya(const Test& _tt){ // 传递const引用，避免临时对象构造，提升效率
        cout<<_tt.get_cm();
    }
    
    int main(void){
        Test t(8);
        douya(t);
        system("pause");
        return 0;
    }
    ```

对于一般的`int`,`double`等内置类型，一般不采用引用的传递方式。

## const修饰返回值

const修饰返回值分三种情况

1. const修饰内置类型的返回值
2. const修饰自定义类型作为返回值
3. const修饰返回的指针或者引用

**同样的，当返回值时，没必要加const，因为函数会为返回值复制一个副本。只有当我们返回引用或指针时才需要考虑是否加const**。

## const修饰类成员函数

`const`修饰类成员函数，是为了**防止成员函数被修改被调用对象的值**，如果我们不想修改一个调用对象的值，所有的成员函数都应当声明为`const`成员函数。

## const 和define的区别

+ 就起作用的阶段而言： #define是在编译的预处理阶段起作用，而const是在编译的时候起作用                       

+ 就起作用的方式而言： #define只是简单的字符替换，没有类型检查,存在边界的错误；const对应数据类型，进行类型检查；                                                                                                    
+ 就存储方式而言：#define，有多少地方使用，就替换多少次，它定义的宏常量在内存中有若干个备份,占用代码段空间；const定义的只读变量在程序运行过程中只有一份备份，占用数据段空间。

  定义的角度而言： const不足的地方，是与生俱来的，const不能重定义，而#define可以通过#undef取消某个符号的定义，再重新定义。

# Static

1. static 局部变量：指令执行到定义处才分配内存，将一个变量声明为函数的局部变量，**使其变为静态存储方式(静态数据区)**，那么这个局部变量在函数执行完成之后不会被释放，而是继续保留在内存中。
2. statoc 全局变量：全局变量即定义{}外面，其本身就是静态变量，编译时就分配内存，这只会改变其连接方式，**使其只在本文件内部有效，而其他文件不可连接或引用该变量。**
3. static 函数：对函数的连接方式产生影响，**使得函数只在本文件内部有效，对其他文件是不可见的。**这样的函数又叫作静态函数。使用静态函数的好处是，不用担心与其他文件的同名函数产生干扰，另外也是对函数本身的一种保护机制。如果想要其他文件可以引用本地函数，则要在函数定义时使用关键字extern，表示该函数是外部函数，可供其他文件调用。另外在要引用别的文件中定义的外部函数的文件中，使用extern声明要用的外部函数即可。

**最后对 static 的三条基本作用做一句话总结。首先 static 的最主要功能是隐藏，其次因为 static 变量存放在静态存储区，所以它具备持久性和默认值0。**

## C++增加的作用

1. 类中的静态成员变量：有时候需要为某个类的所有对象分配一个单一的存储空间。如果使用全局变量，因为可以被任何人修改和与其他变量名冲突很不安全。

   如果用`static`，不管创建了多少个该类的对象，所有这些对象的静态数据成员都共享**这一块静态存储空间**，这就为所有对象提供了相互通信的方法。

2. 类中的静态成员函数：静态成员函数主要用来访问静态成员变量，而不访问非静态成员变量

# new 和 malloc 的区别

new、delete是C++中独有的操作符，而malloc和free是C/C++中的标准库函数。

+ **new**创建对象在分配内存的时候会**自动调用构造函数**，同时也可以**完成对对象的初始化**，同理要记得**delete**也能自动**调用析构函数**。

+ malloc和 free是**库函数**而不是**运算符**，不在编译器控制范围之内，所以不能够自动调用构造函数和析构函数。也就是mallloc只是单纯地为变量分配内存，free也只是释放变量的内存。

## malloc的底层实现

+ malloc函数的实质是它有一个将可用的内存块连接为一个空闲**链表**。 调用malloc（）函数时，它沿着连接表寻找一个大到足以满足用户请求所需要的内存块。 然后，将该内存块一分为二（一块的大小与用户申请的大小相等，另一块的大小就是剩下来的字节）。 接下来，将分配给用户的那块内存存储区域传给用户，并将剩下的那块（如果有的话）返回到连接表上。

  因为malloc函数是在程序的**虚拟地址空间**申请的内存，与物理内存没有直接的关系。虚拟地址与物理地址之间的映射是由操作系统完成的，操作系统可通过虚拟内存技术扩大内存。

+  调用free函数时，它将用户释放的内存块连接到空闲链表上。 到最后，空闲链会被切成很多的小内存片段，如果这时用户申请一个大的内存片段， 那么空闲链表上可能没有可以满足用户要求的片段了。于是，malloc（）函数请求延时，并开始在空闲链表上检查各内存片段，对它们进行内存整理，将相邻的小空闲块合并成较大的内存块。

# volatile 和 mutable

## mutable

为了突破`const`的限制而设置的。

被`mutable`修饰的变量，将永远处于**可变**的状态，即使在一个`const`函数、`const`修饰的结构体变量或类对象中，都可以被修改。

`mutable`在类中只能够修饰非静态数据成员。

## volatile

变量可能会被改变。提示编译器每次要从**内存**重新读取变量，而不是在**寄存器**读取。

特别是在**多线程**编程中，变量的值在内存中可能已经被修改，而编译器优化优先从寄存器读取，可能是个旧值。

>当变量值在本线程里改变时，会同时把变量的新值copy到该寄存器中，以便保持一致。
>
>当变量在因别的线程等而改变了值，该寄存器的值不会相应改变，从而造成应用程序读取的值和实际的变量值不一致。

## volatile 在嵌入式中的应用

1. 外围设备的特殊功能寄存器
2. 在中断服务函数中修改全局变量
3. 在多线程中修改全局变量

## 一个参数可以是volatile和const的吗？

可以的，比如说是一个只读的状态寄存器。它是`volatile`表示它可能会被意想不到的改变，它是`const`表示程序不应该试图去修改它。

## 一个参数可以是volatile的吗？

可以的，比如中断程序去修改一个缓冲区的指针。

---

# 原子操作

原子操作指的是由多步操作组成的一个操作。即如果操作不能**原子**的执行，则要么执行完所有操作，要么不执行操作。

原子操作比**互斥锁**更接近与底层。

实现原理是基于**总线加锁**和**缓存加锁**

---

作为多线编程的变量值可能会被修改的解决方案，

+ 全局变量加关键字 volatile
+ 使用原子操作，效率比互斥锁高
+ 使用互斥锁

# 引用与指针的区别

(这里的引用均值**左值引用**。)

1. 指针是实体，占用内存空间。引用是别名，与变量共享内存空间。
2. 指针可以为`Null`或不初始化，引必须初始化。
3. 指针中途可以修改指向，引用不行。
4. `sizeof(指针)`计算的是指针本身大小，`sizeof(引用)`计算的是引用对象的大小。
5. 如果返回的是动态分配的内存或对象，必须使用**指针**。使用引用会产生内存泄漏。
6. 指针使用时需要解引用，引用使用时不需要解引用。
7. 有二级指针，没有二级引用。

> + 声明时的*为声明指针类型，&为声明引用
> + 单独的*为解引用，单独的&为取地址。

C++才可以使用**引用**这个概念，单独的取地址C也可以使用。

## 使用引用的好处

指针比引用更加灵活，但是也更加危险

1. 使用指针时必须判断指针是否为空
2. 在指针指向的地址被释放了以后，将指针也置空，否则容易出现**野指针**。

引用只是别名，

1. 引用必须初始化，绑定唯一变量，且不允许被更改。
2. 当本变量被销毁时，引用也会被销毁。

# 右值引用

# 什么是左值？什么是右值？

+ 左值是既可以出现在等号左边，也可以出现在等号右边。如表达式

  ```c++
  int a;
  int b;
  
  a = 3;
  b = 4;
  b = a;
  //下列写法不合法
  3 = a;
  a + b = 4;
  ```

  对左值的引用，就是给左值取别名。

  ```
  Type &引用名 =  左值;
  ```

  例如：

  ```
  int main(){
  	int a = 0;
  	int &b = a;
  	b  = 11;
  	return 0;
  }
  ```

+ 右值指的是只能出现在等号右边的变量(表达式)，如运算操作(**加减乘除**、**函数调用返回值**、**数值常量**等)产生的中间结果。

  右值指的是存储在内存中某些地址的**数值**。右值是不能对其进行赋值的表达式，也就是说，右值可以出现在赋值号的右边，但不能出现在赋值号的左边。

  ```
  Type &&引用名 = 右值表达式;
  ```

  例子：

  ```c++
  Test TestFunc(void);
  int main(){
  	Test &&a = TestFunc();
      return 0;
  }
  ```

---

左值引用我们知道**可以用于函数传参，减少不必要的对象拷贝，提升效率**；或者**用于替代指针的使用**。

C++引入右值引用主要是为了实现**移动语义**和**完美转发**。

## 移动语义

![img](https://uploadfiles.nowcoder.com/images/20210329/675098158_1617015872225/FB5C81ED3A220004B71069645F112867)

移动语义为了避免临时对象的拷贝，为类增加移动构造函数。移动构造函数与拷贝构造不同，它并不是重新分配一块新的空间同时将要拷贝的对象复制过来，而是"拿"了过来，将**自己的指针**指向别人的资源，然后将**别人的指针**修改为`nullptr`

## 完美转发

通过一个函数将参数继续转交给另一个函数进行处理，原参数可能是右值，也可能是左值，如果还能保持参数的原有特征，就叫**完美转发**。

# C++是面向对象的编程思想

+ C是**面向过程**的编程语言
+ C++是**面向对象**的编程语言

为了实现面向对象的编程机制，C++引入了新的数据类型---**类**

引申出三大特性---封装、继承、多态。

## 面向过程

C语言是面向过程的编程语言，由函数实现结构化编程，编写程序顺序执行，但是会存在两个问题：

+ 机器是按照顺序执行的，但当整个软件规模庞大时，无法有效的维护和扩展。
+ 结构化设计是以**功能**为目标来构造应用系统，这样程序员不得不将客体所构成的现实世界**映射**到由功能模块组成的**解空间**中，这种转换过程，**背离了**人们**观察和解决**问题的基本思路。

## 面向对象

所以就有了思考，能不能让计算机直接**模拟现实的环境**，

因为真实世界就是**各个对象交互**的世界，这就是面向对象的编程思想。

## 面向过程和面向对象的区别

+ 面向过程：关注问题解决的过程，按照顺序一步步的解决问题。
+ 面向对象：将问题的各个事务分成各个**对象**。**建立对象的目的不是为了完成一个步骤，而是为了描述一个事务在解决问题中经过的步骤和行为**。

# 类

```c++
class 类名{
    private:私有的数据和成员函数;
    public:共有的数据和成员函数:
    protected:受保护的数据和成员函数;
};
```

举个例子：

```c++
class Student{
	protected:
		int age;
		int number;
};
int main(){
    Student studen1;
}
//Student为一个类
//student1为新创建的一个对象
```



## 类的成员访问属性

+ private:私有成员，只限于类成员访问。
+ public:公有成员，允许类外和类内访问。
+ protected:受保护成员，允许类成员和派生类成员访问。

类可以**继承**，有三种继承关系

+ **派生类中新增成员可以访问基类的公有成员和保护成员，无法访问私有成员。**但是只有**公有继承**中，派生类的**对象**能访问**基类的公有成员**。使用友元（friend）可以访问保护成员和私有成员。

总结下：

![img](https://uploadfiles.nowcoder.com/images/20210329/675098158_1617016026906/4A47A0DB6E60853DEDFCFDF08A5CA249)

**题1：问Student1有几个受保护的成员？**

```C++
class Student{  
public:  
    void display();  
protected:  
    int num;  
    string name;  
    char sex;  
};  

class Student1:protected Student{  
public:  
    void display1();  
private:  
    int age;  
    string addr;  
}; 
```

基类的public1个和protected3个，子类没有protected，一共四个。

## struct和类(class)的区别

两者最大区别是struct里面默认的访问控制是public，而class中的默认访问控制是private。

## 类的成员

### 构造函数

类的构造函数是一种特殊的成员函数，会在每次创建类的新对象时执行。

+ 构造函数的名称与类的名称完全相同，且不会返回任何类型，也不会返回void
+ 构造函数可用于为某些成员变量赋初值
+ 构造函数分为无参和有参。

```c++
#include <iostream>
using namespace std;

class Sprout{
public:
   void setLength( double len );
   double getLength( void );
   Sprout(void);  // 这是无参数构造函数
   Sprout(double len);  // 这是带参数构造函数
private:
   double length;
};

// 成员函数定义，包括构造函数
Sprout::Sprout(void){
    cout << "Object is being created" << endl;
}

Sprout::Sprout( double len){
    cout << "Object is being created, length = " << len << endl;
    length = len;
}

void Sprout::setLength( double len ){
    length = len;
}

double Sprout::getLength( void ){
    return length;
}
// 程序的主函数
int main(){
   Sprout douya1;  //调用无参数构造函数
   Sprout douya2(10.0);  //调用带参数构造函数
   // 获取默认设置的长度
   cout << "Length of line : " << douya2.getLength() <<endl;
   // 再次设置长度
   douya2.setLength(6.0); 
   cout << "Length of line : " << douya2.getLength() <<endl;
   return 0;
}
```

+ 在类中声明的地方写函数具体内容，或者先声明，在下面写写具体内容也可以

+ 运行结果如下：

  ```c++
  Object is being created
  Object is being created, length = 10
  Length of line : 10
  Length of line : 6
  ```

**父子类构造函数的顺序：**

**基类构造函数，派生类对象成员构造函数，派生类本身的构造函数。**

### 析构函数

在每次删除对象时执行。

+ 析构函数的名字：`~类名`();

+ 析构函数有助于跳出程序(**关闭文件**、**释放内存**时)释放资源。

  ```C++
  #include <iostream>
  using namespace std;
  
  class Sprout{
  public:
      Sprout(void);  // 这是无参数构造函数
      ~Sprout(void); // 这是析构函数
  private:
      double *length;
  };
  
  // 成员函数定义，包括构造函数
  Sprout::Sprout(void){
      length = new Sprout();
  }
  
  // 如果析构函数这里不及时释放资源，就会造成内存泄露
  Sprout::~Sprout(void){
      delete[] length;
      length = nullptr;
  }
  ```

**析构函数为什么不能抛出异常？**

析构函数不能抛出异常，除了资源泄露还可能造成程序崩溃。

如果析构函数抛出异常，则异常点之后的程序不会执行，如果析构函数在异常点之后执行了某些必要的动作比如释放某些资源，则这些动作不会执行，会造成诸如资源泄漏的问题。

通常异常发生时，c++的机制会调用已经构造对象的析构函数来释放资源，此时若析构函数本身也抛出异常，则前一个异常尚未处理，又有新的异常，会造成程序崩溃的问题。

### 为什么析构函数可以为虚函数但是构造函数不能为虚函数

构造函数不能声明为虚函数，析构函数可以声明为虚函数，而且有时是必须声明为虚函数。

构造函数不能声明为虚函数的原因是:
1 构造一个对象的时候，必须知道对象的实际类型，而虚函数行为是在运行期间确定实际类型的。而在构造一个对象时，由于对象还未构造成功。编译器无法知道对象 的实际类型，是该类本身，还是该类的一个派生类，或是更深层次的派生类。无法确定。。。
2 虚函数的执行依赖于虚函数表。而虚函数表在构造函数中进行初始化工作，即初始化vptr，让他指向正确的虚函数表。而在构造对象期间，虚函数表还没有被初 始化，将无法进行。

### 拷贝构造函数

拷贝构造函数是在创建对象时，是使用同一类中**之前创建**的对象来初始化新创建的对象。

```c++
classname (const classname &obj){
//构造函数主体
}
```

拷贝构造函数通常用于：

+ 通过使用另一个同类型的对象来初始化新建的对象。
+ 复制对象把它作为参数传递给函数
+ 复制对象，并从函数返回这个对象。

如果在类中没有定义拷贝构造函数，编译器会**自行**定义一个。

如果**类中带有指针变量**，**并有动态内存分配**，则它必须要有一个拷贝构造函数，因为涉及到浅拷贝和深拷贝。

## 深拷贝和浅拷贝

+ 浅拷贝：一个对象的一个指针只复制另一个个对象的指针，而不复制本身，新旧对象共享同一块内存。

+ 深拷贝：**创造**一个一模一样的对象，新旧对象不会共享内存，且各自更改不会影响对方。

![img](https://uploadfiles.nowcoder.com/images/20210329/675098158_1617016091295/10FB15C77258A991B0028080A64FB42D)

两者的区别在于**有没有申请新的内存**

**调用拷贝构造函数**时，为指针申请了新的内存空间，这就是**深拷贝**；如果不申请新的内存空间，那么就只会单纯复制指针，指向同一快内存资源，这就是**浅拷贝**。**浅拷贝存在的问题是，一块内存，两次析构释放，自然就会出现问题**。

**如果自定义类里面有指针成员变量，一定要自定义拷贝构造函数，完成深拷贝的操作**。

## 友元函数

类的友元函数是定义在**类**外部，但有权访问类的所有private、protected成员。

类的友元函数**并不是成员函数**，尽管可以在类的定义中出现。

声明函数为一个类的友元，需要在类定义中该函数原型前使用关键字`friend`。

使用友元函数直接调用即可，不需要通过对象或指针。

```c++
class Box{
   double width;
public:
   double length;
   friend void printWidth( Box box );
   void setWidth( double wid );
};
```

## 初始化列表

除了通过用**构造函数**来初始化我们的成员变量，还可以用**初始化列表**来初始化我们的成员变量。

```c++
class Sprout{
public:
	int a;
    float b;
    Sprout():a(0),b(8.8){}//初始化列表 初始化
    //
    Sprout(){
        a=0;b=8.8;
    }    //构造函数 初始化
}
```

### 初始化列表与构造函数的区别

必须带有初始化列表的构造函数：

+ 成员类型是没有默认构造函数的类。

  ```c++
  class Sprout1{
  public:
      Sprout1(int a):i(a){}
  private:
      int i;
  };
  class Sprout2{
  public:
      Sprout1 test1;
  
      Sprout2(Sprout1 &t1){
          test1 = t1;
      }
  };
  ```

  第11行，Sprout1没有默认的构造函数，t1的类成员取值并明确，并不能直接赋值给test1。正确代码如下：

  ```c++
  class Sprout2{ //使用初始化列表
  public:
      Sprout1 test1;
  
      Sprout2(Sprout1 &t1):test1(t1){}
  };
  ```

  

+ `const`成员或**引用类型**的成员

  由于`const`和引用类型的成员只能**初始化**，不能赋值。

在性能上：

+ 对于内置数据类型、复合类型(指针、引用)的成员变量赋值，性能和结果是一样的。
+ 对于**用户定义类型**(类类型)-结果上相同，但是性能上存在很大差别。初始化列表直接调用拷贝构造函数。

### 初始化列表的成员初始化顺序问题

C++ 初始化类成员时，是按照**声明的顺序**初始化的，而不是按照出现在初始化列表中的顺序。

```c++
class Sprout {
    Sprout(int x, int y);
    int m_x;
    int m_y;
};

Sprout::Sprout(int x, int y) : m_y(y),m_x(x){};
```

编译器按照(int x,int y)，首先初始化m_x，再初始化m_y，

## this指针

每一个**成员函数**都包含一个特殊的指针，名字是固定的，称为**this指针**。它是指向本类对象的指针，它的值是当前被调用的成员函数**所在对象**的**起始地址**

```c++
class Cylinder{//圆柱类  
public:  
    float volume(){return PI*radius*radius*hight;};//求体积  
    static float PI;  
private:  
    float radius;//半径  
    float hight;//高  
};  
float Cylinder::PI = 3.1415926;  
Cylinder obj;  
int a  = obj.volume(); 
```

当调用成员函数`obj.volume()`时，编译器就把对象`obj`的起始地址赋给`this`指针。在成员函数开始运行时，就按照**this**的指向找到对象obj的**数据成员**。

该函数实际上时执行

```c++
PI*(this->radius)*(this->radius)*(this->hight);
```

`*this`就是对象`obj`，但**静态成员变量和函数**从不属于某个对象，也不会有`this`指针，所以这里的`PI`没有通过this指针访问。

### this指针的注意

1. 只能在成员函数中使用，在**全局函数**和**静态函数**都不能使用this
2. this指针是在成员函数的开始前构造，并在成员函数的结束后清楚。
3. this指针会因编译器不同而有不同的存储位置，可能是栈、寄存器或全局变量。
4. this是类的指针。

# 继承

**继承**允许我们依据另一个类来定义一个类。

+ 已有的类称为**基类**
+ 新建的类称为**派生类**

## 继承类型和访问属性

通常使用public继承

不管哪种继承方式：**派生类中新增成员可以访问基类的公有成员和保护成员，无法访问私有成员。**但是只有**公有继承**中，派生类的**对象**能访问**基类的公有成员**。使用友元（friend）可以访问保护成员和私有成员。

## 多重继承二义性问题

![img](https://uploadfiles.nowcoder.com/images/20210329/675098158_1617016373644/7AFBB1602613EC52B265D7A54AD27330)

如上图，类A派生出类B和类C。类D继承自类B和类C。

这个时候类A的成员变量和成员函数继承给类D会变成2份：

+ A->B->D
+ A->C->D

这样在一个派生类D中保留间接类的**多份同名成员**。这是多余的行为：**因为保留多份成员变量不仅占用较多的存储空间，还容易产生命名冲突**。

为了解决这一问题，需要使用**虚继承**。使得在派生类中只能保留一份**间接**基类的成员

```c++
//间接基类A
class A{
protected:
    int m_a;
};

//直接基类B
class B: virtual public A{  //虚继承
protected:
    int m_b;
};

//直接基类C
class C: virtual public A{  //虚继承
protected:
    int m_c;
};

//派生类D
class D: public B, public C{
public:
    void seta(int a){ m_a = a; }  //正确
    void setb(int b){ m_b = b; }  //正确
    void setc(int c){ m_c = c; }  //正确
    void setd(int d){ m_d = d; }  //正确
private:
    int m_d;
};

int main(){
    D d;
    return 0;
}
```

用图片表示就是

![img](https://uploadfiles.nowcoder.com/images/20210329/675098158_1617016409498/586E508F161F26CE94633729AC56C602)

当然，在实际情况中不提成使用多继承，能用单一继承解决的问题就不要使用多继承。

# 多态

人类希望：指针指向基类则调用基类的方法，指针指向派生类则调用派生类的方法。这样程序变得十分灵活，增强了代码的**可复用性,方便维护**。

## 虚函数

C++为实现这个目的，引入了多态的机制，多态的实现机制是**虚函数**。

**虚函数的作用是允许在派生类中重新定义与基类同名的函数，并且可以通过基类指针或引用来访问基类和派生类的同名函数**。

方法是在基类中为同名函数添加关键字`virtual`

```c++
#include <iostream>  
using namespace std;  
class Parent {  
public:  
    Parent() {}  
    virtual void func() { cout << "Parent" << endl; }  
};  

class Child :public Parent {  
public:  
    Child() {}  
    void func() { cout << "Child" << endl; }  
};  

int main() {  
    Parent *Pparent;  
    Parent parent;  
    Pparent = &parent;  
    Pparent->func();  
    Child child;  
    Pparent = &child;  
    Pparent->func();  
    system("Pause");  
    return 0;  
}  
```

输出结果：

```
Parent
Child
```

像这样，基类指针可以按照基类的方式来做事，也可以按照派生类的方式来做事，它有**多种形态**，或者说有多种表现方式，我们将这种现象称为**多态（Polymorphism）**。

## 实现虚函数的原理

原理：虚函数表+虚表指针。

当一个类里存在虚函数时，编译器会为类创建一个虚函数表，**虚函数表**是一个数组，数组的元素存放的是类中**虚函数的地址。**

同时为**每个**类的对象添加**一个**隐藏成员，该隐藏成员保存了指向该函数表的指针。该隐藏成员占据该对象的内存布局的最前端。

**即对于一个类来说，虚函数表只有一个，但是有多少个类的对象，就对应多少个虚函数表指针。**

举个例子来说：

```
#include <iostream>  
using namespace std;  
class Base {  
public:  
    Base() {}  
public:  
    virtual void Bf() { cout << "Base::Bf()" << endl; }  
     virtual void Bg() { cout << "Base::Bg()" << endl; }  
    virtual void Bh() { cout << "Base::Bh()" << endl; }  
};  

class Son:public Base {  
public:  
    Son() {}  
    void func() { cout << "Douya" << endl; }  
};  

int main() {  
    Base b;  
    return 0;  
}  
```

![img](https://uploadfiles.nowcoder.com/images/20210329/675098158_1617016474462/8266E4BFEDA1BD42D8F9794EB4EA0A13)

当创建一个b对象时，在b内存分布的地址**最前端**，编译器

为b分配了一个隐藏成员。该隐藏成员就是**虚函数表指针**

,存储的是虚函数表的**地址**。

虚函数表是个数组，数组的元素存放的是**类中虚函数的地址**。当对象调用虚函数时，就会先通过虚函数表指针找到虚函数表，访问虚函数表对应的与元素，也就找到相应虚函数的地址了。

如果存在继承关系，

## 纯虚函数

纯虚函数的作用是在基类中为其派生类保留一个函数的名字，以便派生类根据需要对他进行定义。

```
virtual void func()==0;
```

因为纯虚函数没有函数体，不是完整的函数，无法为其分配内存空间，也让包含**纯虚函数**的类无法被实例化，称为**抽象类**。

抽象类通常作为**基类**，让派生类去实现纯虚函数。派生类必须实现纯虚函数才能被实例化。

## 虚构造函数与虚析构函数

+ 构造函数不能为虚函数。
  + 因为若构造函数为虚函数，说明调用方式是通过虚函数表调用，但是根本没有构造对象，哪里来的虚函数表。
+ C++默认析构函数不是虚函数。当存在继承关系时，析构函数必须申明为**虚函数**。
  + 这样父类指针指向子类对象，释放基类指针时才会调用子类的析构函数释放资源，否则导致**内存泄漏**。

# 重载与重写

除了多态还有一种代码复用的形式，那就是函数**重载**

对于**函数重载**：在类成员里，可以有一组具有相同函数名，不同参数列表(类型、个数)的函数。

由于重载函数在经过编译后有着不同的**函数签名**，所以可以实现函数的重载

## 重载与重写的区别

+ 重载(overload)：函数名相同，参数列表不同(参数类型、参数顺序)，不能使用**返回值**区分
  + 作用域相同
  + 函数名相同
  + 参数列表必须不同，返回值无要求

  + 是一种语言特性
  + 若一重载版本的函数前面有`virtual`关键字，则表示它是虚函数，但**它是重载的一个版本**。

+ 重写(override)：派生类重定义基类的虚函数，既会覆盖基类的虚函数(多态)。

  + 作用域不同
  + 函数名、参数列表、返回值与基类相同
  + 基类函数是virtual

# 运算符重载

对于C++提供的所有操作符，通常只支持对于基本数据类型(int、float)和标准库中提供的类(string)的操作

而对于用户**自定义**的类，如果想通过操作符实现一些基本操作（比如判断大小、判断相等）就需要用户自己来定义这个操作符的具体实现了。

```c++
#include <iostream>  
using namespace std;  

class myselfclass {  
public:  
    myselfclass(int val) :m_val(val) {};  
    ~myselfclass() {};  
    bool operator==(const myselfclass &other);  
private:  
    int m_val;  
    bool m_flag;  
};  

bool myselfclass::operator==(const myselfclass &other) {  
    if (this->m_val == other.m_val) {  
        return true;  
    }  
    return false;  
}  

int main() {  
    myselfclass a(10), b(10);  
    if (a == b)  
        printf("a is equal to b!\n");  
    else  
        printf("a is not equal to b!\n");  
    return 0;  
}  
```

运算结果：

```
a is equal to b!
```

## 可以重载的运算符列表

| 双目算术运算符 |            + (加)，-(减)，*(乘)，/(除)，% (取模)             |
| :------------: | :----------------------------------------------------------: |
|   关系运算符   | ==(等于)，!= (不等于)，< (小于)，> (大于)，<=(小于等于)，>=(大于等于) |
|   逻辑运算符   |             \|\|(逻辑或)，&&(逻辑与)，!(逻辑非)              |
|   单目运算符   |              + (正)，-(负)，*(指针)，&(取地址)               |
| 自增自减运算符 |                      ++(自增)，--(自减)                      |
|    位运算符    | \| (按位或)，& (按位与)，~(按位取反)，^(按位异或),，<< (左移)，>>(右移) |
|   赋值运算符   |       =, +=, -=, *=, /= , % = , &=, \|=, ^=, <<=, >>=        |
| 空间申请与释放 |                new, delete, new[ ] , delete[]                |
|   其他运算符   | **()**(函数调用)，**->**(成员访问)，**,**(逗号)，**[]**(下标) |

## 不可重载的运算符列表

| 运算符名称         | 运算符  |
| ------------------ | ------- |
| 成员访问运算符     | .       |
| 成员指针访问运算符 | .*  ->* |
| 域运算符           | ：      |
| 长度运算符         | sizeof  |
| 条件运算符         | ？：    |
| 预处理预算         | #       |

操作符被重载的基本前提：

1、只能为自定义类型重载操作符；

2、不能对操作符的语法(优先级、结合性、操作数个数、语法结构)、语义进行颠覆；

3、不能引入新的自定义操作符。

# 迭代器和指针

## STL

迭代器是一个**变量**，从属于**STL**六大成分之一。

STL(Standard Template Library):标准模板库

是C++标准库的一部分。

STL就是借助模板把常用的数据结构与算法都实现了一遍，并且做到了数据结构和算法的分离。包括但不限于

+ `vector`-顺序表(数组)
+ `list`-双向链表
+ `deque`-循环队列
+ `set`-红黑树
+ `hash_set`-哈希表

STL由六大部分组成，其中后四部分是为前两部分服务的。

| STL的组成  |                             含义                             |
| :--------: | :----------------------------------------------------------: |
|    容器    | 一些封装**数据结构**的模板类，例如 vector 向量容器、list 列表容器等。 |
|    算法    | STL 提供了非常多（大约 100 个）的数据结构算法，它们都被设计成一个个的模板函数，这些算法在 std 命名空间中定义，其中大部分算法都包含在头文件 <algorithm> 中，少部分位于头文件 <numeric> 中。</numeric></algorithm> |
|   迭代器   | 在 C++ STL 中，对容器中数据的读和写，是通过迭代器完成的，迭代器就是容器和算法之间的桥梁。 |
|  函数对象  | 如果一个类将 () 运算符重载为成员函数，这个类就称为函数对象类，这个类的对象就是函数对象（又称仿函数）。 |
|   适配器   | 可以使一个类的接口（模板的参数）适配成用户指定的形式，从而让原本不能在一起工作的两个类工作在一起。值得一提的是，容器、迭代器和函数都有适配器。 |
| 内存分配器 | 为容器类模板提供自定义的内存申请和释放功能，由于往往只有高级用户才有改变内存分配策略的需求，因此内存分配器对于一般用户来说，并不常用。 |

# 迭代器

迭代器可以指向容器中的某个元素，通过迭代器就可以读写它指向的元素。类似于指针。

迭代器按照定义方式可以分为四种：

+ 正向迭代器   ---   容器类名::iterator 迭代器名
+ 常量正向迭代器  --- 容器类名::const_iterator 迭代器名
+ 反向迭代器 --- 容器类名::reverse_iterator 迭代器名
+ 常量反向迭代器 --- 容器类名::const_reverse_iterator 迭代器名

---

通过迭代器可以读取它指向的元素   `*迭代器名`

比如

+ 对正向迭代器进行++操作，迭代器会**指**向容器中的后一个元素。

+ 对反向迭代器进行++操作，迭代器会**指**向容器中的前一个元素。

```c++
#include <iostream>  
#include <vector>  
using namespace std;  
int main(){  
    vector<int> v;  //v是存放int类型变量的可变长数组，开始时没有元素  
    for (int n = 0; n<5; ++n)  
        v.push_back(n);  //push_back成员函数在vector容器尾部添加一个元素  
    vector<int>::iterator i;  //定义正向迭代器  
    for (i = v.begin(); i != v.end(); ++i) { 
    //用迭代器遍历容器，begin 成员函数返回指向容器中第一个元素的迭代器。++i 使得 i 指向容器中的下一个元素。end 成员函数返回的不是指向最后一个元素的迭代器，而是指向最后一个元素后面的位置的迭代器，因此循环的终止条件是i != v.end()  
        cout << *i << " ";  //*i 就是迭代器i指向的元素  
        *i *= 2;  //每个元素变为原来的2倍  
    }  
    cout << endl;  
    //用反向迭代器遍历容器  
    for (vector<int>::reverse_iterator j = v.rbegin(); j != v.rend(); ++j)  
        cout << *j << " ";  
    return 0;  
}  
```

运行结果如下：

```c++
0 1 2 3 4
8 6 4 2 0
```

**要特别注意的是！**

+ `begin()`成员函数指向容器中的第一个元素迭代器
+ `end()`成员函数指向最后一个元素**后面的位置的迭代器**

## 迭代器的替代

如果觉得定义迭代器类型太麻烦，可以使用`auto`

`auto`可以根据初始化表达式自动推断出该变量的类型

```
for(auto i = v.begin();i! = v.end(); ++i){
//定义正向迭代器
}
for(auto i = v.rbegin();j != v.rend; ++i){
//定义反向迭代器 
}
```

## 迭代器和指针的区别

+ 迭代器不是指针，是**类模板**，通过重载了指针的一些操作来模拟指针的一些功能。

## ++i与i++的区别

```c
# include<stdio.h>
int main(){
    int i = 2;
    int j = 2;
    j += i++; //先赋值后加
    printf("i= %d, j= %d\n",i, j); //i= 3, j= 4
    i = 2;
    j = 2;
    j += ++i; //先加后赋值
    printf("i= %d, j= %d",i, j); //i= 3, j= 5
}
```

+ 赋值顺序不同
  + ++i是先加后赋值
  + i++是先赋值后加
+ 效率不同：后者比前者要慢
+ 左值：i++不能当为左值，++i可以。
+ 两者都不是原子操作，因为分为两步进行。

+ 对循环控制变量i，要尽量写`++i`

# 模板

```c++
template<typename T>;
//template为关键字
//typename为类型名
//T为类型参数名
```

## 函数模板

比如一个交换函数模板：

```c++
//交换 int 变量的值
void Swap(int *a, int *b){
    int temp = *a;
    *a = *b;
    *b = temp;
}

//交换 float 变量的值
void Swap(float *a, float *b){
    float temp = *a;
    *a = *b;
    *b = temp;
}

//交换 char 变量的值
void Swap(char *a, char *b){
    char temp = *a;
    *a = *b;
    *b = temp;
}

//交换 bool 变量的值
void Swap(bool *a, bool *b){
    char temp = *a;
    *a = *b;
    *b = temp;
}
```

通过写**函数模板**

```c++
template<typename T> void Swap(T *a, T *b){
    T temp = *a;
    *a = *b;
    *b = temp;
}

//交换 int 变量的值
int n1 = 100, n2 = 200;
Swap(&n1, &n2);

//交换 float 变量的值
float f1 = 12.5, f2 = 56.93;
Swap(&f1, &f2);
```

**函数模板与模板函数**:

+ **函数模板**的重点是模板
+ **模板函数**是函数模板的一个实例化

## 类模板

```c++
template<class T> //声明一个模板，虚拟类型名为T  
class Operation{//操作整数的类  
public:  
    Operation （T a, T b) : x(a), y(b){}  
    T add() {  
        return x+y;  
    }  
    T subtract(){  
        return x-y;  
    }  
private:  
    T x, y;  
}; 
```

记得声明一个类模板的**对象**时，要把实际类型名去掉虚拟的类型。

```
Operation<int> obj(1,2);
```

类模板同样可以继承

## 可变参数模板

可变参数模板对参数进行了高度泛化，可以表示任意数目、任意类型的参数

```
template<typename ... T>
```

给出实例

```c++
#include <iostream>  
using namespace std;  
template <typename ... T>  
void func(T ... args){  
    cout << "num is " << sizeof ... (args) << endl;  
}  

int main(){  
    func();//没有参数  
    func(1);//一个int参数  
    func(1, 1.2, 'A');//一个int，一个double，一个char  
    return 0;  
}  
```

运行结果如下

```
num is 0
num is 1
num is 3
```

其中**T叫做模板参数包**，**args叫函数参数包**

省略号的作用：

1. 声明一个包含0到任意个模板参数的参数包
2. 到模板定义的右边，可以将参数包展开成一个个独立的参数。

那参数怎么展开呢？

C++11可以使用递归函数的方式展开参数包

```c++
#include <iostream>  
using namespace std;  
//递归终止函数，特化的模板函数  
void print() {  
    cout << "empty" << endl;  
}  
//展开函数  
template <class T, class... Args>  
void print(T head, Args... args) {  
    cout << "parameter " << head << endl;  
    print(args...);  
}  

int main(){  
    print(1, 2, 1.2, 'B');  
    return 0;  
}  
```

运行结果如下：

```c++
parameter 1
parameter 2
parameter 1.2
parameter B
empty
```

递归函数展开参数包是一种标准做法，也比较好理解，但也有一个**缺点**,就是必须要一个重载的递归终止函数，即必须要有一个同名的终止函数来终止递归。

# 智能指针

使用普通指针，容易造成堆内存泄漏(忘记释放)等问题。

C++引入了**智能指针**，利用**RAII**对普通指针进行封装，实质是个对象，行为表现是个指针。

因为智能指针是个类，当超出了类的作用域时，类会自动调用**析构函数**，自动释放资源，这样就不会担心内存泄漏的问题。



# 类型转换

+ 自动转换
+ 强制转换

## C++的四种强制转换

```
xxxx_cast<类型说明符>(变量或表达式)
```

**1. const_cast**

​	用于强制去掉不能被修改的常数特性，在日常生活中尽量少用。

​	**去除常量性的对象必须为指针或引用**。

​	**常量指针被转化成非常量指针，并且仍然指向原来的对象；常量引用被转换成非常量引用，并且仍然指向原来的对象。**

```c++
#include <stdio.h>  
#include <stdlib.h>  
#include <string>  
#include <iostream>  
using namespace std;  
int main() {  
    const double a = 7;  
    const double* p = &a;  
    double* q = const_cast<double*>(p);  
    *q = 20; //通过q写值是未定义的行为  
    cout << "a=" << a << endl;  
    cout << "*p=" << *p << endl;  
    cout << "*q=" << *q << endl;  
    system("Pause");      
    return 0;  
}  
```

运行结果如下：

```c++
a=7
*p=20
*q=20
```

2. **static_cast**

   将一种类型强制转换为另一种类型,什么都可以转,最常用

   但是在向下转换(基类->子类)时,没有动态类型检查,是不安全的.

3. **dynamic_cast**

   用于含**虚函数**的转换,用于向上和向下的转换.

4. reinterpret_cast

   主要有三种强制转换用途:

   + 改变指针或引用的类型
   + 将指针或引用转换为一个足够长度的整型
   + 将整型转换为指针或引用类型.

# lambda

C++11 引入了Lambda表达式，用于定义并创建匿名的函数对象。

```
[函数对象参数] (操作符重载函数参数) mutable 或 exception 声明 -> 返回值类型 {函数体}  
```

+ 函数对象参数：

  **[函数对象参数]**标识一个Lambda表达式的开始，不能省略。方括号`[ ]`内的参数集合叫做一个闭包，允许Lambda函数可以引用在它之外声明的变量，这个机制允许这些变量被按值或按引用**捕获**

  | 函数对象参数 |    形式     |                             含义                             |
  | :----------: | :---------: | :----------------------------------------------------------: |
  |      空      |     []      |                     没有任何函数对象参数                     |
  |      =       |     [=]     |                     表示按值捕获外部参数                     |
  |      &       |     [&]     |                    表示按引用捕获外部参数                    |
  |     this     |      -      |          函数体内可以使用 Lambda 所在类中的成员变量          |
  |      a       |     [a]     | 将参数a按值进行捕获。**按值进行捕获时，函数体内不能修改传递进来的 a 的拷贝，因为默认情况下函数是 const 的，要修改传递进来的拷贝，可以添加 mutable 修饰符** |
  |      &a      |    [&a]     |                   表示将 a 按引用进行捕获                    |
  |    a，&b     |   [a, &b]   |             表示将 a 按值捕获，b 按引用进行捕获              |
  |  =，&a，&b   | [=, &a, &b] |    表示除 a 和 b 按引用进行捕获外，其他参数都按值进行捕获    |
  |   &，a，b    |  [&, a, b]  |    表示除 a 和 b 按值进行捕获外，其他参数都按引用进行捕获    |

2. (操作符重载函数参数)

   标识重载的()操作符的参数，没有参数时，这部分可以省略。参数可以通过按值(如：(a,b) )和按引用(如：(&a,&b))两种方式来进行传递。

3. mutable或exception 声明

   该部分可以胜率。

4. 返回值类型

5. {函数体}

给出实例

```c++
[] (int x, int y) { return x + y; } // 隐式返回类型  
[] (int& x) { ++x; } //没有return语句，Lambda函数的返回类型是void
[] () { ++global_x; } //没有参数，仅访问某个全局变量  
[] { ++global_x; } //与上一个相同，省略了(操作符重载函数参数)  
[] (int x, int y)->int{ int z = x+y; return z; }//可以指定返回类型  
aotu cmp = [](int x, int y){ return x > y; } //匿名函数也可以有名称 
```

## lambda实现原理

编译器会把一个lambda表达式生成一个匿名类的匿名对象，并在类中**重载**函数调用运算符`()`。

我们举个简单的例子：

```c++
auto print = []{cout << "Douya" << endl; };
```

那么编译器生成一个匿名类的匿名对象，形式可能如下：

```c++
//用给定的lambda表达式生成相应的类
class print_class{
public:
    void operator()(void) const{
        cout << "Douya" << endl;
    }
};
//用构造的类创建对象，print此时就是一个函数对象
auto print = print_class();
```

可以看到匿名类里重载了函数调用运算符`()`。还生成了一个函数对象，那么我们就直接可以使用这个函数对象了。

## lambda值捕获可以修改嘛

默认情况下，**按值捕获**的变量是**不可以被修改**的。非要修改的话，可以在参数列表后加关键字mutable。

## sizeof(lambda表达式)等于多少

既然编译器为lambda创建匿名类，那当然是sizeof(匿名类)的大小了呀。sizeof(类)=sizeof(所有成员变量)