# VSCode使用经验

## 函数声明

在对应头文件声明一个函数后，就可以使用右键快速将同一个源文件的函数声明到对应的头文件。

## 插件

### 文档注释

**插件：Doxygen Documentation Generator**



Doxdocgen › C: Comment Prefix

The prefix that is used for each comment line except for first and last.

```c
这个配置项是用来设置在生成 C 语言（以及类似 C 语法的语言，比如 C++, Java, C# 等）风格的多行注释时，每一行注释（除了第一行和最后一行）所使用的前缀。
简单来说，它定义了注释主体部分每一行开头的符号。
假设你想要为一个函数生成 Doxygen 注释，并且你的 Doxdocgen › C: Comment Prefix 设置为默认值，通常是 * （一个空格加一个星号）。当你使用 Doxygen Documentation Generator 快捷键触发注释生成时，可能会得到类似下面的结果：
/**
 * @brief 这是函数的功能描述
 *
 * @param arg1 这是参数 arg1 的描述
 * @param arg2 这是参数 arg2 的描述
 * @return 这是函数返回值的描述
 */
void myFunction(int arg1, char arg2) {
  // 函数体
}
在这个例子中，你可以看到除了第一行 /** 和最后一行 */ 之外，中间每一行注释都以 * 开头。这个 * 就是由 Doxdocgen › C: Comment Prefix 这个配置项控制的。
```



Doxdocgen › C: Factory Method Text  (**不配置**)

Smart text snippet for factory methods/functions.

```c
这个配置项的主要目的是为了方便你在工厂方法/函数的 Doxygen 注释中添加一些特定的、有用的信息。例如，你可以使用它来提醒用户：

这个函数是一个工厂方法，用于创建特定类型的对象。
返回的对象需要在使用后进行适当的清理（例如，调用 delete 或其他释放资源的方法）。
返回的对象是某个特定类的实例。
```



Doxdocgen › C: First Line

The first line of the comment that gets generated. If empty it won't get generated at all.

```
这个配置项用于设置生成 Doxygen 注释块的第一行内容。
默认情况下，对于 C 语言风格的注释，这个设置通常是 /**。 这就是我们常见的 Doxygen 注释块的开头。
```



Doxdocgen › C: Getter Text (**不配置**)

 Smart text snippet for getters.

```c
在纯粹的 C 语言中，虽然没有类的概念，但我们仍然会编写一些函数来获取某个结构体（struct) 或模块内部数据的状态或值。这些函数在功能上类似于面向对象语言中的 Getter。

这个配置项有什么用？

Doxdocgen › C: Getter Text 这个配置项的主要目的是为了方便你在这类 "Getter" 函数的 Doxygen 注释中添加一些特定的、有用的信息。例如，你可以使用它来提醒用户：

这个函数用于获取某个特定变量或状态的值。
返回的值是只读的。
返回的值可能受到某些内部状态的影响。
```



Doxdocgen › C: Last Line 

 The last line of the comment that gets generated. If empty it won't get generated at all.

```
这个配置项用于设置生成 Doxygen 注释块的最后一行内容。
默认情况下，对于 C 语言风格的注释，这个设置通常是 */。 这就是我们常见的 Doxygen 注释块的结尾。
```



Doxdocgen › C: Setter Text （**不配置**）

 Smart text snippet for setters.

```
类似于 Getter 函数，在面向对象的编程语言中，Setter 方法通常是类的一部分，用于修改（设置）对象的私有成员变量的值。

在纯粹的 C 语言中，我们也会编写一些函数来修改某个结构体（struct) 或模块内部数据的状态或值。这些函数在功能上类似于面向对象语言中的 Setter。
```



Doxdocgen › C: Trigger Sequence 

 Doxygen comment trigger. This character sequence triggers generation of Doxygen comments.

```
这个配置项定义了触发 Doxygen 注释自动生成的字符序列。换句话说，当你输入这个特定的字符序列并按下 Enter 键时，扩展就会尝试为你当前光标所在的代码元素（例如函数、结构体、变量等）生成一个 Doxygen 风格的注释块。

默认情况下，对于 C 语言风格，这个触发序列通常是 /**。
```



