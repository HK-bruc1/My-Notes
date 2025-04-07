# P5_标准IO

## 文件IO对应的标准IO

标准IO是C标准库提供的文件操作接口，构建在文件IO之上，提供了缓冲区管理和高级操作功能。标准IO是**缓冲**的，能够减少直接系统调用的次数，通常在处理文本文件或需要格式化IO时更方便。

### 打开文件：fopen

```
FILE *fopen(const char *pathname, const char *mode);
```

- pathname：文件路径。

- mode：打开模式，如 "r"（读）、"w"（写）、"a"（追加）。

- 返回值：成功返回文件流指针（FILE *），失败返回 NULL。

```
FILE *fp = fopen("example.txt", "w");
```

在 Linux（以及 C 语言标准库）中，`fopen` 函数用于打开文件，其模式 `"a"` 和 `"a+"` 的主要区别如下：

| 模式   | 说明                                                         |
| ------ | ------------------------------------------------------------ |
| `"a"`  | 以**追加**模式打开文件。如果文件不存在，则创建它。写入时，文件指针始终位于文件末尾，无法读文件。 |
| `"a+"` | 以**读写**追加模式打开文件。如果文件不存在，则创建它。可以读取文件内容，但写入时文件指针始终位于文件末尾。 |

关键区别：

1. **`"a"` 只能写**，不能读。
2. **`"a+"` 既可以读又可以写**，但写入时，文件指针仍然只能在文件末尾。

### 读取二进制数据：fread

```
size_t fread(void *ptr, size_t size, size_t nmemb, FILE *stream);
```

- ptr：存储读取数据的缓冲区。

- size：每个元素的字节大小

- nmemb：元素的个数。

- stream：文件流。

- 返回值：成功返回读取的数据块数。
  - 返回值的含义：fread 的返回值表示**实际成功读取的数据块数量**，具体含义取决于读取的情况：
    - **成功读取所有请求的数据块**：如果成功读取了 nmemb 个数据块，则返回 nmemb。
    - **数据不足**：如果文件中剩余的数据不足以填充 nmemb 个数据块，则返回实际读取的数据块数量（小于 nmemb）。
    - **到达文件末尾**：如果已经到达文件末尾且没有读取到任何数据，返回 0。
    - **发生错误**：如果读取过程中发生错误（例如文件流无效），也可能返回 0，此时需要调用 ferror 来确认是否有错误发生。
  - **重要点**：返回值是数据块数量，而不是直接的字节数。实际读取的字节数可以通过返回值乘以 size 计算得出，即 实际字节数 = 返回值 × size。
- **使用 fread 读取数据时，它不会自动在读入的数据末尾添加字符串结束符 `\0`**

```
size_t n = fread(buffer, 1, sizeof(buffer) - 1, fp);
buffer[n] = '\0';
printf("%s", buffer);
```

### 写入二进制数据：fwrite

```
size_t fwrite(const void *ptr, size_t size, size_t nmemb, FILE *stream);
```

- 参数与 fread 类似。

- 返回值：成功返回写入的数据块数。

```
const char *data = "Hello, Linux!";
fwrite(data, 1, strlen(data), fp);
```

### 读取一行：fgets

```
char *fgets(char *s, int size, FILE *stream);
```

- s：存储读取行的缓冲区。

- size：缓冲区大小。

- stream：文件流。

- 返回值：成功返回 s，失败或到达文件末尾返回 NULL。

- **使用 fgets：对于文本数据读取，可以考虑使用 `fgets`，它会自动在字符串末尾加上终止符。**

```
char line[256];
fgets(line, sizeof(line), fp);
```

### 写入一行：fputs

```
int fputs(const char *s, FILE *stream);
```

- s：要写入的字符串。

- stream：文件流。

- 返回值：成功返回非负值，失败返回 EOF。

```
fputs("Hello, Linux!\n", fp);
```

### 格式化读取：fscanf

```
int fscanf(FILE *stream, const char *format, ...);
```

- stream：文件流。

- format：格式字符串（如 "%d %s"）。

- 返回值：成功返回匹配的项数，失败返回 EOF。

