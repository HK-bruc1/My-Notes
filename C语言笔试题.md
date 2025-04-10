```
#define OK 0 
#define ERR 1
#define ERROR (-1)
#define BUFFER_SIZE 256
Char *GetMemory(unsigned long ulSize) 
{
    char *pcBuf=NULL;
    pcBuf = (char *)malloc(ulSize);
    if(NULL == pcBuf) 
    {
    	retum ERROR;
    }
 	return pcBuf;
}
 void Test(void)
{
    char *pszBuf= NULL;
    pszBuf= GetMemory(BUFFER_SIZE);
    if(NULL != pszBuf)
	{
        strcpy(pszBuf,"Hello World!\r\n");
        printf(pszBuf);
        free(pszBuf);
	}
	return;
}
```

如下描述正确的是:
A、pszBuf 指向的内存不能超过255
B、GetMemory 函数的异常分支返回了-1 ，是一个非法地址 
C、GetMemory 中异常分支没有释放空间:
D、pcBuf 为局部指针，指向的内存将在 GetMemory 被调用后释放.

```c
pszBuf 指向的内存不能超过255

错误。 BUFFER_SIZE 被定义为 256。GetMemory 函数使用 BUFFER_SIZE 作为 malloc 的参数，尝试分配 256 字节的内存。因此，pszBuf 可以指向最多 256 字节的内存，而不是 255 字节。
    
B、GetMemory 函数的异常分支返回了-1 ，是一个非法地址
正确。 在 GetMemory 函数中，如果 malloc(ulSize) 分配内存失败（返回 NULL），代码会进入 if(NULL == pcBuf) 分支，并执行 return ERROR;。
ERROR 被宏定义为 (-1)。
函数 GetMemory 的返回值类型声明为 char *（字符指针）。
当函数尝试返回 ERROR (-1) 时，编译器会将整数 -1 隐式转换为 char * 类型。
整数 -1 转换为指针类型后，通常被认为是一个无效或非法的内存地址。 在大多数系统架构中，-1 或接近 -1 的负数指针值不太可能对应于进程可以合法访问的内存区域。 因此，返回 -1 作为指针在内存分配失败的情况下，确实代表返回了一个非法地址。
    
C、GetMemory 中异常分支没有释放空间
不完全正确，但容易产生误导。 在 GetMemory 函数的异常分支（即 malloc 返回 NULL 的情况），确实没有释放内存的操作。
然而，关键在于 此时根本没有成功分配到内存。 当 malloc 返回 NULL 时，意味着内存分配请求失败，pcBuf 始终为 NULL，没有指向任何已分配的内存。
因此，在这种情况下，不存在 "应该释放但没有释放" 的内存泄漏问题。 因为根本没有分配成功，所以也就没有内存需要释放。
选项 C 的描述是字面上正确的，异常分支没有释放操作，但容易让人误解为存在内存泄漏，而实际情况是内存分配就没成功。
    
D、pcBuf 为局部指针，指向的内存将在 GetMemory 被调用后释放.
错误。 pcBuf 是 GetMemory 函数的局部变量，其生命周期仅限于 GetMemory 函数内部。
但是，pcBuf 指向的内存是通过 malloc 函数动态分配的。 通过 malloc 分配的内存在堆 (heap) 上，必须通过 free 函数显式地释放，而不会因为 GetMemory 函数的结束而自动释放。
在 Test 函数中，可以看到在调用 GetMemory 后，确实通过 free(pszBuf); 来释放了 pszBuf 指向的内存，这验证了内存需要手动释放。
```







考虑函数 void foo(int *a, int *b) { int *temp; temp = a; a = b; b = temp; }， 哪种说法正确？ 

A. 函数交换 a 和 b 的值 

B. 函数不改变 a 和 b 的值 

C. 函数产生编译错误 

D. 函数交换 a 和 b 的地址

```
关键点：
在这个函数中，只修改了函数内部的指针变量 a 和 b，并没有改变外部传入的 a 和 b 的实际值（即原来指向的地址）。
由于 a 和 b 是通过指针传递给函数的（即传递的是指针的副本），所以在函数内部交换的是 a 和 b 指向的地址，而外部的 a 和 b 并未改变。

为什么其他选项不对：
A. 函数交换 a 和 b 的值：错误，a 和 b 是指针，交换的是它们所指向的地址，而不是它们的值。
C. 函数产生编译错误：错误，代码语法没有问题，是合法的。
D. 函数交换 a 和 b 的地址：不完全准确，函数内部交换的是 a 和 b 的指向关系（即地址的副本），但外部的 a 和 b 地址不变。
```

- 交换两个变量的值

```
void swap(int *a, int *b) { 
    int temp = *a; 
    *a = *b; 
    *b = temp; 
}
核心要点：

这个函数通过指针正确地交换了两个整数变量的值
使用了解引用操作符（*a、*b）来访问并修改指针所指向的内存位置中的值
调用方式：swap(&x, &y)
结果：x和y的值被成功交换
```

- 错误的指针交换尝试
- **这和直接传入两个整数，直接交换不改变外部值一个道理**
  - 改变的值副本值。
  - 通过地址修改值还差不多。

```
void foo(int *a, int *b) { 
    int *temp; 
    temp = a; 
    a = b; 
    b = temp; 
}
核心要点：

这个函数看似要交换两个变量的值，但实际上不起作用
函数只交换了函数参数a和b这两个局部指针变量的值
关键错误：没有使用解引用操作符修改指针所指向的内存内容
由于函数参数是按值传递的，函数内部的指针交换不会影响调用者的原始指针
调用foo(&x, &y)后，x和y的值不会改变

最本质的区别

第一个函数修改了指针所指向的实际内存内容（即变量的值）
第二个函数只修改了函数内的局部指针副本，没有触及到原始内存内容
要交换变量值，必须使用解引用操作符（*）来访问并修改指针指向的实际内存

这个区别展示了C语言中指针操作和参数传递机制的重要概念。
```





