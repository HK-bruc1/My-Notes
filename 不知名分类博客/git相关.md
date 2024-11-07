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


**日志按如上配置后，日志太多会变成分页模式浏览日志具体操作如下：**

1. 按空格向下翻一页
2. 按q退出分页模式
   因为输入命令后窗口就是一页日志，只需空格即可，其他按键不需要。

# 同步已存在的库

我想把github上已经存在的库同步到我本地仓库，但是我本地仓库已经初始化了并且有东西，我该如何同步呢？

我们需要将远程仓库和本地仓库进行关联并同步。以下是具体步骤：

1.首先，将远程仓库添加为本地仓库的远程源：

```c
git remote add origin GitHub仓库URL
```

2.获取(fetch)远程仓库的所有内容：

```
git fetch origin
```

3.将远程仓库的内容合并到本地分支。由于本地已有内容，建议先尝试：

```
git pull origin master --allow-unrelated-histories
```

(注：如果你的主分支名是master，则替换main为master)

如果出现冲突，你需要：

1. 解决冲突文件中的冲突标记
2. 使用 `git add .` 添加解决后的文件
3. 使用 `git commit -m "解决合并冲突"` 提交更改

如果你想保留本地的所有更改，也可以使用：

```
git pull origin master --rebase
```

最后，将更新后的内容推送到远程：

```
git push --set-upstream origin master
```

需要注意的是，如果你的本地内容与远程内容差异较大，建议先备份本地重要文件，以防发生意外。

# git合并问题

1. 分支修改后是否提交再到主分支合并提交合并信息后推送仓库？（合并后本地主分支会领先远程分支两个提交，感觉有一点重复操作）
   **是的，在你不提交修改时切换分支，会提醒你提交修改。**
1. 在主分支合并使用快速模式导致日志不会出现分支合并历史。

# 添加版本标签

1. `git tag [选项] <标签名> [提交哈希]`
2. **选项**

- `-a`：在创建标签时，自动生成 GPG 签名。
- `-f`：强制更新已存在的标签。
- `-m`：指定标签的注释信息。
- `-s`：在创建标签时，使用 GPG 签名。

```bash
# 创建一个名为 v1.0.0 的标签 
git tag v1.0.0 
# 创建一个名为 v2.0.0 的标签，并附带注释信息 
git tag -m "This is the v2.0.0 release" v2.0.0 
# 强制更新 v1.0.0 标签 
git tag -f v1.0.0 
# 创建一个名为 v3.0.0 的签名标签 
git tag -s v3.0.0
```

## **查看标签**

可以使用 `git tag` 命令查看所有标签。

```bash
git tag
```

## **删除标签**

可以使用 `git tag -d` 命令删除标签。

```bash
# 删除 v1.0.0 标签
git tag -d v1.0.0
```

## **推送标签**

要将标签推送到远程仓库，可以使用 `git push` 命令。

```bash
# 将所有标签推送到远程仓库
git push origin --tags

# 将 v2.0.0 标签推送到远程仓库
git push origin v2.0.0
```

## **使用标签**

可以使用标签来检出特定的版本。

```bash
# 检出 v1.0.0 版本
git checkout v1.0.0

# 检出 v2.0.0 版本
git checkout v2.0.0
```

- **推送标签和推送最新修改是分开的**

# 连接远程仓库

## 新仓库第一次连接（推送）

1. **获取远程仓库地址**：在远程仓库页面找到仓库的 URL 地址，通常以 HTTPS 或者 SSH 协议提供。例如，GitHub 的仓库地址类似于 `https://github.com/username/repository.git` 或 `git@github.com:username/repository.git`。
2. **在本地仓库中设置远程仓库地址**：在命令行或者 Git GUI 工具中，使用 `git remote add` 命令将远程仓库地址添加到本地仓库中，命令格式如下：

```bash
git remote add origin <remote_repository_url>
```

3. **将本地更改推送到远程仓库**：如果你已经在本地进行了一些提交，并且想要将这些更改推送到远程仓库，可以使用 `git push` 命令，命令格式如下：**(建立上游分支后，可直接在分支中使用git push推送)**

```bash
git push -u origin main
```

## 如果另一个本地仓库（2.0 版本）想连接到同一个远程仓库，1.0版本此前已经连接了。

1. 在本地仓库 2.0 的目录中运行以下命令来添加远程仓库：

```bash
git remote add origin <远程仓库 URL>
```