```c
#include <stdio.h>
#include <stdlib.h>

int main() {
    FILE *file = fopen("data.txt", "r");
    if (file == NULL) {
        perror("Error opening file");
        return EXIT_FAILURE;
    }

    char name[50];
    int age;
    float height;

    while (fscanf(file, "%s %d %f", name, &age, &height) == 3) {
        printf("Name: %s, Age: %d, Height: %.2f\n", name, age, height);
    }

    fclose(file);
    return EXIT_SUCCESS;
}
```

**读取方式**

- **每次 `fscanf` 调用**，它会尝试按 `"%s %d %f"` 读取 **一组** 数据（**即 3 个值**）。
- `fscanf` 返回成功匹配的字段数，如果正好读取 **3 个值**，返回 `3`，循环继续执行；否则循环终止。

**匹配次数**

- `fscanf` **不会重复读取相同的内容**，它是 **顺序** 读取文件，每次读取 **3 个值**，然后文件指针向后移动。
- 读取成功后，下一次循环会从上次读取结束的位置继续读取，直到文件结束或匹配失败。

只要保证**每 3 个数据严格符合 `%s %d %f` 的格式**，无论数据是**分行**还是**全在一行**，`fscanf` 都能正确读取。

**关键点**

1. **数据按 `%s %d %f` 规律排列**（必须严格匹配）。
2. **数据之间用空格、制表符或换行符分隔**（`fscanf` 会自动跳过空白字符）。
3. **文件指针会继续向后读取，不会重复读取相同的数据**。

### 格式化写入：fprintf

```
int fprintf(FILE *stream, const char *format, ...);
```

- stream：文件流。

- format：格式字符串。

- 返回值：成功返回写入的字符数，失败返回负值。

```c
  1 #include <stdio.h>                                                          
  2 #include <stdlib.h>
  3 
  4 
  5 int main(void){
  6    FILE *fd = fopen("./data.txt","w");
  7    if(fd == NULL){
  8         printf("open erro");
  9         return 0;
 10    }
 11    fprintf(fd,"%s %d %.2f","wang",18,69.69);
 12    fclose(fd);
 13    return 1;
 14 }
```

### 关闭文件：fclose

```
int fclose(FILE *stream);
```

- stream：文件流。

- 返回值：成功返回 0，失败返回 EOF。

```
fclose(fp);
```

### 使用场景

- 标准IO适合需要高级功能（如格式化IO、按行读取）或处理文本文件的场景。
- 提供 stdin（标准输入）、stdout（标准输出）、stderr（标准错误）三个预定义流。
- 错误处理通过 ferror(stream) 检查，并结合 errno 获取详细信息。

## 综合示例

以下是一个涵盖C语言标准IO接口的综合示例，展示了如何使用标准IO进行文件的打开、写入、读取、定位以及错误处理等操作。标准IO是C语言标准库提供的一套高级文件操作接口，通过缓冲机制减少系统调用，提升IO效率，特别适合处理文本文件和格式化输入输出。

```c
#include <stdio.h>
#include <stdlib.h>  // 为了使用 exit()

int main() {
    // 1. 打开文件用于写入（如果文件存在则覆盖，不存在则创建）
    FILE *fp = fopen("example.txt", "w");
    if (fp == NULL) {
        perror("打开文件失败");  // 打印错误信息
        exit(1);  // 退出程序
    }

    // 2. 使用 fprintf 写入格式化数据
    fprintf(fp, "这是第 %d 行\n", 1);
    fprintf(fp, "这是第 %d 行\n", 2);

    // 3. 使用 fputs 写入一行字符串
    fputs("这是第三行\n", fp);

    // 4. 关闭文件
    fclose(fp);

    // 5. 重新打开文件用于读取
    fp = fopen("example.txt", "r");
    if (fp == NULL) {
        perror("打开文件失败");
        exit(1);
    }

    // 6. 使用 fseek 移动文件指针到文件开头（演示用法）
    fseek(fp, 0, SEEK_SET);

    // 7. 使用 fgets 逐行读取并打印文件内容
    char buffer[256];
    while (fgets(buffer, sizeof(buffer), fp) != NULL) {
        printf("%s", buffer);
    }

    // 8. 关闭文件
    fclose(fp);

    return 0;
}
```