若定义了 char ch[]={″abc\0def″｝ ， p=ch;则执行 printf(″%c″,*p+4);语句的输出结果是

```
题目中定义了一个字符数组 char ch[] = {"abc\0def"};。在 C 语言中，这种初始化方式等同于 char ch[] = "abc\0def";，因为花括号包裹一个字符串字面量时，效果与直接赋值相同。字符串字面量 "abc\0def" 包含以下字符：
ch[0] = 'a'

ch[1] = 'b'

ch[2] = 'c'

ch[3] = '\0'（空字符，字符串终止符）

ch[4] = 'd'

ch[5] = 'e'

ch[6] = 'f'

由于初始化时没有指定数组长度，数组 ch 的长度由字符串字面量确定。"abc\0def" 总共有 7 个字符，因此 ch 是一个长度为 7 的字符数组，内容为 'a', 'b', 'c', '\0', 'd', 'e', 'f'。注意，这里没有额外的隐式 \0，因为字符串字面量中已经显式包含了一个 \0，并且之后的 'd', 'e', 'f' 是数组的一部分。

题目中定义 p = ch;。在 C 语言中，数组名 ch 表示数组首元素的地址，因此 p 是一个指向 ch[0] 的字符指针，即指向字符 'a'。

*p 表示解引用指针 p，获取它所指向的元素的值。

因为 p 指向 ch[0]，所以 *p 的值是 'a'。

在 C 语言中，字符 'a' 会被视为其 ASCII 码值，'a' 的 ASCII 码是 97（十进制）。

表达式 *p + 4 就是在 'a' 的 ASCII 值 97 上加 4，即：
97 + 4 = 101

因此，*p + 4 的结果是整数 101。
所以，printf("%c", 101); 会输出字符 'e'。
```

- 大写字母是ASCII小的那个：（A）65
- 小写字母是ASCII大的那个：（a）97
- 两者相差32
- 对于任意数字字符 '0' 到 '9'，通过减去 48（即 '0' 的 ASCII 码），可以得到它代表的十进制整数：
  - '0' - 48 = 0，
  - '1' - 48 = 1，
  - '9' - 48 = 9。
- '0' 的ASCII码是 48 





在一个32位机器和编译器上，我们需要确定以下C代码的打印输出：

```c
int len = sizeof("1234567");
char *p = "1234567";
printf("%d, %d,%d", len, sizeof(p), sizeof(*p));


第一步：计算 len = sizeof("1234567")
在C语言中，"1234567" 是一个字符串字面量，它是一个字符数组，包含字符串中的每个字符以及末尾的空终止符 \0。
字符串 "1234567" 包含以下字符：'1'、'2'、'3'、'4'、'5'、'6'、'7'，加上末尾的 \0。
总共有 7 个字符加上 1 个空终止符，共 8 个字符。
每个字符占用 1 个字节，因此 sizeof("1234567") 的值是 8。
int len 将这个值赋给 len，所以 len = 8。
    
第二步：计算 sizeof(p)
char *p = "1234567"; 定义了一个字符指针 p，它指向字符串字面量 "1234567" 的起始地址。
sizeof(p) 计算的是指针变量 p 本身的大小，而不是它所指向的字符串的大小。
在32位机器上，指针的大小通常是 32 位，即 4 个字节。
因此，sizeof(p) = 4。
    
第三步：计算 sizeof(*p) 是一个字符不是整数，看清楚数据类型
*p 表示指针 p 解引用后得到的值，即 p 指向的第一个字符 '1'。
*p 是一个 char 类型的值。
在C语言中，char 的大小始终是 1 个字节（这是标准定义）。
因此，sizeof(*p) = 1。
```







**要对绝对地址 0x8000000 赋值，我们可以用(unsigned int*)0x8000000= 5:那么要是想让程序跳转到绝对地址是 0x8000000 去执行，应该怎么做?**

要让程序跳转到绝对地址 0x8000000 去执行，你需要将该地址强制转换为一个函数指针，然后调用这个函数指针。假设该地址处的代码是一个不接受任何参数且不返回任何值的函数，你可以这样做：

```c
typedef void (*funcPtr)(void); // 定义一个函数指针类型

int main() {
    funcPtr jump_address = (funcPtr)0x8000000; // 将绝对地址强制转换为函数指针

    jump_address(); // 调用该地址处的函数

    // 注意：执行到这里之后的代码可能不会被执行，
    // 因为程序的控制权已经转移到 0x8000000 地址处的代码。

    return 0;
}
```

**`typedef void (\*funcPtr)(void);`**: 这一行定义了一个新的类型 `funcPtr`，它表示一个指向函数的指针，该函数不接受任何参数 (`void`) 并且不返回任何值 (`void`)。你需要根据实际跳转地址处的代码的函数签名来调整这个定义。如果目标地址的代码是一个接受整型参数并返回整型值的函数，那么 `funcPtr` 的定义就应该是 `typedef int (*funcPtr)(int);`。

**`funcPtr jump_address = (funcPtr)0x8000000;`**: 这一行做了两个关键的操作：

- `(funcPtr)0x8000000`: 将十六进制地址 `0x8000000` 强制转换为 `funcPtr` 类型，也就是告诉编译器将这个内存地址看作是一个函数的入口地址。
- `funcPtr jump_address = ...`: 将转换后的函数指针赋值给一个名为 `jump_address` 的变量。

**`jump_address();`**: 这一行通过函数指针 `jump_address` 来调用位于地址 `0x8000000` 的函数。程序的执行流程会跳转到该地址处的代码开始执行。





想要让程序跳转到绝对地址0x10000去执行，该怎么做？

```c
定义一个函数指针，指向地址 0x10000。
调用该函数指针，使程序跳转到该地址执行。
#include <stdio.h>

int main() {
    void (*func)() = (void (*)())0x10000; // 将函数指针设置为 0x10000
    func();                               // 调用指针，跳转到该地址
    return 0;
}

void (*func)()
这是一个函数指针的声明。
让我们分解一下：
void：表示函数的返回值类型是 void，即没有返回值。
()：表示函数不接受任何参数。
*func：表示 func 是一个指针，指向某种类型的对象。
(*func)()：整体表示 func 是一个指向函数的指针，这个函数没有参数且返回 void。
简单来说，func 是一个变量，它可以存储某个函数的地址，而这个函数的签名是 void 函数名()。
    
2. (void (*)())0x10000
这部分是将地址 0x10000 转换为一个函数指针。
分解：
0x10000：一个十六进制数，表示内存中的某个绝对地址（这里是 65536）。
(void (*)())：这是一个类型转换（cast），将 0x10000 强制转换为 void (*)() 类型。
void (*)() 是函数指针类型，表示指向一个返回 void 且无参数的函数。
(void (*)())0x10000：将数值 0x10000 显式地转换为一个函数指针，表示地址 0x10000 是一个函数的入口地址。
换句话说，这告诉编译器：“把地址 0x10000 看作是一个函数的地址，这个函数没有参数，返回值为 void。”
3. void (*func)() = (void (*)())0x10000;
将上面转换后的函数指针赋值给变量 func。
结果是：func 现在指向内存地址 0x10000，并且可以像调用函数一样使用它。
后续通过 func() 调用时，程序会跳转到地址 0x10000 开始执行那里的代码。
```





数组定义为：int a[4];表达式(  )是错误的

**这就是为什么很多都赋值给指针，再进行自增等操作。**

**或者a+1偏移。但是不能赋值**

```c
A、*a

B、a[0]

C、a

D、a++

A、*a:
a 在数组上下文中，代表数组的首地址，也就是数组第一个元素 a[0] 的地址。
*a 是指针解引用操作符。它作用于指针 a，取出 a 指向的内存地址中存储的值。
因此，*a 等价于访问数组的第一个元素 a[0] 的值。这是一个合法的表达式。
    
B、a[0]:
a[0] 是标准的数组元素访问方式，表示访问数组 a 的第一个元素。
在C语言中，数组的索引从 0 开始，a[0] 就是访问第一个元素，这是完全合法的表达式。
    
C、a:
单独使用 a 时，在大多数情况下（除了 sizeof 和 & 运算符），a 会被隐式转换为指向数组首元素的指针，类型为 int *。
虽然你不能直接修改 a 的值（例如赋值 a = ...），但是 a 本身可以作为一个表达式使用，例如可以传递给函数，进行指针运算等。
因此，a 本身作为一个表达式在很多语境下是合法的。
    
D、a++:
a 在这里被视为数组的首地址（一个指针）。
++ 是自增运算符。 a++ 试图将指针 a 的值（即数组的首地址）增加 1。
关键问题在于：数组名 a 在C语言中被视为常量指针，它存储的是数组的首地址，这个地址是固定的，不能被修改。
尝试使用 a++ 这样的操作，就如同试图修改一个常量的值，这是不允许的。编译器会报错，提示 a 不是一个可修改的左值 (lvalue)。
```





**指针四要素是哪些？请用例子说明**

```c
C语言指针的四大要素分别是：

指针类型
指针名
指针所指向的内存地址
指针运算符
    
#include <stdio.h>

int main() {
  int number = 100;          // 定义一个整型变量 number，并初始化为 100
  int *p_number;          // 声明一个整型指针变量 p_number  (指针类型: int *, 指针名: p_number)

  p_number = &number;       // 将变量 number 的内存地址赋值给指针 p_number (指针所指向的内存地址)

  printf("指针 p_number 指向的值: %d\n", *p_number); // 使用解引用运算符 * 访问指针指向的值 (指针运算符: *)

  return 0;
}
```



## 数据结构

在有限的内存中， 哪种数据结构更节省内存？ A. 链表 B. 数组 C. 二叉树 D. 哈希表

```
A. 链表：链表是一种动态数据结构，每个节点除了存储数据外，还需要存储指向下一个节点的指针。每个节点需要额外的内存空间来存储指针（通常是 4 或 8 字节，取决于系统架构），因此链表的内存开销相对较大。

B. 数组：数组是一种静态数据结构，数据是连续存储的，且没有额外的指针或链接。数组的大小在创建时固定，因此内存分配是连续的，没有额外的内存开销。只需要存储数据本身，因此它是最节省内存的。

C. 二叉树：二叉树是一种树形结构，每个节点除了存储数据外，还需要存储指向左右子节点的指针。这意味着每个节点有两个指针（左子节点和右子节点），因此内存开销相对较大，尤其是当树的深度较大时。

D. 哈希表：哈希表通过数组和链表（或其他冲突解决方式）结合的方式来存储数据。它需要额外的空间来存储哈希表数组的槽位以及可能发生冲突的链表或树等结构，因此内存开销较大。
```

在双向循环链表中， 在 p 所指的结点之后插入 s 指针所指的结点， 其操作是（） 。

A.p->next = s; s->prior = p; (p->next)->prior = s; s->next = p->next; 

B.s->prior = p; s->next = p->next; p->next = s; p->next->prior = s; 

C.p->next = s; p->next->prior = s; s->prior = p; s->next = p->next; 

D.s->prior = p; s->next = p->next; p->next->prior = s; p->next = s;

```
先建立新节点的连接，再断开原有连接
这个原则适用于大多数链表操作：

先连接新节点：让新节点正确指向它将要连接的节点
再修改原有连接：调整原有节点的指针
3步口诀（单向链表）
"先右后左，最后中"

先处理右边连接：s->next = p->next
再处理左边连接：p->next = s

4步口诀（双向链表）
"先两头，后中间"

处理新节点的两个指针：s->prior = p; s->next = p->next;
处理两侧节点指向新节点的指针：p->next->prior = s; p->next = s;

安全顺序法则
确保在每一步操作后，链表结构仍然是完整的，或者至少能够通过已保存的指针恢复。这就是为什么我们先设置s的指针，因为这些操作不会影响现有链表结构。
```





在嵌入式软件开发中，关于以下数据结构的描述，下列选项正确的是（  ）（单项或多项选择题 5分） 

A、链表：由于嵌入式系统中的内存资源有限，链表可以提供动态的数据存储，减少内存的浪费。 

B、数组：与链表相比，数组提供动态的大小调整，适用于数据大小频繁变化的场景。 

C、队列：在嵌入式系统中，队列常用于任务调度、事件处理或数据流管理。 

D、哈希表：由于哈希表的大小和复杂性，它们在嵌入式系统中很少使用。 

E、栈：在函数调用和局部变量存储中，栈结构被广泛使用。它还常用于实现某些算法，如深度优先搜索。

```c
A、链表
链表是一种动态数据结构，可以根据需要在运行时分配内存，虽然每个节点有额外的指针开销，但在需要动态管理数据时有助于减少固定分配造成的内存浪费。

B、数组
数组在C语言中通常是固定大小的，不具备动态大小调整的能力。因此，描述数组“提供动态的大小调整”是不正确的。

C、队列
队列在嵌入式系统中常用于任务调度、事件处理和数据流管理，是一种非常常见且实用的数据结构。

D、哈希表
"由于哈希表通常涉及较复杂的数据结构和较大的内存需求"，在资源受限的嵌入式系统中不常用，描述符合实际情况。

E、栈
栈在嵌入式系统中用于函数调用、局部变量存储，"并在某些算法（如深度优先搜索）中广泛应用，因此描述正确。
```







## 宏定义

使用一个宏FIND求一个结构体struct里某个变量相对struct的偏移量，例如： typedef struct test{  int a;  int b; } t_test 请实现FIND(t_test, b)

```c
#define FIND(type, member) ((size_t) &((type *)0)->member)
这个宏的工作原理：

(type *)0将0强制转换为type类型的指针
->member访问该"零指针"的成员
&获取该成员的地址
(size_t)将地址转换为整数类型，即偏移量
    
在C/C++中的带参宏，括号中的参数名不会被直接替换，
而是在宏展开时，宏定义体中出现的参数名会被替换为调用宏时提供的实际参数。
当你使用FIND(t_test, b)调用这个宏时，预处理器会：
将宏定义体中的type替换为t_test
将宏定义体中的member替换为b
替换后的结果是：
((size_t) &((t_test *)0)->b)
```

假设我宏定义体中有参数，但是要替换的文本中，我不引用参数呢？会不会出错？

```
如果你在宏定义体中不引用传入的参数，不会导致语法错误或预处理错误，但这些未使用的参数实际上就被忽略了。
例如，如果你定义了这样的宏：
#define UNUSED_PARAM(x, y) 100

然后调用 UNUSED_PARAM(10, 20)，预处理器会简单地将其替换为 100，而参数 x 和 y 虽然被传入，但并未在展开中使用。
这是完全合法的，只是这样的宏设计可能会让人困惑，因为它接受参数却不使用它们。实际上，有些编译器可能会对宏中未使用的参数发出警告，类似于函数中未使用的参数警告。
在实际编程中，这种不使用参数的宏通常没有太大意义，除非你是在做某种特殊的预处理技巧或者是为了保持接口的一致性。
```





写一个标准宏，这个宏输入两个参数并返回较小的一个；

```c
#define min(int a,int b)     (a>b? b:a)

#define MIN(a, b) ((a) < (b) ? (a) : (b))

#define 宏是简单的文本替换，它不关心类型，只做“机械替换”。
"C语言的宏是由 预处理器 处理的，而不是编译器。
```



## C++与C

写出C++源代码调用C函数void func(void)的函数声明。

```c
在C++中调用C函数需要使用extern "C"语法来告诉C++编译器这个函数使用C语言的命名约定。以下是正确的声明方式：
#ifdef __cplusplus
extern "C" {
#endif

void func(void);

#ifdef __cplusplus
}
#endif

如果你是在C++源文件中包含这个声明，你可以简化为：
extern "C" void func(void);

C++源文件中完全可以写函数声明。在C++中，你可以在源文件中定义和声明函数，就像在头文件中一样。
通常，函数声明放在头文件(.h 或 .hpp)中是为了更好的组织代码和实现模块化，但在技术上，你可以在.cpp文件的任何地方声明函数。
// 在C++源文件中
extern "C" void func(void);  // 在源文件中声明C函数

int main() {
    func();  // 调用C函数
    return 0;
}
```

## 位运算

UART的配置寄存器（32位）地址为0x10000000，其中31~11为A区域，10~2为B区域，1~0为C区域。写出将B域置为0x1FF的代码片断

```
// UART寄存器基地址
#define UART_CONFIG_REG *((volatile unsigned int*)0x10000000)

void set_B_region() {
    unsigned int value;
    
    // 读取当前寄存器值
    value = UART_CONFIG_REG;
    
    // 清除B区域(位10~2)本来就是0的话，可以不用清零的。
    value &= ~(0x1FF << 2);
    
    // 设置B区域为0x1FF
    value |= (0x1FF << 2);
    
    // 写回寄存器
    UART_CONFIG_REG = value;
}
```

在公有派生类的成员函数不能直接访问基类中继承来的某个成员，则该成员一定是基类中 的________。

 A. 私有成员   B. 公有成员   C. 保护成员   D. 保护成员或私有成员

```c
在C++的公有继承中，基类成员的访问属性会影响派生类如何访问这些成员。  以下是不同访问属性的成员在公有派生类中的访问情况：

公有成员 (public members):

基类的公有成员在公有派生类中仍然是公有的。
这意味着公有派生类的成员函数 可以 直接访问基类继承来的公有成员。
公有派生类的对象也可以通过对象名直接访问这些公有成员。
保护成员 (protected members):

基类的保护成员在公有派生类中是保护的。
这意味着公有派生类的成员函数 可以 直接访问基类继承来的保护成员。
保护成员的主要目的是允许派生类访问，但不允许类外直接访问。
私有成员 (private members):

基类的私有成员在任何派生类中都是不可访问的，包括公有派生类。
公有派生类的成员函数 不能 直接访问基类继承来的私有成员。
私有成员只能在基类自身的成员函数或友元函数中访问。
```



## 结构体

32 位系统下设有以下说明和定义:

则语句 `printf(“%d”,sizeof(struct data)+sizeof(max));` 的执行结果是:

```c
typedef union
{
    int i;
    void *ptr;
    char c;
} DATE;

struct data
{
    int cat;
    DATE COW;
    double dog;
    char a:1;
    char a:2;
    char a:3;
    char a:4;
} too;
DATE max;
```

共用体 `DATE` 的大小取决于其最大的成员。成员包括 `int` (4 字节), `void *ptr` (4 字节), `char` (1 字节)。因此，`sizeof(DATE)` = 4 字节。

由于 `max` 是 `DATE` 类型的变量，所以 `sizeof(max)` = `sizeof(DATE)` = 4 字节。

结构体 `data` 的成员及其占用空间和对齐如下：

- **`int cat;`**:
  - 对齐要求：4 字节
  - 偏移地址：0
  - 占用空间：0 ~ 3
- **`DATE COW;`**:
  - 对齐要求：4 字节
  - 偏移地址：从地址 4 开始偏移
  - 占用空间：4 ~ 7
- **`double dog;`**:
  - 对齐要求：8 字节
  - 偏移地址：从地址 8 开始偏移
  - 占用空间：8 ~ 15
- **位域：**
  - `char a:1;` 占用 1 位。
  - `char a:2;` 占用 2 位。
  - `char a:3;` 占用 3 位。
  - `char a:4;` 占用 4 位。
  - 这些位域会连续存储在字节中。
  - 第一个字节存储 `a:1` (1 位), `a:2` (2 位), `a:3` (3 位)，共占用 6 位。
  - 第二个字节存储 `a:4` (4 位)。
  - 因此，位域总共占用 2 个字节。
  - 对齐要求：1 字节 (因为位域的基础类型是 `char`)
  - 偏移地址：从地址 16 开始偏移
  - 占用空间：16 ~ 17

结构体当前占用的最大字节是 17 (从 0 开始计数，所以是 18 个字节)。结构体的整体对齐要求是其成员中最大的对齐要求，即 `double` 的 8 字节。因此，结构体的大小必须是 8 的倍数。

当前大小为 18 字节，需要填充到下一个 8 的倍数，即 24 字节。

所以，`sizeof(struct data)` = 24 字节。

**3. 计算 `sizeof(struct data) + sizeof(max)`：**

`sizeof(struct data)` = 24 字节 `sizeof(max)` = 4 字节

`sizeof(struct data) + sizeof(max)` = 24 + 4 = 28 字节。





在32位系统中，结构体typedef struct memory{char ch; short s1; int (*p)(int);short s2}Mem;,代码sizeof（Mem）的值是（  ）。（单项或多项选择题 5分）

A、9  B、10

C、12  D、16

```
typedef struct memory {
    char ch;
    short s1;
    int (*p)(int);
    short s2;
} Mem;

char ch：占 1 字节，起始于偏移 0。

short s1：占 2 字节，对齐要求为 2。由于当前偏移为 1，不满足对齐要求，因此在 ch 后会填充 1 字节，s1 从偏移 2 开始，占用偏移 2-3。

int (*p)(int)：在32位系统中，指针占 4 字节，对齐要求为 4。此时下一个偏移为 4，已经满足要求，因此 p 占用偏移 4-7。

short s2：占 2 字节，对齐要求为 2。接下来偏移为 8，满足要求，s2 占用偏移 8-9。

目前累计使用了 10 字节（偏移 0~9），但结构体整体的大小还需要是其最大对齐要求（这里是 4 字节）的倍数，因此在结构体尾部还会填充 2 个字节，使得最终大小为 12 字节。
```





定义一个结构体类型，包含如下成员： 

1.一个指向函数的指针，该函数有一个整型参数并返回一个整型数；

2.一个整型变量；

3.一个最多保存20个字符的变量；

```c
typedef struct {
    int (*func_ptr)(int);  // 指向函数的指针，该函数有一个整型参数并返回一个整型数
    int num;               // 一个整型变量
    char str[20];          // 一个最多保存20个字符的变量
} MyStruct;

"要声明指向函数的指针，需要在变量名后面加上参数列表
```



给定结构

```c
struct A 
{
    char t[4]; 
    char k[4];
    unsigned short i; 
    unsigned long m;
)
```

- 问sizeof(A)=

- **忘记算具体成员大小，只算了类型大小**

```
从地址0开始：
第一个成员占用4个字节，对齐要求为1的倍数，当前偏移位置为1，满足对齐要求，占用0~3
第二个成员占用4个字节，对齐要求为1的倍数，当前偏移位置为4，满足对齐要求，占用4~7
第三个成员占用2个字节，对齐要求为2的倍数，当前偏移位置为8，满足对齐要求，占用8~9
第四个成员占用4个字节，对齐要求为4的倍数，当前偏移位置为10，不满足对齐要求，填充2个字节，从12开始偏移，占用12~15
结构体整体对齐要求为4的倍数，当前偏移位置为16，满足要求。占用16个字节。
```



输出结果是什么？请解释一下。(10分)

```c
#include<stdio.h>
struct data
{   char c;
    int  i;
    float f;
    double d;
};
void main()
{   struct data ST;
    printf("%d\n",sizeof(ST));
}

起始地址为0；
第一个成员，占用一个字节，即占用地址0
第二个成员，占用4个字节，对齐要求为4的倍数，当前偏移位置为1，所以填充3个字节，从地址4开始偏移，占用4~7
第三个成员，占用4个字节，对其要求为4的倍数，从8开始偏移，占用8~11
第四个成员，占用8个字节，对齐要求为8个倍数，当前偏移位置为12，填充4个字节，从16开始偏移，占用16~23
整体对齐的要求为8的倍数，当前偏移位置为24，满足对齐要求。占用24个字节
```



## volatile

c语言嵌入式软件开发中，关于volatile关键字，下列选项错误的是（  ）（单项或多项选择题 5分） 

A、volatile 告诉编译器不要优化与该变量有关的代码，因为它可能在程序外部意外地改变。 

B、volatile 修饰的变量可以确保变量的原子性操作。 

C、对于硬件寄存器映射到的内存地址，通常使用 volatile 修饰符来声明。 

D、使用 volatile 修饰的全局变量可以防止编译器将其缓存到寄存器中。

```c
A 选项： 正确。使用 volatile 告诉编译器该变量可能会在程序外部发生变化，所以不要对其进行优化。
B 选项： 错误。volatile 并不能保证对变量操作的原子性。它只是确保每次访问该变量时都从内存中读取，而不会使用寄存器中的缓存。原子性通常需要借助其他同步机制或硬件支持来实现。
C 选项： 正确。硬件寄存器通常映射到特定的内存地址，为了确保对寄存器的每次访问都是实时有效的，会用 volatile 修饰。
D 选项： 正确。volatile 修饰的全局变量，编译器不会将其缓存到寄存器中，而是每次直接从内存中读写。
```

## 内存优化

以下叙述中错误的是

A)书写风格良好的程序执行效率高

