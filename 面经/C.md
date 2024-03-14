# int main

关于

```c++
int main(int argc,char **argv)函数，两个参数分别代表什么意思
```

+ `int argc`：用来统计程序运行发送给`main`函数的命令行参数的个数
+ `char **argv`：**字符串**数组，用来存放指向字符串的指针元素，每一个指针元素指向一个字符串参数。
  + `argv[0]`:程序运行的全路径名
  + `argv[1]`:指向在命令行中执行程序名后的第一个字符串
  + `argv[2]`:指向在命令行中执行程序名后的第二个字符串
  + `argv[argc]`:为`NULL`

# if else

+ 最近匹配原则：else只跟上面最近的if匹配

  > 不要被错误排版欺骗

+ if else的作用域为{}或紧跟下面第一句话

# switch

+ switch 后面不跟break，程序会继续往下一个case执行，直到遇到break才跳出
+ 要有default
+ switch的（）里面必须是常量表达式，byte，short，char类或枚举类型会自动转换为int整型
+ float，double，string作为小数约算值，不能作为常量表达式

# For，while，do-while

+ 大循环放小循环里面

+ for(a;b;c)，只要b==true，for就会循环下去。

+ ---

  `do{..,}while(0)`的作用：

  `do{..,}while(0)`一般用在宏定义中，来保证宏替换在语法上的正确性

  ```c++
  #define student(x) x+1;x+2;
  #define student(x) do{x+1;x+2;}while(0)
  
  int main()
  {
  	if(x==1)
  		student(x)
      else
      	x=x+3;
  }
  //如果是宏定义1的话，在student(x)替换后只能让if执行第一句话
  ```

  

# 枚举enumeration

+ 关键字 enum

+ 如果一个变量有几种可能的值，就可以定义为枚举类

+ 该变量的值只能在列举值范围内。

  ```
  enum 枚举名{
  		标识符[=整型常数],
  		标识符[=整型常数],
  		...
  		标识符[=整型常数],
  		
  }枚举变量变量
  ```

  比如

  ```
  enum int{1,2,3} number;
  enum color{red,green,blue} c;
  ```

+ 枚举变量的取值，默认从0开始，变量之间间隔为1，可以省略

```
enum color{red,green=5,blue} c;
//这里red=0，green=5，blue=6
```

# 结构体与共用体

+ 结构体(`struct`)：允许用户存储**不同**类型的变量

+ 共用体(`union`)：允许在**某一固定内存位置**存储不同的数据类型

两者区别：

+ 内存上(不考虑字节对齐)
  + 结构体的内存是所有成员各自占用的内存空间之和
  + 共用体的内存是占用内存空间最大的成员

+ 结果上
  + 结构体的变量间不会相互影响
  + 共用体的变量因为会共享内存，新成员赋值会覆盖掉原来成员，且同一时刻只能保存一个成员的值。

# 函数

+ 实参和形参

  函数的形参必须是变量，函数的实参可以是变量、常量或表达式。

```c
//函数声明
double cylinder(double r,double h);
//函数调用
volume = cylinder(r_1,h_1);
volume = cylinder(3,4);
volume = cylinder(r_1+3,h_1+4);
```

+ 函数运行过程

  在函数运行时，会产生两个中间变量`r`、`h`，并根据实际调用的函数，将实参**值**赋值给形参,在函数进行完成后返回一个函数值，形参也会删除。

  **函数会为形参申请新的内存空间，并将实参的值复制给形参。**因为实参和形参是两个变量，内存地址也不一样，形参的值不会影响实参。

+ 如果想要真正的通过形参改变实参怎么办？

  **指针**

  将实参地址赋值给形参指针变量，使形参指针也指向实参的地址空间。

  如果形参为指针，那么形参的值为实参的地址。

  ```c
  //函数声明
  double cylinder(double *r,double *h);
  //函数调用
  volume = cylinder(&r_1,&h_1);
  ```

  为了避免形参(指向实参的指针)在函数中可能受到无意改变，我们需要用到`const`

  ```C
  //函数声明
  double cylinder(double const *r,double const *h);
  
  ```

## 函数传参的三种方式

