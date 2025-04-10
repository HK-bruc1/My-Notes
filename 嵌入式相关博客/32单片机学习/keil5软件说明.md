# keil5软件说明

## 源文件下的树型结构

**keil5工程创建后，每一个源文件下，都会有一个下级目录，有各种头文件的原因？**

在使用 **Keil5** 创建工程时，每个源文件下出现一个包含各种头文件的下级目录，是由于 Keil IDE 的 **"Include Dependencies"** 特性。这个特性会自动分析源文件中所包含的头文件，并以树状结构显示这些文件的依赖关系。

### 原因分析

1. **头文件的依赖分析**
   Keil5 会根据你在源文件中 `#include` 的头文件，解析这些文件的路径，并将其添加到该源文件的依赖树中。
   - 如果头文件再 `#include` 了其他头文件，也会递归分析并显示。
   - 这种方式有助于开发者了解一个源文件所依赖的全部头文件。
2. **工程管理**
   - 帮助开发者在工程中快速找到某些头文件。
   - 如果头文件或路径配置错误，可以通过依赖树快速定位问题。
3. **编译依赖关系**
   - 编译系统通过这些依赖关系来决定是否需要重新编译文件。例如，当某个头文件被修改时，所有包含该头文件的源文件会被标记为需要重新编译。

### 如何解决/调整（如果不需要显示头文件树）

如果你觉得每个源文件下的头文件目录影响了工程的清晰度，可以通过以下方法关闭此功能：

1. 在 **Project** 菜单中，选择 **Options for Target**。
2. 转到 **C/C++** 或 **Preprocessor** 选项卡，检查是否有多余的路径或头文件配置。
3. 如果需要彻底关闭头文件依赖显示：
   - 在 **File** 菜单中，取消勾选 **Show Include Files** 或类似选项。

这样，Keil 将不再显示每个源文件下的头文件依赖关系。

### 头文件依赖树的作用

尽管头文件树可能显得冗余，但它提供了一些关键功能：

- 快速查看文件依赖，方便调试编译问题。
- 理解代码结构，尤其是大型工程中头文件的复杂引用关系。
- 确保所有头文件都可以被正确找到，避免编译报错。

## 包含了头文件却找不到宏定义

动态语法检查打开了，在最前面打一个空格即可，触发检查。直接改好像不行。u8识别不了

```c
void ledX_on(u8 ledNum)
```

## keil安装目录文件结构

**我想问你一下，KEIL5的安装目录结构，我安装了C51和ARM两个版本，PACK安装了F1xx和F4xx芯片支持包**

假设KEIL5安装在默认路径 C:\Keil_v5 下，以下是主要的目录结构：

```
C:\Keil_v5
├── ARM                    # ARM开发工具链
│   ├── BIN                # ARM编译器、链接器等二进制文件
│   ├── CMSIS              # ARM CMSIS标准文件
│   ├── LIB                # ARM库文件
│   └── ...
├── C51                    # C51开发工具链（用于8051单片机）
│   ├── BIN                # C51编译器、链接器等二进制文件
│   ├── INC                # C51头文件
│   ├── LIB                # C51库文件
│   └── ...
├── UV4                    # KEIL IDE主程序文件
│   ├── UV4.exe            # uVision IDE可执行文件
│   └── ...
├── PACK                   # 芯片支持包（Device Family Pack）
│   ├── STMicroelectronics # STMicroelectronics的PACK
│   │   ├── STM32F1xx_DFP  # STM32F1系列芯片支持包
│   │   │   └── <version>  # 具体版本号
│   │   │       ├── CMSIS  # CMSIS文件
│   │   │       ├── Device # 设备相关文件（启动代码、外设驱动等）
│   │   │       └── ...
│   │   └── STM32F4xx_DFP  # STM32F4系列芯片支持包
│   │       └── <version>
│   │           ├── CMSIS
│   │           ├── Device
│   │           └── ...
│   └── ...                # 其他厂商的PACK
├── Examples               # 示例项目（可能在PACK内或单独目录）
├── Documentation          # 文档和帮助文件
└── ...
```

### 关键目录说明

1. ARM 目录
   - 这是ARM Cortex-M系列的开发工具链所在位置，包含以下子目录：
     - BIN：存放ARM编译器（如armcc或armclang）、链接器等可执行文件。
     - CMSIS：包含ARM官方的CMSIS（Cortex微控制器软件接口标准）文件。
     - LIB：存放ARM相关的库文件。
   - 这个目录支持你开发基于ARM架构的程序，例如STM32系列芯片。
2. C51 目录
   - 这是8051单片机的开发工具链所在位置，包含以下子目录：
     - BIN：存放C51编译器、汇编器等可执行文件。
     - INC：存放C51相关的头文件。
     - LIB：存放C51相关的库文件。
   - 这个目录支持你开发基于8051架构的单片机程序
3. UV4 目录
   - 包含KEIL5 IDE的主程序文件，例如 UV4.exe，这是你启动KEIL5时运行的可执行文件。
4. PACK 目录
   - 这是芯片支持包（Device Family Pack, DFP）的安装位置，用于扩展KEIL5对特定芯片的支持。
   - 你安装了STM32F1xx和STM32F4xx的支持包，它们位于：
     - PACK\STMicroelectronics\STM32F1xx_DFP：支持STM32F1系列芯片。
     - PACK\STMicroelectronics\STM32F4xx_DFP：支持STM32F4系列芯片。

### 针对你的安装情况

- **C51和ARM两个版本**
   你安装了C51和ARM工具链，因此在 C:\Keil_v5 下会有独立的 C51 和 ARM 目录，分别支持8051单片机和ARM Cortex-M系列的开发。
