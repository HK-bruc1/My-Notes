# HAL库与cubeMX

## 前置

**在有STM32单片机基础以及常用外设的使用经验后，会使用官方固件库中的标准外设库。接下来可以快速学习HAL库和cubeMX工具吗？**

### HAL库和CubeMX简介

- **HAL库（Hardware Abstraction Layer，硬件抽象层）**
   HAL库是STM32提供的一种现代软件库，通过提供统一的高级API，简化了外设的配置和使用。您无需深入操作底层寄存器，只需调用HAL库函数即可实现功能。相比SPL，HAL库更加模块化，适用于快速开发和跨平台移植。
- 
- **CubeMX**
   CubeMX是STM32Cube套件中的图形化配置工具。它允许您通过界面配置STM32的引脚、外设和时钟，并自动生成基于HAL库的初始化代码。这大大减少了手动编写配置代码的工作量，适合快速搭建项目框架。

### 为什么可以快速学习？

1. 已有SPL基础的帮助
   - SPL和HAL库的核心思想类似，都是为了简化外设操作，提供抽象层。
   - 您熟悉的SPL经验（例如GPIO、UART、定时器等外设的使用）可以直接迁移到HAL库的学习中，尽管API不同，但底层逻辑是相通的。
2. HAL库和CubeMX的设计目标
   - HAL库和CubeMX旨在提高开发效率，降低学习门槛。
   - CubeMX的图形化操作直观易懂，生成的代码结构清晰，适合有基础的用户快速上手。
3. 丰富的学习资源
   - ST官方提供了详细的HAL库文档、示例代码和CubeMX教程，加上活跃的社区支持，您可以快速找到学习资料和解决问题的方法。

### 如何快速学习？

以下是一个清晰的学习路径，帮助您高效掌握HAL库和CubeMX：

1. 了解HAL库的基本概念和架构

- **目标**：掌握HAL库的设计原则和API使用方法。
- **方法**：阅读ST官方的HAL库文档，查看示例代码，理解HAL库与SPL的异同（例如HAL强调回调函数和错误处理）。
- **建议**：从简单的模块（如GPIO）开始，熟悉HAL函数的命名规则和调用方式。

#### 2. 学习CubeMX的使用

- **目标**：掌握CubeMX的配置流程和代码生成机制。
- 方法：下载并安装STM32CubeMX，打开工具后尝试：
  - 配置引脚（例如将某个引脚设为GPIO输出）。
  - 设置时钟树（确保外设时钟正确）。
  - 生成代码并在IDE中打开。
- **练习**：用CubeMX配置一个LED闪烁程序，查看生成的代码结构（如main.c中的初始化函数）。

#### 3. 对比SPL和HAL库

- **目标**：通过对比加深对HAL库的理解。
- **方法**：挑选一个您用SPL写过的简单程序（例如UART发送数据），用HAL库重写。
- **注意**：观察HAL库在中断管理、状态机设计上的改进，适应新API的风格。

#### 4. 实践项目

- **目标**：通过动手巩固知识。
- 建议：
  - 用CubeMX配置一个UART通信程序，实现数据的收发。
  - 尝试更复杂的外设，如SPI、I2C或ADC，逐步提升能力。
- **技巧**：结合HAL库的用户手册，边写代码边查阅函数说明。

------

### 学习难度与时间

- **学习曲线**：由于您已有STM32和SPL的基础，HAL库和CubeMX的学习曲线不会太陡峭。SPL到HAL的迁移主要是API的适应，核心逻辑变化不大。
- 预计时间：
  - 熟悉HAL库和CubeMX的基本使用：1-2周（每天1-2小时）。
  - 能够独立开发中等复杂度的项目：1个月左右（取决于实践量）。

### 目前基于HAL库开发的工具链有哪一些？

目前基于HAL库（Hardware Abstraction Layer）开发的工具链主要包括以下几种：

#### 1. **STM32CubeIDE**

- 由STMicroelectronics官方提供，专为STM32系列微控制器设计。
- 集成了STM32CubeMX工具，支持图形化配置和代码生成，基于HAL库开发。
- 提供完整的开发环境，包括编辑器、编译器、调试器等。

### 2. **Keil MDK（Microcontroller Development Kit）**

