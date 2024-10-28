# ESP32 WROOM开发板 CH340驱动芯片

#### ESP32-WROOM-32 主要参数：

- **双核 32 位 LX6 微控制器**：提供强大的处理能力，可以同时处理多个任务。
- **Wi-Fi 和 Bluetooth 双模**：支持 2.4GHz Wi-Fi 和 Bluetooth 4.2 BLE，可以实现无线连接和数据传输。
- **丰富的 GPIO 接口**：可以连接各种传感器、执行器等外围设备。
- **低功耗**：支持多种低功耗模式，适用于电池供电的应用。
- **集成多种外设**：包括 ADC、DAC、I2C、SPI、UART 等，方便扩展功能。

CH340 是一款常用的 USB 转串口芯片，用于将开发板通过 USB 接口连接到电脑，并建立串行通信。以下是对 CH340 的一些介绍。

## 开发板介绍

### **基本功能**

- **USB 转串口**：CH340 可以将 USB 信号转换为串行接口信号，允许你通过串口调试工具与开发板进行通信，例如 ESP32 的串行调试、烧录固件等。
- **支持多种波特率**：它支持各种常见的波特率设置，能够适应不同的通信速度需求。

### **驱动支持**

- 在 Windows、macOS 和 Linux 系统上都可以通过安装相应的驱动程序来使用 CH340。很多时候，Windows 需要手动安装 CH340 的驱动，才能让电脑识别出设备并进行通信。

### **应用场景**

- **嵌入式开发调试**：在使用 ESP32 开发时，CH340 负责将你通过 USB 发送的数据传输给 ESP32，同时将 ESP32 的输出数据通过串口返回电脑。这是嵌入式开发中常见的调试方法。
- **固件烧录**：通过 CH340，可以将 ESP32 的固件从电脑烧录到开发板上。

### **ESP32 与 CH340 的结合**

- 在 ESP32 WROOM 开发板上，CH340 的作用是让你通过 USB 接口进行串口通信和固件下载。它可以提供稳定的串口连接，帮助你监控 ESP32 的输出数据，或通过电脑向 ESP32 发送指令。

## Arduino IDE

**Arduino IDE** 在开发板和电脑之间主要承担着开发和通信的角色，类似于其他集成开发环境（IDE）如 **IDEA** 或 **PyCharm**，但它的作用更适合嵌入式开发的场景。以下是它在开发过程中的几个关键角色和功能：

### **代码编写和编辑平台**

- **Arduino IDE** 提供了一个简单易用的代码编辑器，支持编写针对 Arduino 兼容开发板的 C/C++ 代码。类似于 **IDEA** 和 **PyCharm**，你可以在里面写代码、调试、自动补全以及查看编译结果。
- 它还包括了一些内置的示例代码，比如 `Blink`（LED 闪烁）项目，非常适合初学者快速上手。

### **编译器和工具链管理**

- **Arduino IDE** 通过内置的编译工具链将你编写的代码（通常是 C 或 C++）编译成适合开发板的机器代码。
- 对于每一种开发板（如 ESP32、Arduino UNO 等），IDE 会自动处理相关的库文件、硬件配置，**并生成适合特定微控制器的二进制文件**。这类似于在 **IDEA** 中为 Java 项目编译成字节码，或在 **PyCharm** 中为 Python 脚本准备运行环境。

### **固件烧录**

- **Arduino IDE** 负责将编译好的程序上传（烧录）到开发板上。在你点击“上传”按钮后，IDE 会通过串口（USB）与开发板通信，将二进制文件传输到开发板的闪存中，供其执行。这与传统的 IDE 有些不同，因为嵌入式开发中的烧录步骤是将代码物理上传到硬件上，而不是仅仅在本地执行。

### **串口通信监控**

- Arduino IDE 提供了一个串口监视器，让你可以实时查看开发板的调试输出或通过串口发送指令与开发板交互。这类似于在 **PyCharm** 中调试 Python 脚本时使用控制台输出，或在 **IDEA** 中查看日志信息。

