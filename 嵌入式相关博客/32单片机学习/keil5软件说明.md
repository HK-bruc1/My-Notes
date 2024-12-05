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

# 新建基于STM32F40x固件库的MDK5工程模板

## 新建工程模板 

在建立工程之前，我们建议用户在电脑的某个目录下面建立一个文件夹，后面所建立的工程都可以放在这个文件夹下面，这里我们建立一个文件夹为工程模板。 这是工程的根目录文件夹。然后为了方便我们存放工程需要的一些其他文件，这里我们还新建下面 5 个子文件夹： `CORE ,FWLIB,OBJ,SYSTEM,USER`。至于这些文件夹名字，实际上是可以任取的，我们这样取名只是为了方便识别。对于这些文件夹用来存放什么文件，我们后面的步骤会一一提到。

![image-20241130101301101](C:\Users\admin\Desktop\个人博客\嵌入式相关博客\32单片机学习\keil5软件说明.assets\image-20241130101301101.png)

接下来， 打开 Keil,点击 Keil 的菜单： Project –>New Uvision Project ，然后将目录定位到刚才建立的文件夹 Template 之下的 USER 子目录， 同时，工程取名为 Template 之后点击保存，**我们的工程文件就都保存到 USER 文件夹下面。**  

接下来会出现一个选择 Device 的界面，就是选择我们的芯片型号，这里我们定位到STMicroelectronics 下面的 STM32F407VG。这里我们选择 STMicroelectronics STM32F4 SeriesS TM32F407 STM32F407ZG（如果使用的是其他系列的芯片，选择相应的型号就可以了， 例如我们的战舰 STM32 开发板是STM32F407VGT6。 特别注意： 一定要安装对应的器件 pack (**芯片支持包**)才会显示这些内容哦！！）。  

选择芯片后点击 OK， MDK 会弹出 Manage Run-Time Environment 对话框 ：

![image-20241130101859201](C:\Users\admin\Desktop\个人博客\嵌入式相关博客\32单片机学习\keil5软件说明.assets\image-20241130101859201.png)

这是 MDK5 新增的一个功能，在这个界面，我们可以添加自己需要的组件，从而方便构建开发环境，不过这里我们不做介绍。我们直接点击 Cancel，即可  。

现在我们看看 USER 目录下面包含 2 个文件  ：

![image-20241130102008761](C:\Users\admin\Desktop\个人博客\嵌入式相关博客\32单片机学习\keil5软件说明.assets\image-20241130102008761.png)

这里我们说明一下， Template.uvprojx 是工程文件，非常关键，不能轻易删除， MDK51.4生成的工程文件是以.uvprojx 为后缀。 Listings 和 Objects 文件夹是 MDK 自动生成的文件夹，用于存放编译过程产生的中间文件。这里，我们把两个文件夹删除，我们会在下一步骤中新建一个 OBJ 文件夹，用来存放编译中间文件。当然，我们不删除这两个文件夹也是没有关系的，只是我们不用它而已。  

下面我们要将**官方的固件库包里的源码文件复制到我们的工程目录文件夹下面**。打开官方固件库包，定位到我们之前准备好的固件库包的目录: \STM32F4xx_DSP_StdPeriph_Lib_V1.4.0\Libraries\STM32F4xx_StdPeriph_Driver （**库函数开发**）下面，将目录下面的 src,inc 文件夹 copy 到我们刚才建立的 FWLib 文件夹下面。src 存放的是固件库的.c 文件， inc 存放的是对应的.h 文件，您不妨打开这两个文件目录过目一下里面的文件，每个外设对应一个.c 文件和一个.h 头文件。 （**要什么就留什么，寄存器开发就把STM32F4xx_StdPeriph_Driver删除，否则就留下**）。

下面我们要将固件库包里面相关的**启动文件复制到我们的工程目录 CORE 之下**。

打开官方固件库包，定位到目录`\STM32F4xx_DSP_StdPeriph_Lib_V1.4.0\Libraries\CMSIS\Device\ST\STM32F4xx\Source\Templat es\arm` 下面，将文件 startup_stm32f40_41xxx.s 复制到 CORE 目录下面。然后定位到目录`\STM32F4xx_DSP_StdPeriph_Lib_V1.4.0\Libraries\CMSIS\Includ` ， 将 里 面 的 四 个 头 文 件 ： `core_cm4.h、 core_cm4_simd.h、 core_cmFunc.h` 以及 `core_cmInstr.h` 同样复制到 CORE 目录下面。现在看看我们的 CORE 文件夹下面的文件：

![image-20241130102858564](C:\Users\admin\Desktop\个人博客\嵌入式相关博客\32单片机学习\keil5软件说明.assets\image-20241130102858564.png)

接下来我们要复制工程模板需要的一些其他头文件和源文件到我们工程。首先定位到目录： `STM32F4xx_DSP_StdPeriph_Lib_V1.4.0\Libraries\CMSIS\Device\ST\STM32F4xx\Include` 将里面的 2 个头文件 `stm32f4xx.h` 和 `system_stm32f4xx.h` 复制到 USER 目录之下。 这两个头文件是STM32F4 工程非常关键的两个头文件。后面我们讲解相关知识的时候会给大家详细讲解。然后进入目录`\STM32F4xx_DSP_StdPeriph_Lib_V1.4.0\Project\STM32F4xx_StdPeriph_Templates` ，将 目 录 下 面 的 5 个 文 件 `main.c ， stm32f4xx_conf.h ， stm32f4xx_it.c ， stm32f4xx_it.h ， system_stm32f4xx.c` 复制到 USER 目录下面。   