### **1. 头文件和文件打开**

- **头文件**：包含 <stdio.h> 以使用标准IO函数，<stdlib.h> 以使用 exit()。
- **打开文件**：使用 fopen("example.txt", "w") 以写入模式打开文件 "example.txt"。如果文件存在则覆盖，不存在则创建。
- **错误处理**：检查 fopen 返回值是否为 NULL，若失败则用 perror 打印错误信息并退出。

### **2. 写入数据**

- **格式化写入**：使用 fprintf 写入格式化字符串，例如 "这是第 %d 行\n"，支持类似 printf 的占位符。
- **字符串写入**：使用 fputs 写入普通字符串，例如 "这是第三行\n"。

### **3. 关闭文件**

- 使用 fclose(fp) 关闭文件，释放相关资源。

### **4. 重新打开文件**

- 使用 fopen("example.txt", "r") 以只读模式重新打开文件，并再次检查是否成功。

### **5. 文件定位**

- 使用 fseek(fp, 0, SEEK_SET) 将文件指针移动到文件开头。虽然刚打开文件时指针已在开头，但此步骤演示 fseek 用法。
- fseek 的参数：
  - 第1个参数：文件流指针。
  - 第2个参数：偏移量（字节数）。
  - 第3个参数：起始位置（SEEK_SET 表示开头，SEEK_CUR 表示当前位置，SEEK_END 表示末尾）。

### **6. 读取数据**

- 使用 fgets(buffer, sizeof(buffer), fp) 逐行读取文件内容到缓冲区 buffer，并通过 printf 打印。
- fgets 返回 NULL 表示到达文件末尾或出错，用于控制循环。

### **7. 关闭文件**

- 再次使用 fclose(fp) 关闭文件。

### 输出结果

运行上述代码后，会生成一个名为 example.txt 的文件，内容如下：

```
这是第 1 行
这是第 2 行
这是第三行
```

### 疑问

**在示例中，文件关闭后重新以读取模式打开，文件指针已经回到了开头，所以 fseek(fp, 0, SEEK_SET) 是多余的。它出现在这里的唯一原因是为了教学或演示目的，向读者介绍 fseek 的功能和用法。在实际应用中，除非需要调整文件指针到非默认位置，否则在这种特定场景下不需要使用 fseek。**

## 其他常用标准IO函数

以下是未在示例中展示但常用的标准IO函数，可根据需要扩展使用：

**rewind**：将文件指针重置到开头。

```
void rewind(FILE *stream);
```

**ferror**：检查文件流是否发生错误。

```
int ferror(FILE *stream);
```

## 文件IO与标准IO的区别

标准IO（Standard IO）和文件IO（File IO）在处理读取大小时确实存在明显的区别，尤其是在指定读取数据量的方式上。标准IO并不要求你“精准”地指定要读取的字节数，而是传递缓冲区的大小或希望读取的数据块数量；而文件IO则需要明确指定希望读取的字节数。下面我来详细解释两者的差异。

### 文件IO：传递的是“希望读取的字节数”

文件IO是Linux系统提供的低级接口，直接通过文件描述符（File Descriptor）与内核交互。它的核心读取函数是 read：

```
ssize_t read(int fd, void *buf, size_t count);
```

**参数说明**：

- fd：文件描述符。
- buf：存储数据的缓冲区。
- count：希望读取的字节数。

**行为**：

- read 会尝试从文件中读取**最多** count 个字节的数据。
- 如果文件中剩余的数据少于 count 字节，它会读取所有剩余数据，并返回实际读取的字节数。
- 如果到达文件末尾，返回 0。

**特点**：

- 你需要**明确指定希望读取的字节数**，系统会尽力满足你的请求。
- 没有额外的缓冲层，每次调用都会直接与内核通信。
- 适合需要精确控制读取量的场景，比如处理二进制文件。

```
char buf[100];
ssize_t n = read(fd, buf, 100); // 尝试读取100字节
```

