# git个性化设置

首先设置用户信息（请替换成你的信息）：

```bash
# 设置用户名和邮箱
git config --global user.name "你的名字"
git config --global user.email "你的邮箱"
```

设置常用命令别名：

```bash
# 常用命令缩写
git config --global alias.st status
git config --global alias.br branch
git config --global alias.co checkout
git config --global alias.ci commit
git config --global alias.df diff
git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"
```

设置颜色和显示：

```bash
# 启用颜色
git config --global color.ui true
git config --global color.branch auto
git config --global color.status auto
git config --global color.diff auto

# 设置显示编码，解决中文显示问题
git config --global core.quotepath false
git config --global gui.encoding utf-8
git config --global i18n.commit.encoding utf-8
git config --global i18n.logoutputencoding utf-8
```

设置默认行为：

```bash
# 设置默认推送行为（推送当前分支到远程同名分支）
git config --global push.default current

# 设置默认合并行为（在合并时创建一个合并提交）
git config --global merge.ff false

# 设置拉取行为（git pull 时自动变基）
git config --global pull.rebase true

# 设置换行符处理
git config --global core.autocrlf input  # 在Linux/Mac下使用
# git config --global core.autocrlf true # 在Windows下使用

# 设置默认编辑器（如果你使用VSCode）
git config --global core.editor "code --wait"
```

设置一些有用的工具：

```bash
# 设置差异化比较工具
git config --global diff.tool vscode
git config --global difftool.vscode.cmd 'code --wait --diff $LOCAL $REMOTE'

# 设置合并工具
git config --global merge.tool vscode
git config --global mergetool.vscode.cmd 'code --wait $MERGED'
```

设置一些安全相关的配置：

```bash
# 禁止合并不相关的历史
git config --global merge.ff false

# 在做git push时要求验证远程服务器的SSL证书
git config --global http.sslVerify true
```

验证配置：

```bash
# 查看所有全局配置
git config --global --list
```

这些设置提供了：

1. 清晰的命令别名，提高工作效率
2. 良好的中文支持
3. 安全的默认行为（如合并策略）
4. 美观的日志显示
5. 合适的工具集成

一些使用提示：

- 使用 `git lg` 可以看到漂亮的提交历史图
- 使用 `git st` 快速查看状态
- 合并时会默认创建合并提交，保持历史清晰
- pull 时会自动使用变基，保持提交历史整洁

## 禁用快速合并

保留完整的分支历史，这样可以清楚地看到每个分支的开发过程。

```bash
# 设置默认的合并提交信息模板
git config --global merge.log true

# 设置合并时的提交信息包含被合并的分支信息
git config --global merge.branchdesc true
```

使用示例：

```bash
# 当你在主分支上合并功能分支时
git checkout main
git merge feature-branch
```

这样合并时会：

1. 强制创建一个合并提交（merge commit）
2. 保留完整的分支图历史
3. 在合并信息中包含被合并分支的描述

如果在某些特殊情况下，你确实想要快速合并，可以在合并时使用 `--ff` 参数：

```bash
git merge --ff feature-branch
```

然后使用 `git lg` 查看，你会看到清晰的分支合并历史。要看具体哪些配置生效了，可以用：

```bash
git config --global --list | grep merge
```

## 合并别名修改禁用快速合并

```bash
# 设置 mg 别名，禁用快速合并
git config --global alias.mg "merge --no-ff"
```

这样设置后：

1. 当你使用 `git mg feature-branch` 时，就会自动创建合并提交
2. 保留完整的分支历史记录
3. 可以清楚地看到哪些提交是从哪个分支合并过来的

你可以用以下命令查看所有的别名设置：

```bash
git config --global --list | grep alias
```

一个完整的工作流程示例：

```bash
git co -b feature    # 创建并切换到特性分支
# 做一些修改...
git ci -m "新功能开发"   # 提交修改
git co main         # 切回主分支
git mg feature      # 合并特性分支，会创建合并提交
git lg              # 查看分支历史图
```