B)书写风格良好的程序易读性好

C)C 程序可以在一行上写多条语句

D)C 程序允许将一条语句分写在多行上

```c
A) 书写风格良好的程序执行效率高
这条叙述是错误的。程序的执行效率主要取决于所选择的算法、数据结构以及编译器的优化等因素，而良好的书写风格（如适当的缩进、清晰的命名、添加注释等）主要目的是提高程序的可读性和可维护性，并不直接影响程序的执行效率。当然，好的风格可能间接促使程序员选择更优的算法，但风格本身与效率没有必然的直接关系。

B) 书写风格良好的程序易读性好
这条叙述是正确的。良好的书写风格能够使代码结构清晰、逻辑易懂，方便他人阅读和理解，也方便程序员自己日后维护和修改。

C) C 程序可以在一行上写多条语句
这条叙述是正确的。在 C 语言中，可以使用分号 ; 作为语句的结束符。因此，可以在同一行书写多条语句，每条语句之间用分号隔开。例如：a=1; b=2; c=a+b; 是合法的。

D) C 程序允许将一条语句分写在多行上
这条叙述是正确的。C 语言以分号作为语句的结束标志，而不是行尾。因此，一条语句可以跨越多行书写，只要在语句结束时加上分号即可。这在书写较长的语句时可以提高代码的可读性。
```