- 流行的ARM微控制器开发工具，支持STM32系列。
- 可与STM32CubeMX结合使用，生成基于HAL库的代码后在Keil MDK中开发和调试。

### 3. **IAR Embedded Workbench**

- 专业的嵌入式开发环境，支持STM32微控制器。
- 同样可与STM32CubeMX配合，生成HAL库代码后进行开发。

### 4. **GCC + Eclipse**

- 开源工具链，使用GCC编译器和Eclipse IDE。
- 可通过STM32CubeMX生成代码后，在Eclipse中配置GCC进行开发，适合对成本敏感的项目。

### 5. **PlatformIO**

- 开源的嵌入式开发平台，支持STM32和HAL库。
- 可与Visual Studio Code等编辑器集成，提供灵活的开发环境。

## SW和JTAG两种DEBUG模式的区别

**SW和JTAG两种DEBUG模式的区别？ 我使用STLink下载，为什么选SW?**

JTAG 和 SW（SWD，即 Serial Wire Debug）都是 ARM Cortex-M 系列微控制器常用的调试接口，但它们有几个主要区别：

- **引脚数量：**
  - **JTAG** 需要使用至少 4~5 根引脚（TCK、TMS、TDI、TDO 和可选的 TRST），
  - **SWD** 仅需 2 根主要引脚（SWDIO 和 SWCLK），这对于引脚资源有限的系统来说更友好。
- **调试功能与复杂度：**
  - **JTAG** 提供更全面的调试和边界扫描功能，可以同时调试多个器件，但也因此连线较复杂。
  - **SWD** 虽然功能上相对简化，但已能满足大部分常规调试需求，并且实现更简单、连线更紧凑。
- **兼容性与应用：**
  - 许多 STM32 等 Cortex-M 微控制器默认和推荐使用 SWD 调试，因为这不仅节省引脚，还避免了与其他外设复用引脚时可能产生的冲突。
  - 部分器件在 JTAG 模式下可能会占用部分引脚或存在兼容性问题，而 SWD 模式则通常更稳定可靠。

对于使用 STLink 下载时选择 SW 模式，原因主要在于：

1. **资源节省和布线简化：** SWD 只需要两根调试线，可以减少硬件设计的复杂度。
2. **默认调试模式：** 很多 STM32 微控制器在出厂设置或开发工具配置中默认采用 SWD 作为调试接口。
3. **信号稳定性：** 较少的信号线意味着更低的干扰风险，从而提高了调试的稳定性。
4. **兼容性考虑：** 使用 SW 模式通常能确保与 STM32CubeMX 及其他工具生成的工程的兼容性，避免 JTAG 模式可能带来的额外问题。

综上，选择 SW 模式既符合大多数 STM32 应用的实际需求，也能带来更简单稳定的调试体验。

## 使用CUBEMX的流程

**初始化PC13引脚点亮LED**

点击创建新项目，选择具体芯片后。出现配置界面：

- 选择system core
  - 配置debug模式
  - 不然下载一次后会被锁死
- 点击右侧的图中的PC13
  - 选择作为GPIO的输出引脚
- 返回system core
  - 看到选中的PC13,进行具体的配置
  - 配置初始的电平
  - GPIO模式
  - 上下拉
  - 速度
- 点击project manager
  - 起一个项目名字
  - 选择项目文件夹
- 点击生成代码

### 使用STM32 advanced模式生成代码后的项目结构



### application structure选项

**STM32CUBEMX 中：application structure选项是干嘛的？ basic和advanced的区别是什么？**

在 STM32CubeMX 中，**Application Structure** 选项用于选择生成代码的结构方式，影响代码的组织形式和初始化方式。主要有 **Basic** 和 **Advanced** 两种模式。

#### **Basic 模式**

- 适合 **简单项目** 或 **刚接触 STM32CubeMX 的用户**。
- 生成的代码结构相对简单，所有初始化代码都放在 `main.c` 文件里。
- 适用于不太需要修改框架的应用。
- **缺点**：代码可读性较低，不够模块化，不利于扩展。

#### **Advanced 模式**

