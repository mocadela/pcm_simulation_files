# Git 从 0 到 1 安装与上手全指南

# Git 安装完成后，从0到1上手全指南

安装完 Git 后，核心分 **「首次环境配置」→「基础使用流程」→「常用命令速查」→「避坑指南」** 四步，新手直接照着做就能用。

---

## 一、第一步：首次全局配置（只做1次，永久生效）

打开 **Git Bash**（右键任意文件夹 → Git Bash Here），执行以下命令，完成身份和环境配置：

### 1. 配置提交身份（GitHub/GitLab 会显示这个信息）

```Bash

# 配置用户名（建议和 GitHub/GitLab 用户名一致）
git config --global user.name "你的名字"
# 配置邮箱（必须和 GitHub/GitLab 绑定的邮箱完全一致）
git config --global user.email "你的邮箱@xxx.com"
```

### 2. 验证配置是否生效

```Bash

# 查看所有全局配置
git config --global --list
# 单独查看用户名/邮箱
git config user.name
git config user.email
```

### 3. 可选优化配置（解决中文乱码、提升体验）

```Bash

# 解决中文文件名乱码
git config --global core.quotepath false
# 配置 VS Code 为默认编辑器（如果用 VS Code 写代码）
git config --global core.editor "code --wait"
# 开启 Git LFS 大文件支持（AI/大模型项目必备）
git lfs install
```

---

## 二、第二步：核心使用流程（本地+远程，日常开发循环）

### 场景1：从零新建项目（本地 → 远程 GitHub/GitLab）

#### 1. 初始化本地仓库

进入你的项目文件夹（终端用 `cd 文件夹路径` 跳转），执行：

```Bash

git init
```

文件夹会生成隐藏的 `.git` 目录，代表 Git 仓库创建成功。

#### 2. 提交代码到本地仓库（三步循环，日常必用）

```Bash

# 1. 查看文件状态（哪些文件修改/新增/删除）
git status
# 2. 添加文件到暂存区（. 代表所有文件，也可指定单个文件名）
git add .
# 3. 提交到本地仓库（-m 后写提交说明，必须清晰）
git commit -m "feat: 完成项目初始化"
```

#### 3. 关联远程仓库 + 推送代码

1. 先在 GitHub/GitLab 上新建一个空仓库，复制仓库的 HTTPS/SSH 链接

2. 终端执行：

```Bash

# 关联远程仓库（只做1次）
git remote add origin 你的仓库链接
# 首次推送，关联本地 main 分支到远程
git push -u origin main
# 后续提交，直接执行 git push 即可
git push
```

---

### 场景2：克隆已有远程仓库（直接拉取别人的项目）

复制远程仓库的 HTTPS/SSH 链接，终端执行：

```Bash

# HTTPS 方式（新手推荐，不用配密钥）
git clone https://github.com/用户名/仓库名.git
# SSH 方式（免密，需提前配置 SSH 密钥）
git clone git@github.com:用户名/仓库名.git
```

克隆后自动进入项目目录，直接修改代码、提交推送即可。

---

### 场景3：多人协作，同步远程最新代码

```Bash

# 拉取远程 main 分支最新代码，合并到本地
git pull origin main
# 拉取后再提交自己的代码，避免冲突
git add .
git commit -m "fix: 修复xxbug"
git push
```

---

## 三、第三步：常用命令速查表（日常开发直接用）

|命令|作用|常用场景|
|---|---|---|
|`git status`|查看文件修改状态|每次提交前必看，确认修改内容|
|`git add .`|添加所有修改到暂存区|提交代码前的准备|
|`git commit -m "说明"`|提交到本地仓库|完成代码修改后提交|
|`git push`|推送到远程仓库|同步代码到 GitHub/GitLab|
|`git pull`|拉取远程最新代码|协作开发前同步最新代码|
|`git log`|查看提交历史|回溯代码、查看修改记录|
|`git branch`|查看本地分支|管理项目分支|
|`git branch 分支名`|创建新分支|开发新功能时新建分支|
|`git checkout 分支名`|切换分支|切换到不同功能分支|
|`git merge 分支名`|合并分支|把功能分支合并到 main 主分支|
|`git remote -v`|查看远程仓库关联|确认远程仓库地址是否正确|
---






## 四、第四步：新手避坑指南（常见问题解决）

### 1. 第一次 push 报错：fatal: remote origin already exists

说明已经关联过远程仓库，执行以下命令重新关联：

```Bash

git remote remove origin
git remote add origin 新的仓库链接
git push -u origin main
```

### 2. HTTPS 方式每次 push 都要输密码

用 **Git Credential Manager**（安装时默认开启），第一次输入账号/个人访问令牌（PAT）后，会自动记住，后续免密。

注意：GitHub 已不再支持密码登录，需用「个人访问令牌（PAT）」代替密码。

### 3. 中文文件名乱码

执行配置命令解决：

```Bash

git config --global core.quotepath false
```

### 4. 代码冲突（多人协作必遇）

- 先执行 `git pull` 拉取最新代码，Git 会自动标记冲突文件

- 打开冲突文件，手动修改冲突内容（`<<<<<<<` 到 `>>>>>>>` 之间的内容）

- 修改完成后，重新 `git add .` → `git commit` → `git push`

---

## 五、进阶：SSH 密钥配置（HTTPS 免密替代方案）

如果不想用 HTTPS + 令牌，推荐配置 SSH 密钥，全程免密：

1. 终端生成密钥：

```Bash

ssh-keygen -t ed25519 -C "你的邮箱"
```

一路回车，密钥生成在 `C:\Users\用户名.ssh\` 目录下

1. 打开 `id_ed25519.pub`，复制全部内容

2. 登录 GitHub → Settings → SSH and GPG keys → New SSH key，粘贴保存

3. 测试连接：

```Bash

ssh -T git@github.com
```

出现成功提示后，即可用 SSH 链接克隆/推送代码，全程免密。

---

## 六、验证安装是否成功

打开 CMD/Git Bash，执行：

```Bash