Doxdocgen › Cpp: Ctor Text （**不配置**）

 Smart text snippet for constructors.

```
这个配置项允许你为 C++ 类 (class) 的构造函数 (constructor) 定义一个智能文本片段（snippet），当使用 Doxygen Documentation Generator 为这类构造函数生成注释时，这个文本片段会被自动插入到注释中。
```



Doxdocgen › File: Copyright Tag 

 File copyright documentation tag. Array of strings will be converted to one line per element. Can template {year}.

```c
这个配置项用于设置在生成文件（file）的 Doxygen 文档时，自动添加的版权（copyright）标签及其内容。

主要特点：

数组形式： 这个配置项接受一个字符串数组（Array of strings）。数组中的每个字符串元素都会在生成的 Doxygen 文档中占据一行。这允许你创建多行的版权声明。
{year} 模板： 你可以在版权声明的字符串中使用 {year} 这个模板变量。当生成文档时，这个变量会被自动替换为当前的年份。
假设你有一个名为 my_file.c 的文件，并且你配置了上述的 copyrightTag。当你为这个文件生成 Doxygen 文档时，可能会在文件的注释块中看到类似下面的内容：
/**
 * @file my_file.c
 * @brief This file contains some important functions.
 *
 * @copyright Copyright (c) 2025 Your Company Name
 * @copyright All rights reserved.
 */
    
配置：
    "doxdocgen.file.copyrightTag": [
    	"Copyright (c) {year} Bruce",
    	"All rights reserved."
]
```



Doxdocgen › File: Custom Tag 

 Additional file documentation. Array of strings will be converted to one line per element. Can template {year}, {date}, {author}, {email} and {file}. You have to specify the prefix.

```c
这个配置项允许你向文件的 Doxygen 文档中添加自定义的标签。与 Copyright Tag 不同，你需要自己指定标签的前缀（例如 @author, @version, @date 等）。

配置：
"doxdocgen.file.customTag": [
    "@author {author}",
    "@date {date}"
]
```



Doxdocgen › File: File Order （**不配置**）

 The order to use for the file comment. Values can be used multiple times. Valid values are file, author, brief, version, date, empty, copyright and custom.

```
这个配置项允许你自定义在生成文件（file）的 Doxygen 注释块时，各个部分的排列顺序。
Doxygen Documentation Generator 扩展通常会有一个默认的文件注释顺序。如果你不配置 File Order，它会使用这个默认顺序。
```



Doxdocgen › Generic: Bool Returns True False（**不配置**） 

 If this is enabled, the documentation for a bool return value will be split into true and false entries.

```c
当这个设置被启用（enabled）时，对于返回 bool 类型的函数，生成的 Doxygen 文档将会把 true 和 false 两种返回值的情况分别进行描述。

对于纯粹使用 int 来表示布尔值的 C 代码，Doxdocgen 扩展的这个设置可能不会像在 C++ 中那样直接生效。 扩展很可能基于代码的语法类型（是否使用了 bool 关键字）来判断是否应用这个设置。

然而，如果你的 C 代码中使用了 <stdbool.h> 引入的 bool 类型，那么这个设置可能会起作用。 扩展可能会将 bool 识别为布尔类型，并按照设置进行处理。
```



Doxdocgen › Generic: Brief Template （**不配置**）

 The template of the brief Doxygen line that is generated. If empty it won't get generated at all.

```
这个配置项允许你自定义生成 Doxygen 注释时 @brief 标签行的模板。@brief 标签用于提供被文档化的元素（例如函数、类、变量、文件等）的简短描述，通常只有一行。
```



Doxdocgen › Generic: Command Suggestion 

 Provide intellisense and snippet for doxygen commands