### **库管理和板子管理**

- Arduino IDE 有 **库管理器**，允许你轻松导入各种第三方库（如传感器驱动、通信协议库等），让开发更加高效。
- 它还包含 **开发板管理器**，你可以在其中安装不同的硬件支持包，比如为 ESP32、Arduino Uno、Mega 等不同的开发板下载相应的板卡定义和工具链支持。

## 在 Windows 10 上开始第一个 ESP32 WROOM 开发板项目

### **安装驱动程序**

- 首先确保你的开发板可以被 Windows 识别。因为你的开发板使用的是 

  CH340芯片，需要安装驱动：

- win10设备管理器中没有端口选项，需要先通过usb链接电脑和开发板，看是否有端口选项来测试数据线功能是否完好

  - 没有显示端口选项，但是显示了未知设备，在视频的预料情况之内，可以安装驱动程序

  - 安装 CH340 驱动时**不需要**提前连接开发板，但建议在驱动安装完成后再连接开发板。这是为了避免 Windows 在你连接开发板时尝试自动安装不合适的驱动程序。

    安装步骤建议：

    1. **断开开发板**：确保你的 ESP32 WROOM 开发板在安装驱动之前没有插入电脑。
    2. 安装 CH340 驱动：（可以直接从 **CH340 的制造商**——**WCH (南京沁恒微电子)** 的官网上下载。）
       - 运行你下载的 CH340 驱动安装程序CH341SER.EXE，按照提示进行安装。
       - 以管理员身份运行，按照指示安装，显示成功即可。
    3. **驱动安装完成后**，将 ESP32 WROOM 开发板通过 USB 线连接到电脑。此时，电脑应该可以正确识别开发板的串口。

  如果安装成功，你可以在 Windows 的 **设备管理器** 中看到开发板的 COM 端口，通常会显示为 "USB-SERIAL CH340"。

- 打开设备管理器，再一次通过usb接口连接开发板和电脑，查看端口情况

![image-20241006223849248](C:\Users\admin\Desktop\个人博客\ESP32 WROOM开发板 CH340驱动芯片.assets\image-20241006223849248.png)

### **安装开发环境**

你可以使用 **Arduino IDE** 或 **PlatformIO** 来开发和烧录程序。我推荐从 **Arduino IDE** 开始，因为它的界面简单且易于使用。

**步骤：**

1. **下载并安装 Arduino IDE：**

   - 前往 Arduino IDE 官方网站 下载适用于 Windows 的版本并安装。

2. **配置 ESP32 支持：**

   - 打开 Arduino IDE，依次点击 **文件 (File)** → **首选项 (Preferences)**。
   - 在 **附加开发板管理器网址 (Additional Boards Manager URLs)** 中，输入以下 URL：（配置下载源）

   ```c
   https://dl.espressif.com/dl/package_esp32_index.json
   ```

   - 点击 **工具 (Tools)** → **开发板 (Board)** → **开发板管理器 (Boards Manager)**，然后搜索 `ESP32`，选择 **ESP32 by Espressif Systems** 并点击 **安装**。

3. **连接开发板并选择端口**

   1. 插入 ESP32 WROOM 开发板到电脑的 USB 端口。
   2. 在 Arduino IDE 中，点击 **工具 (Tools)**，选择 **开发板 (Board)** → **ESP32 Dev Module**。
   3. 在 **工具 (Tools)** → **端口 (Port)**，选择与开发板对应的串口（如果驱动安装正确，会看到类似 `COMx` 的选项）。