- 适合 **复杂项目**，尤其是 **使用 RTOS 或者需要模块化管理代码的应用**。
- 生成的代码采用更清晰的分层结构：
  - `Core/Src` 和 `Core/Inc` 目录下有不同模块的代码。
  - **`app_xxx.c`** 这样的应用层文件，用于用户代码的管理。
  - **HAL（Hardware Abstraction Layer，硬件抽象层）代码更规范化**，方便后续维护和移植。
- **适合 FreeRTOS、LVGL 等复杂应用的开发**，因为它允许更清晰地管理任务、外设和中断。
- **缺点**：初学者可能会觉得代码层次复杂。

## 使用STM32 advanced模式生成代码后的项目结构？

在 **STM32CubeMX** 选择 **Advanced（高级）** 模式后，生成的 Keil（MDK-ARM）项目结构会比 Basic 模式更加清晰和模块化。一般的项目结构如下：

```
ProjectName/                <-- 你的工程文件夹
│── Core/                   <-- 核心代码（应用层）
│   │── Inc/                <-- 头文件（包含 main.h、stm32xxxx_it.h 等）
│   │── Src/                <-- 源代码（包含 main.c、stm32xxxx_it.c、syscalls.c 等）
│   └── Startup/            <-- 启动文件（包含 startup_stm32xxxx.s）
│
│── Drivers/                <-- STM32 HAL 驱动库
│   │── CMSIS/              <-- ARM CMSIS 头文件（如 core_cm4.h）
│   │── STM32xxxx_HAL_Driver/  <-- STM32 HAL 库代码（包含 stm32xxxx_hal_xxx.c）
│
│── Middlewares/            <-- 中间件（如果启用，比如 FreeRTOS、LVGL）
│   └── FreeRTOS/           <-- FreeRTOS 相关文件（仅在启用 FreeRTOS 时存在）
│
│── Utilities/              <-- 可能包含一些 STM32 提供的工具代码（可能没有）
│
│── MDK-ARM/                <-- Keil MDK 相关文件
│   │── ProjectName.uvprojx  <-- Keil MDK5 工程文件
│   │── ProjectName.uvoptx   <-- Keil MDK5 工程配置文件
│
│── .ioc                    <-- STM32CubeMX 配置文件
│── README.md               <-- 可能包含的说明文档
```

### **详细解析**

