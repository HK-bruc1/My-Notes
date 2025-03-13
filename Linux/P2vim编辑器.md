# vim编辑器

## 安装

在大多数Linux发行版中，可以通过包管理器安装Vim。根据你使用的发行版，命令会有所不同：

**Ubuntu/Debian系**：

```bash
sudo apt update
sudo apt install vim
```

安装完成后，可以通过以下命令检查是否成功安装：

```bash
vim --version
```

## 配置Vim为C语言友好

Vim的配置文件通常位于 ~/.vimrc。如果没有这个文件，可以创建一个：

```
touch ~/.vimrc
```

然后用Vim打开它进行编辑：

```
vim ~/.vimrc
```

在文件中添加以下设置，这些是为C语言开发优化的常用配置：

```
" 基本设置
set number              " 显示行号
set tabstop=4           " Tab键等于4个空格
set shiftwidth=4        " 缩进宽度为4个空格
set expandtab           " 将Tab键转换为空格
set autoindent          " 自动缩进
set smartindent         " 智能缩进，适用于C语言
set cindent             " C语言风格的缩进

" 语法高亮
syntax on               " 开启语法高亮
set showmatch           " 高亮显示匹配的括号

" 文件类型检测
filetype on
filetype plugin on
filetype indent on

" 搜索优化
set hlsearch            " 高亮搜索结果
set incsearch           " 实时搜索
set ignorecase          " 搜索时忽略大小写
set smartcase           " 启用智能大小写敏感搜索

" 可选：设置颜色主题（如果有支持）
colorscheme desert      " 使用desert主题，具体取决于你的Vim版本支持的主题

" C语言特定的快捷键（可选）
autocmd FileType c setlocal makeprg=gcc\ -Wall\ -o\ %<\ %  " 启用警告选项
autocmd FileType c nnoremap <F5> :w<CR>:make<CR>          " 按F5编译当前C文件

" 自动保存
autocmd BufLeave * :w

" 显示当前行和列
set ruler

" 行尾空格高亮
set list
set listchars=trail:·

" 其他优化设置
set syntax=on           " 开启语法高亮
set cmdheight=1         " 设定命令行的行数为 1
set hidden              " 允许在有未保存的修改时切换缓冲区
set backupcopy=yes      " 设置备份时的行为为覆盖
set autochdir           " 自动切换当前目录为当前文件所在的目录
set smartcase           " 启用智能大小写敏感搜索
set incsearch           " 输入搜索内容时就显示搜索结果
set hlsearch            " 搜索时高亮显示被找到的文本
set matchtime=2         " 短暂跳转到匹配括号的时间
set cursorline          " 突出显示当前行
set laststatus=2        " 显示状态栏
set statusline=\ %<%F[%1*%M%*%n%R%H]%=\ %y\ %0(%{&fileformat}\ %{&encoding}\ %c:%l/%L%)\ " 设置在状态行显示的信息
```

保存并退出：

- 按 Esc 进入命令模式
- 输入 :wq 并按回车

### 验证配置

**保存配置文件**： 确保你的配置文件（通常是 `~/.vimrc` 或 `~/.config/nvim/init.vim`，如果你使用的是Neovim）已保存好，并且配置项没有拼写错误。

**重新加载配置**：

- 如果你已经在Vim中，可以通过以下命令重新加载配置：

- ```
  :source $MYVIMRC
  ```

- 这会重新加载 `~/.vimrc` 文件并使更改立即生效。

**退出并重新打开Vim**： 如果你不想手动重新加载配置，退出Vim并重新打开它也会使新的配置生效。

重新打开Vim，编辑一个C文件（例如 test.c），检查是否应用了以上设置：

```
vim test.c
```

输入一些C代码，比如：

```
#include <stdio.h>
int main() {
    printf("Hello, world!\n");
    return 0;
}
```

## vim常用操作

### **命令模式下的操作**

- **光标移动**
  - `h`：左移
  - `l`：右移
  - `j`：下移
  - `k`：上移
  - `w`：跳到下一个单词
  - `b`：跳到上一个单词
  - `0`：跳到行首
  - `^`：跳到行首（非空字符）
  - `$`：跳到行尾
  - `gg`：跳到文件开头
  - `G`：跳到文件结尾
  - `Ctrl-d`：向下滚动半屏
  - `Ctrl-u`：向上滚动半屏
- **编辑操作**
  - `x`：删除光标所在字符
  - `dd`：删除当前行
  - `D`：删除当前行光标后的所有内容
  - `yy`：复制当前行
  - `p`：粘贴到光标后
  - `P`：粘贴到光标前
  - `u`：撤销
  - `Ctrl-r`：重做
  - `.`：重复上一次操作
- **查找与替换**
  - `/关键词`：向下搜索
  - `?关键词`：向上搜索
  - `n`：跳转到下一个匹配
  - `N`：跳转到上一个匹配
  - `:%s/old/new/g`：全局替换 `old` 为 `new`
  - `:s/old/new/g`：替换当前行的 `old` 为 `new`
- **窗口与标签页**
  - `:vsp` 或 `:split`：垂直/水平拆分窗口
  - `Ctrl-w h/j/k/l`：在拆分窗口间移动
  - `:tabnew`：打开新标签页
  - `gt` / `gT`：切换标签页
- **保存和退出**
  - `:w`：保存文件
  - `:q`：退出
  - `:wq` 或 `ZZ`：保存并退出
  - `:q!`：强制退出（不保存）

### **插入模式下的操作**

- `i`：进入插入模式（在光标前插入，常用）
- `a`：进入插入模式（在光标后插入）
- `o`：在当前行下方插入新行并进入插入模式
- `Esc`：退出插入模式回到命令模式
  - 按住冒号才是回到命令模式。