2. 确认远程仓库已经成功添加，可以使用以下命令查看(此命令会列出所有已配置的远程仓库。)：

```bash
git remote -v
```

3. 现在你可以将本地仓库 2.0 中的更改推送到远程仓库的主分支（通常是 `main` 或 `master` 分支），使用以下命令：

```bash
git push origin main
```

- 当你执行 `git push origin main` 命令时，如果远程仓库已经存在 `main` 分支，并且你本地的 `main` 分支与远程的 `main` 分支存在关联，那么它应该会自动建立上游分支。**上游分支是指远程仓库中与你本地分支相对应的分支。如果成功远程推送一次之后，就可只用git push了。git会自动建立上有分支。**
- 本地分支可能处于一个**没有远程分支对应的新的、独立的分支上**：在这种情况下，你需要使用 **`git push -u origin main`** 命令来将本地分支推送到远程仓库，并将其设置为上游分支。 `-u` 选项用于将本地分支与远程分支建立关联，并设置为上游分支。**（新本地仓库第一次推送可以这么搞）**

4. 在你试图推送到远程仓库时，而**远程仓库中存在你本地没有的提交**。这种情况可能是**由于其他人在相同的分支上进行了提交（之前1.0版本进行了提交）**，或者你在**其他地方（其他本地仓库）进行了提交。**
   **两个选择：**
   - 运行 `git pull` 命令，将远程仓库的变更拉取到你本地仓库。这样可以合并远程仓库的变更到你的本地分支上。

```bash
git pull origin main
```

如果在拉取时出现冲突，需要解决冲突后再次提交。你可以使用 `git status` 查看冲突文件，并手动解决冲突。

- 如果不关心远程仓库中的变更，可以使用 `git push -f` 强制推送本地分支到远程仓库。但这可能会覆盖远程仓库中的变更，所以请谨慎使用。

```bash
git push -f origin main
```

请注意，强制推送可能会导致其他协作者的工作丢失或造成代码不一致，所以在使用时请务必谨慎。最好先确保你理解了远程仓库中的变更，以及你的推送对其他人的影响。(**因为我的2.0版本跟1.0完善不一样的开发，所以我并不在乎1.0。直接使用git push -f命令强制推送你的2.0版本到远程仓库，覆盖了远程仓库中的所有变更。这种操作会抹去远程仓库中除了标签之外的所有提交记录，并将远程仓库的内容替换为你本地的2.0版本)

- 如果两个版本的代码完全不同，那么合并操作可能不会产生有意义的结果，因为两个版本的代码结构和内容不同。在这种情况下，你可以选择直接推送2.0版本到远程仓库，覆盖远程仓库中的内容，而不需要进行合并操作。
- 如果两个版本的代码完全不同，那么合并操作可能会产生冲突，需要手动解决。**在合并过程中，Git 会尝试自动合并两个版本之间的差异。**

## 删除远程文件，保留本地的

- 要删除远程仓库中的.idea文件，但保留本地的，可以使用以下命令：

```bash
git rm --cached .idea -r
git commit -m "Remove .idea files from remote repository"
git push origin main
```

# Git工作流程

在开发新功能时，遵循以下步骤是常见的工作流程：

## **创建功能分支**

从 `main` 分支创建一个新的功能分支，以便独立开发新功能。

```bash
git checkout -b feature/your-feature-name
```

## **开发和提交代码**

- 在新创建的功能分支上进行开发，并频繁提交代码。

```bash
git add .
git commit -m "Add initial implementation of your feature"
```

## **同步远程仓库**

- 如果需要，可以将功能分支推送到远程仓库，以便与团队成员共享或备份。

```bash
git push origin feature/your-feature-name
```

## **测试和代码审查**

- 进行本地测试，确保新功能工作正常。

- 提交 Pull Request (PR) 或 Merge Request (MR) 到 `main` 分支，以便团队成员进行代码审查。

- 代码审查通过后，合并到 `main` 分支。

## **合并功能分支**

将功能分支合并到 `main` 分支。

```bash
git checkout main
git pull origin main  # 确保本地 main 是最新的
git merge feature/your-feature-name
```

**部署更新**：

- 合并后，将更新后的 `main` 分支部署到线上环境。(触发一个工作流)

```bash
git push origin main
```

## **删除功能分支**

- 合并成功后，可以删除本地和远程的功能分支。

```bash
git branch -d feature/your-feature-name
git push origin --delete feature/your-feature-name
```