| 调用类型 |                             描述                             |
| :------: | :----------------------------------------------------------: |
| 传值调用 | 该方法把参数的实际值赋值给函数的形式参数。在这种情况下，修改函数内的形式参数**不会**对实际参数没有影响。 |
| 指针调用 | 该方法把参数的地址赋值给形式参数。在函数内，该地址用于访问调用中要用到的实际参数。这意味着，修改形式参数**会**影响实际参数。 |
| 引用调用 | 该方法把参数的引用赋值给形式参数。在函数内，该引用用于访问调用中要用到的实际参数。这意味着，修改形式参数**会**影响实际参数。(C++特性) |

## 形参参数个数不定的函数

类似printf

# 全局变量与局部变量

+ 局部变量：函数或代码块内部声明的变量。，只能被函数内部或者代码块内部的语句使用

+ 全局变量：所有函数外部声明的变量。全局变量的值在程序的整个生命周期都有效。

**提示：**

+ 局部变量和静态变量的名称可以相同，但是在局部变量位于的函数内，局部变量会**被优先使用**。

  > int main()也算一个函数。

+ 只声明局部变量和全局变量时，系统不会对局部变量初始化(是个随机值)，对全局变量自动初始化。

  | 数据类型     | 初始化默认值 |
  | ------------ | ------------ |
  | int          | 0            |
  | char         | '\0'         |
  | float,double | 0            |
  | pointer      | Null         |

  ```c
  #include <stdio.h>  
  #include <stdlib.h>  
  
  int n = 10;  //全局变量  
  void func1() {  
      int n = 20;  //局部变量  
      printf("func1 n: %d\n", n);  
  }  
  void func2(int n) {  
      printf("func2 n: %d\n", n);  
  }  
  void func3() {  
      printf("func3 n: %d\n", n);  
  }  
  int main() {  
      int n = 30;  //局部变量  
      func1();  
      func2(n);  
      func3();  
      //代码块由{}包围  
      {  
          int n = 40;  //局部变量  
          printf("block n: %d\n", n);  
      }  
      printf("main n: %d\n", n);  
      system("Pause");  
      return 0;  
  }  
  ```

  输出答案为：

  ```C
  func1 n: 20
  func2 n: 30
  func3 n: 10
  block n: 40
  main n: 30
  ```

提示：在执行一个函数后，如果找变量，函数内没有局部变量就去找所以函数外部的全局变量。int main(){}也要看成一个函数。

# 数据类型

## 原码和补码

+ 原码：数的二进制形式,第一位表示符号位。正0负1
+ 反码：正数的反码=原码，负数的反码=原码取反
+ 补码：正数的补码与原码相反，负数的补码为反码+1

