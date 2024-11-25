# 项目结构组织

假设我们有一个简单的图书管理系统，其中涉及到一些全局变量。为了方便管理，我们将不同的功能分开到不同的源文件中。

## 项目结构

```c
project/
├── main.c        // 主函数文件
├── book.c        // 书籍管理相关函数
├── book.h        // 书籍管理相关声明
├── config.h      // 宏定义
├── auth.c        // 用户认证相关函数
├── auth.h        // 用户认证相关声明
└── Makefile      // 编译文件
```

## 文件详解

**config.h** — 宏定义（通常用于常量和宏）

首先，我们将所有的常量和宏定义放入一个单独的头文件 `config.h` 中，以便其他源文件引用。

```c
// config.h
#ifndef CONFIG_H
#define CONFIG_H

// 宏定义
#define MAX_BOOKS 100
#define MAX_PASSWORD_LENGTH 10

#endif // CONFIG_H
```

## **book.h** — 声明书籍相关的结构体和全局变量

在 `book.h` 中，我们声明与书籍相关的结构体和全局变量。`extern` 用于声明全局变量，表示它们的定义在其他源文件中。

```c
// book.h
#ifndef BOOK_H
#define BOOK_H

#include "config.h"//头文件中如果用到了其他头文件中的东西，也可以包含

// 书籍的结构体定义
typedef struct {
    int id;
    char name[50];
    char author[50];
    double price;
} Book;

// 声明全局变量（使用 extern 声明）,这里只是声明而已，类型+变量名
extern Book books[MAX_BOOKS];  // 在其他文件中定义该变量
extern int book_count;         // 记录当前书籍的数量

// 函数声明
void listBooks(void);
void addBook(Book book);

#endif // BOOK_H
```

## **book.c** — 定义书籍相关的全局变量和函数

在 `book.c` 中，我们会定义全局变量和实现函数。

```c
// book.c
#include <stdio.h>
#include "book.h"

// 定义全局变量（没有 extern）
Book books[MAX_BOOKS] = {
    {1, "红楼梦", "曹雪芹", 99.99},
    {2, "三国演义", "罗贯中", 88.00}
}; // 初始定义部分书籍

int book_count = 2;  // 目前有两本书

// 函数实现
void listBooks(void) {
    for (int i = 0; i < book_count; i++) {
        printf("书籍 ID: %d, 名称: %s, 作者: %s, 价格: %.2f\n",
            books[i].id, books[i].name, books[i].author, books[i].price);
    }
}

void addBook(Book book) {
    if (book_count < MAX_BOOKS) {
        books[book_count++] = book;
        printf("书籍 '%s' 添加成功。\n", book.name);
    } else {
        printf("书籍库已满，无法添加新书。\n");
    }
}
```

## **auth.h** — 用户认证相关声明

在 `auth.h` 中，我们声明与用户认证相关的函数以及全局变量。

```c
// auth.h
#ifndef AUTH_H
#define AUTH_H

//#define MAX_PASSWORD_LENGTH 10
#include "config.h"  // 引用 config.h，而不是重复定义

extern char password[MAX_PASSWORD_LENGTH];  // 用户密码（全局变量）

// 函数声明
void getPassword(void);
int checkPassword(const char* inputPassword);

#endif // AUTH_H
```

## **auth.c** — 实现用户认证相关的功能

在 `auth.c` 中，我们定义密码变量和相关函数。

```c
// auth.c
#include <stdio.h>
#include <string.h>A
#include "auth.h"

// 定义全局变量（没有 extern）
char password[MAX_PASSWORD_LENGTH] = "123456";  // 初始密码为 "123456"

// 获取用户输入的密码
void getPassword(void) {
    printf("请输入密码: ");
    fgets(password, MAX_PASSWORD_LENGTH, stdin);
    password[strcspn(password, "\n")] = '\0';  // 去掉末尾的换行符
}

// 校验密码
int checkPassword(const char* inputPassword) {
    return strcmp(inputPassword, password) == 0;
}
```

## **main.c** — 主程序

在 `main.c` 中，我们会引用上述头文件，调用函数，并且使用全局变量。