在这个例子中，read 会尽量读取100字节，但如果文件中数据不足100字节，它会返回实际读取的字节数。

### 标准IO：传递的是“缓冲区大小”或“数据块数量”

标准IO是C标准库提供的高级接口，构建在文件IO之上，内部使用缓冲区来管理数据。它的读取函数（如 fgets 和 fread）在处理读取大小时的方式与文件IO不同。

#### fgets：按行读取，指定缓冲区最大容量

```
char *fgets(char *s, int size, FILE *stream);
```

**参数说明**：

- s：存储数据的缓冲区。
- size：缓冲区的最大容量（包括终止符 \0）。
  - 他会自动加上\0所以包括

- stream：文件流。

**行为**：

- fgets 会读取一行数据（直到遇到换行符 \n 或文件末尾），但读取的字节数不会超过 size - 1（留空间给 \0）。
- 如果一行数据超过 size - 1，它会分多次读取。

**特点**：

- 你传递的是**缓冲区的最大容量**，而不是精确的读取字节数。
- 更适合文本文件，按行读取，自动处理字符串终止符。

```
char buf[100];
fgets(buf, sizeof(buf), fp); // 读取一行，最大99个字符
```

这里，fgets 会读取一行数据，但最多不超过99个字符。如果一行超过99个字符，下次调用会继续读取剩余部分。

#### fread：按块读取，指定数据块数量

```
size_t fread(void *ptr, size_t size, size_t nmemb, FILE *stream);
```

**参数说明**：

- ptr：存储数据的缓冲区。
- size：每个元素的字节大小。
- nmemb：元素的个数。
- stream：文件流。

**行为**：

- fread 尝试读取 nmemb 个大小为 size 的数据块，总共 size * nmemb 字节。
- 如果文件剩余数据不足，它会读取所有剩余数据，并返回实际读取的数据块数量。

**特点**：

- 你指定的是**希望读取的数据块数量**，但实际读取量取决于文件内容。
- 适合二进制数据或按块读取的场景。

```
char buf[100];
size_t n = fread(buf, 1, 100, fp); // 尝试读取100字节
```

这里，fread 会尝试读取100字节，但如果数据不足，返回值 n 会反映实际读取的字节数。

### 为什么标准IO不需要“精准”指定读取大小？

标准IO和文件IO在读取大小时的差异，主要源于它们的设计目标和实现机制：

1. 缓冲机制：
   - 标准IO内部使用缓冲区（通常大小为 BUFSIZ，如8192字节），会一次性从文件中读取一大块数据到缓冲区，然后按需提供给用户。
   - 这减少了对底层系统调用的依赖，提高了效率。
   - 你传递的“大小”（如缓冲区容量或数据块数量）只是一个上限，实际读取量由缓冲区管理和文件内容决定。
2. 用户友好性：
   - 标准IO旨在简化操作，比如 fgets 自动处理文本行，fread 按块读取数据。
   - 它不需要用户精确计算每次读取的字节数，而是通过缓冲区大小灵活处理。
3. 与文件IO的对比：
   - 文件IO的 read 直接与内核交互，要求你指定具体的读取字节数，适合需要精确控制的场景。
   - 标准IO则更“宽松”，通过缓冲区抽象了底层细节，适合通用场景。

## 默认的三个文件流

在 Linux（以及类 Unix 系统）中，标准 IO 默认有 **三个文件流**（file streams）：

1. **标准输入（Standard Input, `stdin`）**
   - **文件描述符**：`0`
   - **默认指向**：键盘输入
   - **用途**：程序通常从 `stdin` 读取输入，例如 `scanf()`、`fgets()` 等。
2. **标准输出（Standard Output, `stdout`）**
   - **文件描述符**：`1`
   - **默认指向**：终端（屏幕）
   - **用途**：程序通常使用 `stdout` 输出正常信息，例如 `printf()`。
3. **标准错误（Standard Error, `stderr`）**
   - **文件描述符**：`2`
   - **默认指向**：终端（屏幕）
   - **用途**：用于输出错误信息，避免与 `stdout` 混淆。例如 `fprintf(stderr, "Error!\n")`。