git --version
```

出现 `git version 2.52.0` 等版本信息，说明安装和配置完全正常。

---

## 七、下一步学习建议

- 新手先熟练掌握 **`git init`** **/** **`add`** **/** **`commit`** **/** **`push`** **/** **`pull`** 这5个核心命令，满足90%日常开发

- 再学习分支管理（`branch`/`checkout`/`merge`），掌握多人协作流程

- 最后学习进阶操作（变基、回退、标签等）

---

需要我给你一份 **「GitHub 个人访问令牌（PAT）生成步骤」**，帮你一次性搞定 HTTPS 免密登录吗？
> （注：文档部分内容可能由 AI 生成）



# 为什么文件名是绿色——Git 版本控制解析

# 为什么文件名是绿色？

这是 **Git 版本控制** 的状态标记，不是 PyCharm 自己的样式，说明你这个项目已经被 Git 管理了。

---

## 一、绿色代表什么？

✅ **绿色 = 文件已经被 Git 追踪（Tracked），并且是「已修改/已暂存」状态**

- 简单说：这个文件已经被 Git 记录，你修改了它，还没提交到本地仓库

- 对应你执行过 `git add`，把文件加入了暂存区，等待 `git commit`

---

## 二、PyCharm 里 Git 颜色的完整含义（你以后会经常遇到）

|颜色|含义|对应 Git 状态|
|---|---|---|
|🟢 绿色|已暂存（Staged），等待提交|`git add` 之后，`git commit` 之前|
|🔵 蓝色|已追踪（Tracked），已修改（Modified），未暂存|改了文件，还没 `git add`|
|⚪ 白色/黑色|已追踪，未修改（Unmodified），最新状态|刚提交完，文件没改动|
|🔴 红色|未追踪（Untracked），Git 还没记录这个文件|新建的文件，没 `git add`|
|🟡 黄色|合并冲突（Conflict），需要手动解决|多人协作拉代码时出现冲突|
---

## 三、你现在的状态怎么来的？

你这个 `pcm_simulation` 项目，大概率是：

1. 执行了 `git init` 初始化了 Git 仓库

2. 用 `git add .` 把 `config.py`、`main.py` 加入了暂存区

3. 还没执行 `git commit` 提交，所以 PyCharm 标成绿色

---

## 四、怎么把它变回正常白色？

执行一次完整的提交流程，绿色就会消失：

```Bash

# 1. 确认状态（会看到绿色的文件）
git status
# 2. 提交到本地仓库
git commit -m "feat: 完成项目初始化，添加config和main文件"
```

提交完成后，文件名就会变成白色（未修改状态），代表 Git 已经记录了这个版本。

---

## 五、补充：如果不想用 Git 管理这个项目

可以直接删除项目里的 `.git` 隐藏文件夹，Git 标记就会完全消失：

1. 在项目根目录（`D:\project\Python\pcm_simulation`）

2. 显示隐藏文件，找到 `.git` 文件夹，直接删除

3. PyCharm 里的 Git 颜色标记会立刻消失，恢复普通文件样式

---

## 六、总结

- 绿色是 Git 的「已暂存」标记，**不是错误，是正常状态**

- 代表文件已经被 Git 追踪，等待提交

- 提交后自动变回白色，不影响代码运行，只是版本控制的状态提示

需要我给你一份这个项目的**完整 Git 提交流程**，帮你把代码推到 GitHub 吗？
> （注：文档部分内容可能由 AI 生成）







# git 常用命令大全


好的，这里为您整理一份 Git 常用命令大全，按照不同的使用场景进行分类：

## **一、 配置 (Configuration)**

用于设置用户信息、别名、默认编辑器等。

*   `git config --global user.name "[name]"`: 设置全局提交用户名。
*   `git config --global user.email "[email address]"`: 设置全局提交用户邮箱。
*   `git config --list`: 显示当前所有的 Git 配置信息。
*   `git config --global core.editor "[editor]"`: 设置默认编辑器（如 vim）。
*   `git config --global alias.[alias] '[command]'`: 为常用命令设置别名（例如 `git config --global alias.st status` 可以用 `git st` 代替 `git status`）。

## **二、 新建代码库 (Creating Repositories)**

用于初始化或克隆代码库。

*   `git init`: 在当前目录初始化一个新的 Git 仓库（生成 `.git` 文件夹）。
*   `git clone [url]`: 克隆一个远程仓库到本地。
*   `git clone [url] [directory]`: 克隆到指定目录。
*   `git clone -b [branch-name] [url]`: 克隆指定分支。

## **三、 增加/删除文件 (Adding & Removing Files)**

用于将文件添加到暂存区或从工作区移除。

*   `git add [file]`: 将指定文件添加到暂存区。
*   `git add .`: 将当前目录下所有修改过的文件（新增、修改、删除）添加到暂存区。
*   `git add -u`: 将所有已被 Git 跟踪（tracked）的文件的修改和删除操作添加到暂存区，但不包括新文件。
*   `git rm [file]`: 从工作区和暂存区删除文件，并准备提交。
*   `git rm --cached [file]`: 从暂存区移除文件，但保留在工作区（让该文件不再被 Git 追踪）。

## **四、 代码提交 (Committing)**

用于将暂存区的文件提交到本地仓库。

*   `git commit -m "[message]"`: 提交暂存区的文件，并附上提交说明。
*   `git commit --amend`: 修改最近一次的提交（可以修改提交信息或添加遗漏的文件）。

## **五、 查看信息 (Viewing Information)**

用于查看仓库状态、历史记录等。

*   `git status`: 查看工作区和暂存区的状态，哪些文件被修改、添加或删除。
*   `git log`: 查看提交历史（按 q 退出）。
*   `git log --oneline`: 以简洁的一行格式显示提交历史。
*   `git diff`: 查看工作区与暂存区之间的差异。
*   `git diff --staged` 或 `git diff --cached`: 查看暂存区与最近一次提交（HEAD）之间的差异。
*   `git show [commit-id]`: 显示某次提交的具体内容。

## **六、 分支 (Branching & Merging)**

用于管理和操作分支。

*   `git branch`: 列出本地所有分支，当前所在分支前有 `*` 标记。
*   `git branch [branch-name]`: 创建一个新分支。
*   `git checkout [branch-name]`: 切换到指定分支。
*   `git checkout -b [new-branch-name]`: 创建一个新分支并立即切换过去（相当于 `git branch [new-branch-name]` + `git checkout [new-branch-name]`）。
*   `git merge [branch-name]`: 将指定分支合并到当前分支。
*   `git branch -d [branch-name]`: 删除一个本地分支（如果该分支未被完全合并，可能会失败）。
*   `git branch -D [branch-name]`: 强制删除一个本地分支（即使未被完全合并）。

## **七、 标签 (Tagging)**

用于给特定提交打标签（通常用于发布版本）。

*   `git tag`: 列出所有标签。
*   `git tag [tag-name]`: 为最新的提交打上标签。
*   `git tag [tag-name] [commit-id]`: 为指定提交打上标签。
*   `git push origin [tag-name]`: 推送单个标签到远程仓库。
*   `git push origin --tags`: 推送所有标签到远程仓库。

## **八、 远程同步 (Remote & Syncing)**

用于与远程仓库进行交互。

*   `git remote -v`: 查看当前配置的远程仓库地址。
*   `git fetch [remote]`: 从远程仓库获取最新数据，但不自动合并到当前分支。
*   `git pull [remote] [branch]`: 从远程仓库获取最新数据并自动合并到当前分支（相当于 `git fetch` + `git merge`）。
*   `git push [remote] [branch]`: 将本地分支的更新推送到远程仓库。
*   `git push [remote] --delete [branch]`: 删除远程仓库上的一个分支。

## **九、 撤销 (Undoing)**

用于撤销各种操作。

*   `git checkout -- [file]`: 撤销工作区对指定文件的修改（丢弃工作区的更改）。
*   `git reset HEAD [file]`: 撤销 `git add` 操作，将文件从暂存区移回工作区。
*   `git reset --soft [commit-id]`: 撤销到指定提交，保留工作区和暂存区的更改。
*   `git reset --mixed [commit-id]` (默认): 撤销到指定提交，保留工作区更改，清空暂存区。
*   `git reset --hard [commit-id]`: 彻底撤销到指定提交，丢弃工作区和暂存区的所有更改。**（危险操作，请谨慎使用）**

这些命令基本涵盖了日常开发中 Git 的主要使用场景。您可以根据需要查找和使用。


# git所有的命令有哪些，一一列举，并详细说明什么功能.



# Git 命令速查手册：涵盖常用 20 条命令及场景

# Git 最全常用命令清单（带详细功能 + 场景）

我按**使用场景**给你分类整理，所有日常开发 99% 都会用到，清晰、好记、能直接背。

## 一、仓库初始化（第一次用）

```Bash