```c
// main.c
#include <stdio.h>
#include "book.h"
#include "auth.h"

int main(void) {
    // 1. 用户认证
    char inputPassword[MAX_PASSWORD_LENGTH];
    getPassword();
    if (checkPassword(inputPassword)) {
        printf("登录成功!\n");
    } else {
        printf("密码错误!\n");
        return 0;
    }

    // 2. 图书管理
    Book newBook = {3, "围城", "钱钟书", 56.80};
    addBook(newBook);  // 添加新书
    listBooks();       // 列出所有书籍

    return 0;
}
```

**总结与解释**

- **`extern` 关键字**：
  - `extern` 用于**声明在其他文件中定义的全局变量**。它告诉编译器该变量的定义不在当前文件中，而是在其他源文件中定义的。
  - 例如，在 `book.h` 中**声明**了 `extern Book books[MAX_BOOKS];` 和 `extern int book_count;`，这些变量在 `book.c` 中**定义**。
- **头文件和源文件的分离**：
  - 头文件（如 `book.h`、`auth.h`）只包含声明（如结构体、函数和外部变量的声明），不包含实际的实现或定义。这样可以让多个源文件共享相同的声明。
  - 源文件（如 `book.c`、`auth.c`）包含具体的实现。**全局变量的定义通常放在源文件中，其他文件通过 `extern` 关键字来引用这些全局变量。**
- **宏定义**：
  - 宏定义通常放在一个单独的头文件中（如 `config.h`），以便全局使用。比如我们定义了 `MAX_BOOKS` 和 `MAX_PASSWORD_LENGTH`，这些宏在整个项目中都可以引用。
- **全局变量**：
  - **全局变量通常放在源文件中**，其他文件通过 `extern` 来引用它们。在我们的示例中，`books` 和 `book_count` 是全局变量，它们存储和管理书籍信息。

## 疑问

### 疑问1:源文件的自包含

**我在auth.c定义的全局变量和函数，我都需要在auth.h中声明？并且auth.c自己也要包含auth.h？ auth.c定义的全局变量，在auth.h中使用extern声明后，其他源文件只要包含了auth.h就可以直接使用吗？**

1.**在 `auth.c` 中定义的全局变量和函数需要在 `auth.h` 中声明吗？**

- **全局变量**：是的，`auth.c` 中定义的全局变量（如 `password`）需要在 `auth.h` 中用 `extern` 声明。这样，其他源文件包含 `auth.h` 时就能知道这个变量的存在，并且能够访问它。
- **函数**：所有函数的声明（函数原型）也应该放在 `auth.h` 中。这样其他源文件就能知道这些函数的名字、参数类型和返回类型，并能够调用它们。

2.**`auth.c` 自己也要包含 `auth.h` 吗？**

- 是的，`auth.c` 必须包含 `auth.h`，这样 `auth.c` 中定义的函数和全局变量可以与头文件中的声明保持一致。**通常每个源文件都需要包含自己的头文件，这样可以确保所有声明和定义是一致的。**


3.**`auth.c` 定义的全局变量在 `auth.h` 中使用 `extern` 声明后，其他源文件只要包含 `auth.h` 就可以直接使用吗？**

- 是的，其他源文件只要包含了 `auth.h`，就可以直接使用在 `auth.c` 中定义的全局变量和函数。因为通过 `extern` 声明，编译器知道这些变量或函数的定义在其他源文件中，链接器会负责在链接时将它们关联起来。

### 疑问2:头文件用法

**在源文件定义结构体的声明，在对应的头文件声明？ 还是直接在头文件定义？ 因为结构体我其他源文件可能要用**

结构体的定义位置需要根据使用场景决定：

- 如果结构体只在某个源文件中使用，可以将其定义放在该源文件中。
- 如果结构体需要被多个源文件共享，那么应该将其定义放在头文件中。

头文件（如 `book.h`、`auth.h`）通常包含声明（如函数声明、外部变量声明）和供多个源文件共享的定义（如结构体定义、宏定义、内联函数等）。头文件避免包含非内联函数的实现或变量的实际定义，以免引发重复定义问题。

在头文件 `person.h` 中:

```c
#ifndef PERSON_H
#define PERSON_H

struct Person {
    char name[50];
    int age;
    float height;
};

// 可以在这里声明使用该结构体的函数
void printPerson(struct Person p);
struct Person createPerson(const char* name, int age, float height);

#endif
```