### **从键盘获取数据**并**写入到一个文件**

- **使用 `scanf()` 或 `fgets()` 从标准输入（`stdin`，即文件描述符 `0`）获取用户输入。**

- **使用 `fwrite()` 或 `fprintf()` 将数据写入到文件。**

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

int main() {
    char buffer[256];  // 存储用户输入的数据
    FILE *file = fopen("output.txt", "w");  // 以写模式打开文件

    if (file == NULL) {
        perror("无法打开文件");
        return EXIT_FAILURE;
    }

    printf("请输入数据（按 Enter 结束）：\n");

    // 从标准输入读取用户输入
    if (fgets(buffer, sizeof(buffer), stdin) != NULL) {
        size_t len = strlen(buffer);  // 计算实际输入的长度
        fwrite(buffer, sizeof(char), len, file);  // 只写入有效数据
    }

    fclose(file);
    printf("数据已写入 output.txt\n");

    return 0;
}
```

这里的 `fgets` 从标准输入读取数据并将其存入 `buffer` 中，直到遇到换行符或文件结束符（EOF），或者读取的字符数达到了 `sizeof(buffer) - 1` 的最大值为止。`fgets` 会自动在输入的末尾加上一个字符串结束符 `\0`。

具体地，代码中的 `len` 是通过 `strlen(buffer)` 计算的，它返回的是 **不包括结束符 `\0`** 的输入字符串的实际长度。所以如果用户输入了 10 个字符，`len` 就是 10，而不是 11（因为最后有一个结束符）。

然后，`fwrite(buffer, sizeof(char), len, file)` 会将 `buffer` 中的有效数据（即 `len` 长度的数据）写入文件。这是因为 `sizeof(char)` 始终为 1，所以 `fwrite` 的实际作用就是将 `len` 个字符写入到文件。

## fread和fwrite的区别

**`fread` 的使用：**

- `fread` 用来从文件中读取数据，它的原型是：

- ```
  size_t fread(void *ptr, size_t size, size_t count, FILE *stream);
  ```

  - `ptr` 是目标缓冲区的指针。
  - `size` 是每个数据单元的大小（例如，一个 `char` 的大小是 1 字节）。
  - `count` 是要读取的数据单元数。

- 通过 `fread`，你指定了要从文件读取的总数据量（`size * count` 字节）。即使文件中没有那么多数据，`fread` 也不会报错。它会读取尽可能多的数据，直到遇到文件结尾（EOF）或达到请求的最大字节数。

- **关键点：** 如果缓冲区比文件中的数据多，`fread` 会读取实际的数据量，剩下的部分不会被修改或写入。

**`fwrite` 的使用：**

- `fwrite` 用来将数据写入文件，它的原型是：

- ```
  size_t fwrite(const void *ptr, size_t size, size_t count, FILE *stream);
  ```

  - `ptr` 是源数据的指针。
  - `size` 是每个数据单元的大小（同样，如一个 `char` 的大小是 1 字节）。
  - `count` 是要写入的数据单元数。

- 在 `fwrite` 的情况下，你应该确保写入的**数据有效**。如果你写入的字节数超过了你实际想写入的有效数据量（例如，超出了字符串的长度），你可能会写入一些无效的数据（如多余的空字符、未初始化的内存内容等），这可能会导致问题。

- **关键点：** 在写入数据时，最好确保你只写入有效的内容。例如，在你示例中的代码中，通过 `len` 确保只写入有效的用户输入数据，而不是整个缓冲区（即使缓冲区大小是 256 字节）。

### 总结

- `fread` 在读取时，即使缓冲区大于实际读取的数据量，也不会报错，读取到的数据会在缓冲区中实际填充。
- `fwrite` 则要求你写入的是有效数据。如果写入的字节数超过了有效数据的长度，可能会写入一些不必要的“垃圾”数据。
- ```c
  #include <stdio.h>
  #include <stdlib.h>
  
  int main(){
      FILE *fp = fopen("./cat.txt", "r");
      if (fp == NULL) {
          perror("打开文件失败");
          exit(1);
      }
      
      char buf;
      int n = 0;
      while (fread(&buf, sizeof(buf), 1, fp) > 0) { // 修改这一行
          n++;
          printf("%c", buf);
      }
      printf("bytes:%d", n);
      
      fclose(fp);
      return 0;
  }
  ```

  将 `fread` 函数的参数改为 `fread(&buf, sizeof(buf), 1, fp)` 更加清晰明了，原因是：

  1. `&buf` 正确地传递了 char 变量的地址
  2. `sizeof(buf)` 清楚地表明了我们要读取的每个元素的大小（在这里是 1 字节）
  3. `1` 表示我们每次只读取一个元素
  4. 条件判断 `> 0` 正确地检查是否成功读取了元素

  这样的写法更符合 C 语言的习惯用法，也更能表达出代码的意图：每次读取一个字符，直到文件结束。对于理解和维护代码的人来说，这种写法确实更加清晰。

- **您的结果显示程序确实读取了19个字节，而不是预期的18个字节。最后一个 `n:` 后面没有显示字符，这表示文件中有一个额外的字符，很可能是一个换行符(`\n`)或回车符(`\r`)。**

  **这是非常常见的情况。当您在文本编辑器中创建文件并输入内容后，大多数编辑器会自动在文件末尾添加一个换行符。这个换行符在屏幕上通常是不可见的，但在读取文件时会被计算在内。**

  

## 其他标准IO

### ftell

- **用法**: 返回文件流的当前位置。
- 参数:
  - FILE *stream: 文件流指针。
- **返回值**: 返回当前文件位置（从文件开头起的字节数），失败返回 -1。

```
long pos = ftell(fp); // 获取当前位置
```

### feof

- **用法**: 检查文件流是否已到达文件末尾。
- 参数:
  - FILE *stream: 文件流指针。
- **返回值**: 到达文件末尾返回非零值，否则返回 0。

```
if (feof(fp)) {
    printf("已到达文件末尾\n");
}
```

### fgetc

- **用法**: 从文件流中读取一个字符。
- 参数:
  - FILE *stream: 文件流指针。
- **返回值**: 返回读取的字符（int 类型），到达文件末尾或出错时返回 EOF。

### fputc

- **用法**: 向文件流中写入一个字符。
- 参数:
  - int ch: 要写入的字符。
  - FILE *stream: 文件流指针。
- **返回值**: 成功返回写入的字符，失败返回 EOF。

```c
#include <stdio.h>