1. **Core/**
   - Inc/（头文件）
     - `main.h`：主程序头文件，一些通用定义。
     - `stm32xxxx_it.h`：中断处理函数的声明。
   - Src/（C 源文件）
     - `main.c`：主程序，包含 `main()` 函数。
     - `stm32xxxx_it.c`：存放中断服务程序（ISR）。
     - `syscalls.c`、`sysmem.c`（如果启用 semihosting 可能会生成）。
   - Startup/
     - `startup_stm32xxxx.s`：启动汇编文件。
2. **Drivers/**
   - `CMSIS/`：包含 ARM Cortex-M 处理器相关的 CMSIS 头文件。
   - `STM32xxxx_HAL_Driver/`：HAL（硬件抽象层）驱动，比如 `stm32xxxx_hal_gpio.c`、`stm32xxxx_hal_uart.c` 等。
3. **Middlewares/**
   - 这里存放 FreeRTOS、USB、LWIP、LVGL 等中间件代码，只有启用了对应组件时才会生成。
4. **MDK-ARM/**
   - Keil 相关的 `.uvprojx` 工程文件，双击 `.uvprojx` 文件可以直接打开 Keil 进行开发。
5. **.ioc 文件**
   - STM32CubeMX 的配置文件，使用 CubeMX 重新打开可以修改工程配置并重新生成代码。

### **总结**

- **Advanced 模式** 比 Basic 模式更模块化，代码更清晰，适合复杂应用（如 FreeRTOS + LVGL）。
- HAL 驱动和中间件（如 FreeRTOS）被放在独立的文件夹中，便于管理和更新。
- 适用于 **结构化开发、多人协作、代码维护**，但对于初学者来说，层次较多，需要适应。

## 生成代码后keil编译报错

```
led_test\led_test.sct(7): error: L6236E: No section matches selector - no section to be FIRST/LAST.
Not enough information to list image symbols.
Not enough information to list load addresses in the image map.
Finished: 2 information, 0 warning and 1 error messages.
"led_test\led_test.axf" - 1 Error(s), 0 Warning(s).
```

**路径不能含有中文。代码生成后，默认不会开启下载后复位执行，需要自己设置一下。**

## 创建工程的几种方式

使用 CubeMX 方式:

- CubeMX 在生成代码时会**自动包含完整的 HAL 库**
- 会在工程中创建 Drivers 文件夹,包含:
  - HAL 库源码 (Drivers/STM32xxxx_HAL_Driver/Src)
  - HAL 库头文件 (Drivers/STM32xxxx_HAL_Driver/Inc)
  - CMSIS 相关文件 (Drivers/CMSIS)
- **自动配置好所有必要的工程设置和编译选项**
- **不需要额外下载其他库文件**

不使用 CubeMX 方式:

- 需要从 ST 官网手动下载 HAL 库软件包
- 需要自己搭建工程结构
- 需要手动添加 HAL 库源文件到工程
- 需要自己配置编译选项
- 需要自己编写时钟初始化等底层配置代码

所以从开发效率角度来说,使用 CubeMX 是更好的选择,因为它:

- 避免了手动配置的繁琐
- 降低了出错的可能性
- 保证了库文件版本的匹配性
- 提供了图形化的配置界面

不过了解不使用 CubeMX 的方式也是很有必要的,因为:

- 帮助深入理解 HAL 库的结构
- 在一些特殊项目中可能无法使用 CubeMX
- 对学习和理解底层原理很有帮助

**所以到现在为止有几种开发方式对于keil来讲。 第一个：使用标准外设库文件手动搭建工程，启动文件，时钟配置文件等 第二个就是使用DFP，利用keil自带的工程搭建助手创建工程。 第三个就是使用CUBEMX生成工程，里面自带了HAL库。 第四个就是跟标准外设库软件包一样，去官网下载HAL库软件包，手动搭建工程，提取对应的文件，启动文件，时钟配置文件等？**

标准外设库(StdPeriph Library)方式:

- **手动**下载固件库包
- **手动**添加启动文件、外设库文件
- **手动**配置工程设置和时钟
- 优点: 代码简洁,执行效率高
- 缺点: 配置繁琐,ST已不再维护

DFP + Keil Pack方式:

- **使用Keil自带的工程向导**
- 基于CMSIS标准
- **自动添加启动文件和基础配置**
- 优点: 配置相对简单,标准化程度高
- 缺点: 功能相对基础,需要手写外设初始化代码

CubeMX + HAL库方式:

- 图形化配置
- 自动生成工程和初始化代码
- 包含完整HAL库
- 优点: 开发效率最高,出错率低
- **缺点: 代码体积较大,部分性能损失**

手动配置HAL库方式:

- 手动下载HAL库包
- 手动搭建工程结构
- **类似标准外设库的搭建流程**
- 优点: 对HAL库理解更深入
- **缺点: 配置繁琐,容易出错**

总的趋势是从1到3越来越自动化、标准化,开发效率越来越高。目前主流是使用方式3(CubeMX),除非有特殊需求。**第4种方式相对少用,主要用于学习或特殊场景。**

### DFP的作用

DFP（Device Family Pack）是由设备制造商（如STMicroelectronics）提供的一组支持特定微控制器家族（比如STM32系列）的文件集合。这些文件通常包括：

- **启动代码（Startup Code）**：用于初始化微控制器的基本设置。
- **中断向量表**：定义了中断处理的入口地址。
- **系统配置文件**：提供底层硬件支持，例如系统时钟的配置。

在嵌入式开发中，这些文件是搭建工程的基础。DFP通过Keil MDK的Pack系统，以一种标准化的方式将这些文件集成到开发环境中，方便开发者使用。

### 手动搭建工程 vs. 使用DFP

**手动搭建工程**

如果你选择手动搭建一个工程（例如基于标准外设库或HAL库），你需要：

1. 从固件包（如STM32的固件库）中找到并提取必要的文件，比如启动文件（startup_xxx.s）、系统时钟配置文件（system_xxx.c）等。
2. 将这些文件手动添加到你的工程目录中。
3. 配置工程设置，比如包含路径、中断向量偏移等，确保编译和链接正常。

这种方式虽然灵活，但需要开发者熟悉固件包的结构，并且手动操作可能会耗费时间，尤其是当你需要支持多个不同型号的设备时。

**使用DFP**

如果你使用DFP，通过Keil MDK的Pack系统：

1. 在Keil中打开“Manage Run-Time Environment”（运行时环境管理），选择对应的DFP包（例如某个STM32型号的支持包）。
2. Keil会自动将DFP中包含的启动文件、系统配置文件等必要文件添加到你的工程中。
3. 无需手动提取和配置，工程可以直接使用这些文件开始开发。

换句话说，DFP通过Keil的集成工具，省去了手动查找和添加文件的步骤，让工程搭建更加高效。

## 如何寻找封装的函数

**对于STM23来说，我是用标准外设库时，需要对应的编程接口直接去提供的库函数中去找。但是我使用CUBEMX生成的代码时，对应的hal库函数似乎少了很多？还提供了不同类型的函数封装。我应该如何去寻找对应的编程接口？**

对于STM32来说，当你从标准外设库（SPL）转向使用CubeMX生成的HAL库代码时，可能会注意到HAL库的函数数量似乎比SPL少，而且还提供了一些不同类型的函数封装。这种变化源于两者的设计理念和使用方法的不同。

### **SPL与HAL库的区别**

- **标准外设库（SPL）**
   SPL是STM32早期提供的库，特点是直接操作底层寄存器。它提供了大量细粒度的函数，用户可以通过调用这些函数精确地控制外设的每一个细节。例如，你可能需要分别调用函数来配置GPIO的模式、速度和状态。这种方式虽然灵活，但函数数量多，代码复杂度较高。
- **HAL库（硬件抽象层库）**
   HAL库是ST推出的一种新库，旨在提高代码的可移植性和易用性。它的设计理念是通过更高层次的抽象，隐藏底层寄存器的具体操作。相比SPL，HAL库的函数数量更少，但每个函数通常封装了更多的功能。例如，一个HAL_GPIO_Init函数就能一次性完成GPIO的多个寄存器配置，而不需要你手动操作每个细节。

因此，HAL库函数“变少”并不是功能减少，而是通过更高级的封装和抽象，用更少的函数实现了相同的功能。这也意味着你需要适应新的编程方式。

### 如何在HAL库中寻找对应的编程接口

为了在CubeMX生成的HAL库代码中找到适合的编程接口，你可以按照以下步骤操作：

#### **1. 查看CubeMX生成的代码**

CubeMX会根据你在图形界面中的配置，自动生成相应的HAL库代码。这些代码通常包括外设的初始化函数和基本操作函数。例如：

- 在main.c中，你可能会找到类似于MX_GPIO_Init()或MX_USART1_UART_Init()的初始化函数。
- 这些函数是由CubeMX基于你的配置生成的，直接调用HAL库的底层函数。

**建议**：打开生成的代码，查看这些函数的实现细节，了解它们调用了哪些HAL函数。

#### **2. 参考HAL库官方文档**

ST官方提供了HAL库的详细文档（通常以PDF形式发布），其中列出了每个外设支持的HAL函数及其用法。例如：

- GPIO相关的函数：HAL_GPIO_Init、HAL_GPIO_WritePin等。
- UART相关的函数：HAL_UART_Transmit、HAL_UART_Receive等。

#### **3. 熟悉HAL库的命名规范**

HAL库的函数命名遵循统一规范，便于识别和使用：

- 函数以HAL_开头。
- 紧接着是外设名称（如GPIO、UART、I2C）。
- 最后是操作类型（如Init、Transmit、Receive）。

**示例**：

- HAL_GPIO_Init：初始化GPIO。
- HAL_UART_Transmit：通过UART发送数据。

通过这种命名规则，你可以快速猜测某个功能的函数名，并在代码或文档中验证。

#### **4. 查看HAL库的头文件**

HAL库的头文件（位于CubeMX生成的项目文件夹中的Drivers/STM32xx_HAL_Driver/Inc目录）包含所有可用函数的声明。例如：

- stm32f4xx_hal_gpio.h：GPIO相关函数。
- stm32f4xx_hal_uart.h：UART相关函数。

**建议**：打开这些头文件，浏览函数声明，了解每个函数的参数和用途。这能帮助你快速找到需要的接口。

## 如果忘记分配调试接口

就会被锁死，无法再次下载程序。[使用CUBEMX的流程](##使用CUBEMX的流程)

可以一直按着复位，然后下载即可。