git init
```

**功能**：把当前文件夹变成 Git 仓库，开始跟踪文件变化。

```Bash

git clone 仓库地址
```

**功能**：从 GitHub/GitLab 下载完整项目 + 历史记录。

---

## 二、查看状态（最常用）

```Bash

git status
```

**功能**：查看哪些文件被修改、新增、暂存、可提交。

```Bash

git log
```

**功能**：查看提交历史（谁、何时、改了什么）。

```Bash

git log --oneline
```

**功能**：简洁版日志，只显示提交号 + 说明。

---

## 三、文件操作（增删改）

```Bash

git add 文件名
```

**功能**：把文件加入**暂存区**，准备提交。

```Bash

git add .
```

**功能**：把**所有修改**加入暂存区（最常用）。

```Bash

git restore --staged 文件名
```

**功能**：把文件**移出暂存区**（不删除文件）。

```Bash

git restore 文件名
```

**功能**：放弃文件修改，还原成上一次提交的版本（慎用！）。

```Bash

git rm 文件名
```

**功能**：删除文件，并让 Git 记录这次删除。

---

## 四、提交代码（保存版本）

```Bash

git commit -m "提交说明"
```

**功能**：把暂存区的内容**永久保存到本地仓库**。

```Bash

git commit -am "提交说明"
```

**功能**：git add + git commit 一步完成（仅限已跟踪文件）。

---

## 五、分支操作（核心功能）

```Bash

git branch
```

**功能**：查看所有本地分支。

```Bash

git branch -r
```

**功能**：查看远程分支。

```Bash

git branch 分支名
```

**功能**：创建新分支。

```Bash

git checkout 分支名
```

**功能**：切换到某个分支。

```Bash

git checkout -b 分支名
```

**功能**：创建并直接切换到新分支。

```Bash

git merge 分支名
```

**功能**：把别的分支合并到当前分支。

```Bash

git branch -d 分支名
```

**功能**：删除本地分支。

---

## 六、远程仓库（GitHub/GitLab）

```Bash

git remote -v
```

**功能**：查看关联的远程仓库地址。

```Bash

git remote add origin 仓库地址
```

**功能**：本地仓库绑定远程仓库。

```Bash

git push
```

**功能**：把本地提交推送到 GitHub（第一次要加 -u）。

```Bash

git push -u origin 分支名
```

**功能**：第一次推送，并建立分支关联。

```Bash

git pull
```

**功能**：从 GitHub 下载最新代码并合并到本地。

```Bash

git fetch
```

**功能**：只下载最新状态，不自动合并（更安全）。

---

## 七、撤销与回退（非常重要）

```Bash

git commit --amend
```

**功能**：修改上一次的提交信息，或补充漏提交的文件。

```Bash

git reset --soft HEAD^
```

**功能**：撤销上一次提交，但保留代码修改。

```Bash

git reset --hard HEAD^
```

**功能**：彻底回退到上一版（删除所有未提交修改，慎用！）。

---

## 八、标签（发布版本）

```Bash