int main(void) {
    FILE *fp = fopen("output.txt", "w");
    if (fp == NULL) {
        perror("File open failed");
        return 1;
    }

    // 定义字符串数组
    const char *strings[] = {
        "Hello, Line 1\n",
        "This is Line 2\n",
        "Third line here\n",
        "End of message\n"
    };

    // 写入每一行（每个字符串）
    for (size_t i = 0; i < sizeof(strings) / sizeof(strings[0]); i++) {
        //挨个字符串，单个字符输入
        const char *str = strings[i];
        while (*str) {
            fputc(*str++, fp);
        }
    }

    fclose(fp);
    printf("Write completed.\n");
    return 0;
}
```

- `sizeof(strings)` 是**整个数组的大小**，也就是4个指针的大小。
- `sizeof(strings[0])` 是**单个字符串指针的大小**（一个指针的长度，通常是4或8个字节，取决于平台）。

- 两者一除，就得到数组里**有几个元素**，即字符串个数，等于4。

### getline

- **用法**: 从文件流中读取一行数据，并动态分配内存存储。
- 参数:
  - char **lineptr: 指向存储行的指针的指针。
  - size_t *n: 指向缓冲区大小的指针。
  - FILE *stream: 文件流指针。
- **返回值**: 成功返回读取的字节数，失败或到达文件末尾返回 -1。

```
char *line = NULL;
size_t len = 0;
ssize_t read = getline(&line, &len, fp); // 读取一行
```



### fflush

**用法**: 刷新文件流的缓冲区，将缓冲区中的数据写入文件。

**参数**:

- FILE *stream: 文件流指针。

**返回值**: 成功返回 0，失败返回 EOF。

```
fflush(fp); // 刷新缓冲区
```