>[原码、补码、反码的关系 - 二进制的奥秘 - 博客园 (cnblogs.com)](https://www.cnblogs.com/goahead--linux/p/10904701.html)

| 数   | 原码      | 反码      | 补码      |
| ---- | --------- | --------- | --------- |
| 10   | 0000 1010 | 0000 1010 | 0000 1010 |
| -10  | 1000 1010 | 0111 0101 | 0111 0110 |

---

## 数据类型的存储

+ win32还是win64的数据类型占用字节大小一样，**指针**类型占用大小是两倍关系。
+ 需要说明的是，在linux系统sizeof(long)=8

![img](https://uploadfiles.nowcoder.com/images/20210329/675098158_1617008612470/C9BACA3CDA1C39194C04FE2170C3DA65)

![img](https://uploadfiles.nowcoder.com/images/20210329/675098158_1617008684347/88399FDCF82E54C15EBBAABE86FF3E5E)

1字节= 8位，那么每个类型的最大值最小值都可以计算。

当char a = 127时，如果a+1，那么a = -128；

`unsigned`为无符号数，取值范围从0开始

## 类型转换

+ 自动类型转换：低字节数与高字节数作运算，会进行自动类型转换。

  ![img](https://uploadfiles.nowcoder.com/images/20210329/675098158_1617008641089/BA6BEB7AE28EF0A97D7A0A038FEB5060)

+ 强制类型转换

# 运算符

## 运算符优先级

单目：单个参数

双目：两个参数

![img](https://uploadfiles.nowcoder.com/images/20210329/675098158_1617008726170/079F4FB55B755F6F198BEE97D7C95390)

+ 把握不准的情况就+括号
+ `^`为异或运算符，同为0，异为1。

## 位运算符

位运算符作用于位，将数按照二进制**逐位**执行位运算操作。

+ 优点：直接作用于内存数的二进制形式，执行效率高。
+ 作用
  + 判断奇偶
  + 无需第三个变量进行两变量交换
  + 求绝对值
  + 左移x2 右移/2
  + 高四位，低四位。 置1，置0，交换

# 关键字

## sizeof

+ sizeof(数组名) = 数组所占字节数
+ sizeof(指针) = 指针所占字节数。

## strlen

+ strlen(数组名) = 数组所占字节数
+ strlen(字符串指针名) = 指针所占字节数。
+ strlen来计算字符串所占字节数，但不包括结束字符`\0`

> string为C++才有的，C不支持string类型，必须写成char

## register

（1）作用：编译器会将register修饰的变量尽可能地放在CPU的寄存器中，以加快其存取速度，一般用于频繁使用的变量。

（2）注意：register变量可能不存放在内存中，所以不能用&来获取该变量的地址；只有局部变量和形参可以作为register变量；寄存器数量有限，不能定义过多register变量。

# 数组指针和指针数组

+ 数组：一组相同数据类型的数据集合称为数组

+ 元素：数组中的每一个数据
+ 数组名：数组首地址

+ 数组均从0下标开始

---

+ **指针：数据在内存中的地址**

+ 指针变量：该变量存储了一份数据的指针

  > 这个数据可以是变量、数组、字符串和函数等

+ 指向：指针存储的数据是一个数据的地址，就称指针指向该数据

  ```
  int a = 100;
  int* p = &a;
  //&为取地址符
  ```

## 数组首地址等

```
int arr[10] = {0};
```

+ `arr`：数组名，同时也是**第一个元素的首字节地址**。

+ `arr[i]`：数组的第`i`元素，等价于`*(arr+i)`

+ `&arr[0]`：等价于`arr`，是一个地址常量，且只能作为右值。

+ `&arr`：**数组首地址**,值与`arr`相等。但是在程序编写时意义完全不同。

  >+ &arr+1：代表下一个10个元素的数组的数组首地址。
  >+ arr+1：代表该数组的下标为1的数组元素的地址。

## 数组指针，行指针

即**指向一个数组**的指针,该指针存储的数据为**数组首地址**。

```
int (*p)[n];
```

以上定义表面，指针`p`指向含有`n`个整型元素的一维数组。

```
int a[3][4];//定义一个3行4列的二维数组
int (*p)[4];//定义一个1行4列的一维数组
p = a;
p++;
```

第`3`行：p，a实际上都是指针变量，a是二维数组的首地址，将a值赋予p值，且p指针指向一个一维数组，可存储a[0] [4]，故又称数组指针为**行指针**。

## 访问i行i列的二维数组元素

```
//声明一个二维数组
// 数据类型 数据名[行数][列数]
int a[3][4];
int (*p)[4];
p=a;
//方法1
result = a[i][j];
//方法2
result = *(*(p+i)+j);
//方法3
result = *(p[i]+j);
```

![image-20210704115747017](https://i.loli.net/2021/07/04/Utl4VZYq6EHMG2r.png)

首先要明确的是，二维数组`a[3][4]`的存储是类比一维数组，先存放连续的地址空间3个一维数组数组

+ `a[0]、a[1]、a[2]`

每个一维数组里存放4个`int`型元素,所以每个一维数组是16字节。

![img](http://images2017.cnblogs.com/blog/972070/201708/972070-20170816185356006-1998682844.jpg)

由于`a`为二维数组首地址和第0行的一维数组的首地址，那么

+ `a+1`为第一行的一维数组首地址。
+ `a+2`为第二行的一维数组首地址。
+ **必须要注意的是：**现在的`a[0]`不再是一个整型元素，而是`a[0]`这一段一维数组的首地址，因为它包含四个元素。

![img](http://images2017.cnblogs.com/blog/972070/201708/972070-20170816185422693-1138306214.jpg)

如果要找到元素`a[i][j]`

+ 首先可以先找到第i行一维数组的首地址，即`a[i]`

+ 再找到此一维数组的第j个元素地址，即`a[i]+j`
+ 通过寻址，找到对应元素，即`*(a[i]+j)`

由于`a[i]`=`*(a+i)`，所以也可以通过`*(*(a+i)+j)`来找到该元素。

## 指针数组

一个存放指针类型的数组

```
int* p[n];
```

//因为`[]`优先级高，可以理解为先与`p`结成一个数组,再通过`int*`说明这是一个整型指针数组。

# 指针与字符串

字符串有两种风格：

+ C风格字符串(char 数组)
+ C++引入的string类型

---

1. C风格字符串

以`\0`风格结尾的一维字符数组

```c
//写法1
char str[7]={'D','O','U','Y','A','!','\0'};
//写法2
char str[]="DOUYA!";
```

在内存中的表示形式为

![img](https://uploadfiles.nowcoder.com/images/20210329/675098158_1617010238760/FB5C81ED3A220004B71069645F112867)

**C++编译器放在初始化数组时，自动把\0放在字符串的末尾**

2. C++引入的string类型

   ```c++
   char str[]="I am jiangdouya";
   char *str1 = "I am in Douya";
   char *str2[] = {"I am a master."};
   string str3 = "Douya loves you!";
   ```

   可以看出来

   + `str1`是指针变量，指向字符串常量。且字符串变量被程序放在常量区，不能被修改

   + `str2`是指针变量，指向字符串数组。

   + `str3`是一个字符串变量，是C++的string类型。

# 指针与函数

**函数指针**是指向函数的指针变量。

因为每一个函数都有一个入口地址，函数指针就是存储该地址。

```c
int func(int a);
int (*f)(int a);
f = &func;
```

函数指针的应用场景是**回调(callback)**

```c
//以库函数qsort排序函数为例，它的原型如下：
void qsort(void *base,//void*类型，代表原始数组
           size_t nmemb, //第二个是size_t类型，代表数据数量
           size_t size, //第三个是size_t类型，代表单个数据占用空间大小
           int(*compar)(const void *,const void *)//第四个参数是函数指针
          );
//第四个参数告诉qsort，应该使用哪个函数来比较元素，即只要我们告诉qsort比较大小的规则，它就可以帮我们对任意数据类型的数组进行排序。在库函数qsort调用我们自定义的比较函数，这就是回调的应用。

//示例
int num[100];
int cmp_int(const void* _a , const void* _b){//参数格式固定
    int* a = (int*)_a;    //强制类型转换
    int* b = (int*)_b;
    return *a - *b;　　
}
qsort(num,100,sizeof(num[0]),cmp_int); //回调
```

# 指针与结构体

```c
#include <stdio.h>

typedef struct Register {//寄存器
    int register1;
    int register2;
    int register3;
    int register4;
}tregister;  

int main(){
       int initArr[] = {1,0,1,1};
    tregister *p = (tregister *)initArr;

    printf("Register1 = %d\n", p->register1);  // 1
    printf("Register2 = %d\n", p->register2);  // 0
    printf("Register3 = %d\n", p->register3);  // 1
    printf("Register4 = %d\n", p->register4);  // 1

       return 0;
}
```

> 这里注意typedef的用法[结构体定义 typedef struct 用法详解和用法小结_会写Bug的攻城狮-CSDN博客](https://blog.csdn.net/qq_41848006/article/details/81321883)

+ 第`12`行：将指针`initArr`强制转换为`tregister`,再赋值给`tregister`数据类型指针`p`，会发现数组里的值被一一赋给了**结构体**的成员。

# 内存布局

C语言通过`malloc`和`calloc`函数、用`realloci`调整内存空间大小，C++通过`new`申请内存空间。

## 生存周期

**变量从定义开始分配存储单元，到运行结束存储单元被回收，整个过程称为变量的生存周期。**

## 变量的内存布局模型

![img](https://uploadfiles.nowcoder.com/images/20210329/675098158_1617010890605/7AFBB1602613EC52B265D7A54AD27330)

+ 代码区：存放程序执行代码的一段内存区域。
+ 静态区：`static`变量是专门存储在数据段data或BSS段，一直到完整程序结束。
  + 数据段data：存放程序中**已初始化**的全局变量和静态变量的一块内存区域。
  + BSS段：存放程序中**未初始化**的全局变量和静态变量的一块内存区域。
+ 可执行程序**在运行期间**又多出两个区域
  + **堆区**：**动态申请内存**用。堆从**低地址**到高地址增长。
  + 栈区：存放**局部变量**、**函数参数值**。栈从**高地址**到低地址增长，是一块连续区域。
+ 最后还有一个文件映射区(共享区)，位于堆、栈之间。

# 大端与小端

+ 小端：一个数据的低位字节存储在低地址
+ 大端：一个数据的高位字节存储在低地址。

## 如何判断一个系统的大小端存储模式？

定义一个字符数组，用`int *`强制转换为`char*`,对地址解引用，看第一个字符是何值。

```c
void checkCpuMode(void){
	int c = 0x12345678;
    char *p = (char*)&c;
    if(p[0]==0x12){
        printf("Big endian");
    }
    else if(p[0]==0x78)
        printf("Little endian");
    else
        printf("Uncertain");
        
}
```

# 内存分配空间

## malloc

`malloc`是C标准库`<stdlib.h>`的一个库函数，来分配`size`所需的内存空间，并返回一个**指针地址**。

```c
void *malloc(size_t size)
```

### malloc 动态申请一个数组

```c
#include <iostream>//输入输出流头文件
#include <cstdlib>//调用malloc()函数所需头文件，也可使用stdlib.h

using namespace std;

int main()
{
    int n, k;
    cin>>n;
    int *a = (int*)malloc(sizeof(int)*n);//给数组a申请存储空间，空间长度为n
    for(int i = 0; i < n; i++)
    {
        cin>>a[i];
        cout<<a[i]<<' ';
    }
    return 0;
}
```

## realloc 

`realloc`是C标准库`<stdlib.h>`的一个库函数，尝试重新调用之前调用的`malloc`和`calloc`分配`ptr`的内存空间大小`size`，**以字节为单位**

```c
void *realloc(void *ptr, size_t size)
```

比如

```
#include <stdio.h>
#include <string.h>
#include <stdlib.h>
 
int main()
{
   char *str;
 
   /* 最初的内存分配 */
   str = (char *) malloc(15);
   strcpy(str, "runoob");
   printf("String = %s,  Address = %u\n", str, str);
 
   /* 重新分配内存 */
   str = (char *) realloc(str, 25);
   strcat(str, ".com");
   printf("String = %s,  Address = %u\n", str, str);
 
   free(str);
 
   return(0);
}
```

输出结果如下

```
String = runoob,  Address = 3662685808
String = runoob.com,  Address = 3662685808
```



# 野指针和内存泄漏

野指针是指针指向的位置不可知的。(随机的、不正确的或没有明确限制的)。

空指针是一个指针值为`null`的指针，而野指针是指向一段实际的内存，但是指向哪里并不知道。

## 什么情况会产生野指针

+ **指针变量的值未被初始化**:如果指针声明在全局数据区，那么未初始化的指针缺省为空。如果指针声明在栈区，那么该指针会随意指向一个地址空间。

+ **指针指向的地址空间被free或delete**：在堆上`malloc`或`new`出来的地址空间，如果被`free`或`delete`，那么此时堆上的内存已被释放，但是如果指向该内存的指针没有人为修改，那么指针仍然会被指向堆上已经被释放的内存。

+ **指针操作超越了作用域**

  ```
  void func(){
  	int *ptr = nullptr;//定义一个指针，赋值为null
  	{
  		int a = 10;
  		ptr = &a;
  	}
  	
  	int b = *ptr;
  }
  ```

## 如何避免野指针

+ 初始化指针时置NULL
+ 申请内存后需要判空
+ 指针释放后置NULL(5-6行)
+ 使用智能指针

```c
int *p = NULL; //初始化置NULL
p = (int *)malloc(sizeof(int)*n); //申请n个int内存空间  
assert(p != NULL); //判空，防错设计
p = (int *) realloc(p, 25);//重新分配内存, p 所指向的内存块会被释放并分配一个新的内存地址
free(p);  
p = NULL; //释放后置空

int *p1 = NULL; //初始化置NULL
p1 = (int *)calloc(n, sizeof(int)); //申请n个int内存空间同时初始化为0 
assert(p1 != NULL); //判空，防错设计
free(p1);  
p1 = NULL; //释放后置空

int *p2 = NULL; //初始化置NULL
p2 = new int[n]; //申请n个int内存空间  
//assert(p2 != NULL); //在现行C++标准中，如C++11，使用new申请内存后不用判空，因为发生错误将抛出异常。
delete []p2;  
p2 = nullptr; //释放后置空  
```

第`2`行：最后的*是×乘，不是指针星

第`3`行：assert是替代if

> [断言(assert)的用法 | 菜鸟教程 (runoob.com)](https://www.runoob.com/w3cnote/c-assert.html)

## 什么情况下会发生内存泄漏

申请了一块内存空间，使用完毕后没有释放掉。

+ new和malloc申请内存后，没有delete和free释放

  ```c
  char *GetDouya(){
  	char *ptr = 			    	    (char*)malloc(sizeof(char)*100); 
      return ptr;
  }
  void Douya(void){
      	char *str = GetDouya();
      	//free(str);//这里没有及时释放内存，造成内存泄露
      	str = Null;
  }
  ```

+ 子类继承父类时，父类析构函数不是虚函数

  ```c++
  class A{
  public:
      A(){}
      ~A(){} //这里父类析构函数不是虚函数。注意B类中有申请内存的操作，而这个时候父类无法调用B类的析构函数及时释放内存，造成内存泄露，正确语法是需要加上virtual关键字。
      //virtual ~A(){};
  };
  class B:public A{
  public:
      B(){ptr = new int[10];}
      ~B(){delete []ptr;}
  private:
      char *ptr;
  };
  ```

+ windows句柄资源使用后没有释放

## 怎么避免内存泄漏

+ 养成良好的编码习惯，使用内存分配的函数，要记得释放。
+ 将分配的内存的指针以**链表**的形式管理，使用完毕后从链表中删除，程序结束可以检查链表
+ 使用智能指针

## 内存溢出是什么

内存溢出（Out Of Memory）是指应用系统中存在无法回收的内存或使用的内存过多，最终使得**程序运行要用到的内存大于系统能提供的最大内存**。此时程序无法运行，系统提示内存溢出。有时候会自动关闭软件。

## 造成内存溢出的原因

①内存泄漏的堆积最终导致内存溢出。

②需要保存多个耗用内存过大的对象或加载单个超大的对象时，其大小超过了当前剩余的可用内存空间。

## 内存溢出与内存越界的区别

+ 内存溢出：要求分配的内存超出了系统所能给予的，于是产生溢出。

+ 内存越界：向系统申请了一块内存，而在使用时超出了申请的范围，常见的是数组访问越界。

# 野指针与内存申请

## 如何有效的申请动态内存

+ 用**二级指针**

  ```c
  char *GetDouya(char **ptr){
  	*ptr = (char *)malloc(sizeof(char)*100);
  }
  void Douya(void){
  	char *p = NULL;
  	GetDouya(&p);
  	free(p);
  	p = NULL;
  }
  ```

  

+ 用函数返回指针来传递动态内存，

```c
char *GetDouya(){
	char *ptr = (char *)malloc(sizeof(char)*100);
	return ptr;
}
void Douya(void){
    char *str  = GetDouya();
    free(str);
    str = NULL;
}
```

# 内存碎片

+ 内部碎片：由于采用**固定大小**的内存分区，当一个进程不能完全使用分给它的固定内存区域时就产生了内部碎片，通常内部碎片难以完全避免；

+ 外部碎片：由于某些未分配的连续内存区域太小，以至于不能满足任意进程的内存分配请求，从而不能被进程利用的内存区域。再比如堆内存的频繁申请释放，也容易产生外部碎片。

  举例说明：

  首先，使用最原始的标记分配方法，系统需要维护一个简单的内存信息表：

  ![img](https://uploadfiles.nowcoder.com/images/20210329/675098158_1617010965519/9EB60BC8BF2B004E4DB7D1CC0D5F1D8C)

当程序申请一个长度为3的内存空间后：

![img](https://uploadfiles.nowcoder.com/images/20210329/675098158_1617010989298/C00B57557743E709B8B96933432E0DFA)

当程序再申请一个长度为2，以及长度为4的内存空间后：

![img](https://uploadfiles.nowcoder.com/images/20210329/675098158_1617011014167/7B6FBD4C592D356E087A0F1053751007)

此时，只剩1个可用空间。如果这时程序再来申请**长度大于1**的空间，就申请不了，也就是内存不够。

现在，释放掉ID=2的空间：

![img](https://uploadfiles.nowcoder.com/images/20210329/675098158_1617011038442/D642F8C3D2D6C1AB174D170D2DC8ED78)

我们发现，现在可用内存空间为3，但是，这3个空闲空间，并不是连续的。所以，如果程序现在申请长度为3的内存空间，同样会申请不了，会出现内存不够。业界把这种情况，称之为【内存碎片】，这里是**外部碎片**。

## 怎么解决内存碎片

+ 段页式管理

  将内存空间->段->页->固定大小的内存地址

+ 内存池

  我们在使用内存对象之前，先申请分配一定数量的内存块留作备用。当有新的内存需求时，就从内存池中分出一部分内存块，若内存块不够再继续申请新的内存。当不需要此内存时，重新将此内存放入预分配的内存块中，以待下次利用。这样合理的分配回收内存使得内存分配效率得到提升。

# 码制

## ASCII码

计算机所有的字符均以ASCII码的形式来存储，其实是0-127共128个16进制数字。

ASCII码是单字节编码系统，0-127。

`A` = 0x41;

`a` = 0x61

`0`= 0x30

## 8421 BCD码

用**4位**二进制来表示1位十进制0-9。

# 手撕代码-函数

## strcpy-字符串拷贝

```
char * strcpy(char * des, const char * src){
    assert( (des != NULL) && (src != NULL));
    char * p = des;
    while( (*p++ = *src++) != '\0' );
    return des;
}
```

## memcy-内存拷贝，内存重叠不理想

```C
void * memcpy(void * des, const void * src, size_t size){
    assert( (des != NULL) && (src != NULL));
    char * tmpd = (char *)des;
    char * tmps = (char *)src;
    while(size-- > 0)
        *tmpd++ = *tmps++;
    return des;
}
// des-目的地址
//src-源地址
//size--拷贝字节数
```

## memmove-内存拷贝，理想效果

```c
void *memmove(void *dest, const void *src, size_t count)  
{  
    if(dest == NULL || src == NULL || count <= 0)  return NULL;  
    if(dest < src)  
    {  
        char *d = (char *)dest;  
        char *s = (char *)src;  
        while (count--)  
        {  
            *d++ = *s++;  
        }  
    }  
    else  
    {  
        char *d = (char *)dest + count;  
        char *s = (char *)src + count;  
        while (count--)  
        {  
            *--d = *--s;  
        }  
    }      
    return dest;  
}  
```



## strcmp--字符串长度比较

```
//功能：str1 > str2 返回1，反之返回-1，相等返回0
int strcmp(const char * str1, const char * str2){
    assert( (str1 != NULL) && (str2 != NULL));
    int ret = 0;
    while( *str1 != '\0' && *str2 != '\0' && *str1 == *str2 ){
        str1++;
        str2++;
    }
    if(  *str1 != '\0' && *str2 == '\0') return 1;//str1比str2长
    else if( *str1 == '\0' && *str2 != '\0') return -1;//str1比str2短
    else if( *str1 > *str2) return 1;
    else if( *str1 < *str2) return -1;
    return 0;//如果前面都没return，那就说明都一样
}
```

## memcmp-内存比较

```
//功能：比较内存值是否相同
int memcmp(const void *buffer1,const void *buffer2, size_t size){
    if (!size)  return 0;
    char * m1 = (char *)buffer1;
    char * m2 = (char *)buffer2;
    while ( --size && *m1 == *m2){
        m1++;
        m2++;
    }
    if(size == 0) return 0;
    else if(*m1 > *m2) return 1;
    else return -1;
}
```

## strcat-字符串拼接

```
char * strcat( char * des, const char * src){
    assert( (str1 != NULL) && (str2 != NULL));
    char * p = des;
    while(*p != '\0') p++;//找到des的结尾
    while( (*p++ = *src++) != '\0' );//开始往后复制，不用管'\0'，会自动添加
    return des;
}
```

# 手撕代码-刷题函数

## 翻转一个变量的8位

```c
unsigned char bit_reverse(unsigned char input)  
{  
    unsigned char result = 0;  
    int bit = 8;  
    while(bit--)  
    {  
        result |= ((input & 0x01) << bit);  
        input >>= 1;  
    }  
    return result;  
}  
```

## 字符串倒序

```c++
#include<string.h>  
void inverted_order(char *p)  
{  
    char *s1, *s2, tem;  
    s1 = p;  
    s2 = s1 + strlen(p) - 1;  
    while(s1 < s2)  
    {  
        tem = *s1;  
        *s1 = *s2;  
        *s2 = tem;  
        s1++;  
        s2--;  
    }  
} 
//双指针法，一个指向头，一个指向尾，依次向对方靠近，相互交换指向的内容，直到相遇
```

## 判断是不是质数

+ 质数是只有1和它本身因数的自然数

+ 一个大于1的自然数不是质数就是合数，因此可以将问题转换为判断合数。

+ 合数一定可以由两个自然数相乘得到，一个小于或等于它的平方根（大于1），另一个大于或等于它的平方根。因此可以判断**“2 ---输入参数的平方根”的范围中是否有能被输入参数整除的数**，若有则该数是合数，若没有则该数是质数。

  ```
  int IsPrime (unsigned int p)  
  {  
      unsigned int i;  
      if(p <= 1)  
      {  
          printf("请输入大于1的自然数。\n");  
          return -1;  
      }     
      for(i = 2; i <= sqrt(p); i++)  
      {  
          if(p % i == 0)  
          {  
              printf("该数不是质数。\n");  // 是合数  
              return 0;  
          }  
      }  
      printf("该数是质数。\n");  
      return 0;  
  }  
  ```

## 对一个整型数进行大小端存储模式转换

```c
int endian_convert(int input)  
{  
    int result = 0;  
    int size = sizeof(input);  
    while(size--)  
    {  
        result |= ((input & 0xFF) << (size * 8));  
        input >>= 8;  
    }  
    return result;  
}  
```

## 判断是不是回文数

+ 只考虑字符和数字字符
+ 忽略字母的大小写
+ 如Aba，12321

```
bool isPalindrome(char * s)  
{  
    char *left = s, *right = s + strlen(s) - 1;  
  
    if(strlen(s) == 0) return true;  
      
    while(left < right)  
    {  
        while(!((*left >= 'a' && *left <= 'z') || (*left >= 'A' && *left <= 'Z') || (*left >= '0' && *left <= '9')) && left < right)  // 找到字母或数字  
            left++;  
        while(!((*right >= 'a' && *right <= 'z') || (*right >= 'A' && *right <= 'Z') || (*right >= '0' && *right <= '9')) && right > left)  // 找到字母或数字  
            right--;  
          
        if(left < right)  
        {  
            if((*left >= 'A' && *left <= 'Z'))  // 若为大写，则转为小写  
                *left = *left + 32;  
            if((*right >= 'A' && *right <= 'Z'))  // 若为大写，则转为小写  
                *right = *right + 32;  
  
            if(*left == *right)  // 比较  
            {  
                left++;  
                right--;  
            }     
            else  
                return false;  
        }  
        else  
            return true;  
    }  
      
    return true;  
}  
```



# 手撕代码-宏

## 某一位置1

```
#define setbit(x,y) x|=(1<<y) //将X的第Y位置1
```

## 某一位置0

```
#define clrbit(x,y) x&=!(1<<y) //将X的第Y位清0
```

## 交换