```c
这个配置项控制着是否在你的 VS Code 编辑器中启用 Doxygen 命令的智能感知（intellisense）和代码片段（snippet）功能。

什么是智能感知 (Intellisense)？

在 VS Code 中，智能感知是一种功能，它在你编写代码时提供实时的建议和帮助。对于 Doxygen 命令来说，当你在注释块中输入 @ 符号时，如果启用了这个设置，VS Code 应该会：

弹出建议列表： 显示可用的 Doxygen 命令（例如 @param, @return, @brief 等）。
提供命令的简要说明： 当你选择一个命令时，可能会显示该命令的用途和语法。
自动完成： 允许你通过选择列表中的建议来快速完成 Doxygen 命令的输入。
```



Doxdocgen › Generic: Command Suggestion Add Prefix （**不启用**）

 Add \ in doxygen command suggestion for better readbility (need to enable commandSuggestion)

```
这个设置是用来控制在使用 Doxygen 命令建议（你刚才问的 Doxdocgen › Generic: Command Suggestion）时，是否在建议的命令前面添加一个反斜杠 (\) 作为前缀。

重要前提： 这个设置只有在你启用了 Doxdocgen › Generic: Command Suggestion 之后才会生效。如果命令建议被禁用，那么这个设置也就没有意义了。

Doxygen 命令的前缀：

在 Doxygen 中，你可以使用两种字符作为命令的前缀：

@ 符号 (例如：@param, @brief)
反斜杠 \ (例如：\param, \brief)
```



Doxdocgen › Generic: Custom Tags （**不配置**）

 Custom tags to be added to the generic order. One tag per line will be added. Can template {year}, {date}, {author}, {email} and {file}. You have to specify the prefix.

```
这个设置允许你定义额外的自定义 Doxygen 标签，这些标签可以随后在你的代码注释中使用。
```



Doxdocgen › Generic: Filtered Keywords 

 Array of keywords that should be removed from the input prior to parsing.

```c
这个配置项允许你定义一个关键词数组，这些关键词会在 Doxygen Documentation Generator 扩展解析你的代码之前被移除。

主要特点：

数组形式： 这个设置的值是一个字符串数组。
移除关键词： 扩展会在尝试理解你的代码结构（例如函数名、参数名、返回值类型等）以生成合适的 Doxygen 注释之前，先从代码中移除列表中的所有关键词。
这个设置有什么用？

在某些情况下，你的代码中可能包含一些特定的关键词，这些关键词可能会干扰 Doxygen Documentation Generator 扩展的解析过程，或者你希望在生成的文档中忽略它们。通过将这些关键词添加到 Filtered Keywords 列表中，你可以确保它们不会影响到自动生成的文档。

一些可能的使用场景：

宏定义： 某些宏定义可能看起来像函数或变量，但你并不希望为它们生成文档，或者它们会误导扩展的解析。你可以将这些宏的名称添加到过滤列表中。
特定语言的保留字或关键字： 虽然扩展通常会处理编程语言的关键字，但在某些特殊情况下，你可能需要手动添加一些。
自定义标记或注释： 如果你的代码中使用了特定的注释标记或关键词，并且你希望在 Doxygen 文档中忽略它们，你可以将它们添加到过滤列表中。
```



Doxdocgen › Generic: Generate Smart Text 

 Decide if you want to get smart text for certain commands.

```c
这个设置决定了你是否希望扩展为某些 Doxygen 命令生成更智能的、上下文相关的文本。

什么是 "智能文本"？

智能文本是指扩展在生成 Doxygen 注释时，不仅仅插入一些基本的占位符，而是会尝试根据你正在注释的代码元素的类型和上下文，自动生成一些更有意义的描述或提示。

例如，对于以下情况，智能文本可能包括：

函数参数 (@param): 扩展可能会尝试提取参数的名称和类型，并为你生成一个包含这些信息的 @param 行，例如：@param int count The number of items.
返回值 (@return): 扩展可能会尝试提取返回类型，并生成一个包含返回类型信息的 @return 行，例如：@return int The result of the operation.
构造函数、析构函数、Getter、Setter、工厂方法： 正如你之前问到的 Ctor Text, Getter Text, Setter Text, Factory Method Text 这些设置，它们就是用来定义这些特定类型函数的 "智能文本" 片段。当 "Generate Smart Text" 启用时，扩展会使用这些配置的文本。
当这个设置被启用（设置为 true）时：

扩展会尝试根据代码上下文和你的配置，为 Doxygen 命令生成更详细和有用的初始文本。
你可能会看到更多的信息被自动填充到你的 Doxygen 注释中，从而减少你手动编写的工作量。
当这个设置被禁用（设置为 false）时：

扩展可能会生成更基本的 Doxygen 注释，通常只包含标签和一些通用的占位符，例如：@param paramName, @return.
你可能需要更多地手动编写注释的内容。
```