4. **编写第一个程序**

   - 你可以从 **Blink（LED 闪烁）程序** 开始，这是嵌入式开发中的入门项目。

   **步骤：**

   1. 在 Arduino IDE 中，点击 **文件 (File)** → **示例 (Examples)** → **01.Basics** → **Blink**。
   2. 这段代码会让板载的 LED 持续闪烁。你可以通过 `delay()` 函数更改闪烁的频率：

   ```cpp
   void setup() {
     pinMode(2, OUTPUT); // GPIO2 是 ESP32 上常用的 LED 引脚
   }
   
   void loop() {
     digitalWrite(2, HIGH);   // LED 亮
     delay(1000);             // 延时1秒
     digitalWrite(2, LOW);    // LED 灭
     delay(1000);             // 延时1秒
   }
   ```

5. **编译并上传程序**

   1. 点击 Arduino IDE 中的 **√ (编译)** 按钮，确保代码没有错误。

   ![image-20241006231336682](C:\Users\admin\Desktop\个人博客\ESP32 WROOM开发板 CH340驱动芯片.assets\image-20241006231336682.png) 

   ```
   Sketch uses 268237 bytes (20%) of program storage space：你的代码占用了 268,237 字节的程序存储空间，相当于总可用空间的 20%（最大空间是 1,310,720 字节）。
   Global variables use 20192 bytes (6%) of dynamic memory：你的全局变量使用了 20,192 字节的动态内存（RAM），占总内存的 6%（最大动态内存是 327,680 字节）。
   leaving 307488 bytes for local variables：剩下的 307,488 字节是供局部变量使用的内存空间。
   ```

   1. 点击 **→ (上传)** 按钮，将程序上传到 ESP32。
   2. 如果上传成功，ESP32 开发板上的 LED（通常连接到 GPIO2）会开始以 1 秒间隔闪烁。

   ![image-20241006231517287](C:\Users\admin\Desktop\个人博客\ESP32 WROOM开发板 CH340驱动芯片.assets\image-20241006231517287.png) 

6. **查看串口输出**

   - 你可以通过串口监视器查看开发板的调试信息。

   1. 点击 **工具 (Tools)** → **串口监视器 (Serial Monitor)**。
   2. 设置波特率为 `115200`，观察开发板的输出信息（如果你在代码中加入 `Serial.print` 语句）。

### 如何停止程序

在你的 ESP32 开发板上，通常会有两个主要按钮，分别是 **EN（或 N3）** 和 **BOOT**，它们的作用如下：

#### 1. **EN（N3）按钮**

- **EN** 是 **Enable** 按钮，也称为 **Reset（复位）按钮**。
- 作用：按下这个按钮会重启开发板，停止当前正在运行的程序并重新启动板子上的固件。这个按钮可以用来重启开发板以重新开始执行烧录的程序。

#### 2. **BOOT 按钮**

- **BOOT** 按钮主要用于**进入下载模式**，当你想要将新的程序上传到 ESP32 时，可能需要用到它。
- 作用：
  - 在某些开发板上，上传代码时可能需要按住 **BOOT** 按钮，直到程序上传完成。这可以让开发板进入固件烧录模式。
  - 通常，你不需要手动按住 **BOOT** 按钮，因为大多数 ESP32 开发板有自动刷写功能。但是，如果自动上传不工作，你可以手动按下 **BOOT** 按钮。

#### 停止程序的操作

- **按下 N3（EN/Reset）按钮**：这会重启开发板，停止当前程序的执行并重新运行板子上的程序。如果你只是想暂停当前程序的执行，可以按这个按钮。
- **BOOT 按钮**：除非你在上传程序时需要手动进入烧录模式，否则平时不会用到这个按钮来停止程序。

**按下 N3（EN/Reset）按钮** 并不会永久停止程序的运行，它实际上是**重启了开发板**，这意味着程序会重新启动并从头开始执行。

**原因：**当你按下 **N3（EN/Reset）按钮** 时，开发板会断电并立即重启。由于烧录在开发板上的程序会被保存到板子的闪存中，因此每次重启时，开发板会从闪存中读取并执行之前烧录的程序。所以，按下 **N3** 后，程序并没有被停止，而是重新运行了。

**如何永久停止程序？**

要真正停止程序的运行，你可以考虑以下方法：

**上传一个空程序**