- F1xx和F4xx芯片支持包这两个支持包安装在 `PACK\STMicroelectronics` 下，分别对应：
  - STM32F1xx_DFP：支持STM32F1系列（如STM32F103）。
  - STM32F4xx_DFP：支持STM32F4系列（如STM32F407）。 这些文件夹包含了启动代码、外设驱动和示例项目，方便你在开发STM32F1和F4系列芯片时使用。

### 想要编译运行需要安装的东西

**所以编译ARM架构的单片机，需要下载KEIL,再下载ARMCC，对应芯片的DFP支持包。 这三者的关系为：keil本身只支持8051系列，想要支持ARM架构的单片机需要在安装一个ARMCC，DFP芯片支持包是用来选择芯片型号的，还需要一个对应单片机的固件库，里面包含了启动文件和对应芯片的时钟配置与描述**

#### **KEIL IDE**

- **KEIL** 是一个集成开发环境（IDE），最初主要支持8051系列单片机（即C51工具链）。后来，KEIL被ARM公司收购，逐渐扩展了对ARM架构的支持。
- KEIL IDE 本身是一个通用的开发平台，可以通过安装不同的工具链和支持包来支持多种微控制器架构，包括8051和ARM Cortex-M系列。
- **重要说明**：如果你想开发ARM架构的单片机（如STM32），建议直接安装 **KEIL MDK（Microcontroller Development Kit）** 版本，因为它已经包含了ARMCC工具链，专门用于ARM Cortex-M和Cortex-R系列的开发。

#### **ARMCC**

- **ARMCC** 是ARM公司提供的编译器工具链，专门用于编译ARM架构的代码。它包括编译器、链接器等工具，负责将C/C++代码编译成ARM微控制器可执行的机器码。
- 在KEIL中，如果你安装的是KEIL MDK版本，ARMCC工具链通常已经包含在内，无需额外下载。如果你安装的是KEIL C51版本（专门用于8051），则需要额外安装ARMCC工具链以支持ARM开发。

#### **DFP（Device Family Pack）**

- DFP

   是设备家族包（Device Family Pack），为特定系列的微控制器提供支持。DFP包含以下内容：

  - 启动代码（Startup Code）
  - 系统文件（System Files）
  - 头文件（Header Files）
  - 基础外设驱动（Peripheral Drivers）

- 在KEIL中，DFP允许你选择具体的芯片型号，并自动配置项目所需的启动文件和外设库。例如：

  - STM32F1xx_DFP 支持 STM32F1 系列。
  - STM32F4xx_DFP 支持 STM32F4 系列。

- DFP 是 KEIL 的软件包管理系统（Pack）的一部分，可以通过 KEIL 的 Pack Installer 轻松安装和管理。

#### **固件库（Firmware Library）**

- 固件库是为特定微控制器系列提供的预编写代码库，通常由芯片厂商提供，旨在简化外设的使用和开发。例如：
  - STMicroelectronics 为 STM32 系列提供了 **STM32Cube** 固件库。
- 固件库通常包括：
  - **启动文件**：负责系统初始化、堆栈设置等（通常由DFP提供，但固件库可能提供更灵活的版本）。
  - **时钟配置**：用于配置微控制器的时钟系统。
  - **外设驱动**：如 GPIO、UART、SPI 等的外设操作函数。
  - **示例代码**：帮助开发者快速上手。
- **重要说明**：固件库是可选的。如果你使用 DFP，DFP 已经包含了基本的启动文件和系统配置代码。但固件库（如 STM32Cube）提供了更高级的外设抽象和示例代码，方便开发者快速开发。

#### DFP与标准外设固件库的关系

**DFP与标准外设库的关系是什么？ 好像如果我不安装DFP的话，KEIL创建工程时并没有STM系列芯片选项，只有ARM系列。 安装了DFP之后，我选择STM芯片之后，会弹出一个界面，如果在这上面创建项目的话就是依赖DFP包是吗？如果我点击关闭，我从标准外设库STM32F4xx_DSP_StdPeriph_Lib_V1.4.0提取启动文件和时钟配置文件才能编译项目吧？**

DFP的作用：

- DFP是芯片支持包，包含了特定芯片系列的基本支持文件
- 提供芯片的启动文件、链接脚本、寄存器定义等底层支持
- 让KEIL能够识别并支持特定的STM32芯片型号
- 提供基本的硬件抽象层(HAL)支持

标准外设库(Standard Peripheral Library)：

- 是ST公司提供的一层固件库
- 提供了更高层的外设驱动API
- 包含了各种外设的初始化和操作函数
- 让开发者能更方便地操作外设，而不用直接操作寄存器

关于工程创建：

- 不安装DFP时只显示ARM系列是正常的，因为缺少STM32的支持文件
- 安装DFP后创建项目时弹出的界面是用于配置DFP基础支持的
- 如果使用这个界面创建，项目会基于DFP包构建
- 如果关闭该界面，则需要手动从标准外设库中添加必要文件：
  - 启动文件(startup_stm32f4xx.s)
  - 系统配置文件(system_stm32f4xx.c)
  - CMSIS核心文件
  - 外设库文件等

两种开发方式：

- 基于DFP：更现代的方式，维护更新较好
- 基于标准外设库：传统方式，**代码相对稳定**

总的来说，DFP提供基础芯片支持，而标准外设库提供了更高层的API。两者可以独立使用，也可以结合使用。你的理解是正确的，如果不使用DFP创建向导，就需要手动添加标准外设库中的必要文件。

需要补充说明的是，现在ST官方更推荐使用HAL库替代旧的标准外设库，HAL库提供了更好的兼容性和可移植性。但如果你的项目本来就是基于标准外设库的，继续使用也完全没问题。