**对于一个频繁使用的短小函数,在 C语言中应用什么实现?**

对于一个频繁使用的短小函数，在 C 语言中主要有两种实现方式来提高效率：

1. **使用宏定义 (`#define`)**
2. **使用内联函数 (`inline`)**

**场景：**

假设在一个图形处理程序中，你需要频繁地计算一个点的平方距离原点的距离。这个计算涉及到对 x 和 y 坐标分别平方，然后相加。由于这个计算在图形处理中可能会非常频繁地执行，为了提高性能，可以考虑将计算平方的函数定义为内联函数。

```c
#include <stdio.h>
#include <math.h> // 为了对比，我们使用库函数计算平方根

// 定义一个内联函数来计算一个整数的平方
inline int square(int n) {
    return n * n;
}

// 计算点 (x, y) 到原点的平方距离
inline int distance_squared(int x, int y) {
    return square(x) + square(y);
}

int main() {
    int point_x = 3;
    int point_y = 4;

    // 使用内联函数计算平方距离
    int sq_distance = distance_squared(point_x, point_y);
    printf("点 (%d, %d) 到原点的平方距离是: %d\n", point_x, point_y, sq_distance);

    // 为了验证，计算实际距离
    double distance = sqrt(sq_distance);
    printf("点 (%d, %d) 到原点的实际距离是: %f\n", point_x, point_y, distance);

    // 在一个循环中频繁调用内联函数
    for (int i = 0; i < 10; i++) {
        int x = i;
        int y = i * 2;
        int sq_dist = distance_squared(x, y);
        // 在实际应用中，这里可能会有更复杂的逻辑
    }

    return 0;
}
```

**代码解释：**

1. **`inline int square(int n)`:**
   - `inline` 关键字告诉编译器，我们希望将 `square` 函数的代码在调用它的地方展开，以避免函数调用的开销。
   - 这个函数非常短小，只包含一个乘法操作，非常适合内联。
2. **`inline int distance_squared(int x, int y)`:**
   - 这个函数也使用了 `inline` 关键字。
   - 它调用了另一个内联函数 `square` 两次，并将结果相加。编译器可能会将 `distance_squared` 和 `square` 都进行内联展开。
3. **`main` 函数:**
   - 演示了如何调用 `distance_squared` 函数来计算一个点的平方距离。
   - 通过一个简单的循环，模拟了频繁调用 `distance_squared` 的场景。
4. **这不就是函数嵌套函数？加了inline的好处在哪里？**
   1. **函数嵌套本身并不直接带来性能上的提升。** 函数嵌套的主要目的是为了代码的组织和模块化，使得代码更易于理解和维护。在你的例子中，`distance_squared` 调用了 `square`，这只是将计算任务分解为更小的逻辑单元。
   2. **`inline` 关键字的加入，其好处在于尝试消除函数调用的开销，无论函数是否嵌套。**
      1. 实现跟宏定义类似的效果
   3. **`inline` 函数:** 仍然是函数，编译器会对参数和返回值进行类型检查，这有助于在编译时发现类型错误，提高代码的健壮性。
   4. **函数内部使用宏定义:** 宏定义只是简单的文本替换，不会进行任何类型检查。这可能导致在运行时出现难以调试的类型不匹配错误。
   5. `inline` 函数旨在实现与宏定义类似的代码展开效果，以提高性能，但它通过编译器进行更智能的处理，提供了类型安全、避免了宏的副作用问题，并且更易于调试和维护。因此，在需要这种性能优化时，`inline` 函数通常是比宏定义更好的选择。









在针对资源受限的嵌入式系统进行软件开发时，考虑以下关于内存优化策略的描述，下列选项错误的是（ ）。（单项或多项选择题 5分）  

A、使用位域(bit-fields)可以压缩结构体的大小，从而节省RAM。

B、函数内联可以减少函数调用开销，同样也减小代码的ROM占用。

C、数据压缩可以在ROM中节省空间，但在运行时解压缩可能会增加CPU开销。

D、动态内存分配（如malloc()）通常比静态分配更节省RAM，因为它只在需要时分配内存。

E、使用缓冲技术，如循环缓冲区，可以有效地管理有限的RAM资源，但需要注意溢出和下溢的情况。

```c
B 选项：
函数内联确实可以消除函数调用的开销，但它的实现方式是将函数体复制到每个调用点，这往往会导致代码膨胀，从而增加 ROM 占用，而不是减少 ROM 的使用。因此，描述“同样也减小代码的ROM占用”是错误的。

C 选项：
虽然数据压缩能在 ROM 中存储更多数据，但在嵌入式系统中，由于资源有限，运行时解压缩带来的 CPU 开销和额外的算法复杂度通常难以接受，从而使得这种方法在实时或资源极度受限的场合不具备优势。也就是说，依赖数据压缩来节省 ROM 空间可能会对系统性能产生不可忽视的负面影响，因此把它作为一种内存优化策略来推荐是不合适的。

其他选项说明：

A 选项： 使用位域可以压缩结构体的大小，节省 RAM，这一描述是正确的。
D 选项： 尽管动态内存分配（如 malloc()）在嵌入式开发中常因碎片化和不可预测性受到限制，但其“只在需要时分配内存”的特点在一定条件下能够减少静态分配时可能造成的内存浪费，所以在某些情况下这种策略是可行的。
E 选项： 循环缓冲区等缓冲技术是管理有限 RAM 的常用手段，但必须防范溢出和下溢，其描述是正确的。
```

## 填空

**请写出bool fag 与“零值"比较的if语句**

在 C 语言中，bool 类型通常定义在 `<stdbool.h>` 中，其值为 `true` 或 `false`，而对应的整数值分别为 1 和 0。若要判断变量 `fag`（假设它是一个 bool 类型的变量）是否等于“零值”（即 false）

```c
if (fag == 0) {
    // 当 fag 为 false（零值）时执行的代码
}
或者
if (!fag) {
    // 当 fag 为 false（零值）时执行的代码
}
```



**请写出char*p与“零值"比较的if语句**

在 C 语言中，“零值”通常指的是 `NULL`。因此，对于指针 `p`，可以这样写：

```c
if (p == NULL) {
    // 当 p 为零值（即指向空地址）时执行的代码
}
或者
if (!p) {
    // 当 p 为零值时执行的代码
}
错误写法：
使用 if((*p)==0) 时，必须确保 p 不为 NULL，否则会导致未定义行为（例如程序崩溃）。
```



**算术运算符赋值运算符和关系运算符的运算优先级按从高到低依次为**

在 C 语言中，不同种类的运算符具有不同的优先级，其中：

- **算术运算符**（如 `+`, `-`, `*`, `/` 等）的优先级最高，
- **关系运算符**（如 `<`, `>`, `<=`, `>=`, `==`, `!=`）的优先级次之，
- **赋值运算符**（如 `=`, `+=`, `-=` 等）的优先级最低。

因此，从高到低依次为：

**算术运算符 > 关系运算符 > 赋值运算符**



请说出以下代码存在的问题:  

```c
int main()
{
	char a;
	char*str=&a; 
	strcpy(str,"hello"); 
	printf(str); //字符串输出格式不对，其他没有问题。
	return 0;
}
```

内存不足（缓冲区溢出）:

这里变量 `a` 只分配了一个字符的内存空间，但 `"hello"` 至少需要6个字符（包括结尾的空字符 `'\0'`）。因此，使用 `strcpy` 拷贝字符串会造成缓冲区溢出，导致未定义行为。把他看成`strcpy(&a,"hello");`而a只有一个char的空间，所以会溢出。

直接将指针指向字符串常量是合法的：

`char *str = "hello";`

这种写法没有缓冲区溢出问题，因为它并不需要额外的内存分配，而是让 `str` 指向编译器存储的字符串常量。但需要注意以下几点：

1. **只读问题**
   字符串常量通常存放在只读存储区，因此你不能修改它们。如果后续有代码试图修改 `str` 指向的内容，如 `str[0] = 'H';`，将会导致未定义行为或运行时错误。



## 编程题

### 链表1                             

实现一个C函数， 用于反转一个单链表。

这种链表反转题目通常不考虑哨兵节点，而是直接对真实数据节点进行反转。

```
struct Node { 
	int data;
	struct Node* next; 
	};
struct Node* reverse_list(struct Node* head);
```

请给出函数的完整实现， 并附上注释。

```c
思路
反转一个单链表是一个经典的编程问题，最容易想到的方法是迭代法。这种方法通过遍历链表并逐步反转每个节点的指针来实现，既直观又易于理解。以下是实现这一方法的思路：

初始化三个指针：
prev：指向前一个节点，初始为 NULL。
current：指向当前处理的节点，初始为链表的头节点 head。
next：指向下一个节点，初始为 NULL，用于在反转时保存后续节点。

遍历并反转：
从头节点开始，逐个处理每个节点。

在每一步：
将 next 保存为 current 的下一个节点（避免断链）。

将 current 的 next 指针反转，指向 prev。

将 prev 移动到 current。

将 current 移动到 next，进入下一次循环。

结束条件：
当 current 变为 NULL 时，说明整个链表已遍历完成。

此时，prev 指向反转后的新头节点。

返回结果：
返回 prev 作为反转后链表的头节点。

struct Node {
    int data;
    struct Node* next;
};

struct Node* reverse_list(struct Node* head) {
    struct Node* prev = NULL;    // 前一个节点，初始为 NULL
    struct Node* current = head; // 当前节点，初始为头节点
    struct Node* next = NULL;    // 下一个节点，初始为 NULL
    
    // 遍历链表直到 current 为空
    while (current != NULL) {
        next = current->next;    // 保存下一个节点，避免处理当前节点时断链
        
        current->next = prev;    // 反转当前节点的指针，把当前节点的指针指向前一个节点prev,这里已经反转了
        
        //为下一个节点反转做准备，当前节点会成为下一个处理节点的前一个节点即prev
        //先更新前一个节点，在更新当前节点，关于是否保存后续节点要看当前节点是不是遍历完了
        prev = current;          // 移动 prev 到当前节点,
        current = next;          // 移动 current 到下一个节点
    }
    
    // 反转后，最后一个节点处理后，current给了prev,prev为新的头节点
    //current是null了
    return prev; // 返回新的头节点
}
```

#### 代码解释

初始化`prev = NULL;`相当于当前节点（head）的前一个节点指向NULL。跟链表最后一个节点的指向是一样的。
当第一次`current->next = prev;`，一下就实现了**第一个节点变成最后一个节点的样式**了。

反转之后为下一个节点做准备：

- 下一个处理节点的前一个节点的更新
  - 就是已经处理好了的当前节点
- 当前处理节点的更新
  - 就是保存好的next

当处理完原链表的最后一个节点（**指向NULL的那个节点**）后，更新时：

- prev是原链表的最后一个有效节点即反转后的第一个有效节点
- current是NULL，不满足循环跳出。

返回的 prev 是反转后链表的头节点，也就是第一个有效节点。在这个代码中，头节点和第一个有效节点是同一个概念（没有哨兵节点）。

#### 推演过程

**没有哨兵节点。**

假设原始链表是：

```
1 -> 2 -> 3 -> NULL
```

其中：

- head 是节点 1，它的 data 是 1，next 指向 2。
- 节点 2 的 next 指向 3。
- 节点 3 的 next 指向 NULL。

现在，我们一步步执行代码：

1. 初始化：
   - prev = NULL
   - current = head = 1
   - next = NULL
2. 第一次循环：
   - next = current->next = 1->next = 2 （保存节点 2）
   - current->next = prev，即 1->next = NULL （反转指针）
   - prev = current，即 prev = 1
   - current = next，即 current = 2
   - 此时链表状态：1 -> NULL，剩余部分 2 -> 3 -> NULL
3. 第二次循环：
   - next = current->next = 2->next = 3 （保存节点 3）
   - current->next = prev，即 2->next = 1 （反转指针）
   - prev = current，即 prev = 2
   - current = next，即 current = 3
   - 此时链表状态：2 -> 1 -> NULL，剩余部分 3 -> NULL
4. 第三次循环：
   - next = current->next = 3->next = NULL （保存 NULL）
   - current->next = prev，即 3->next = 2 （反转指针）
   - prev = current，即 prev = 3
   - current = next，即 current = NULL
   - 此时链表状态：3 -> 2 -> 1 -> NULL
5. 循环结束：
   - current 为 NULL，退出循环。
   - 返回 prev，即节点 3。

所以，反转后的链表是：

```
3 -> 2 -> 1 -> NULL
```

### 链表2

使用C语言：

- **我还以为要先遍历到位置呢。**

- **都双向链表了，前后都知道，直接改就行。**
- **单链表的话需要遍历到前一个节点，不然不好修改。**

```
typedef struct node_t {
	data_t  data;
	struct node_t   *next;
	struct node_t   *prior;
} linknode_t;
```

请写出删除一个节点p的步骤: （5分）

```
void delete_node(linknode_t *p) {
    // 处理p的前一个节点的next指针
    if (p->prior != NULL) {
        p->prior->next = p->next;
    }
    
    // 处理p的后一个节点的prior指针
    if (p->next != NULL) {
        p->next->prior = p->prior;
    }
    
    // 释放p节点的内存
    free(p);
}
```

请写出在n节点后面增加一个节点s的操作步骤：（5分）

```
void insert_after(linknode_t *n, linknode_t *s) {
    // 设置s的next指针指向n的后一个节点
    s->next = n->next;
    
    // 设置s的prior指针指向n
    s->prior = n;
    
    // 如果n有后继节点，更新后继节点的prior指针指向s
    if (n->next != NULL) {
        n->next->prior = s;
    }
    
    // 最后更新n的next指针指向s
    n->next = s;
}
```

### 链表3

有一个C语言用来删除单链表的头元素的函数，请找出其中的问题，并加以纠正。（5分） 

```
void RemovHead(node *head) {   		
free(head);   		
head = head->next; 
}
```

这个函数的主要问题有两个：

#### 传参问题

```
void RemovHead(node *head)
这里传入的是一个值传递的指针，head是形参，函数内部对head的修改不会影响到调用者的head，所以head = head->next这行根本没有实际效果。正确做法是传指针的指针，这样才能修改外面的head。
```

#### 释放和访问顺序问题

```
free(head);
head = head->next;
free(head)之后，head已经是非法的内存了，紧接着的head = head->next访问了已经释放的内存，导致未定义行为。正确顺序应该是先保存next指针，然后再释放当前节点。
```

#### 正确版本示例

```
void RemoveHead(node **head)
{
    if (head == NULL || *head == NULL) {
        return;  // 空链表处理
    }

    node *temp = *head;        // 保存当前头节点
    *head = (*head)->next;     // 更新头节点
    free(temp);                 // 释放旧头节点
}
```

### 链表4

定义一个双向链表，并实现节点创建和查询功能函数

```c
#include <stdio.h>
#include <stdlib.h>

// 定义双向链表节点结构
typedef struct Node {
    int data;              // 节点数据
    struct Node* prev;     // 指向前一个节点的指针
    struct Node* next;     // 指向后一个节点的指针
} Node;

// 定义双向链表结构
typedef struct DoublyLinkedList {
    Node* head;            // 指向链表头部的指针
    Node* tail;            // 指向链表尾部的指针
    int size;              // 链表大小
} DoublyLinkedList;

// 创建一个新的双向链表
DoublyLinkedList* createList() {
    DoublyLinkedList* list = (DoublyLinkedList*)malloc(sizeof(DoublyLinkedList));
    if (list == NULL) {
        printf("内存分配失败！\n");
        return NULL;
    }
    list->head = NULL;
    list->tail = NULL;
    list->size = 0;
    return list;
}

// 创建一个新的节点
Node* createNode(int data) {
    Node* newNode = (Node*)malloc(sizeof(Node));
    if (newNode == NULL) {
        printf("内存分配失败！\n");
        return NULL;
    }
    newNode->data = data;
    newNode->prev = NULL;
    newNode->next = NULL;
    return newNode;
}

// 根据值查找节点，返回第一个匹配的节点
Node* findNode(DoublyLinkedList* list, int data) {
    Node* current = list->head;
    
    while (current != NULL) {
        if (current->data == data) {
            return current;
        }
        current = current->next;
    }
    return NULL; // 未找到
}
```



### 综合

请编写函数int sum(int n)，求1+2+3+…+n的值。可写出多种实现方法，并给出每种方法的优缺点描述

**循环实现（迭代法）**

```
int sum(int n) {
    int result = 0;
    for (int i = 1; i <= n; i++) {
        result += i;
    }
    return result;
}
```

优点：

1. 简单直观：代码易于理解，逻辑清晰，适合初学者。
2. 空间效率高：只使用常数额外空间（O(1) 空间复杂度）。

缺点：

1. 时间复杂度较高：需要循环 n 次，时间复杂度为 O(n)。
2. 性能瓶颈：当 n 很大时，循环会导致计算时间增加。