Doxdocgen › Generic: Include Type At Return

Whether include type information at return.

```
这个设置决定了在为函数生成 Doxygen 注释时，是否在 @return 标签后面自动包含函数的返回类型信息。
```



Doxdocgen › Generic: Lines To Get (**不配置**)

 How many lines the plugin should look for to find the end of the declaration. Please be aware that setting this value too low could improve the speed of comment generation by a very slim margin but the plugin also may not correctly detect all declarations or definitions anymore.

```
当你想为一个函数、类、结构体或其他代码元素生成 Doxygen 注释时，Doxygen Documentation Generator 扩展需要知道这个代码元素的完整范围，以便正确地提取名称、参数、返回值等信息。这个设置就决定了扩展会向下搜索多少行来找到这个范围的末尾。
```



Doxdocgen › Generic: Order（**不配置**）

The order to use for the comment generation. Values can be used multiple times. Valid values are `brief`, `empty`, `tparam`, `param`, `return`, `custom`, `author`, `date`, `version` and `copyright`.

```
这个配置项允许你自定义在生成函数、方法等代码元素的 Doxygen 注释块时，各个 Doxygen 标签的排列顺序。
Doxygen Documentation Generator 扩展通常会有一个默认的注释顺序。如果你不配置 Order，它会使用这个默认顺序。常见的默认顺序可能类似于：["brief", "param", "return"]。
```

## VSCode Remote - SSH原理

VSCode Remote - SSH 的核心思想是将 VSCode 的用户界面（UI）运行在你的本地计算机（例如 Windows），而将代码、语言服务、调试器和其他扩展运行在远程服务器（例如 Linux）上。它们之间通过安全的 SSH 连接进行通信。

其工作原理可以概括为以下几个步骤：

1. **建立 SSH 连接：**
   - 你需要在本地计算机上安装并配置好 SSH 客户端。
   - 在 VSCode 中，你使用 Remote - SSH 扩展提供的界面配置要连接的远程 Linux 服务器的 SSH 连接信息，包括主机名或 IP 地址、端口（默认为 22）、用户名以及可能的密码或密钥。
   - 当你尝试连接时，VSCode 会使用本地的 SSH 客户端与远程服务器建立一个安全的 SSH 连接。
2. **在远程服务器上安装 VSCode Server：**
   - 首次连接到新的远程服务器时，VSCode 会自动检测服务器上是否已安装 VSCode Server。
   - 如果未安装，VSCode 会通过已建立的 SSH 连接，将一个轻量级的 VSCode Server 版本上传并安装到远程服务器上。这个 Server 版本是 headless 的，不包含用户界面，主要负责与本地 VSCode 实例进行通信，并执行代码相关的操作。
3. **本地 VSCode 与远程 VSCode Server 通信：**
   - 一旦 VSCode Server 在远程服务器上运行起来，本地的 VSCode 实例就会通过 SSH 连接与它建立双向通信通道。
   - 这个通信通道用于传输各种信息，包括：
     - **文件系统操作：** 当你在本地 VSCode 中浏览、编辑远程文件时，这些操作会通过 SSH 连接发送给远程的 VSCode Server，Server 会在远程文件系统上执行相应的操作。
     - **语言服务：** 当你编写代码时，本地 VSCode 会将代码信息发送给远程的 VSCode Server，Server 会利用其上的语言服务（例如 IntelliSense、代码检查等）进行分析，并将结果返回给本地 VSCode 进行显示。
     - **构建和调试：** 当你执行构建或调试命令时，这些命令实际上是在远程服务器上由 VSCode Server 启动和管理的。相关的输出、错误信息和调试信息会通过 SSH 连接回传到本地 VSCode。
     - **扩展运行：** 大部分的扩展也会在远程服务器上运行，这样它们可以直接访问远程环境和资源。只有一些与用户界面直接相关的扩展会在本地运行。
     - **终端：** 当你在 VSCode 中打开集成终端时，实际上是通过 SSH 连接在远程服务器上创建了一个终端会话。