git tag v1.0
```

**功能**：给当前提交打标签。

```Bash

git push origin v1.0
```

**功能**：把标签推送到远程。

---

## 九、Git stash（临时保存）

```Bash

git stash
```

**功能**：临时储藏当前修改，切分支用。

```Bash

git stash pop
```

**功能**：恢复储藏的修改。

---

## 十、配置 Git

```Bash

git config --global user.name "你的名字"
```

**功能**：设置全局用户名。

```Bash

git config --global user.email "你的邮箱"
```

**功能**：设置全局邮箱。

```Bash

git config --list
```

**功能**：查看所有配置。

---

# 最精简记忆版（日常开发 10 条）

1. `git status` —— 看状态

2. `git add .` —— 暂存所有

3. `git commit -m ""` —— 提交

4. `git branch` —— 看分支

5. `git checkout 分支` —— 切分支

6. `git merge` —— 合并

7. `git push` —— 推送到 GitHub

8. `git pull` —— 拉取更新

9. `git log` —— 看历史

10. `git restore` —— 撤销修改

---

## 你要的话我还能给你：

✅ **Git 全套思维导图（图片版）**

✅ **Git 最常用 20 条速查表（可打印）**

✅ **Git 工作流程（公司标准开发流程）**

你想要哪个？我直接给你整理好！
> （注：文档部分内容可能由 AI 生成）


# Git打标签（tag）完整操作指南

Git标签（tag）主要用于标记版本（如发布版本v1.0、v2.1），方便后续回溯特定版本（比如线上出问题时，快速切换到稳定发布版），核心操作分为「创建标签」「查看标签」「推送标签到远程」「删除标签」，以下是详细步骤，附注意事项：

## 一、核心：创建标签（两种常用类型）

Git标签分为「轻量标签」和「附注标签」，日常开发中「附注标签」更常用（包含更多信息，更规范）。

### 1. 附注标签（推荐，带完整信息）

格式：`git tag -a 标签名 -m "标签说明"`

示例（标记当前分支最新提交为v1.0版本）：

```bash
git tag -a v1.0 -m "第一个稳定发布版本，完成PCM仿真核心功能"
```

说明：

- `-a`：表示创建「附注标签」（annotated），必须加，否则是轻量标签；

- `v1.0`：标签名，通常用「v+版本号」格式（如v1.0.1、v2.0），可自定义，但建议规范；

- `-m "说明"`：标签的描述信息，必填（否则会弹出编辑器让你输入），说明标签的用途（如版本功能、发布时间等）。

### 2. 轻量标签（简单标记，无额外信息）

格式：`git tag 标签名`

示例：

```bash
git tag v1.0-beta
```

说明：仅标记当前提交，不包含作者、时间、描述等信息，适合临时标记，不推荐用于正式发布版本。

### 3. 给历史提交打标签（不是最新提交）

如果想给之前的某个历史提交打标签，需要先通过`git log --oneline`获取该提交的「提交哈希值」（如abc1234），再执行标签命令：

格式：`git tag -a 标签名 提交哈希值 -m "标签说明"`

示例（给哈希值为abc1234的历史提交打标签）：

```bash
git tag -a v0.9 abc1234 -m "测试版本，完成基础仿真功能"
```

## 二、查看标签（确认标签是否创建成功）

### 1. 查看所有标签

```bash
git tag
```

输出示例：v0.9、v1.0（按字母/版本号排序）。

### 2. 查看某个标签的详细信息（仅附注标签有）

```bash
git show 标签名
```

示例（查看v1.0标签）：会显示标签作者、创建时间、标签说明、对应提交的修改内容。

## 三、推送标签到远程仓库（GitHub/GitLab）

注意：创建的标签默认只保存在「本地仓库」，不会自动同步到远程，需要手动推送，分两种场景：

### 1. 推送单个标签

```bash
git push origin 标签名
```

示例（推送v1.0标签到远程）：

```bash
git push origin v1.0
```

### 2. 推送所有本地标签（一次性推送多个）

```bash
git push origin --tags
```

## 四、删除标签（本地/远程）

### 1. 删除本地标签

```bash
git tag -d 标签名
```

示例（删除本地v1.0标签）：

```bash
git tag -d v1.0
```

### 2. 删除远程标签（需先删除本地，再推送删除指令）

```bash
git push origin --delete 标签名
```

示例（删除远程v1.0标签）：

```bash
git push origin --delete v1.0
```

## 五、打标签关键注意事项（必看）

- 标签名建议规范：统一用「v+版本号」（如v1.0.0、v1.1.0），避免中文、特殊符号，方便识别和排序；

- 正式发布版本必须用「附注标签」（带-m说明），方便后续追溯版本用途，轻量标签仅用于临时标记；

- 标签一旦推送远程，尽量不要删除（除非标签错误），避免影响其他开发者；

- 打标签前，建议先执行`git status`确认工作区干净（无未提交修改），避免标签关联错误提交；

- 如果标签打错（如关联了错误提交），可先删除本地标签，再重新创建、推送。

## 六、常用场景示例（完整流程）

假设完成v1.0版本开发，打标签并推送到远程的完整命令：

```bash
# 1. 确认工作区干净
git status
# 2. 创建附注标签，添加说明
git tag -a v1.0 -m "v1.0稳定版：完成PCM仿真核心模块、可视化功能"
# 3. 查看标签是否创建成功
git tag
# 4. 推送标签到远程
git push origin v1.0
```
> （注：文档部分内容可能由 AI 生成）


# Git打标签（tag）完整操作指南

Git标签（tag）主要用于标记版本（如发布版本v1.0、v2.1），方便后续回溯特定版本（比如线上出问题时，快速切换到稳定发布版），核心操作分为「创建标签」「查看标签」「推送标签到远程」「删除标签」，以下是详细步骤，附注意事项：

## 一、核心：创建标签（两种常用类型）

Git标签分为「轻量标签」和「附注标签」，日常开发中「附注标签」更常用（包含更多信息，更规范）。

### 1. 附注标签（推荐，带完整信息）

格式：`git tag -a 标签名 -m "标签说明"`

示例（标记当前分支最新提交为v1.0版本）：

```bash
git tag -a v1.0 -m "第一个稳定发布版本，完成PCM仿真核心功能"
```

说明：

- `-a`：表示创建「附注标签」（annotated），必须加，否则是轻量标签；

- `v1.0`：标签名，通常用「v+版本号」格式（如v1.0.1、v2.0），可自定义，但建议规范；

- `-m "说明"`：标签的描述信息，必填（否则会弹出编辑器让你输入），说明标签的用途（如版本功能、发布时间等）。

### 2. 轻量标签（简单标记，无额外信息）

格式：`git tag 标签名`

示例：

```bash
git tag v1.0-beta
```

说明：仅标记当前提交，不包含作者、时间、描述等信息，适合临时标记，不推荐用于正式发布版本。

### 3. 给历史提交打标签（不是最新提交）

如果想给之前的某个历史提交打标签，需要先通过`git log --oneline`获取该提交的「提交哈希值」（如abc1234），再执行标签命令：

格式：`git tag -a 标签名 提交哈希值 -m "标签说明"`

示例（给哈希值为abc1234的历史提交打标签）：

```bash
git tag -a v0.9 abc1234 -m "测试版本，完成基础仿真功能"
```

## 二、查看标签（确认标签是否创建成功）

### 1. 查看所有标签

```bash
git tag
```

输出示例：v0.9、v1.0（按字母/版本号排序）。

### 2. 查看某个标签的详细信息（仅附注标签有）

```bash
git show 标签名
```

示例（查看v1.0标签）：会显示标签作者、创建时间、标签说明、对应提交的修改内容。

## 三、推送标签到远程仓库（GitHub/GitLab）

注意：创建的标签默认只保存在「本地仓库」，不会自动同步到远程，需要手动推送，分两种场景：

### 1. 推送单个标签

```bash
git push origin 标签名
```

示例（推送v1.0标签到远程）：

```bash
git push origin v1.0
```

### 2. 推送所有本地标签（一次性推送多个）

```bash
git push origin --tags
```

## 四、删除标签（本地/远程）

### 1. 删除本地标签

```bash
git tag -d 标签名
```

示例（删除本地v1.0标签）：

```bash
git tag -d v1.0
```

### 2. 删除远程标签（需先删除本地，再推送删除指令）

```bash
git push origin --delete 标签名
```

示例（删除远程v1.0标签）：

```bash
git push origin --delete v1.0
```

## 五、打标签关键注意事项（必看）

- 标签名建议规范：统一用「v+版本号」（如v1.0.0、v1.1.0），避免中文、特殊符号，方便识别和排序；

- 正式发布版本必须用「附注标签」（带-m说明），方便后续追溯版本用途，轻量标签仅用于临时标记；

- 标签一旦推送远程，尽量不要删除（除非标签错误），避免影响其他开发者；

- 打标签前，建议先执行`git status`确认工作区干净（无未提交修改），避免标签关联错误提交；

- 如果标签打错（如关联了错误提交），可先删除本地标签，再重新创建、推送。

## 六、常用场景示例（完整流程）

假设完成v1.0版本开发，打标签并推送到远程的完整命令：

```bash
# 1. 确认工作区干净
git status
# 2. 创建附注标签，添加说明
git tag -a v1.0 -m "v1.0稳定版：完成PCM仿真核心模块、可视化功能"
# 3. 查看标签是否创建成功
git tag
# 4. 推送标签到远程
git push origin v1.0
```

## 七、拉取某一个历史版本的所有代码（核心操作）

拉取历史版本代码，核心是通过「标签」或「提交哈希值」定位历史版本，有两种常用方式（按需选择），均能获取该版本的完整代码，操作前建议先执行`git status`确认工作区干净，避免本地修改冲突。

### 方式1：通过标签拉取（推荐，最便捷，适合已打标签的版本）

如果历史版本已打标签（如v0.9、v1.0），直接通过标签定位，步骤简单，无需记复杂的哈希值：

1. 查看所有标签，确认要拉取的历史版本标签名（如v0.9）：
        `git tag`

2. 拉取该标签对应的历史版本代码（两种模式，按需选择）：
        

    - 模式1：临时查看（不切换分支，查看后可随时回到当前版本）
    `git checkout 标签名`示例（查看v0.9版本代码）：`git checkout v0.9`，执行后工作区所有文件会切换为v0.9版本的状态，查看完毕后，执行`git checkout 原分支名`（如`git checkout traeOne`）即可回到当前分支。

    - 模式2：创建新分支并拉取（推荐，避免影响当前分支开发）
                `git checkout -b 新分支名 标签名`示例（创建v0.9-history分支，拉取v0.9版本代码）：`git checkout -b v0.9-history v0.9`，执行后会创建新分支，且该分支的代码就是v0.9版本的完整代码，可在新分支中查看、修改，不影响原分支。

### 方式2：通过提交哈希值拉取（适合未打标签的历史版本）

如果历史版本未打标签，需先获取该版本的「提交哈希值」，再拉取代码，步骤如下：

1. 查看提交历史，获取目标历史版本的哈希值（哈希值为7-10位字符，如abc1234、def5678）：
        `git log --oneline`输出示例：`abc1234 (tag: v0.9) 测试版本，完成基础仿真功能`，其中abc1234就是该版本的提交哈希值。

2. 拉取该哈希值对应的历史版本代码（同样两种模式）：
        

    - 模式1：临时查看
                `git checkout 提交哈希值`示例：`git checkout abc1234`，查看完毕后执行`git checkout 原分支名`返回当前分支。

    - 模式2：创建新分支并拉取（推荐）
                `git checkout -b 新分支名 提交哈希值`示例：`git checkout -b history-abc1234 abc1234`，新分支会保留该历史版本的所有代码，可安全操作。

### 拉取历史版本的关键注意事项

- 临时查看历史版本后，务必执行`git checkout 原分支名`回到当前分支，避免在“分离头指针”状态下开发（会导致修改无法正常提交）；

- 如果需要修改历史版本的代码，必须创建新分支（模式2），禁止直接在临时查看状态下修改；

- 拉取前若本地有未提交的修改，会报错，需先执行`git stash`临时保存修改，或`git commit`提交修改，再执行拉取操作；

- 如果历史版本在远程仓库（本地未同步），需先执行`git fetch`拉取远程所有历史记录，再执行上述 checkout 操作。

### 常用示例（完整流程）

示例1：通过标签v0.9创建新分支，拉取该历史版本代码：

```bash
# 1. 确认工作区干净
git status
# 2. 查看所有标签，确认目标标签
git tag
# 3. 创建新分支并拉取v0.9版本代码
git checkout -b v0.9-history v0.9
# 4. 查看当前分支（已切换到新分支，代码为v0.9版本）
git branch
```

示例2：通过哈希值abc1234临时查看历史版本：

```bash
# 1. 查看提交历史，获取哈希值
git log --oneline
# 2. 临时查看该版本代码
git checkout abc1234
# 3. 查看完毕，回到原分支（如traeOne）
git checkout traeOne
```
> （注：文档部分内容可能由 AI 生成）



#  GitHub 上创建新仓库时

## Choose visibility：选 Public
### 原因：免费账号只能创建公开仓库。如果你选 Private，虽然也是免费的，但协作人数有限制。对于个人练习项目或开源项目，Public 是标准选择。
1. Add README：选 Off (不勾选)

    原因：因为你是在本地（Android Studio）已经创建好了项目，本地已经有代码了。如果这里勾选了，GitHub 会生成一个远程的 README 文件，导致你的本地代码和远程代码不一致，一会推送时会报错（需要强制推送或先拉取合并），对新手很不友好。保持为空，直接上传你的本地代码即可。
2. Add .gitignore：选 Android

    原因：非常重要！Android 项目会生成很多临时文件（如 build 文件夹、.idea 配置、.iml 文件等），这些文件不需要上传到 GitHub。选择 Android，GitHub 会自动帮你创建一个规则文件，忽略掉这些垃圾文件，只上传核心代码。
3. Add license：选 None

    原因：这是关于代码开源协议的。如果你只是自己练习，或者还没想好怎么授权给别人用，就选 None。以后想加随时可以加。


- 最关键的一点是：千万不要勾选 "Add README"！
- 错误做法：勾选了 README -> 点击创建 -> 回到本地 Android Studio 推送代码 -> 报错 "rejected"（拒绝推送） -> 新手崩溃。
- 正确做法：保持 README 为空 -> 点击创建 -> 按照网页上提示的命令行指令（git remote add origin ...）在本地关联并推送。





# rejected (fetch first) 是 Git 最常见的冲突场景之一
- 你的本地 main 分支，落后于 GitHub 远程仓库的 main 分支，远程已经有了别人（或你在别的电脑上）提交过的新代码，Git 为了防止覆盖别人的修改，直接拒绝了你的推送。

- 先拉取远程最新代码，合并到本地
```bash
git pull origin main --rebase
```


- 解决可能的冲突（如果有）
如果命令行提示有冲突，打开 IDE 里的冲突文件，手动解决代码冲突，然后执行：
```bash
git add .
git rebase --continue
```

- 再次推送你的代码

```bash
git push -u origin main
```

- 如果你确定远程的代码可以被覆盖（比如远程仓库是你刚建的、没有重要代码），可以用强制推送命令：

```bash
git push -f origin main
```

注意：-f 是强制推送，会直接用你的本地代码覆盖远程仓库的所有内容，多人协作的项目里千万不要用，会把别人的代码冲掉。


# 远程仓库里只有一个 .gitignore 文件，而你本地的仓库和它没有共同的提交历史，所以 Git 拒绝合并。

- 你的 GitHub 仓库 AndroidProjects 里，只有一个初始提交（Initial commit）和 .gitignore 文件。
- 你本地的 BroadcastReceiverDemo 项目，是一个独立的、全新的 Git 仓库，和远程仓库没有任何共同的提交历史。
- 直接 git push 会触发 “没有共同祖先” 的错误，Git 不知道怎么合并这两个完全独立的仓库。

## 方案 1：把项目推到这个已有的仓库里（推荐，你现在的情况）

1. 先把远程仓库的 main 分支拉下来，并把你的本地提交 “嫁接” 上去

```bash
git pull origin main --allow-unrelated-histories
```

--allow-unrelated-histories 是关键，它允许合并两个没有共同历史的仓库。
执行后，Git 会自动创建一个合并提交，把 .gitignore 和你的项目文件合并到一起。

2. 再执行推送：

```bash
git push -u origin main
```

## 方案 2：给 BroadcastReceiverDemo 建一个独立的仓库（更干净）

- 删掉本地的 .git 文件夹（让它变成一个普通文件夹）
- 去 GitHub 新建一个仓库，名字就叫 BroadcastReceiverDemo
- 按提示初始化这个新仓库，再把项目推上去。

# 你现在的 AndroidProjects 仓库，本来就是打算放多个 Android 项目的，所以用 方案 1 最合适。执行完第一步的 git pull 后，你就能在 GitHub 上看到你的项目文件了。

# 如果把github上的视为版本1.0本地的视为1.1，怎么打标签，文件是一个md文档，并非exe文件

## 第一步：提交本地修改 (Commit)

1. 首先，必须把你修改过的 MD 文档提交到本地的 Git 仓库。
```bash
# 1. 暂存修改的文件
git add .