**递归实现**

```
int sum(int n) {
    if (n <= 1) {
        return n;
    }
    return n + sum(n - 1);
}
```

优点：

1. 优雅：代码简洁，体现了递归的思想。
2. 易于理解：将问题分解为子问题，符合数学归纳法。

缺点：

1. 效率低：时间复杂度为 O(n)，且每次递归调用都会产生函数调用开销。
2. 栈溢出风险：当 n 很大时，递归深度过大可能导致栈溢出。
3. 空间复杂度高：需要 O(n) 的栈空间。

### 字符串

1， 编写一个函数，作用是把一个char组成的字符串循环右移n个。比如原来是“abcdefghi”如果n=2，移位后应该是“hiabcdefg” 。（15分）

//pStr是指向以’\0’结尾的字符串的指针

//steps是要求移动的n

- 先全部翻转过来，

```c
void LoopMove ( char * pStr, int steps )
{
//请填充… 
}

#include <string.h>

void LoopMove(char *pStr, int steps) {
    // 检查无效输入或无需移动的情况
    if (pStr == NULL || steps <= 0) {
        return;
    }

    // 获取字符串长度（不包括 '\0'）
    size_t len = strlen(pStr);
    if (len == 0) {
        return;
    }

    // 计算实际有效的移动步数（处理 steps >= len 的情况）
    int effective_steps = steps % len;
    if (effective_steps == 0) {
        return;
    }

    // 步骤 1：反转整个字符串
    char *start = pStr;
    char *end = pStr + len - 1;
    //只要 start < end 成立，就继续执行循环。
    //这意味着当 start 和 end 指向同一个位置（start == end）或交叉（start > end）时，循环会停止。
    //交换到最后，只有一个数时，start==end，也不用交换
    //交换到最后，只有两个数时，交换完后，end<start，也不用交换了。
    while (start < end) {
        char temp = *start;
        *start = *end;
        *end = temp;
        start++;
        end--;
    }

    // 步骤 2：反转前 effective_steps 个字符
    start = pStr;
    end = pStr + effective_steps - 1;
    while (start < end) {
        char temp = *start;
        *start = *end;
        *end = temp;
        start++;
        end--;
    }

    // 步骤 3：反转剩余字符
    start = pStr + effective_steps;
    end = pStr + len - 1;
    while (start < end) {
        char temp = *start;
        *start = *end;
        *end = temp;
        start++;
        end--;
    }
}
```

示例 1

- 输入："abcdefghi", steps = 2
  - **原始字符串**："abcdefghi"
  - **反转整个字符串**："ihgfedcba"
  - **反转前 2 个字符**："higfedcba"
  - **反转剩余字符**："hiabcdefg"
  - **输出**："hiabcdefg"

示例 2

- 输入："abcde", steps = 2
  - **原始字符串**："abcde"
  - **反转整个字符串**："edcba"
  - **反转前 2 个字符**："decba"
  - **反转剩余字符**："deabc"
  - **输出**："deabc"

示例 3

- 输入："abcdefghi",steps = 11
  - 字符串长度 len = 9，实际移动步数 effective_steps = 11 % 9 = 2
  - 同示例 1，最终输出："hiabcdefg"





16进制字符串转10进制数, char *a 是输入的16进制字符串

```c
这个字符串表示一个有效的十六进制数，可能包含以下字符：

数字：'0' 到 '9'，分别表示十进制的 0 到 9。
字母：'A' 到 'F' 或 'a' 到 'f'，分别表示十进制的 10 到 15（例如，'A' 或 'a' 为 10，'F' 或 'f' 为 15）。
    
函数的目标是将这个十六进制字符串转换为对应的十进制整数并返回。例如：
输入 "3af"，应返回 943，因为：
'3' × 16² + 'a' (10) × 16¹ + 'f' (15) × 16⁰ = 3 × 256 + 10 × 16 + 15 × 1 = 768 + 160 + 15 = 943。
```

要将十六进制字符串转换为十进制数，我们可以从左到右依次处理字符串中的每个字符，逐步累积结果。

具体步骤如下：

1. **初始化结果**：用一个变量 result 表示十进制值，初始值为 0。
2. 遍历字符串：对于每个字符：
   - 将当前结果乘以 16（十六进制的基数）。
   - 根据字符的值（0-9 或 A-F/a-f）计算其十进制值，并加到结果中。
3. 处理字符：
   - 如果字符是 '0' 到 '9'，其值是字符减去 '0'。
   - 如果字符是 'A' 到 'F'，其值是 10 加上字符与 'A' 的差。
   - 如果字符是 'a' 到 'f'，其值是 10 加上字符与 'a' 的差。
4. **返回结果**：处理完所有字符后，返回最终的十进制值。

由于输入是C风格字符串（以空字符 '\0' 结尾），我们可以使用 <string.h> 中的 strlen 函数来获取字符串长度，并通过索引遍历字符。

```c
#include <string.h>

int fun(char *a) {
    int result = 0;          // 初始化结果为0
    int len = strlen(a);     // 获取字符串长度

    for (int i = 0; i < len; i++) {  // 遍历每个字符
        char ch = a[i];      // 当前字符
        int value;           // 当前字符的十进制值

        // 根据字符范围计算十进制值
        if (ch >= '0' && ch <= '9') {
            value = ch - '0';          // 数字 '0' 到 '9'
        } else if (ch >= 'A' && ch <= 'F') {
            value = 10 + (ch - 'A');   // 大写字母 'A' 到 'F'
        } else if (ch >= 'a' && ch <= 'f') {
            value = 10 + (ch - 'a');   // 小写字母 'a' 到 'f'
        }

        // 更新结果：当前结果乘以16并加上新值
        result = result * 16 + value;
    }

    return result;  // 返回十进制结果
}
```

strlen一下就可以直到最大的权重了，直接顺序遍历字符串即可。

**按照权重来讲，最左边不是16的3次方再乘以本身？**

**十六进制数的权重原理**

在十六进制数中，每一位的权重确实是基于16的幂次方。具体来说，从**右向左**数（索引从0开始），每一位的权重是 16^n，其中 n 是该位的位置索引。例如，对于一个十六进制数 "3af"（有3位）：

- 最右边的位（f）是第0位，权重为 16^0 = 1；
- 中间的位（a）是第1位，权重为 16^1 = 16；
- 最左边的位（3）是第2位，权重为 16^2 = 256

**从左到右的计算方式**

在编程中，我们常从左到右处理字符串，但这并不改变权重的本质。例如，计算 "3af"：

初始结果 result = 0；

处理 '3'：result = 0 * 16 + 3 = 3；

处理 'a'：result = 3 * 16 + 10 = 58；

处理 'f'：result = 58 * 16 + 15 = 943。

这种方法本质上是通过累积实现从右到左的权重计算，结果与直接按权重计算一致：

**从右到左的权重计算等于从左到右的依次累加计算**





请编写函数char *octalString(int value)，将int型参数转换为8进制字符串。

```c
#include <stdio.h>

char *octalString(int value) {
    static char result[12];
    int i = 0;
    unsigned int uvalue = (unsigned int)value; // 转换为无符号处理

    // 特殊情况：value为0
    if (value == 0) {
        result[0] = '0';
        result[1] = '\0';
        return result;
    }

    // 逐字节处理
    while (uvalue > 0 || i == 0) { // 至少执行一次
        int byte = uvalue & 0xFF;  // 取最低字节
        while (byte > 0) {
            result[i++] = (byte % 8) + '0';
            byte /= 8;
        }
        uvalue >>= 8; // 右移8位处理下一个字节
    }

    result[i] = '\0';

    // 反转字符串
    int left = 0, right = i - 1;
    while (left < right) {
        char temp = result[left];
        result[left] = result[right];
        result[right] = temp;
        left++;
        right--;
    }

    return result;
}

// 测试代码
int main() {
    printf("%s\n", octalString(10));   // 输出 "12"
    printf("%s\n", octalString(255));  // 输出 "377"
    return 0;
}
```



- 写一个函数，实现memcpy功能目的：将源内存块数据，复制到目的内存块数据已知函数原型void memcpy(unsigned char*dest,unsigned char *scr,unsigned long len)

- 已知strcpy 面数的原型是 char *strcpy(char *strDest, const char *strSrc);其中strDest是目的字符串，strSrc 是源字符串,不调用C++/C的字符串库面数，请编写面数strcpy。


- 不使用库函数，以最优的代码实现字符串拷贝函数char* strncpy（char *dest， const char *src, size_t count）
- 2，已知strcpy 函数的原型是 char *strcpy(char *strDest, const char *strSrc);其中strDest是目标字符串，strSrc 是源字符串,不调用C++/C的字符串库函数，请编写函数strcpy。


```c
/**
 * 优化的字符串拷贝函数 - 将src中的字符拷贝到dest，最多拷贝count个字符
 * 
 * @param dest   目标字符串
 * @param src    源字符串
 * @param count  最多拷贝的字符数
 * @return       目标字符串的指针
 *
 * 注意事项:
 * 1. 如果src的长度小于count，则在dest的剩余空间用'\0'填充
 * 2. 如果src的长度大于等于count，则不会在结果字符串末尾自动添加'\0'
 * 3. 此函数不像标准库中的strncpy那样检查重叠，使用时需小心
 */
char* strncpy(char *dest, const char *src, size_t count) {
    // 参数检查
    if (dest == NULL || src == NULL) {
        return dest;
    }
    
    char *ptr = dest;
    
    // 主要拷贝循环 - 拷贝源字符串直到遇到'\0'或达到count
    //拷贝完或者达到数量要求
    while (count > 0 && *src != '\0') {
        *ptr++ = *src++;
        count--;
    }
    
    // 如果count > 0，用'\0'填充剩余空间
    //拷贝完如果没有达到数量要求使用\0填充
    while (count > 0) {
        *ptr++ = '\0';
        count--;
    }
    
    return dest;
}
```