4. **用户体验：**
   - 尽管代码和大部分操作都在远程服务器上进行，但由于 VSCode 及其 Remote - SSH 扩展的优化，用户在本地操作时感觉就像是在直接操作本地文件一样，具有良好的响应速度和流畅性。

**总结来说，VSCode Remote - SSH 的核心原理是利用 SSH 建立安全连接，并在远程服务器上运行一个 headless 的 VSCode Server，通过本地 VSCode 客户端与远程 Server 之间的通信，实现了在本地编辑和操作远程代码的效果。**

**在 Windows 下进行远程 Linux 开发**

正如前面所说，VSCode Remote - SSH 完全支持从 Windows 客户端连接到 Linux 服务器进行开发。你只需要在你的 Windows 电脑上安装 VSCode 和 Remote - SSH 扩展，并配置好连接到 Linux 服务器的 SSH 信息即可。

**使用 Windows 进行远程 Linux 开发的优势：**

- **利用 Windows 的桌面环境：** 你可以在熟悉的 Windows 操作系统下使用 VSCode 进行开发，无需切换操作系统。
- **强大的 VSCode 功能：** 你可以享受到 VSCode 提供的所有强大功能，包括代码编辑、智能感知、调试、版本控制等等。
- **访问远程 Linux 环境：** 你可以直接访问和操作远程 Linux 服务器上的文件、运行环境和工具。
- **统一的开发体验：** 无论你的代码是在本地还是远程，你都可以在 VSCode 中获得一致的开发体验。

**配置步骤（简要）：**

1. **在 Windows 上安装 VSCode。**
2. **在 VSCode 中安装 "Remote - SSH" 扩展。**
3. **点击 VSCode 左侧的远程资源管理器图标（或者使用命令面板搜索 "Remote-SSH: Connect to Host..."）。**
4. **选择 "Add New SSH Host..." 并输入你的 Linux 服务器的 SSH 连接信息（例如 `user@hostname_or_ip`）。**
5. **选择你添加的主机并点击 "Connect"。**
6. **根据提示输入密码或选择密钥文件（如果需要）。**

连接成功后，你就可以像操作本地文件一样浏览和编辑远程 Linux 服务器上的代码了。



**代码是保存在Linux服务器还是windows下的vscode? 如果保存在Linux服务器岂不是连烧录代码可以不用了？**

**代码是保存在 Linux 服务器上的。**

当你使用 VSCode Remote - SSH 连接到远程 Linux 服务器时，你本地的 Windows 上的 VSCode 只是一个用户界面。你打开、编辑和保存的所有文件都直接存储在远程 Linux 服务器的文件系统中。

可以把这个过程想象成你通过一个远程桌面连接到一台 Linux 电脑。虽然你在本地的 Windows 电脑上操作，但实际的文件和应用程序都在远程的 Linux 电脑上运行。

**关于“连烧录代码可以不用了”的理解：**

你的理解在某些情况下是正确的，但在其他情况下可能需要更细致的区分。这取决于你所说的“烧录代码”具体指的是什么场景。

- **如果你指的是在 Linux 服务器上直接运行和测试代码：** 那么是的，使用 VSCode Remote - SSH 后，你就可以直接在 Linux 服务器上进行代码的编辑、编译（如果需要）、运行和调试。你不需要将代码从 Linux 服务器下载到本地再进行操作。这省去了在本地和服务器之间来回传输代码的步骤。
- **如果你指的是将代码部署到特定的硬件设备（例如嵌入式系统、单片机等）：** 那么 **不一定** 可以不用烧录代码。VSCode Remote - SSH 让你方便地在 Linux 服务器上开发和构建针对这些硬件的代码，但最终将代码部署到目标硬件设备上通常仍然需要一个独立的“烧录”或“刷写”过程。这个过程可能涉及到特定的工具和协议，例如：
  - 使用 USB、JTAG、SWD 等接口连接到目标硬件。
  - 使用特定的烧录工具（例如 OpenOCD、ST-Link Utility 等）将编译好的固件文件传输到硬件设备的存储器中。