# 2. 提交并写上说明（这一步是生成新版本的基础）
git commit -m "更新文档内容，升级至 v1.1"
```
## 第二步：打上 v1.1 标签 (Tag)
2. 推荐使用“附注标签”（Annotated Tag），因为它包含作者、日期和说明信息，更适合正式版本。
```bash
# 命令格式：git tag -a [标签名] -m "[说明信息]"
git tag -a v1.1 -m "发布文档版本 1.1，更新了部分章节"
```

## 第三步：推送到 GitHub (Push)

3. 默认情况下，git push 不会推送标签。你需要显式地把标签推上去，这样 GitHub 上才会显示 v1.1。
```bash
# 方法 A：只推送刚才打的 v1.1 标签（推荐）
git push origin v1.1

# 方法 B：一次性推送所有本地新增的标签
git push origin --tags
```
| 操作场景 | 命令示例 | 说明 |
| :--- | :--- | :--- |
| 创建标签 | `git tag -a v1.1 -m "说明"` | 给当前最新提交打标签（推荐） |
| 查看标签 | `git tag` | 列出所有本地标签 |
| 查看标签详情 | `git show v1.1` | 查看该标签的说明和对应的代码变动 |
| 推送标签 | `git push origin v1.1` | 将标签上传到 GitHub |
| 删除本地标签 | `git tag -d v1.1` | 如果打错了，可以删除本地标签 |
| 删除远程标签 | `git push origin --delete v1.1` | 如果远程也打错了，可以删除 |<websource>source_group_web_3</websource>

# 既然你的项目是文档（MD）而非代码（exe），打标签还有以下两个实用场景

1. 在 GitHub 上生成“发行版” (Release)
- 当你把 v1.1 标签推送到 GitHub 后，你可以去 GitHub 网页端。
- 进入仓库的 Releases 页面，点击 Draft a new release。
- 在 "Tag" 下拉框中选择你刚才推上去的 v1.1。
- 在这里你可以写下 v1.1 版本的更新日志（比如：修正了错别字、新增了第三章等），这对阅读文档的人非常友好。

2. 查看版本差异

```bash
git diff v1.0 v1.1
```

# 给历史提交打标签
- 你发现之前的某次提交（比如 commit ID 为 abc123）其实才是稳定的 1.0 版本，你想回过头给它补一个标签。

1. 找到提交 ID：

```bash
git log
```
查看历史记录，找到你想标记的那次提交的哈希值（例如 a1b2c3d）。

2. 打标签：

```bash
# 在命令末尾加上提交 ID
git tag -a v1.0 a1b2c3d -m "回溯标记版本 1.0"
```

3. 推送：
```bash
git push origin v1.0
```
---
🛠️ 常用标签管理命令速查

| 操作 | 命令 | 说明 |
| :--- | :--- | :--- |
| 查看标签 | `git tag` | 列出本地所有标签 |
| 查看详情 | `git show v1.0` | 查看该标签对应的代码快照和说明 |
| 删除本地 | `git tag -d v1.0` | 如果打错了，先删除本地标签 |
| 删除远程 | `git push origin --delete v1.0` | 如果远程也打错了，需删除远程标签 |

---
💡 进阶：在 GitHub 上发布 Release

当你把 v1.0 标签推送到 GitHub 后，建议去网页端做一个 Release，这样用户能更直观地看到版本更新：
1. 进入你的 GitHub 仓库页面。


2. 点击右侧的 Releases，然后点击 Draft a new release。


3. 在 Tag 下拉菜单中选择你刚才推上去的 v1.0。


4. 填写发布标题和说明（比如更新日志），点击 Publish release。

这样，你的项目就拥有了一个正式的 v1.0 版本了！

---

# 哈希值就是 commit 后面那一长串字符

具体的完整哈希值是： e4ac917b0be32dd1ee1a6e61759eb442e0d9c34c

通常情况下，你只需要复制前 7 位就足够了，即： e4ac917

```bash
git tag -a v1.0 e4ac917 -m "发布版本 1.0"