在源文件 `person.c` 中:

```c
#include "person.h"
#include <stdio.h>

void printPerson(struct Person p) {
    printf("Name: %s, Age: %d\n", p.name, p.age);
}

// 函数实现...
```

其他想使用这个结构体的源文件:

```c
#include "person.h"

int main() {
    struct Person p = createPerson("John", 25, 175.5);
    printPerson(p);
    return 0;
}
```

需要注意：

1. 使用 include guard (#ifndef, #define, #endif) 防止重复包含
2. 把相关的函数声明也放在头文件中
3. 函数的具体实现放在源文件中

### 疑问3：防重复包含的写法

**C语言中，防重复包含的写法？不管源文件的命名大小写如何，统统大写？**

在C语言中，为了防止头文件被重复包含，通常使用**包含防护（Include Guard）**技术。写法如下：

```c
#ifndef HEADER_NAME_H
#define HEADER_NAME_H

// 头文件内容

#endif // HEADER_NAME_H
```

这里的 `HEADER_NAME_H` 是一个宏，用来表示头文件的唯一标识符。

#### 命名习惯

为了确保不同头文件的防护宏不会冲突，通常采用以下命名习惯：

1. **基于文件名命名**：将文件名转为大写，并用下划线替代非字母字符。
   - 例如，`file.h` 的宏定义为 `FILE_H`。
   - 对于更复杂的路径，可以加上目录名或项目名前缀，例如 `project_dir_file_h`。
2. **统一大写**：采用大写字母更显眼，并且避免大小写敏感问题。
3. **特殊前缀**：为避免潜在的冲突，可以给防护宏添加特殊前缀（如项目名）。
   - 例如，`MYPROJECT_FILE_H`。

假设头文件名为 `config.h`，可以这样写：

```c
#ifndef CONFIG_H
#define CONFIG_H

// 配置相关代码

#endif // CONFIG_H
```

或者，对于更复杂的目录结构：

```c
#ifndef MYPROJECT_UTILS_CONFIG_H
#define MYPROJECT_UTILS_CONFIG_H

// 配置相关代码

#endif // MYPROJECT_UTILS_CONFIG_H
```

通过这种方式，即使文件在不同的操作系统或编译器中因大小写规则有所不同，也可以确保防护宏唯一且统一。

**大小写有区别，也能找到对应的源文件？**

在C语言的编译过程中，**是否区分文件名的大小写**，取决于**操作系统的文件系统**：

**常见操作系统对文件名大小写的处理**

1. **大小写敏感的系统**：
   - **Linux/Unix** 系统：文件名是大小写敏感的。这意味着 `file.h` 和 `File.h` 被视为两个不同的文件。
   - 编译器在这些系统上处理头文件时，文件名大小写需完全匹配，否则会找不到对应的文件。
2. **大小写不敏感的系统**：
   - **Windows** 的 NTFS 文件系统默认情况下是大小写不敏感（但可以配置为大小写敏感）。
   - **macOS** 的默认 APFS 文件系统也通常是大小写不敏感。
   - 在这些系统上，`file.h` 和 `File.h` 会被视为同一个文件，编译器能找到对应的头文件。

**对开发的影响**

为了确保代码在不同操作系统上的**可移植性**：

1. **统一头文件命名规范**：
   - 所有文件名采用**小写字母**，或者全用**大写字母**。
   - 避免文件名大小写混用，例如不要有 `File.h` 和 `file.h` 两个文件同时存在。
2. **在代码中严格匹配文件名**：
   - `#include "file.h"` 时，要与文件实际名称完全一致。
   - 避免因为大小写不一致导致代码在某些平台无法编译。
3. **构建工具注意事项**：
   - 如果使用了工具（如自动生成的头文件路径），需确保它们在大小写敏感的环境中生成正确的路径。

**Include Guard 的作用**

防重复包含与文件名是否大小写敏感无关，而是针对同一头文件被多次包含的场景。防护宏的命名与文件名一致（通常使用大写字母），是一种良好的习惯，与文件名的大小写一致性无直接关系，但有助于代码的易读性和一致性。

只是一种标识？为了不被二次编译器处理？？？