# 配置自动设置远程

## 设置上游分支并推送

1. **推送并设置上游分支**： 你可以使用以下命令，将本地 `master` 分支推送到远程 `origin` 并同时设置上游分支：

2. ```
   git push --set-upstream origin master
   ```

   这条命令将会把本地 `master` 分支的更改推送到远程 `origin` 的 `master` 分支，并将其设置为本地分支的上游分支。

3. **后续推送**： 一旦设置了上游分支，以后你只需使用 `git push` 就可以将更改推送到远程。

如果你希望以后所有新创建的分支在推送时都能自动设置上游分支，可以运行以下命令：

```
git config --global push.autoSetupRemote true
```

这样在创建新分支并推送时，Git 会自动将其与远程分支关联。

# 因为网络问题推送失败

怎么推送不了？ $ git push fatal: unable to access 'https://github.com/HK-bruc1/My-Notes/': Failed to conne ct to github.com port 443 after 21098 ms: Could not connect to server

这个错误通常是网络连接问题导致无法访问 GitHub。让我给你几个解决方案：

1. **检查网络连接**

```bash
# 测试与GitHub的连接
ping github.com

# 或者使用curl测试
curl -v https://github.com
```

2. **配置代理**（如果你使用代理上网）

```bash
# 设置 HTTP 代理
git config --global http.proxy http://127.0.0.1:端口号

# 设置 HTTPS 代理
git config --global https.proxy https://127.0.0.1:端口号

# 查看当前代理设置
git config --global --get http.proxy
git config --global --get https.proxy
```

3. **取消代理**（如果代理设置有问题）

```bash
# 取消代理设置
git config --global --unset http.proxy
git config --global --unset https.proxy
```

4. 使用SSH而不是HTTPS

```
# 检查当前远程URL
git remote -v

# 更改为SSH URL
git remote set-url origin git@github.com:用户名/仓库名.git

在 GitHub 上获取 SSH 链接的步骤：（待整理）

打开你的 GitHub 仓库（My-Notes）
点击绿色的 "Code" 按钮（通常在页面右上方）
在弹出的下拉菜单中：

默认可能显示 HTTPS
点击 "SSH" 选项卡
会显示类似这样的链接：git@github.com:HK-bruc1/My-Notes.git
点击复制按钮

在本地更改远程仓库 URL：
git remote set-url origin git@github.com:HK-bruc1/My-Notes.git

验证更改：
git remote -v
# 应该显示：
# origin  git@github.com:HK-bruc1/My-Notes.git (fetch)
# origin  git@github.com:HK-bruc1/My-Notes.git (push)

注意：如果你之前没有设置过 SSH key，需要：
生成 SSH key：
ssh-keygen -t ed25519 -C "319334854@qq.com"

将公钥添加到 GitHub：

复制 ~/.ssh/id_rsa.pub 文件内容
在 GitHub 设置中添加 SSH key：

点击头像 -> Settings
左侧菜单找到 "SSH and GPG keys"
点击 "New SSH key"
粘贴公钥并保存

测试 SSH 连接：
ssh -T git@github.com
```

5. 刷新DNS缓存

```bash
# Windows
ipconfig /flushdns

# MacOS
sudo killall -HUP mDNSResponder
```

6. **检查防火墙设置**

- 确保Git可以通过防火墙访问网络
- 端口443需要允许访问

7. **更新hosts文件**

```bash
# 在hosts文件中添加GitHub的IP
# Windows: C:\Windows\System32\drivers\etc\hosts
# Linux/Mac: /etc/hosts

140.82.114.4 github.com
```

如果以上方法都不行，你可以：

1. 尝试使用VPN
2. 检查是否是SSL证书问题
3. 暂时禁用SSL验证（不推荐）：

```bash
git config --global http.sslVerify false
```

最后，如果问题仍然存在：

1. 尝试清除Git凭据
2. 重新安装Git
3. 检查系统时间是否正确（SSL验证需要）

记得操作完成后再次尝试推送：

```bash
git push -v origin main  # 使用-v查看详细输出
```

## 使用SSH连接GitHub

是的,你可以通过添加 SSH key 到 ssh-agent 来避免重复输入密码。我来帮你解决这个问题。

对于 Windows 系统(Git Bash),你可以按照以下步骤操作:

1.首先确保 ssh-agent 正在运行:

```bash
eval "$(ssh-agent -s)"
```