## 查看提交历史别名修改

### 优雅清晰的提交历史格式

这是一个增强的别名，能清楚展示分支的图形结构和各类信息。你已经提供了一个非常好的别名设置，我再给你优化一些细节。

```bash
git config --global alias.llg "log --color --graph --pretty=format:'%Cred%h%Creset - %C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit --all"
```

`--color`: 启用颜色显示。

`--graph`: 展示提交历史的分支结构。

`--pretty=format`: 自定义格式：

- `%h`: 提交的缩写哈希。
- `%d`: 引用名称（如标签、HEAD 等）。
- `%s`: 提交信息。
- `%cr`: 相对时间（如 2 天前）。
- `%an`: 作者名字。
- 颜色部分：`%Cred`, `%Creset` 等调整颜色。

### 黑框适配长度的提交历史（限制单行长度）

有时终端窗口较小，可以通过设置限制显示的字符数，让输出在黑框中更加美观。

```bash
git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset - %C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit --max-count=10"
```

`--max-count=10`: 限制输出的提交记录为最近的 10 条。

这个别名适用于在 **小窗口或黑框** 中查看。

你可以进一步调整 Git 的配色方案。如果需要更改 Git 日志的颜色设置，可以使用：

以下是一个经过优化的配色方案，可以让 **分支**、**状态**、以及**提交历史**更清晰：

```bash
# 启用自动颜色支持
git config --global color.ui auto

# 分支相关配色
git config --global color.branch.current "yellow reverse"  # 当前分支高亮
git config --global color.branch.local "green bold"         # 本地分支
git config --global color.branch.remote "cyan"              # 远程分支

# 状态信息配色
git config --global color.status.added "green bold"         # 新增文件
git config --global color.status.changed "yellow"           # 修改的文件
git config --global color.status.untracked "red bold"       # 未跟踪文件

# 差异(diff)相关配色
git config --global color.diff.meta "blue bold"             # 元信息
git config --global color.diff.frag "magenta bold"          # 区块信息
git config --global color.diff.old "red"                    # 删除的内容
git config --global color.diff.new "green"                  # 新增的内容

# 日志配色（为不同类型的引用设置颜色）
git config --global color.decorate.branch "blue"            # 分支名
git config --global color.decorate.remoteBranch "cyan"      # 远程分支名
git config --global color.decorate.tag "magenta"            # 标签
git config --global color.decorate.head "red bold"          # HEAD 指针
```

配色优化的效果

- **高亮当前分支**：`yellow reverse` 背景翻转，确保在众多分支中一眼看到当前分支。
- **文件状态颜色区分**：绿色表示新增，黄色表示已修改，红色表示未跟踪，避免视觉混淆。
- **差异信息更直观**：蓝色和洋红色用于元信息和区块信息，新增/删除的内容使用红绿配色。

### 与日志配色会不会冲突？

不会有冲突。Git 的日志命令 (`log`) 和引用装饰（`decorate`）的配色设置分别作用于不同的部分。

配色的作用范围：

1. **日志配置 (`lg` alias)**：
   - `--color`: 为 **日志输出**中的内容添加颜色，例如提交哈希、提交者、日期等。
   - `--pretty=format`: 用指定的格式显示提交日志，这里你自定义了 `hash` 为红色、时间为绿色、作者为蓝色等。
2. **引用装饰 (`decorate`) 配置**：
   - 用于 **引用的显示**（如分支名、远程分支、标签、`HEAD` 等）。
   - 在使用 `--decorate` 选项时，这些引用会出现在日志的输出中，但它们的颜色由 `color.decorate` 中的配置控制。

配置生效的逻辑：

- 优先级：

  - 如果你定义的 `alias.lg` 日志命令里使用了 `%C` 相关的颜色配置（如 `%Cred`），这些配色只影响日志输出的具体部分（如哈希、作者）。
  - **引用**（分支、标签等）的颜色仍由 `color.decorate` 决定，因为这些颜色不会被 `--pretty=format` 的自定义影响。

  

  