上传一个空的程序，这样开发板在启动时不会执行任何操作。你可以使用下面的代码：

```cpp
void setup() {
  // 空的 setup
}

void loop() {
  // 空的 loop
}
```

上传后，开发板将不再执行任何代码。

**断开电源**

如果你不希望开发板继续运行任何程序，你可以直接**断开电源**（拔掉 USB 线）。这将停止所有程序的执行，直到你再次供电。

**直接断电会不会损坏开发板？**

​	直接断电**不会**损坏你的开发板。ESP32 开发板设计时就考虑到了这种使用场景，因此频繁的拔插电源或断电操作是安全的。

​	**嵌入式设备设计**：ESP32 等嵌入式设备通常在各种供电条件下工作，包括断电、重启等操作，它们的设计是为了能够处理这些常规电源操作。

​	**闪存数据安全**：当你断电时，程序会保存在板子的闪存中，而不是易失性存储器（RAM）。因此，断电后，程序不会丢失。下次重新通电时，板子会继续从闪存中启动并运行已经烧录的程序。

​	虽然断电对硬件本身没有问题，但在一些特定情况下，断电可能会导致：

- **正在写入闪存时**：如果在闪存进行写操作（如你正在烧录程序时）时突然断电，可能会导致程序未正确烧录，板子可能无法正常工作。这种情况非常少见，因为在正常运行时开发板不会频繁写入闪存。

**重新烧录新程序**

如果你想更改板子上运行的程序，也可以直接通过 Arduino IDE 上传一个新的程序来替换当前的程序。

## 0.96 寸 OLED 1306 显示屏和 ESP32 开发板结合使用

**0.96 寸 OLED 1306 显示屏** 和 ESP32 开发板结合使用，同时实现闪烁 LED 的功能

### 连接 OLED 显示屏到 ESP32

你的 OLED 1306 显示屏有四根引脚：**VCC**、**GND**、**SCL**（时钟）、**SDA**（数据）。通常它使用 I2C 通信协议与 ESP32 连接。

- **VCC**：接 ESP32 的 3.3V（有些模块支持 5V，但 ESP32 通常使用 3.3V）。
- **GND**：接 ESP32 的 GND。
- **SCL**：接 ESP32 的 GPIO 22（默认 I2C 时钟引脚）。
- **SDA**：接 ESP32 的 GPIO 21（默认 I2C 数据引脚）。

### 通过杜邦线（母对母）来连接显示屏和主板

#### **确定 OLED 显示屏的引脚**

你需要确认 OLED 显示屏上的引脚标识，通常它有 4 个引脚：

- **VCC**（电源）
- **GND**（地）
- **SCL**（I2C 时钟线）
- **SDA**（I2C 数据线）

#### **确定 ESP32 的 I2C 引脚**

ESP32 的默认 I2C 引脚是：

- **SCL（时钟）**：GPIO 22
- **SDA（数据）**：GPIO 21

#### **杜邦线连接**

使用 **母对母杜邦线** 按以下方式连接 OLED 和 ESP32：

| OLED 显示屏引脚 | ESP32 引脚    | 连接说明                 |
| --------------- | ------------- | ------------------------ |
| **VCC**         | 3.3V          | 为 OLED 提供电源（3.3V） |
| **GND**         | GND           | 接地                     |
| **SCL**         | GPIO 22 (SCL) | I2C 时钟线               |
| **SDA**         | GPIO 21 (SDA) | I2C 数据线               |

#### **实际连接步骤**

1. 拿出 4 根母对母杜邦线。

2. 将每根杜邦线一端插入 OLED 模块上的对应引脚，另一端插入 ESP32 开发板的相应引脚：

   - **VCC** 杜邦线一端插入 OLED 的 VCC 引脚，另一端插入 ESP32 的 3.3V 引脚。
   - **GND** 杜邦线一端插入 OLED 的 GND 引脚，另一端插入 ESP32 的 GND 引脚。
   - **SCL** 杜邦线一端插入 OLED 的 SCL 引脚，另一端插入 ESP32 的 GPIO 22 引脚。
   - **SDA** 杜邦线一端插入 OLED 的 SDA 引脚，另一端插入 ESP32 的 GPIO 21 引脚。