2.添加你的 SSH 私钥到 ssh-agent:

```bash
ssh-add ~/.ssh/id_ed25519
```

3.为了让这个设置永久生效,你可以修改或创建 `~/.bashrc` 或 `~/.bash_profile` 文件,添加以下内容:

```bash
env=~/.ssh/agent.env
agent_load_env () { test -f "$env" && . "$env" >| /dev/null ; }
agent_start () {
    (umask 077; ssh-agent >| "$env")
    . "$env" >| /dev/null ; }
agent_load_env
# agent_run_state: 0=agent running w/ key; 1=agent w/o key; 2=agent not running
agent_run_state=$(ssh-add -l >| /dev/null 2>&1; echo $?)
if [ ! "$SSH_AUTH_SOCK" ] || [ $agent_run_state = 2 ]; then
    agent_start
    ssh-add
elif [ "$SSH_AUTH_SOCK" ] && [ $agent_run_state = 1 ]; then
    ssh-add
fi
unset env
```

你分享的这段脚本是一个更完整的解决方案。这个脚本确实可以实现永久生效,原理是:

1. 将 SSH agent 的环境变量保存到文件中 (~/.ssh/agent.env)
2. 每次启动时检查 agent 的状态
3. 根据状态自动启动 agent 和添加密钥

给文件添加执行权限:

```bash
chmod +x ~/.bashrc  # 或 ~/.bash_profile
```

使配置立即生效:

```bash
source ~/.bashrc  # 或 source ~/.bash_profile
```

这样设置后:

- 每次打开新的终端窗口时,脚本都会自动运行
- 如果 SSH agent 没在运行,会自动启动
- 如果密钥没有添加,会自动添加
- 即使重启电脑后也会自动生效

这个方案比之前的更健壮,因为它会处理各种可能的状态。要验证是否生效,你可以:

1. 关闭当前终端
2. 打开新终端
3. 尝试 git push 操作,看是否还需要输入密码

4.如果你想让密钥在电脑重启后仍然保持,可以在 `~/.ssh/config` 文件中添加以下配置(如果文件不存在则创建):

```bash
Host github.com
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_ed25519
    AddKeysToAgent yes
    UseKeychain yes
```

完成这些设置后,你应该就不需要频繁输入密码了。需要注意的是,这样设置虽然方便,但要确保你的电脑安全,因为这意味着任何可以访问你电脑的人都能使用你的 GitHub 账户。

## bash开代理加速push

我的git bash推送到远程仓库很慢，我用clash开了代理，我可以使用代理吗？ 

```bash
export https_proxy=http://127.0.0.1:7897 

ttp_proxy=http://127.0.0.1:7897 

all_proxy=socks5://127.0.0.1:7897
```

你可以在 Git Bash 中使用代理来加速推送。你的配置已经很接近正确了，只需要根据代理的实际协议和端口调整一下。如果你使用 Clash 并且代理端口确实是 `7897`，那你的配置可能看起来像这样：

```c
export https_proxy="http://127.0.0.1:7897"
export http_proxy="http://127.0.0.1:7897"
export all_proxy="socks5://127.0.0.1:7897"
```

可以按照以下步骤检查和验证代理是否生效：

1. **确认 Clash 代理端口**：确保 Clash 的端口与配置的一致。可以在 Clash 配置文件或控制面板中查看。
2. **验证网络连接**：在 Git Bash 中执行 `curl https://www.google.com` 或其他命令，确认代理是否成功连接外网。
3. **测试 Git 推送**：在 Git Bash 中执行 `git push`，查看速度是否有所提升。

如果仍然很慢，可以尝试将 `all_proxy` 改成 `http://127.0.0.1:7897` 或其他代理协议，或通过 Clash 面板调整规则。

接下来，你可以尝试运行 `git push` 看是否能加速推送到远程仓库。如果 `git push` 速度仍然较慢，可能是网络环境的因素，你可以考虑以下几点来进一步优化：

1. **确认代理协议**：如果当前使用的是 `socks5` 代理，可以尝试改成 `http` 或 `https`，看是否能带来不同的效果。
2. **代理模式**：确认 Clash 处于全局模式（Global Mode）或者正确设置了规则模式（Rule Mode），这样 Git Bash 的流量才能被代理处理。
3. **检查端口和连接稳定性**：如果有其他的代理端口，可以尝试不同端口的配置，或检查网络环境是否稳定。