**总结来说：**

- VSCode Remote - SSH 主要解决了在远程 Linux 服务器上进行**开发**的问题，包括代码编辑、构建、运行和调试。
- 如果你开发的目标是在 Linux 服务器上运行的应用程序或服务，那么确实可以省去将代码下载到本地再操作的步骤。
- 如果你开发的目标是运行在特定硬件上的固件，那么 VSCode Remote - SSH 可以极大地简化你在 Linux 环境下的开发过程，但最终将代码部署到硬件上通常仍然需要一个独立的烧录步骤。

### 插件配置

- **Remote - SSH:** **这是你需要的插件。** 它允许你通过 SSH 连接到远程服务器，并在本地 VSCode 中像操作本地文件一样进行开发。
- **Remote - SSH: Editing Configuration Files:** 这个插件本身并不是一个独立的扩展，它通常是 "Remote - SSH" 扩展的一部分功能，用于方便你编辑本地的 SSH 配置文件。
- **Remote - Tunnels:** 这个扩展允许你通过 VSCode 的服务创建一个隧道连接到远程机器，而无需配置 SSH。这对于某些网络环境或临时访问非常方便。
- **Remote Development:** 这是一个扩展包，它包含了所有主要的 VSCode Remote 功能，包括 **Remote - SSH**、Remote - Containers 和 Remote - WSL。如果你安装了这个扩展包，那么 "Remote - SSH" 也会被安装。
- **Remote Explorer:** 这个扩展提供了一个用户界面，用于管理你的远程连接、容器和 WSL (Windows Subsystem for Linux) 环境。它通常与 "Remote - SSH" 等扩展一起使用。
- **Remote Repositories:** 这个扩展允许你直接在 VSCode 中浏览和编辑存储在远程 Git 仓库（如 GitHub 或 Azure Repos）中的代码，而无需将整个仓库克隆到本地。

**总结一下，为了通过 SSH 连接到远程 Linux 服务器进行开发，你应该安装以下其中一个：**

1. **直接安装 "Remote - SSH" 扩展。** 这是最直接的方式，如果你只需要 SSH 功能。
2. **安装 "Remote Development" 扩展包。** 这个包包含了所有 Remote 功能，如果你未来可能需要使用其他 Remote 功能（例如 Docker 容器开发），安装这个包会更方便。

**推荐你至少安装 "Remote - SSH" 扩展。** 如果你对其他的 Remote 功能也感兴趣，可以考虑安装 "Remote Development" 扩展包。

### 疑问

**vscode remote ssh相当于在ssh中再安装vscode?**

VS Code Remote SSH **不是**在 SSH 会话中再安装一个完整的 VS Code 应用程序。它的工作方式更巧妙和轻量级。

更准确地说，VS Code Remote SSH 的工作原理如下：

1. **本地运行完整的 VS Code 应用程序：** 你在你的本地计算机上运行的是你已经安装好的完整 VS Code 应用程序。
2. **SSH 连接：** Remote SSH 扩展通过标准的 SSH 协议连接到你的远程 Linux 服务器。
3. **在远程服务器上安装一个轻量级的 "VS Code Server"：** 当你第一次通过 Remote SSH 连接到远程服务器时，VS Code 会自动在远程服务器上安装一个轻量级的后台服务，这个服务通常被称为 "VS Code Server"。**注意，这并不是一个完整的 VS Code GUI 应用程序。**
4. **本地 VS Code 与远程 Server 通信：** 本地的 VS Code 应用程序通过 SSH 连接与远程服务器上的 "VS Code Server" 进行通信。
5. **功能转发：** 本地的 VS Code 将你的操作（例如打开文件、编辑代码、运行命令等）通过 SSH 发送给远程的 "VS Code Server"。远程 Server 在 Linux 服务器上执行相应的操作，并将结果（例如文件内容、命令输出、调试信息等）通过 SSH 返回给本地的 VS Code。
6. **本地渲染：** 本地的 VS Code 接收到远程 Server 返回的数据后，将其渲染在你的本地 VS Code 界面上。

