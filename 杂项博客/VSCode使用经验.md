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