git push origin v1.0
```

# 完整的流程

```bash
git add gitUse.md
git commit -m "更新内容，准备发布 1.1"

git push

git tag -a v1.1 -m "发布版本 1.1"
git push origin v1.1
```


# GitHub 网页上创建一个新的发布版本release
## 核心填写步骤
1. Tag version (标签版本)
- 这里已经默认选中了 v1.0，并且下面显示“Existing tag”，说明你本地推送的 v1.0 标签已经被 GitHub 识别到了。
- 不用动，保持 v1.0 即可。
2. Release title (发布标题)
- 这里默认也是 v1.0。
- 建议修改为更友好的名字，比如：**`GitUse 1.0 正式版`** 或者 `Version 1.0 Release`，这样用户看起来更直观。当然，保持 `v1.0` 也没问题。
3. Release notes (发布说明)
- 这是给用户看的更新日志。
- 你可以手动输入，比如：

🎉 **这是 GitUse 项目的第一个稳定版本！**

主要更新内容：
- ✅ **完成了基础功能开发**
- 🐛 **修复了已知 Bug**

或者，点击页面上的 "Generate release notes" 按钮，GitHub 会自动帮你根据 Commit 记录生成一份草稿，你可以再手动修改。
4. Attach binaries (附加文件)
- 如果你的项目有编译好的 .exe 或其他安装包，可以拖拽到这里。如果是纯代码项目，这里留空即可。
5. Set as a pre-release
- 这是一个复选框，勾选后表示这是“预发布版本”（测试版）。 既然你打的是 1.0，通常是正式版，不要勾选。


## 最后一步
填完后，点击绿色的 "Publish release" 按钮，你的 v1.0 版本就正式发布到 GitHub 上了！



# 在github仓库里没有exe文件，那么这些release的exe等文件是怎么打标签，并且release的

你看到的这些 .exe、.msi、.7z 文件，并不是直接存在仓库代码里的，而是在发布 Release 时「附加」上去的产物。

---

## 一、 核心概念：Release 是 GitHub 的独立发布功能

Release 是 GitHub 提供的「版本发布页」，它和代码仓库是分开的：

- 仓库里存的是源代码（.py/.cpp/.java 等）
- Release 里存的是构建产物（.exe/.msi/.zip 等二进制文件）
- 两者可以完全独立，仓库里不需要提前放这些文件


## 二、 两种常见实现方式

### 方式 1：手动发布（简单项目常用）

1. **本地构建**：开发者在自己电脑上，把源代码编译 / 打包成 .exe、.msi、.7z 等文件。
2. **打 Git 标签**：给当前代码版本打一个标签，比如 git tag v8.9.3，再推送到 GitHub git push origin v8.9.3。
3. **创建 Release**：在 GitHub 仓库的 Releases 页面，基于这个 v8.9.3 标签新建 Release。
4. **上传附件**：在 Release 编辑页，把本地生成的 .exe 等文件拖进去上传，发布即可。

这种方式的缺点是每次都要手动打包，不适合频繁更新的项目。


## 方式 2：GitHub Actions 自动化构建（主流方式，你截图里的 Notepad++ 就是这类）
这是现在开源项目最常用的方案，完全自动，不用手动操作：

1. **配置工作流文件**：在仓库根目录新建 .github/workflows/release.yml，定义构建流程。
2. **触发条件**：设置为「推送版本标签时触发」，比如 tags: ["v*.*.*"]，只要你推送 v8.9.3 标签，就会自动运行工作流。
3. **云端构建**：GitHub 的服务器会自动拉取代码，根据配置文件编译、打包成 .exe、.msi、.7z，甚至生成校验和 .sha256 与签名 .sig 文件。
4. **自动发布 Release**：构建完成后，工作流会自动基于标签创建 Release，并把所有产物上传到附件里，就是你截图里看到的 Assets 列表。


# 三、给你一个可以直接用的简易示例（以 Windows exe 为例）

```yaml
# .github/workflows/release.yml
name: Build & Release
on:
  push:
    tags:
      - 'v*'  # 推送v开头的标签就触发