**请编写一个C 函数，该函数在给定的内存区域搜索给定的字符，并返回该字符所在位置索引值。**

```c
Int * find(int *p,int size,char target){
	While(size--){
		If(*p==target){
			Retrun p
        }else{
            P++;
			}
		}
	Return null;
}
```

**返回类型错误：** 你的函数声明返回 `Int *` (指向 `int` 的指针)，但根据题目要求，应该返回字符所在位置的**索引值**，索引值应该是一个整数类型 (`int` 或 `size_t`)。

**指针类型不匹配：** 你的函数接收一个 `int *p` (指向 `int` 的指针)，并将内存区域视为整数数组。然而，你要搜索的是一个 `char` 类型的目标字符。更符合题意的是将 `p` 声明为 `char *` (指向 `char` 的指针)。

**返回指针而不是索引：** 当找到目标字符时，你的代码返回的是指针 `p`。题目要求返回的是该字符所在的**索引值**。你需要计算当前指针 `p` 相对于起始位置的偏移量。

**返回 `null` 而非索引：** 当没有找到目标字符时，你的代码返回 `null`。虽然 `NULL` 在 C 语言中表示空指针，但你的函数声明返回的是 `Int *`，并且题目要求返回的是索引值。通常在这种情况下，会返回一个特殊值（例如 -1）来表示未找到。

```c
#include <stddef.h> // 包含 size_t 和 NULL 的定义

int find(const char *p, size_t size, char target) {
    const char *start = p; // 保存起始地址，用于计算索引
    while (size--) {
        if (*p == target) {
            return (int)(p - start); // 返回找到字符的索引（相对于起始地址的偏移量）
        } else {
            p++;
        }
    }
    return -1; // 如果没有找到目标字符，返回 -1
}
```



### 查找算法

编写出二分查找的代码（2）

```c
int binarySearch(int arr[], int low, int high, int target) {
    // arr[]:  待查找的已排序数组
    // low:   查找范围的起始索引
    // high:  查找范围的结束索引
    // target: 要查找的目标值

    while (low <= high) { // 循环条件：只要查找范围有效
        int mid = low + (high - low) / 2; // 计算中间索引。这样写可以防止 (low + high) 直接相加可能导致的溢出

        if (arr[mid] == target) { // 如果中间元素正好是目标值
            return mid;         // 返回中间元素的索引，表示找到了
        } else if (arr[mid] < target) { // 如果中间元素小于目标值
            low = mid + 1;      // 目标值可能在右半部分，调整 low 到 mid 的右边
        } else { // arr[mid] > target  如果中间元素大于目标值
            high = mid - 1;     // 目标值可能在左半部分，调整 high 到 mid 的左边
        }
    }

    return -1; // 如果循环结束都没有找到目标值，返回 -1 表示未找到
}
```

这一行代码计算当前查找范围的中间索引。使用 `low + (high - low) / 2` 而不是 `(low + high) / 2` 的目的是为了防止当 `low` 和 `high` 都很大时，`low + high` 可能超出 `int` 类型的最大值，导致溢出。这种写法更安全。

### 位运算

请编写函数void swap(int *a, int *b)，使a，b指向的值交换。（要求不能使用额外的变量）

```c
void swap(int *a, int *b) {
    if (a != b) { // 检查是否指向同一地址
        *a = *a ^ *b;
        *b = *a ^ *b;
        *a = *a ^ *b;
    }
}
```

### 排序算法

#### 优雅的冒泡排序算法（2次）

- n个数，总共只需要排序n-1次即可，所以最外层是n-1。
- 每个数需要比较的次数为：除开自己的话，需要跟每一个数比较一次，也就是需要比较N-1次，这个前提是减去已经冒泡的次数，因为每一次冒泡都会得到一个最大值，不用跟已经得出结果比较。
  - 所以二次循环的值n-i-1
  - 判断大小
    - 下标小的的元素大的话，就与后面交换即可。
    - 直到arr[j]>arr[j+1]

```c
// 冒泡排序函数
void bubbleSort(int arr[], int n) {
    int i, j, temp;
    for (i = 0; i < n - 1; i++) {
        // 内层循环比较相邻元素
        for (j = 0; j < n - i - 1; j++) {
            if (arr[j] > arr[j+1]) {
                // 交换两个元素
                temp = arr[j];
                arr[j] = arr[j+1];
                arr[j+1] = temp;
            }
        }
    }
}
```

有三个数A,B,C，编写一个函数，将其按照从大到小的顺序排列并打印出来！（**两次**）

- 使用if判断，每一个数最大的情况
  - 再判断次大的
- 简单手动冒泡
  - 依次跟后面比，大的换到前面来。

```c
#include <stdio.h>
#include <stdlib.h>

// 方法一：使用 if-else 语句进行比较和打印
void sortAndPrint_ifElse(int a, int b, int c) {
    if (a >= b && a >= c) {
        printf("%d ", a);
        if (b >= c) {
            printf("%d %d\n", b, c);
        } else {
            printf("%d %d\n", c, b);
        }
    } else if (b >= a && b >= c) {
        printf("%d ", b);
        if (a >= c) {
            printf("%d %d\n", a, c);
        } else {
            printf("%d %d\n", c, a);
        }
    } else { // c >= a && c >= b
        printf("%d ", c);
        if (a >= b) {
            printf("%d %d\n", a, b);
        } else {
            printf("%d %d\n", b, a);
        }
    }
}

// 方法二：使用临时变量进行交换 (类似冒泡排序的思想，但更简洁)
void sortAndPrint_swap(int a, int b, int c) {
    int temp;
    if (a < b) {
        temp = a;
        a = b;
        b = temp;
    }
    if (a < c) {
        temp = a;
        a = c;
        c = temp;
    }
    if (b < c) {
        temp = b;
        b = c;
        c = temp;
    }
    printf("%d %d %d\n", a, b, c);
}
```





输入10个整数，将它们从小到大排序后输出，并给出现在每个元素在原来序列中的位置。

Sample Imput

1 2 3 5 4 6 8 9 10 7

Sample Output

排序后位置：1 2 3 4 5 6 7 8 9 10

原序列位置：1 2 3 5 4 6 10 7 8 9

使用冒泡排序实现，并加入了记录元素原始位置的功能。代码尽量保持简洁易懂，同时满足您的所有需求。

```c
#include <stdio.h>
#include <stdlib.h>

int main(int argc, char *argv[]) {
    if (argc != 11) {
        printf("请输入10个整数作为参数。\n");
        return 1;
    }

    int numbers[10];
    int original_positions[10];

    // 将输入的参数转换为整数并存储，同时初始化原始位置
    for (int i = 0; i < 10; i++) {
        numbers[i] = atoi(argv[i + 1]);
        original_positions[i] = i + 1; // 记录原始位置，从1开始
    }

    // 冒泡排序及同步调整原始位置
    for (int i = 0; i < 9; i++) {
        for (int j = 0; j < 9 - i; j++) {
            if (numbers[j] > numbers[j + 1]) {
                // 交换数值
                int temp_num = numbers[j];
                numbers[j] = numbers[j + 1];
                numbers[j + 1] = temp_num;

                // 同步交换原始位置
                int temp_pos = original_positions[j];
                original_positions[j] = original_positions[j + 1];
                original_positions[j + 1] = temp_pos;
            }
        }
    }

    // 输出排序后的结果和原始位置
    printf("排序后位置：");
    for (int i = 0; i < 10; i++) {
        printf("%d ", numbers[i]);
    }
    printf("\n");

    printf("原序列位置：");
    for (int i = 0; i < 10; i++) {
        printf("%d ", original_positions[i]);
    }
    printf("\n");

    return 0;
}
```





请写出了解的排序方法。

### 数据分离

输入一个整数(任意位数)，倒叙输出（如输入 12345  则 输出54321）

```c
int func(int a)
{
 int b = 0;
    int temp=10;
    While(a!=0){
        b += a%10;
        b = b*temp;
        a/=10;
	}
	 return b/10;
}

优雅：不用再除以10
int main() {
    int a;
    int b = 0;
    int temp = 10;

    printf("请输入一个整数: ");
    scanf("%d", &a);

    printf("倒序输出为: ");
    while (a != 0) {
        b = (b * temp) + (a % 10);
        a /= 10;
    }
    printf("%d\n", b);

    return 0;
}
```



有个16位的整数，每4位为一个数，写函数sun求他们的和

解释:输入参数为整数 1101010110110111，返回值为 1101+0101+1011+0111的结果

```c
/**
 * @brief 拆开16位整数分开计算，数据分离
 * 
 * @param a 
 * @return int 
 */
int sun(short a)
{
    int sum = 0;
    
    for(int i=0;i<4;i++)
    {
        sum += (a % 10000);
        a>>=4;
    }
}

"分清楚是二进制还是十进制数
/**
 * @brief 拆开16位十进制整数，每4位为一个数，求它们的和
 *
 * @param a The 16-bit decimal integer input.
 * @return int The sum of the four 4-digit parts.
 */
int sun(int a) // 注意这里参数类型可能需要根据实际情况调整，如果确实是16位十进制，可能需要考虑更大的整数类型
{
    int sum = 0;

    for (int i = 0; i < 4; i++)
    {
        sum += (a % 10000); // 获取最后四位
        a /= 10000;        // 移除最后四位 (十进制除法)
    }
    return sum;
}
```



将二维数组行列元素互换，存在另外一个数组中（15分）