**因此，你可以将 Remote SSH 理解为：**

- **本地是前端 (Client)：** 负责显示用户界面和接收用户输入。
- **远程是后端 (Server)：** 负责实际的文件存储、代码执行、编译、调试等操作。

**主要区别在于：**

- **没有在远程服务器上运行一个完整的 GUI VS Code 应用程序。** 这意味着你不需要在远程服务器上安装图形界面环境，也不需要进行 X 转发等操作。
- **本地 VS Code 负责所有的用户界面渲染和交互。** 这保证了良好的用户体验和性能。
- **远程服务器上只运行一个轻量级的服务进程。** 这对远程服务器的资源消耗很小。



**但是我本地的vscode插件连接上后，分成了本地和ssh插件？ 远程也要安装插件？**

你观察得很对！当你使用 VS Code Remote SSH 连接上远程服务器后，你会注意到 VS Code 的扩展管理界面确实会将插件分为 "本地" 和 "SSH: [你的连接名称]" 两个部分。

这是 VS Code Remote SSH 的一个重要特性，它的设计目的是为了提供最佳的性能和体验。

**原因和工作原理如下：**

1. **性能优化：** 某些 VS Code 插件（例如主题、界面美化、键盘映射等）主要负责用户界面的显示和交互，它们只需要在你的本地计算机上运行即可。将这些插件安装在本地可以减少通过 SSH 连接传输的数据量，提高响应速度。
2. **远程环境依赖：** 另一些插件（例如语言支持、代码格式化、代码检查（linting）、调试器等）需要与远程服务器上的开发环境进行交互，例如访问远程文件系统、运行远程的编译器或解释器、连接到远程的调试进程等。这些插件需要在远程服务器上运行才能正常工作。
3. **自动同步和管理：** 当你第一次连接到远程服务器时，VS Code 会检测你本地安装的插件。对于需要在远程运行的插件，VS Code 会自动尝试在远程服务器上安装或启用相应的版本。你也可以在扩展管理界面中手动选择哪些插件应该在本地启用，哪些应该在远程启用。
4. **独立管理：** 这样分开管理使得你可以为你的本地开发环境和不同的远程开发环境配置不同的插件组合。例如，你可能在本地使用某个特定的主题，但在远程服务器上使用默认主题。或者，你可能只在特定的远程服务器上需要某种语言的特定插件。



**远程虽然不是完整的vscode但是可以安装插件？ 那本地插件的作用在哪里？那还会轻量级吗？**

**远程虽然不是完整的 VS Code 但是可以安装插件？**

是的，你说的是对的。虽然远程服务器上运行的 "VS Code Server" 不是一个带有完整图形界面的 VS Code 应用程序，但它仍然具备**托管和运行扩展**的能力。你可以把它想象成一个运行在远程服务器上的插件宿主进程。

这个 "VS Code Server" 能够安装和运行那些需要直接与远程文件系统、运行环境或工具链交互的插件。例如：

- **语言服务器 (Language Servers):** 提供代码补全、语法检查、跳转到定义等功能。这些服务器通常需要运行在代码所在的机器上。
- **调试器 (Debuggers):** 连接到远程运行的程序进行调试。
- **Linter 和 Formatter:** 对远程代码进行风格检查和格式化。
- **构建工具集成:** 运行远程的构建命令（如 `make`、`cargo build` 等）。

**那本地插件的作用在哪里？**

本地插件的作用主要集中在以下几个方面：

- **用户界面 (UI) 相关的增强：** 例如主题、图标主题、界面美化、状态栏自定义等，这些只需要在你的本地 VS Code 客户端上运行即可。
- **客户端行为的修改：** 例如键盘映射、代码片段（如果不需要访问远程文件）、某些类型的辅助功能插件等。
- **与本地系统集成的功能：** 例如某些通知插件、与本地剪贴板交互的插件等。