3. 在您的 ESP32 开发板上，标记为 **3V3** 的引脚就是 **3.3V** 引脚，您可以放心使用它为 OLED 显示屏供电。

   至于标记为 **D22** 和 **D21** 的引脚，它们对应的是 **GPIO 22** 和 **GPIO 21**。您可以直接将 OLED 的 **SCL** 连接到 **D22**，将 **SDA** 连接到 **D21**。

### **安装所需库**

- 打开 

  Arduino IDE

  ，然后进入 

  库管理器：

  - 点击 **工具 (Tools)** → **库管理器 (Library Manager)**。

- 搜索并安装以下两个库：

  1. **Adafruit SSD1306**：OLED 屏幕的驱动库。
  2. **Adafruit GFX Library**：图形处理库，支持显示文本、图形等。

### **示例代码：闪烁 LED + OLED 显示文本**

你可以结合闪烁 LED 和 OLED 显示屏的基本功能，使用以下示例代码：

```cpp
#include <Wire.h>
#include <Adafruit_GFX.h>
#include <Adafruit_SSD1306.h>

// OLED 屏幕的定义
#define SCREEN_WIDTH 128
#define SCREEN_HEIGHT 64
#define OLED_RESET    -1 // 没有连接 RESET 引脚
Adafruit_SSD1306 display(SCREEN_WIDTH, SCREEN_HEIGHT, &Wire, OLED_RESET);

// LED 引脚
const int ledPin = 2; // ESP32 的板载 LED 通常接在 GPIO 2

void setup() {
  // 初始化串口监视器
  Serial.begin(115200);

  // 初始化 OLED 屏幕
  if (!display.begin(SSD1306_SWITCHCAPVCC, 0x3C)) {
    Serial.println(F("SSD1306 allocation failed"));
    for (;;);
  }
  display.display();
  delay(2000); // 启动时的等待时间

  // 清屏
  display.clearDisplay();

  // 初始化 LED 引脚
  pinMode(ledPin, OUTPUT);
}

void loop() {
  // OLED 显示内容
  display.clearDisplay();  
  display.setTextSize(1); // 设置文字大小
  display.setTextColor(SSD1306_WHITE); // 设置文字颜色
  display.setCursor(0, 0); // 设置光标位置
  display.println(F("Hello, OLED!"));
  display.println(F("LED is blinking!"));
  display.display(); // 更新屏幕

  // 闪烁 LED
  digitalWrite(ledPin, HIGH); // 打开 LED
  delay(500); 
  digitalWrite(ledPin, LOW);  // 关闭 LED
  delay(500);
}
```

### **步骤解释**

- OLED 屏幕初始化：
  - `Adafruit_SSD1306 display(SCREEN_WIDTH, SCREEN_HEIGHT, &Wire, OLED_RESET);` 定义了 OLED 显示屏的对象，屏幕尺寸为 128x64，I2C 地址为 `0x3C`。
  - `display.begin()` 初始化屏幕，`clearDisplay()` 清除屏幕内容，`display.display()` 更新屏幕。
- **闪烁 LED**：使用 `digitalWrite(ledPin, HIGH)` 和 `digitalWrite(ledPin, LOW)` 控制 LED 的亮灭，每 500ms 切换一次。

### **上传代码并查看效果**

- 将代码上传到 ESP32，屏幕上应该会显示 "Hello, OLED!"，并且 LED 会按设定频率闪烁。

## 实现3D星空效果