## 提取内核文件后的关联

前面 7 个步骤，我们将需要的固件库相关文件复制到了我们的工程目录下面，下面我们将这些文件加入我们的工程中去。右键点击 Target1，选择 Manage Project Items，  在 Groups 一栏删掉一个 Source Group1，建立三个 Groups： USER,CORE,FWLIB(**也只有这三个文件夹中提取了文件**)。然后点击 OK，可以看到我们的 Target名字以及 Groups 情况如下图  :

![image-20241130103712743](C:\Users\admin\Desktop\个人博客\嵌入式相关博客\32单片机学习\keil5软件说明.assets\image-20241130103712743.png)

下面我们往 Group 里面添加我们需要的文件。我们按照步骤 9 的方法， 右键点击点击Tempate，选择选择 Manage Project Items.然后选择需要添加文件的 Group，这里第一步我们选择 FWLIB，然后点击右边的 Add Files,定位到我们刚才建立的目录\FWLIB\src 下面，将里面所有的文件选中(Ctrl+A)，然后点击 Add，然后 Close 可以看到 Files 列表下面包含我们添加的文件：

**这里有个文件 stm32f4xx_fmc.c 比较特殊。这个文件是 STM32F42 和 STM32F43 系列才用到，所以我们这里要把它删掉（注意是 stm32f4xx_fmc.c 要删掉，不要删掉 stm32f4xx_fsmc.c）。**   

![image-20241130104026486](C:\Users\admin\Desktop\个人博客\嵌入式相关博客\32单片机学习\keil5软件说明.assets\image-20241130104026486.png)

**这里需要说明一下，对于我们写代码，如果我们只用到了其中的某个外设，我们就可以不用添加没有用到的外设的库文件。例如我只用 GPIO，我可以只用添加 stm32f4xx_gpio.c 而其他的可以不用添加。这里我们全部添加进来是为了后面方便，不用每次添加，当然这样的坏处是工程太大，编译起来速度慢，用户可以自行选择。**  



用同样的方法，将 Groups 定位到 CORE 和 USER 下面，添加需要的文件。这里我们的 CORE 下面需要添加的文件为 startup_stm32f40_41xxx.s(注意，默认添加的时候文件类型为.c,也就是添加 startup_stm32f40_41xxx.s 启动文件的时候，你需要选择文件类型为 All files才能看得到这个文件)， USER 目录下面需要添加的文件为 main.c， stm32f4xx_it.c， system_stm32f4xx.c。 这样我们需要添加的文件已经添加到我们的工程中去了，最后点击 OK，回到工程主界面。

**这里还是在USER文件夹下建立一个自己的inc和src以作区分吧！**固件库的就放在USER下即可。



**接下来我们要在 MDK 里面设置头文件存放路径。也就是告诉 MDK 到那些目录下面去寻找包含了的头文件。这一步骤非常重要。如果没有设置头文件路径，那么工程会出现报错头文件路径找不到。** 

在魔术棒中：
![image-20241130104743972](C:\Users\admin\Desktop\个人博客\嵌入式相关博客\32单片机学习\keil5软件说明.assets\image-20241130104743972.png)

这里我们需要添加的头文件路径包括： \CORE, \USER\以及\FWLIB\inc。这里大家务必要仔细，固件库存放的头文件子目录是\FWLIB\inc，不是 FWLIB\src。很多朋友都是这里弄错导致报很多奇怪的错误。  **都是提取内核文件后，有.h后缀的文件夹必须是直接包含关系，为了以后自己写的头文件也能找到，把USER下的inc也包含进来。**



接下来对于 STM32F40 系列的工程，还需要添加一个全局宏定义标识符。添加方法是点击魔术棒之后，进入 

C/C++选项卡，然后在 Define 输入框连输入：STM32F40_41xxx,USE_STDPERIPH_DRIVER。注意这里是两个标识符 STM32F40_41xxx和 USE_STDPERIPH_DRIVER， 他们之间是用逗号隔开的，请大家注意。



接下来我们要编译工程，在编译之前我们首先要选择编译中间文件编译后存放目录。方法是点击魔术棒 ，然后选择“

Output”选项下面的“Select folder for objects…” ,然后选择目录为我们上面新建的 OBJ 目录。 同时将下方的三个选项框都勾上。

不选择的话会默认在C:\Users\admin\Desktop\基于F40X库函数的工程模板\USER\Objects，因为创建工程是在USER中。

**这里说明一下步骤 4 的意义。 Create HEX File 选项选上是要求编译之后生成 HEX 文件。Browse Information 选项选上是方便我们查看工程中的一些函数变量定义**  



在编译之前，我们先把 main.c 文件里面的内容替换为如下内容  ：

```c
#include "stm32f4xx.h"                  // Device header

int main(){

	while(1){
		
	}
}
```

与此同时，我们要将 USER 分组下面的 stm32f4xx_it.c 文件内容清空。  

下面我们点击编译按钮 编译工程，可以看到工程编译通过没有任何错误和警告。  

到这里，一个基于固件库 V1.4 的工程模板就建立完成，同时在工程的 OBJ 目录下面生成了对应的 hex 文件(没有写东西可能没有)。



这里还有个非常重要的关键点，就是系统时钟的配置，这在我们的系统时钟章节 4.3 会详细讲解，这里我们要修改 

System_stm32f4xx.c 文件，把 PLL 第一级分频系数 M 修改为 8，这样达到主时钟频率为 168MHz。

未完待续！！！