jobs:
  build:
    runs-on: windows-latest  # 用Windows虚拟机来构建exe
    steps:
      - uses: actions/checkout@v4

      # 这里写你的构建命令，比如用pyinstaller打包Python脚本
      - name: Build exe
        run: |
          pip install pyinstaller
          pyinstaller --onefile main.py

      # 上传构建产物
      - name: Upload artifact
        uses: actions/upload-artifact@v4
        with:
          name: exe-file
          path: dist/main.exe

  release:
    needs: build
    runs-on: ubuntu-latest
    steps:
      - name: Download artifact
        uses: actions/download-artifact@v4
        with:
          name: exe-file
          path: ./artifacts

      - name: Create Release
        uses: softprops/action-gh-release@v1
        with:
          files: ./artifacts/main.exe
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

# 四、为什么仓库里看不到这些 exe 文件？

因为它们是：
- 构建过程中在 GitHub 云端临时生成的
- 只存在于 Release 附件里，不会提交到 Git 仓库
- 仓库里只会有源代码和工作流配置文件，不会包含编译后的二进制文件

💡 补充说明：你截图里的 .sig 签名文件、.sha256 校验和文件，也是在构建流程里自动生成的，用来保证用户下载的文件没有被篡改，是很多正规开源项目的标准做法。