1. 这个版本添加了许多炫酷的新特效：

   1. 三种飞行模式自动切换：
      - 普通模式：标准星空飞行
      - 超空间模式：加速星轨效果
      - 螺旋模式：旋转穿越效果
   2. 视觉特效增强：
      - 星星拖尾效果
      - 闪电效果
      - 速度线条
      - 超空间跳跃时的放射状线条
   3. 界面元素：
      - 速度/能量计显示
      - 模式指示器
      - 平滑的模式转换动画
   4. 动态效果：
      - 全局旋转效果
      - 星星大小随距离变化
      - 亮度动态调整
      - 螺旋运动轨迹

   ```c++
   #include <Wire.h>
   #include <Adafruit_GFX.h>
   #include <Adafruit_SSD1306.h>
   
   #define SCREEN_WIDTH 128
   #define SCREEN_HEIGHT 64
   #define OLED_RESET -1
   #define SCREEN_ADDRESS 0x3C
   Adafruit_SSD1306 display(SCREEN_WIDTH, SCREEN_HEIGHT, &Wire, OLED_RESET);
   
   #define STAR_COUNT 60
   #define NORMAL_MODE 0
   #define HYPER_MODE 1
   #define SPIRAL_MODE 2
   
   struct Star {
     float x, y, z;
     float speed;
     int brightness;
     float angle;  // 用于螺旋效果
   };
   
   Star stars[STAR_COUNT];
   int currentMode = NORMAL_MODE;
   float globalRotation = 0;
   unsigned long modeStartTime = 0;
   bool transitioningMode = false;
   float hyperdriveCharge = 0;
   const unsigned long MODE_DURATION = 5000; // 每种模式持续5秒
   
   struct Lightning {
     int x1, y1, x2, y2;
     bool active;
     int life;
   };
   Lightning lightnings[5];
   
   void initStar(Star &star, bool randomZ = true) {
     star.x = random(-1000, 1000);
     star.y = random(-1000, 1000);
     star.z = randomZ ? random(100, 4000) : 4000;
     star.speed = random(20, 50);
     star.brightness = random(128, 255);
     star.angle = random(360) * PI / 180.0;
   }
   
   void setup() {
     Serial.begin(115200);
     
     if(!display.begin(SSD1306_SWITCHCAPVCC, SCREEN_ADDRESS)) {
       Serial.println(F("SSD1306 初始化失败"));
       for(;;);
     }
     
     for(int i = 0; i < STAR_COUNT; i++) {
       initStar(stars[i]);
     }
     
     display.display();
     delay(1000);
   }
   
   void drawHyperspaceEffect() {
     for(int i = 0; i < 10; i++) {
       int x1 = SCREEN_WIDTH/2;
       int y1 = SCREEN_HEIGHT/2;
       int x2 = random(SCREEN_WIDTH);
       int y2 = random(SCREEN_HEIGHT);
       display.drawLine(x1, y1, x2, y2, SSD1306_WHITE);  // 修正：SSD1006_WHITE -> SSD1306_WHITE
     }
   }
   
   void createLightning() {
     for(int i = 0; i < 5; i++) {
       if(!lightnings[i].active) {
         lightnings[i].x1 = random(SCREEN_WIDTH);
         lightnings[i].y1 = 0;
         lightnings[i].x2 = lightnings[i].x1 + random(-20, 20);
         lightnings[i].y2 = random(10, 30);
         lightnings[i].active = true;
         lightnings[i].life = random(3, 8);
         break;
       }
     }
   }
   
   void updateLightnings() {
     for(int i = 0; i < 5; i++) {
       if(lightnings[i].active) {
         display.drawLine(lightnings[i].x1, lightnings[i].y1, 
                         lightnings[i].x2, lightnings[i].y2, 
                         SSD1306_WHITE);
         
         lightnings[i].life--;
         if(lightnings[i].life <= 0) {
           lightnings[i].active = false;
         }
       }
     }
   }
   
   void drawSpeedlines() {
     for(int i = 0; i < 15; i++) {
       int x = random(SCREEN_WIDTH);
       int len = random(5, 15);
       display.drawFastHLine(x, random(SCREEN_HEIGHT), len, SSD1306_WHITE);
     }
   }
   
   void drawInterface() {
     // 绘制速度计
     display.drawRect(2, 2, 20, 60, SSD1306_WHITE);
     int speedHeight = map(hyperdriveCharge, 0, 100, 0, 56);
     display.fillRect(4, 58-speedHeight, 16, speedHeight, SSD1306_WHITE);
     
     // 绘制模式指示器
     display.setCursor(SCREEN_WIDTH-30, 5);
     display.setTextSize(1);
     display.setTextColor(SSD1306_WHITE);
     switch(currentMode) {
       case NORMAL_MODE:
         display.print("NRM");
         break;
       case HYPER_MODE:
         display.print("HYP");
         break;
       case SPIRAL_MODE:
         display.print("SPR");
         break;
     }
   }
   
   void updateAndDrawStars() {
     for(int i = 0; i < STAR_COUNT; i++) {
       float moveX = 0, moveY = 0, moveZ = stars[i].speed;
       
       switch(currentMode) {
         case NORMAL_MODE:
           break;
         case HYPER_MODE:
           moveZ *= 3;
           moveX = (stars[i].x - SCREEN_WIDTH/2) * 0.1;
           moveY = (stars[i].y - SCREEN_HEIGHT/2) * 0.1;
           break;
         case SPIRAL_MODE:
           stars[i].angle += 0.05;
           moveX = cos(stars[i].angle) * 50;
           moveY = sin(stars[i].angle) * 50;
           break;
       }
       
       float cosR = cos(globalRotation);
       float sinR = sin(globalRotation);
       float rotX = stars[i].x * cosR - stars[i].y * sinR;
       float rotY = stars[i].x * sinR + stars[i].y * cosR;
       stars[i].x = rotX + moveX;
       stars[i].y = rotY + moveY;
       stars[i].z -= moveZ;
       
       if(stars[i].z < 100) {
         initStar(stars[i], false);
       }
       
       float k = 128.0 / stars[i].z;
       int screenX = (stars[i].x * k) + SCREEN_WIDTH/2;
       int screenY = (stars[i].y * k) + SCREEN_HEIGHT/2;
       
       if(screenX >= 0 && screenX < SCREEN_WIDTH && 
          screenY >= 0 && screenY < SCREEN_HEIGHT) {
         int size = max(1, min(4, int(4000/stars[i].z)));
         int brightness = map(stars[i].z, 100, 4000, stars[i].brightness, 50);
         
         if(currentMode == HYPER_MODE) {
           float k2 = 128.0 / (stars[i].z + stars[i].speed * 2);
           int trailX = (stars[i].x * k2) + SCREEN_WIDTH/2;
           int trailY = (stars[i].y * k2) + SCREEN_HEIGHT/2;
           display.drawLine(screenX, screenY, trailX, trailY, SSD1306_WHITE);  // 修正：SSD1006_WHITE -> SSD1306_WHITE
         } else {
           display.fillCircle(screenX, screenY, size/2, SSD1306_WHITE);
         }
       }
     }
   }
   
   void loop() {
     display.clearDisplay();
     
     unsigned long currentTime = millis();
     if(currentTime - modeStartTime > MODE_DURATION) {
       currentMode = (currentMode + 1) % 3;
       modeStartTime = currentTime;
       transitioningMode = true;
       hyperdriveCharge = 0;
     }
     
     if(transitioningMode) {
       hyperdriveCharge += 5;
       if(hyperdriveCharge >= 100) {
         transitioningMode = false;
       }
     }
     
     globalRotation += 0.01;
     if(globalRotation >= 2*PI) globalRotation -= 2*PI;
     
     updateAndDrawStars();
     
     if(currentMode == HYPER_MODE) {
       drawSpeedlines();
       if(random(100) < 20) createLightning();
       updateLightnings();
     }
     
     if(transitioningMode) {
       drawHyperspaceEffect();
     }
     
     drawInterface();
     
     display.display();
     delay(16);
   }
   ```

   