```c
#include <stdio.h>

#define ROWS 3 // 假设原数组有3行
#define COLS 4 // 假设原数组有4列

int main() {
    int originalArray[ROWS][COLS] = {
        {1, 2, 3, 4},
        {5, 6, 7, 8},
        {9, 10, 11, 12}
    };

    int transposedArray[COLS][ROWS]; // 新数组的行列数与原数组相反

    // 遍历原数组，将元素放到新数组的对应位置
    for (int i = 0; i < ROWS; i++) {
        for (int j = 0; j < COLS; j++) {
            transposedArray[j][i] = originalArray[i][j];
        }
    }

    // 打印转置后的数组（可选，用于验证）
    printf("原始数组:\n");
    for (int i = 0; i < ROWS; i++) {
        for (int j = 0; j < COLS; j++) {
            printf("%d\t", originalArray[i][j]);
        }
        printf("\n");
    }

    printf("\n转置后的数组:\n");
    for (int i = 0; i < COLS; i++) {
        for (int j = 0; j < ROWS; j++) {
            printf("%d\t", transposedArray[i][j]);
        }
        printf("\n");
    }

    return 0;
}
```



## 简答题

### 防止重复包含

**头文件中的ifndef/define/endif干什么用?**

```c
主要作用是防止头文件被同一个源文件（或者通过不同的包含路径间接地）多次包含。

多次包含同一个头文件可能会导致以下问题：

重复定义错误： 如果头文件中包含变量、函数或类的声明（注意是声明，定义通常放在源文件中），多次包含会导致编译器报告重复定义的错误。
编译效率降低： 即使没有语义上的错误，多次处理同一个头文件也会增加编译时间。
```

const有什么用途？

```
定义不可修改的常量，提高代码可读性和安全性。
约束函数参数，表明函数不会修改某些输入，增强代码的可靠性。
约束函数返回值，表明返回值不应该被修改。
```



### 链表

请写出单向链表，双向链表的特点以及应用场景。

```c
单向链表
插入：在已知前驱节点时，时间复杂度为 O(1)；若需查找位置，则为 O(n)。
删除：需要知道待删除节点的前驱，时间复杂度为 O(1)；查找前驱为 O(n)。
遍历：只能从头到尾，时间复杂度为 O(n)。
访问：不支持随机访问，必须从头开始遍历。

应用场景：
栈的实现：利用头插法或尾插法实现 LIFO（后进先出）。
队列的实现：尾部插入，头部删除，实现 FIFO（先进先出）。
内存管理：操作系统中空闲内存块的链表（如 malloc/free）。


双向链表
插入：在已知节点位置时，调整前后指针，时间复杂度为 O(1)。
删除：已知节点时，只需调整前后指针，时间复杂度为 O(1)；无需查找前驱。
遍历：可从任一方向遍历，时间复杂度为 O(n)。
访问：仍不支持随机访问，但双向性提高灵活性。

应用场景：
LRU 缓存：需要快速删除和移动节点（如最近最少使用算法）。
文本编辑器：光标移动需要前后导航，双向链表便于实现。
操作系统任务调度：任务队列中需要频繁调整优先级或顺序。
```

请描述单向链表和双向链表的构成，以及链表与数组的优劣势。(没有哨兵节点)

```c
单向链表
构成:

单向链表是由一系列节点组成的线性数据结构，每个节点包含两部分：
数据域 (Data):  用于存储实际的数据。可以是任何类型的数据，例如整数、字符、对象等等。
指针域 (Next Pointer):  存储指向链表中下一个节点的指针 (地址)。单向链表中，每个节点只有一个指向下一个节点的指针。链表的最后一个节点的指针域通常指向 null 或空地址，表示链表的末尾。

双向链表
构成:

双向链表与单向链表类似，也是由一系列节点组成的线性数据结构，但每个节点包含三部分：
数据域 (Data):  与单向链表相同，用于存储实际的数据。
前驱指针域 (Previous Pointer):  存储指向链表中前一个节点的指针 (地址)。链表的第一个节点的前驱指针域通常指向 null 或空地址。
后继指针域 (Next Pointer):  存储指向链表中下一个节点的指针 (地址)。链表的最后一个节点的后继指针域通常指向 null 或空地址。

链表: 适用于频繁进行插入和删除操作，但不太需要随机访问元素的场景。例如，实现栈、队列、表示动态数据集等。
数组: 适用于需要频繁进行随机访问元素，而插入和删除操作较少的场景。例如，存储静态数据集合、实现矩阵、向量等。
```





请写出一种链表实现方式。

为了保持简洁，我们实现最基本的功能： **创建链表节点**、 **插入节点（在头部）** 和 **打印链表**。

```c
#include <stdio.h>
#include <stdlib.h> // 为了使用 malloc 和 free

// 1. 定义链表节点结构体
typedef struct Node {
    int data;           // 节点数据 (这里假设为 int 类型，可以根据需要修改)
    struct Node *next;  // 指向下一个节点的指针
} Node;

// 2. 创建新节点函数
Node* createNode(int data) {
    Node* newNode = (Node*)malloc(sizeof(Node)); // 动态分配节点内存
    if (newNode == NULL) {
        perror("内存分配失败"); // 错误处理：内存分配失败
        exit(EXIT_FAILURE);
    }
    newNode->data = data;  // 初始化节点数据
    newNode->next = NULL;  // 新节点是尾节点，next 指针设为 NULL
    return newNode;         // 返回新节点的指针
}

// 3. 在链表头部插入节点函数
Node* insertNode(Node* head, int data) {
    Node* newNode = createNode(data); // 创建新节点
    newNode->next = head;        // 新节点的 next 指针指向原来的头节点
    return newNode;               // 新节点成为新的头节点，返回新头节点的指针
}

// 4. 打印链表函数
void printList(Node* head) {
    Node* current = head; // 从头节点开始遍历
    printf("链表内容: ");
    while (current != NULL) { // 当 current 指针不是 NULL 时，继续遍历
        printf("%d -> ", current->data); // 打印当前节点的数据
        current = current->next;         // 移动到下一个节点
    }
    printf("NULL\n"); // 链表结尾标记为 NULL
}

int main() {
    Node* head = NULL; // 初始化空链表

    // 插入节点
    head = insertNode(head, 3);
    head = insertNode(head, 7);
    head = insertNode(head, 1);
    head = insertNode(head, 9);

    // 打印链表
    printList(head); // 输出：链表内容: 9 -> 1 -> 7 -> 3 -> NULL

    return 0;
}
```



### IIC协议

关于I2C通信协议，请回答如下问题（14分）： 

- I2C有几根线，分别作用是什么？ 

- 文字描述I2C发送START信号、STOP信号的时序？ 

- 用文字分别描述I2C读、写寄存器操作标准时序？

```c
I2C总线有两根线：
SCL (Serial Clock Line): 时钟线，由主设备产生，用于同步数据传输
SDA (Serial Data Line): 数据线，用于在设备之间传输数据，可双向传输
这两根线都需要上拉电阻，默认为空闲为高电平状态。

START信号时序:
初始状态：SCL和SDA都处于高电平
当SCL保持高电平时，SDA从高电平切换到低电平
这种"SDA在SCL高电平期间从高变低"的特殊情况被识别为START信号
START信号表示通信开始

STOP信号时序:
开始状态：SDA处于低电平，SCL处于低电平
SCL由低电平切换到高电平
当SCL保持高电平时，SDA从低电平切换到高电平
这种"SDA在SCL高电平期间从低变高"的特殊情况被识别为STOP信号
STOP信号表示通信结束


I2C写寄存器操作标准时序：

主设备发送START信号
主设备发送从设备地址（7位）+ 写操作位（0）
从设备发送ACK（应答信号，拉低SDA）
主设备发送要写入的寄存器地址
从设备发送ACK
主设备发送要写入的数据
从设备发送ACK
若需写入多个字节，重复步骤6-7
主设备发送STOP信号结束传输

I2C读寄存器操作标准时序：

主设备发送START信号
主设备发送从设备地址（7位）+ 写操作位（0）
从设备发送ACK
主设备发送要读取的寄存器地址
从设备发送ACK
主设备发送REPEATED START信号（复位起始信号）
主设备发送从设备地址（7位）+ 读操作位（1）
从设备发送ACK
从设备发送寄存器数据
主设备发送ACK（如果继续读取更多字节）或NACK（如果这是最后一个字节）
若需读取多个字节，重复步骤9-10，最后一个字节回复NACK
主设备发送STOP信号结束传输
```

## 单片机

设计一个STM32程序，按键后Speaker响起，再按按键一次可关闭，Speaker响起30s后关闭（可文字描述需注意的细

节）。

**箭头指向阴极的为NPN。箭头从阴极指出来的为PNP。**

- **NPN**
  - **站起来，高电平导通**
- **PNP**
  - **塌着，低电平导通**

![image-20250317155824399](./Linux笔试面试题.assets/image-20250317155824399.png)

```
PB9的GPIO口配置为外部中断模式，出现下降沿触发中断。按键没有按下时，PB9呈现高电平。当按键被按下时PB9呈现低电平。

PB10配置为推挽输出，定义一个翻转PB10电平信号的宏。初始电平为低，使三极管断开。

定义一个全局变量time。

当按键按下时在对应的中断处理函数中，翻转PB10的电平信号，输出高电平，导通三极管，Speaker响起.

将time赋值为0，开始计时。

定义一个1ms的定时中断，中断服务函数中，使time递增。
在外部循环中不断查询time的状态是否到达30，如果到达30，则执行翻转PB10的电平信号，输出低电平，断开三极管，Speaker实现30s后关闭。
```





如图为51单片机模块，请按以下要求完成项目。

 a.在图上画出实现最小系统的所需电路要求或描述出实现最小系统条件

 b.写程序实现图中系统功能，当人按压金属触片后，单片机（P1.1pin）能检测到金属触片两端接触，此时控制P2.1让喇叭响起

 c.喇叭响后，单片机识别到有按键按下（P0.1），喇叭停止发出声音，松开按键喇叭继续响起

 d.如果喇叭响后无人为操作，30s后喇叭停止发出声音

![image-20250317171130738](./Linux笔试面试题.assets/image-20250317171130738.png)