# 版本号又是怎么来的呢？

你看到的 v8.9.3、v1.0.0、v2.5.1 这种版本号，不是 GitHub 自动生成的，而是人定 + Git 标签 + 发布规则 共同决定的。


---

# 一、版本号的来源只有 3 个地方

---

## 1. 开发者自己规定的（最核心）
版本号不是自动生成的，**是项目负责人手动定的**。比如：

- 第一次发布 → v1.0.0
- 修复小 bug → v1.0.1
- 加了新功能 → v1.1.0
- 大重构 → v2.0.0

这叫**语义化版本（Semantic Versioning）**，规则是：
```plaintext
主版本号 . 次版本号 . 修订号
  v     1   .    0    .    0
```


- 主版本：不兼容的大更新
- 次版本：新增功能，兼容旧版
- 修订号：只修 bug


## 2. Git 标签（tag）= 版本号

GitHub 的 **Release 版本号 = Git 标签名**，也就是说，版本号是由你在本地打标签时定义的。比如：

```bash
git tag v8.9.3
git push origin v8.9.3
```

GitHub 就会识别这个 **v8.9.3** 作为版本号


## 3. 发布 Release 时填写

即使没有 Git 标签，你也可以在 GitHub 页面手动创建版本号：

`Choose a tag → Create new tag`

输入：`v1.2.3`

GitHub 会自动帮你创建标签 + 版本号。

---

# 二、那项目里的版本号写在哪里？

通常有 3 个位置：

## 1. 代码里写死（最常见）
Python：`__version__ = "1.2.3"`

JavaScript：`version: "1.2.3"`

C/C++：`#define VERSION "1.2.3"`

## 2. 配置文件里
- package.json
- pyproject.toml
- setup.py
- CMakeLists.txt
## 3. Git 标签自动读取（自动化项目）
很多项目不写死版本号，而是：
```plaintext
版本号 = 从 Git 标签读取

```
# 三、自动化项目怎么自动生成版本号？（你截图那种）

像 Notepad++、VSCode、你看到的所有带 exe 的大型项目，都是：

**自动流程：**
1. 开发者打标签 git tag v8.9.3
2. 推送到 GitHub
3. GitHub Actions 自动读取标签名作为版本号
4. 自动编译 exe
5. 自动生成 Release，版本号就是 v8.9.3

# 四、一句话总结
**版本号 = 开发者规定 → 打成 Git 标签 → GitHub Release 显示这个标签作为版本**


版本号不是**自动生成**的，

是**人定规则 + Git 标签**来的。



# 远程仓库有main主分支，test1，test2两个分支，本地有一个main分支，还有test3，test4分支，项目地址为D:/python/pro，当前分支是test4，所有命令根据这个场景写一遍