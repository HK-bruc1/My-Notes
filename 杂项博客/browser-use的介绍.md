# browser-use的介绍

## browser-use开源项目的实现原理

**这个开源项目的实现原理？让AI大模型能够访问本地资源？是基于MCP上下文协议吗？**

“browser-use” (https://github.com/browser-use/browser-use) 是一个开源项目，旨在使 AI 大模型能够像人类一样“使用”浏览器，通过自动化浏览器操作与网页内容交互。其核心目标是**让网站对 AI 代理变得可访问，使 AI 能够执行诸如导航页面、点击按钮、填写表单和提取信息等任务。**

#### 实现原理

该项目的实现基于 **Playwright**，一个流行的浏览器自动化库。Playwright 支持控制 Chromium、Firefox 和 WebKit 等浏览器，**允许程序化地执行打开页面、与网页元素交互（例如点击、输入文本）和从网页中提取数据等操作**。在 “browser-use” 中，**这种自动化功能被封装为一个接口，使 AI 模型能够根据任务理解发出命令来控制浏览器。**

例如，一个 AI 模型可能收到指令：“访问某个网站，搜索产品并获取其价格。” AI 会将此指令解析为一系列浏览器操作，例如导航到目标网站、在搜索栏中输入文本、点击搜索按钮，然后从结果页面中提取价格信息。这种方式让 AI 能够以程序化的方式完成原本需要人类手动操作的任务。

根据相关信息，存在一个名为 “browser-use-mcp-server” 的关联仓库，这表明 “browser-use” 项目可能使用或集成了一个 **MCP 服务器来管理 AI 模型的上下文**。MCP 协议可能用于在多个请求或会话中维护 AI 的交互状态，支持更复杂的多步骤任务。

例如，MCP 服务器可能负责会话管理，记录浏览器的状态（例如登录状态、当前页面），使 AI 能够执行需要持久性的任务，如填写多页表单或完成复杂的工作流。因此，虽然 “browser-use” 的核心实现依赖 Playwright，但它可能通过 MCP 协议增强了上下文管理和状态保持能力。

- **这意味着可以被支持MCP客户端的AI大模型调用。**
- **比如claude的桌面应用**，让结果输出更加自然和直观。

#### 关键特性

- **AI 模型集成**：支持多种 AI 模型（如 OpenAI、Anthropic、DeepSeek），用于解析任务并生成浏览器操作指令。
- **Web UI**：通过独立的 “browser-use/web-ui” 仓库提供用户界面，用户可以配置任务、监控进度和查看结果。
- **自定义浏览器支持**：允许用户使用自己的浏览器实例，便于利用现有认证状态，避免重复登录。
- **持久会话**：支持在任务间保持浏览器开启，保留历史记录和状态以实现连续交互。
- **安全与权限**：考虑到浏览器自动化的潜在风险，项目可能包含权限管理机制，确保安全运行。

## ui版本的实现

[项目地址](https://github.com/browser-use/web-ui)

### 先决条件

- Python 3.11 或更高版本（项目使用python写）
- Git（用于克隆存储库）

### 搭建项目环境

#### 安装UV工具

建议使用[uv](https://docs.astral.sh/uv/)来管理 Python 环境。

打开powershell执行安装命令：

```c
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
```

给出如下提示就算安装成功了：

```bash
PS C:\Users\admin> powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"                   Downloading uv 0.6.12 (x86_64-pc-windows-msvc)                                                                          Installing to C:\Users\admin\.local\bin
  uv.exe
  uvx.exe
everything's installed!

To add C:\Users\admin\.local\bin to your PATH, either restart your shell or run:

    set Path=C:\Users\admin\.local\bin;%Path%   (cmd)
    $env:Path = "C:\Users\admin\.local\bin;$env:Path"   (powershell)
PS C:\Users\admin>
```

根据提供的命令行输出，安装程序已经成功完成了以下步骤：

- 下载了 uv 0.6.12 版本（Downloading uv 0.6.12 (x86_64-pc-windows-msvc)）。
- 将其安装到 C:\Users\admin\.local\bin 目录下，包含 uv.exe 和 uvx.exe 文件。
- 显示 "everything's installed!"，表明安装过程已顺利完成。

**下一步：确保可以使用 uv**

虽然安装已经成功，但你需要将安装目录添加到 PATH 环境变量中，才能在命令行中直接运行 uv 命令。以下是具体步骤：

**永久设置 PATH(好像admin用户的环境变量这个条目本来就有，如果有可以省略)**

- 在 Windows 中：
  1. 打开“系统属性”窗口（搜索“环境变量”）。
  2. 在“系统变量”或“用户变量”中找到 Path，点击“编辑”。
  3. 添加新条目：C:\Users\admin\.local\bin。
  4. 保存并关闭窗口。

**验证安装**

设置 PATH 后：

1. 打开一个新的命令行窗口（CMD 或 PowerShell）。

2. 运行以下命令：

3. ```bash
   uv --version
   ```

如果显示版本信息（例如 uv 0.6.12），则说明 uv 已完全安装并可用。

#### 克隆项目到本地

本地创建一个文件夹并右键呼出git bash或者打开cmd使用git clone命令

```bash
git clone https://github.com/browser-use/web-ui.git
```

切换到项目目录

```bash
cd web-ui
```

#### 创建激活一个虚拟环境

```bash
uv venv --python 3.11
```

提示信息：

```bash
$ uv venv --python 3.11
Using CPython 3.11.11
Creating virtual environment at: .venv
Activate with: source .venv/Scripts/activate
```

激活虚拟环境：

- Windows（命令提示符）：

```bash
.venv\Scripts\activate
```

- Windows（PowerShell）：

```bash
.\.venv\Scripts\Activate.ps1
```

#### 安装项目的依赖

安装 Python 包：

```bash
uv pip install -r requirements.txt
```

在 Playwright 中安装浏览器：您可以通过运行以下命令安装特定的浏览器：

- 因为我本地使用的是chrome浏览器

```bash
playwright install --with-deps chromium
```

- 安装所有浏览器

  - ```bash
    playwright install
    ```

    

#### 配置环境

1. 创建示例环境文件的副本：

   - Windows（命令提示符）：

   - ```bash
     copy .env.example .env
     ```

   - macOS/Linux/Windows (PowerShell)：

   - ```bash
     cp .env.example .env
     ```

2. 在您喜欢的文本编辑器中打开`.env`并添加您的 API 密钥和其他设置

   1. 我这里使用vdcode

   2. ```
      # OpenAI API 的终端地址
      OPENAI_ENDPOINT=https://api.openai.com/v1
      # OpenAI API 的密钥
      OPENAI_API_KEY=
      
      # Anthropic API 的密钥
      ANTHROPIC_API_KEY=
      # Anthropic API 的终端地址
      ANTHROPIC_ENDPOINT=https://api.anthropic.com
      
      # Google API 的密钥
      GOOGLE_API_KEY=
      
      # Azure OpenAI API 的终端地址
      AZURE_OPENAI_ENDPOINT=
      # Azure OpenAI API 的密钥
      AZURE_OPENAI_API_KEY=
      # Azure OpenAI API 的版本
      AZURE_OPENAI_API_VERSION=2025-01-01-preview
      
      # DeepSeek API 的终端地址
      DEEPSEEK_ENDPOINT=https://api.deepseek.com
      # DeepSeek API 的密钥
      DEEPSEEK_API_KEY=
      
      # Mistral API 的密钥
      MISTRAL_API_KEY=
      # Mistral API 的终端地址
      MISTRAL_ENDPOINT=https://api.mistral.ai/v1
      
      # Ollama API 的终端地址 (通常用于本地部署)
      OLLAMA_ENDPOINT=http://localhost:11434
      
      # 阿里云 DashScope API 的终端地址 (兼容模式)
      ALIBABA_ENDPOINT=https://dashscope.aliyuncs.com/compatible-mode/v1
      # 阿里云 DashScope API 的密钥
      ALIBABA_API_KEY=
      
      # Moonshot API 的终端地址
      MOONSHOT_ENDPOINT=https://api.moonshot.cn/v1
      # Moonshot API 的密钥
      MOONSHOT_API_KEY=
      
      # Unbound API 的终端地址
      UNBOUND_ENDPOINT=https://api.getunbound.ai
      # Unbound API 的密钥
      UNBOUND_API_KEY=
      
      # 设置为 false 以禁用匿名遥测数据发送
      ANONYMIZED_TELEMETRY=false
      
      # 日志级别: 设置为 debug 以启用详细日志记录，设置为 result 以仅获取结果。可用选项: result | debug | info
      BROWSER_USE_LOGGING_LEVEL=info
      
      # Chrome 浏览器设置
      # Chrome 浏览器的可执行文件路径
      CHROME_PATH=
      # Chrome 浏览器的用户数据目录
      CHROME_USER_DATA=
      # Chrome 浏览器调试端口
      CHROME_DEBUGGING_PORT=9222
      # Chrome 浏览器调试主机
      CHROME_DEBUGGING_HOST=localhost
      # 设置为 true 以在 AI 任务之间保持浏览器打开
      CHROME_PERSISTENT_SESSION=false
      # Chrome 开发者工具协议 (CDP) 连接字符串
      CHROME_CDP=
      # 显示设置
      # 格式: 宽度x高度x颜色深度
      RESOLUTION=1920x1080x24
      # 宽度 (像素)
      RESOLUTION_WIDTH=1920
      # 高度 (像素)
      RESOLUTION_HEIGHT=1080
      
      # VNC 设置
      # VNC 密码
      VNC_PASSWORD=youvncpassword
      ```

   3. 将DeepSeek的API填入即可

### 用法

完成上述安装步骤后，启动应用程序：

**运行WebUI**

```bash
python webui.py --ip 127.0.0.1 --port 7788
```

等一会出现提示即可访问：

```bash
(web-ui) C:\Users\admin\Desktop\MCP\web-ui>python webui.py --ip 127.0.0.1 --port 7788
INFO     [browser_use] BrowserUse logging setup complete with level info
INFO     [__main__] Total registered components: 27
* Running on local URL:  http://127.0.0.1:7788

To create a public link, set `share=True` in `launch()`.
```

**WebUI 选项**

- `--ip`：绑定WebUI的IP地址，默认为`127.0.0.1`。
- `--port`：绑定WebUI的端口，默认为`7788`。
- `--theme`：用户界面的主题。默认为`Ocean`。
  - **Default**：具有平衡设计的标准主题。
  - **Soft**：温和、柔和的配色方案，带来轻松的观看体验。
  - **Monochrome**：采用最少颜色的灰度主题，简洁且突出重点。
  - **Glass**：光滑、半透明的设计，具有现代感。
  - **Origin**：经典、复古风格的主题，带来怀旧之感。
  - **Citrus**：充满活力的柑橘风格调色板，颜色明亮清新。
  - **Ocean** （默认）：蓝色的、受海洋启发的主题，给人一种平静的效果。
- `--dark-mode`：为用户界面启用暗模式。

**访问**

**访问 WebUI：**打开您的 Web 浏览器并导航到`http://127.0.0.1:7788`

#### 界面设置

1.**LLM设置**

- 选择模型提供商
- 选择具体型号
- 其他不重要

2.**使用您自己的浏览器（可选）：**

- 可以避免掉一些认证问题。
- 在.env中设置
  - 在左下角本机搜索栏，搜索`CHROME`
  - 右击打开文件位置
  - 接着再右键打开文件位置，找到可执行文件`.exe`
  - 右键复制文件路径
- **设置之后重启一下ui界面**
  - 按ctrl+c结束
  - 界面重新配置一下

设置`CHROME_PATH`为浏览器的可执行路径和`CHROME_USER_DATA`浏览器的用户数据目录。`CHROME_USER_DATA`如果您想使用本地用户数据，**请留空**。

```
 CHROME_PATH="C:\Program Files\Google\Chrome\Application\chrome.exe"
 CHROME_USER_DATA="C:\Users\YourUsername\AppData\Local\Google\Chrome\User Data"
```

- 注意：`YourUsername`对于 Windows 系统，请将其替换为您实际的 Windows 用户名

- 关闭所有 Chrome 窗口
- 在非 Chrome 浏览器（例如 Firefox 或 Edge）中打开 WebUI。这很重要，因为持久浏览器上下文将在运行代理时使用 Chrome 数据。
- 在浏览器设置中选中“使用自己的浏览器”选项。



3.**保持浏览器打开（可选）：**

- 可以看到AI操作本地资源的效果

- `CHROME_PERSISTENT_SESSION=true`在文件中设置`.env`

#### 另一种免登录的方法(更稳定)

把cookie导入进playwright

## 错误日志记录

- 将本地防火墙全部关闭
- 在ui设置界面把使用 Vision选项关闭，因为deepseek没有这个功能。

```
Exception in callback _ProactorBasePipeTransport._call_connection_lost(None)
handle: <Handle _ProactorBasePipeTransport._call_connection_lost(None)>
Traceback (most recent call last):
  File "C:\Users\admin\AppData\Roaming\uv\python\cpython-3.11.11-windows-x86_64-none\Lib\asyncio\events.py", line 84, in _run    self._context.run(self._callback, *self._args)
  File "C:\Users\admin\AppData\Roaming\uv\python\cpython-3.11.11-windows-x86_64-none\Lib\asyncio\proactor_events.py", line 165, in _call_connection_lost
    self._sock.shutdown(socket.SHUT_RDWR)
ConnectionResetError: [WinError 10054] 远程主机强迫关闭了一个现有的连接。
C:\Users\admin\Desktop\MCP\web-ui\.venv\Lib\site-packages\browser_use\agent\message_manager\views.py:59: LangChainBetaWarning: The function `load` is in beta. It is actively being worked on, so the API may change.
  value['message'] = load(value['message'])
INFO     [agent] 🚀 Starting task: go to google.com and type 'OpenAI' click search and give me the first url
INFO     [src.agent.custom_agent]
📍 Step 1
ERROR    [browser] Failed to initialize Playwright browser: HTTPConnectionPool(host='127.0.0.1', port=7897): Read timed out. 
(read timeout=2)
WARNING  [browser] Page load failed, continuing...
ERROR    [browser] Failed to initialize Playwright browser: HTTPConnectionPool(host='127.0.0.1', port=7897): Read timed out. 
(read timeout=2)
ERROR    [agent] ❌ Result failed 1/3 times:
 HTTPConnectionPool(host='127.0.0.1', port=7897): Read timed out. (read timeout=2)
INFO     [src.agent.custom_agent]
📍 Step 1
ERROR    [browser] Failed to initialize Playwright browser: HTTPConnectionPool(host='127.0.0.1', port=7897): Read timed out. 
(read timeout=2)
WARNING  [browser] Page load failed, continuing...
ERROR    [browser] Failed to initialize Playwright browser: HTTPConnectionPool(host='127.0.0.1', port=7897): Read timed out. 
(read timeout=2)
ERROR    [agent] ❌ Result failed 2/3 times:
 HTTPConnectionPool(host='127.0.0.1', port=7897): Read timed out. (read timeout=2)
INFO     [src.agent.custom_agent]
📍 Step 1
ERROR    [browser] Failed to initialize Playwright browser: HTTPConnectionPool(host='127.0.0.1', port=7897): Read timed out. 
(read timeout=2)
WARNING  [browser] Page load failed, continuing...
ERROR    [browser] Failed to initialize Playwright browser: HTTPConnectionPool(host='127.0.0.1', port=7897): Read timed out. 
(read timeout=2)
ERROR    [agent] ❌ Result failed 3/3 times:
 HTTPConnectionPool(host='127.0.0.1', port=7897): Read timed out. (read timeout=2)
ERROR    [src.agent.custom_agent] ❌ Stopping due to 3 consecutive failures
WARNING  [agent] No history to create GIF from
```

日志中提示无法连接到本地 127.0.0.1:7897：

查找占用该端口的进程PID：

```
netstat -aon | findstr :7897
```

从输出结果来看，所有这些记录均与同一个进程有关（PID 为 7216），该进程正在监听 127.0.0.1:7897 端口。出现多个状态（如 ESTABLISHED、TIME_WAIT、CLOSE_WAIT 等）属于 TCP 协议正常的连接管理现象。例如：

- **LISTENING**：表示该端口处于监听状态，等待新的连接。
- **ESTABLISHED**：表示当前已经建立的连接。
- **TIME_WAIT** 与 **CLOSE_WAIT**：分别表示连接终止后的等待状态，这在网络连接中是常见且正常的现象。

如果想进一步确认该进程的详细信息，可执行以下命令：

```
tasklist /FI "PID eq 7216"
```

从查询结果来看，PID 为 7216 的进程映像名称为 **verge-mihomo.exe**。这表明该进程正在使用 127.0.0.1:7897 端口。如果该进程不是您期望中用于运行 Playwright 或相关服务的进程，那么它可能会干扰目标服务的正常启动或通信。

127.0.0.1:7897端口被clash（verge-mihomo.exe）占用

- ```bash
  taskkill /PID 7216 /F
  ```

- 这是clash的代理端口可以去clash中随机换一个端口即可。

还是超时的话，在启动ui时把代理取消掉，等服务启动后再打开。

## 删除

把虚拟环境文件夹删除即可，所有下载的包都在这里了。UV就留着吧。

