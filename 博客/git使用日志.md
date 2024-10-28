# 日志美化
默认的日志不太直观，从网络上参考了一下做出了如下配置：
```bash
git config --global alias.lg 'log --color --graph --decorate --pretty=format:\'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset\' --abbrev-commit --all'
git config --global alias.slg 'log --color --graph --decorate --pretty=format:\'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset\' --abbrev-commit --all -n 10'
```
日志按如上配置后，日志太多会变成分页模式浏览日志具体操作如下：
1. 按空格向下翻一页
2. 按q退出分页模式
    因为输入命令后窗口就是一页日志，只需空格即可，其他按键不需要。

# config文件

```ini
# 用户信息
[user]
    name = 木剑游侠  # 设置 Git 提交时使用的用户名
    email = 86511077+HK-bruc1@users.noreply.github.com  # 设置 Git 提交时使用的电子邮件地址
    # 注意：
    # - 这个电子邮件地址格式（86511077+HK-bruc1@users.noreply.github.com）是 GitHub 提供的，允许用户使用隐私保护的地址。
    # - 在 GitHub 上进行提交时，这个地址会被识别并关联到你的 GitHub 账户。
    # - 如果你想使用真实的电子邮件地址，可以将此处的地址替换为你的实际电子邮件。

# Git LFS (Large File Storage) 过滤器配置
[filter "lfs"]
    clean = git-lfs clean -- %f  # 定义 Git LFS 清理操作
    smudge = git-lfs smudge -- %f  # 定义 Git LFS 涂抹操作
    process = git-lfs filter-process  # 使用 LFS 过滤器处理文件
    required = true  # 设置为必需

# 颜色配置
[color]
    ui = true  # 启用 Git 命令输出中的颜色

# Git 别名
[alias]
    lg = log --color --graph --decorate --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit --all
    # 定义 'lg' 别名，用于显示彩色、图形、装饰和特定格式的完整 Git 日志
    
    mg = merge --no-ff  # 定义 'mg' 别名，用于执行不快进的合并

    slg = log --color --graph --decorate --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit --all -n 10
    # 定义 'slg' 别名，用于显示彩色、图形、装饰和特定格式的最近 10 条 Git 日志

# Push 配置
[push]
    autoSetupRemote = true  # 自动设置推送的远程仓库

# Core 配置
[core]
    autocrlf = true  # 自动处理换行符，根据操作系统进行转换

# 初始化配置
[init]
    defaultBranch = main  # 设置默认的初始化分支名称为 'main'
```



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
1. 1. 运行 `git pull` 命令，将远程仓库的变更拉取到你本地仓库。这样可以合并远程仓库的变更到你的本地分支上。
```bash
git pull origin main
```
如果在拉取时出现冲突，需要解决冲突后再次提交。你可以使用 `git status` 查看冲突文件，并手动解决冲突。
2. 2. 如果不关心远程仓库中的变更，可以使用 `git push -f` 强制推送本地分支到远程仓库。但这可能会覆盖远程仓库中的变更，所以请谨慎使用。
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