**那还会轻量级吗？**

是的，即使远程服务器上可以安装插件，"VS Code Server" 本身仍然是相对轻量级的，原因如下：

- **按需安装：** 远程服务器上只会安装你实际在当前远程会话中需要的插件。如果你连接到不同的远程服务器或项目，可能只需要安装不同的插件子集。
- **没有 GUI 开销：** 最关键的是，远程服务器上运行的 "VS Code Server" **没有图形用户界面**。GUI 是非常消耗资源的。它只是一个在后台运行的进程，负责处理来自本地 VS Code 的请求并执行相应的操作。
- **专门为远程开发优化：** "VS Code Server" 的设计目标就是为了高效地处理远程开发任务，它会尽量减少不必要的资源消耗。



**我编写了一段代码，可以使用vscode自带的编译与调试吗？ 难道需要打开终端再编译运行？这是否有一点麻烦？**

是的，你**可以**使用 VS Code 自带的编译与调试功能，而**不一定需要**每次都打开终端再手动编译运行。Remote SSH 完美支持 VS Code 的这些集成功能。

**你需要进行一些配置，但配置好之后就会非常方便：**

1. **配置编译任务 (tasks.json):**
   - VS Code 的 "任务" 功能允许你定义自定义的构建或编译步骤。
   - 你可以通过 `Ctrl+Shift+P` (或 `Cmd+Shift+P`) 打开命令面板，搜索并选择 "配置任务"。
   - VS Code 会询问你如何创建 `tasks.json` 文件，你可以选择 "Create tasks.json from template" 并选择你的编程语言对应的模板（例如 "C/C++ (gcc)", "C/C++ (g++)", "Go", "Rust" 等）。
   - 在 `tasks.json`文件中，你需要修改 `command`字段来指定你的编译命令，例如：
     - **C/C++ (gcc):** `"command": "/usr/bin/gcc"`，`"args": ["${file}", "-o", "${fileBasenameNoExtension}"]`
     - **C/C++ (g++):** `"command": "/usr/bin/g++"`，`"args": ["${file}", "-o", "${fileBasenameNoExtension}"]`
     - **Go:** `"command": "go"`, `"args": ["build", "-o", "${fileBasenameNoExtension}"]`
     - **Rust:** `"command": "cargo"`, `"args": ["build"]`
   - 你还可以配置其他参数，例如工作目录、问题匹配器等。
2. **配置调试 (launch.json):**
   - 要使用调试功能，你需要配置 `launch.json` 文件。
   - 打开命令面板 (`Ctrl+Shift+P` 或 `Cmd+Shift+P`)，搜索并选择 "打开 launch.json"。
   - 选择你的调试环境（例如 "C++ (GDB/LLDB)", "Go", "Rust" 等）。
   - 在 `launch.json` 文件中，你需要配置 `program` 字段来指定你的可执行文件路径。通常可以使用 `${workspaceFolder}/${fileBasenameNoExtension}` 或类似的变量。
   - 你还可以配置其他调试选项，例如启动参数、断点等。

**配置完成后，你就可以使用以下便捷的方式进行编译和调试：**

- 编译:
  - 你可以通过菜单栏的 "终端" -> "运行任务..." 选择你配置的编译任务来执行编译。
  - 或者，你可以在 `tasks.json` 中配置默认的构建任务，然后使用快捷键 (`Ctrl+Shift+B` 或 `Cmd+Shift+B`) 来执行。
- 调试:
  - 你可以在代码中设置断点。
  - 点击 VS Code 侧边栏的 "运行和调试" 图标 (小虫子图标)。
  - 点击绿色的 "启动调试" 按钮，VS Code 将会编译你的代码（如果配置了预启动任务），然后启动调试器并停在你设置的断点处。
  - 你可以使用调试控制面板来单步执行代码、查看变量等。

**这确实比每次都打开终端手动输入命令要方便得多，也更符合现代 IDE 的使用习惯。**
