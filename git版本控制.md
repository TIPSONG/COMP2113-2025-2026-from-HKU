# 前排提示：极度不建议看这一章，不如你去b站搜一个github教程视频
# COMP2113 Module 2 Version Control 备考笔记（完整版，基于《Version Control.pdf》）
## 一、文档基础信息与学习目标
### 1.1 文档核心信息
- **课程关联**：COMP2113 Programming Technologies  
- **模块名称**：Module 2: Version Control  
- **预计学习时长**：2 hours  
- **核心工具**：Git（分布式版本控制系统，DVCS）  
- **文档页码范围**：共15页（p.1/15 ~ p.15/15）

### 1.2 学习目标（Learning Objectives）
- **英文原文**：At the end of this chapter, you should be comfortable with: ① Setting up your own Git repository. ② Saving changes to your repository. ③ Inspecting your repository. ④ Traversing your repository. ⑤ Concepts of Branching, Merging, Pushing and Pulling.  
- **中文译文**：学完本章后，你应熟练掌握：① 搭建个人Git仓库；② 向仓库保存更改；③ 检查仓库状态；④ 遍历仓库（切换分支/提交）；⑤ 分支、合并、推送（Push）与拉取（Pull）的概念与操作。


## 二、Git与版本控制基础（Git & VCS Basics）
### 2.1 核心概念定义（Key Definitions）
#### 2.1.1 什么是Git（What is Git?）
- **英文原文**：Git is a modern distributed version control system (DVCS) for managing/tracing file changes and coordinating teamwork. It is primarily used for source-code management, with better performance, security, and flexibility than most other VCSs.  
- **中文译文**：Git是用于管理/追踪文件更改、协调团队协作的现代**分布式版本控制系统（DVCS）**，主要用于软件开发中的源代码管理，相比多数其他版本控制系统，具有更优的性能、安全性和灵活性。

#### 2.1.2 什么是版本控制系统（What is a VCS?）
- **英文原文**：Version Control Systems (VCSs) manage changes to source code over time. They track the history of individual changes, let developers revert to earlier versions (to fix bugs without disrupting others), and avoid issues like incompatible code or ignored changes.  
- **中文译文**：版本控制系统（VCS）用于长期管理源代码的更改，追踪每个贡献者的修改历史，允许开发者回滚到早期版本（修复bug且不影响他人工作流），并避免项目独立部分代码不兼容、更改被忽略等问题。

#### 2.1.3 为什么使用Git（Why Use Git?）
- **英文原文**：① Git shows the entire timeline of changes/decisions for a project. ② DVCS enables collaboration anytime while maintaining code integrity (branches let developers safely propose changes). ③ It breaks team communication barriers and keeps focus on high-quality work.  
- **中文译文**：① Git可展示项目更改/决策的完整时间线；② 分布式特性支持随时协作且保证代码完整性（分支功能让开发者安全地提出更改）；③ 打破团队沟通壁垒，让团队专注于高质量工作。

### 2.2 Git基本工作流（Basic Git Workflow）
#### 2.2.1 三大核心区域（Three Core Areas）
| 区域（Area） | 英文定义 | 中文定义 | 关键操作 |
|--------------|----------|----------|----------|
| Working Directory | Where all file changes are made (additions, deletions, modifications). | 进行所有文件更改（添加、删除、修改）的区域。 | 手动编辑文件 |
| Staging Area | Where you "stage" changes from the Working Directory (marks changes to be committed). | 暂存工作目录中更改的区域（标记即将提交的修改）。 | `git add` |
| Repository | Where Git stores all changes as permanent versions of the project. | Git存储所有更改、作为项目永久版本的区域。 | `git commit` |

#### 2.2.2 工作流命令流程（Command Flow）
- **英文原文**：The basic workflow is: `git init` (initialize repo) → Make changes (Working Directory) → `git add` (stage changes) → `git commit` (save to Repository).  
- **中文译文**：基本工作流为：`git init`（初始化仓库）→ 更改文件（工作目录）→ `git add`（暂存更改）→ `git commit`（保存到仓库）。  


## 三、Git入门操作（Getting Started with Git）
### 3.1 Git安装（Installing Git）
#### 3.1.1 检查Git是否已安装
- **英文原文**：Use `git version` to check if Git is installed. If installed, update to the latest version; if not, follow OS-specific instructions.  
- **中文译文**：用`git version`检查Git是否已安装，若已安装则更新到最新版本，未安装则按对应系统步骤安装。  
- 代码示例：
  ```bash
  $ git version
  # 输出示例：git version 2.17.1（表示已安装）
  ```

#### 3.1.2 不同系统的安装命令
| 操作系统（OS） | 英文安装命令 | 中文说明 | 代码示例 |
|----------------|--------------|----------|----------|
| Linux | 1. `sudo apt-get update` (update packages) 2. `sudo apt-get upgrade` (upgrade packages) 3. `sudo apt-get install git` (install Git) | 1. 更新软件包列表 2. 升级软件包 3. 安装Git | 

```bash
$ sudo apt-get update
$ sudo apt-get upgrade
$ sudo apt-get install git
 ```
| Mac | 1. Install Homebrew (if missing): `ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"` 2. Verify Homebrew: `brew doctor` 3. Install Git: `brew install git` | 1. 安装Homebrew（若未安装） 2. 验证Homebrew 3. 安装Git | 

```bash
# 安装Homebrew
$ ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
# 验证
$ brew doctor
# 安装Git
$ brew install git
 ```
| Windows | Visit https://gitforwindows.org/ to download and install Git. | 访问https://gitforwindows.org/下载并安装Git。 | 无命令（图形化安装） |

### 3.2 初始化与克隆仓库（Initialise & Clone Repository）
#### 3.2.1 初始化本地仓库（Initialise a Local Repo）
- **英文原文**：1. Use `cd` to navigate to the project root directory. 2. Run `git init` to initialize a new repo (creates a `.git/` subdirectory for Git tools). 3. Create files (e.g., `work.txt`) in the directory.  
- **中文译文**：1. 用`cd`进入项目根目录；2. 运行`git init`初始化新仓库（创建`.git/`子目录，存储Git工具文件）；3. 在目录中创建文件（如`work.txt`）。  
- 代码示例：
  ```bash
  # 1. 进入项目目录
  $ cd project
  # 2. 初始化仓库
  $ git init
  # 输出：Initialized empty Git repository in /home/research/ra/1801/cklai/project/.git/
  # 3. 创建文件（示例）
  $ echo "Welcome to my Git tutorial." > work.txt
  ```

#### 3.2.2 `git init`常用选项
| 命令（Command） | 英文含义 | 中文含义 |
|-----------------|----------|----------|
| `git init` | Creates a new local repository | 创建新本地仓库 |
| `git init --quiet` / `git init -q` | Only prints critical messages (errors/warnings); silences other output | 仅打印关键信息（错误/警告），隐藏其他输出 |
| `git init --bare` | Creates a bare repository (no working directory, for servers) | 创建裸仓库（无工作目录，用于服务器） |
| `git init --template=<template_dir>` | Uses templates from the specified directory | 从指定目录加载模板 |

#### 3.2.3 克隆远程仓库（Clone a Remote Repo）
- **英文原文**：Use `git clone <remote_URL>` to copy a remote repo to your local machine. If a `.git` directory exists, the repo clones there; otherwise, it clones to the current working directory (pwd).  
- **中文译文**：用`git clone <远程仓库URL>`将远程仓库复制到本地。若目标目录存在`.git`文件夹，仓库会克隆到该目录；否则克隆到当前工作目录（pwd）。  
- 代码示例：
  ```bash
  # 克隆GitHub上的远程仓库
  $ git clone https://github.com/YOUR-USERNAME/YOUR-REPOSITORY
  # 输出示例：
  # Cloning into `Spoon-Knife`...
  # remote: Counting objects: 10, done.
  # remote: Compressing objects: 100% (8/8), done.
  # remote: Total 10 (delta 1), reused 10 (delta 1)
  # Unpacking objects: 100% (10/10), done.
  ```

#### 3.2.4 `git clone`常用选项
| 命令（Command） | 英文含义 | 中文含义 |
|-----------------|----------|----------|
| `git clone --branch <branch_name>` | Clones a specific branch (instead of the default master) | 克隆指定分支（而非默认master分支） |
| `git clone --bare` | Creates a bare copy of the remote repo (no working directory) | 创建远程仓库的裸副本（无工作目录） |
| `git clone --mirror` | Clones all extended references and implies `--bare` | 克隆所有扩展引用，且隐含`--bare`选项 |

### 3.3 检查仓库状态（Inspect Repository）
#### 3.3.1 `git status`：查看工作目录状态
- **英文原文**：`git status` tracks changes in the Working Directory. It shows untracked files (red) or staged changes (green).  
- **中文译文**：`git status`追踪工作目录中的更改，显示未跟踪文件（红色）或已暂存更改（绿色）。  
- 代码示例与输出：
  ```bash
  # 初始化后未跟踪文件的状态
  $ git status
  # 输出：
  On branch master
  No commits yet
  Untracked files:
    (use "git add <file>..." to include in what will be committed)
          work.txt
  nothing added to commit but untracked files present (use "git add" to track)
  ```

### 3.4 暂存更改（Staging Changes）
#### 3.4.1 `git add`：将更改加入暂存区
- **英文原文**：`git add <filename>` adds changes of a specific file to the Staging Area. `git add .` adds all changes in the current directory; `git add -A`/`git add --all` adds all new/updated files in the entire project.  
- **中文译文**：`git add <文件名>`将指定文件的更改加入暂存区；`git add .`将当前目录下所有更改加入暂存区；`git add -A`/`git add --all`将整个项目中所有新增/更新文件加入暂存区。  
- 代码示例与输出：
  ```bash
  # 暂存指定文件
  $ git add work.txt
  # 查看暂存后的状态
  $ git status
  # 输出：
  On branch master
  No commits yet
  Changes to be committed:
    (use "git rm --cached <file>..." to unstage)
          new file:   work.txt  # 绿色表示已暂存
  ```

#### 3.4.2 `git add`常用命令
| 命令（Command） | 英文含义 | 中文含义 |
|-----------------|----------|----------|
| `git add <filename>` | Adds changes of a specific file to Staging Area | 将指定文件的更改加入暂存区 |
| `git add .` | Adds all changes in the current directory | 将当前目录下所有更改加入暂存区 |
| `git add -A` / `git add --all` | Adds all new/updated files in the entire project | 将整个项目中所有新增/更新文件加入暂存区 |

### 3.5 跟踪更改差异（Track Change Differences）
#### 3.5.1 `git diff`：查看工作目录与暂存区差异
- **英文原文**：`git diff <filename>` shows differences between the Working Directory and the Staging Area. Use `--no-pager` to avoid long output being paged (press `Q` to exit pager if enabled).  
- **中文译文**：`git diff <文件名>`查看工作目录与暂存区的差异。若输出过长，Git会使用分页器（`more`/`less`），可加`--no-pager`禁用分页器（若已启用分页器，按`Q`退出）。  
- 代码示例（修改`work.txt`后）：
  ```bash
  # 修改work.txt（添加一行内容）
  $ echo "We start with the Basic Git Workflow." >> work.txt
  # 查看差异
  $ git diff work.txt
  # 输出：
  diff --git a/work.txt b/work.txt
  index 90ac5e6..f0f679f 100644
  --- a/work.txt
  +++ b/work.txt
  @@ -1,2 +1,3 @@
   Welcome to my Git tutorial.
  +We start with the Basic Git Workflow.
   Today we will learn how to get started with Git.
  ```

#### 3.5.2 `git diff`常用选项
| 命令（Command） | 英文含义 | 中文含义 |
|-----------------|----------|----------|
| `git diff <filename>` | Shows differences between Working Directory and Staging Area for a file | 查看指定文件在工作目录与暂存区的差异 |
| `git diff --base <filename>` | Views conflicts against the base file (where two branches diverged) | 查看文件相对于分支分歧点（base file）的冲突 |
| `git diff <source_branch> <target_branch>` | Previews changes before merging two branches | 合并前预览两个分支的差异 |

### 3.6 提交更改（Committing Changes）
#### 3.6.1 `git commit`：将暂存区更改保存到仓库
- **英文原文**：`git commit -m "commit_message"` permanently saves staged changes to the Repository. The `-m` option specifies a meaningful message (describes the changes). Use `git log` to view the commit history.  
- **中文译文**：`git commit -m "提交信息"`将暂存区的更改永久保存到仓库，`-m`选项用于指定有意义的提交信息（描述更改内容）。用`git log`查看提交历史。  
- 代码示例与输出：
  ```bash
  # 提交暂存的更改
  $ git commit -m "Added an introduction and workflow line to work.txt"
  # 输出示例：
  [master (root-commit) 8327731] Added an introduction and workflow line to work.txt
   1 file changed, 3 insertions(+)
   create mode 100644 work.txt

  # 查看提交历史
  $ git log
  # 输出：
  commit 8327731fa6a9108fb6b54d0b38b9b59c7fbf316c (HEAD -> master)
  Author: Chan Tai Man <tmchan@academy11.cs.hku.hk>
  Date:   Mon Jan 14 10:38:43 2019 +0800
          Added an introduction and workflow line to work.txt
  ```

#### 3.6.2 `git log`：查看提交历史
- **英文原文**：`git log` shows all previous commits in chronological order, with details: ① 40-character SHA (unique commit ID); ② Author; ③ Date/time; ④ Commit message.  
- **中文译文**：`git log`按时间顺序显示所有过往提交，包含细节：① 40位SHA哈希（唯一提交ID）；② 提交者；③ 日期/时间；④ 提交信息。


## 四、Git协作操作（Collaborating with Others）
### 4.1 分支（Branching）
#### 4.1.1 分支核心概念
- **英文原文**：A branch is a pointer to the latest commit in a repo. The default branch after `git init` is `master`. `HEAD` is a pointer to the current commit (usually the latest of the current branch). Branches let developers work on different parts of a project simultaneously.  
- **中文译文**：分支是指向仓库中最新提交的指针，`git init`后默认分支为`master`。`HEAD`是指向当前提交的指针（通常是当前分支的最新提交）。分支允许开发者同时在项目的不同部分工作。

#### 4.1.2 分支操作命令
| 命令（Command） | 英文含义 | 中文含义 | 代码示例 |
|-----------------|----------|----------|----------|
| `git branch` | Lists all local branches | 列出所有本地分支 | 
```bash
$ git branch
# 输出：
#   BugFix
# * master  # *表示当前分支
``` 
| `git branch <branchname>` | Creates a new branch | 创建新分支 | 
```bash
$ git branch BugFix  # 创建BugFix分支
``` 
| `git checkout <branchname>` | Switches to the specified branch | 切换到指定分支 | 
```bash
$ git checkout BugFix  # 切换到BugFix分支
# 输出：Switched to branch 'BugFix'
``` 
| `git checkout -b <branchname>` | Creates and switches to a new branch (one-step) | 创建并切换到新分支（一步操作） | 
```bash
$ git checkout -b Feature  # 创建并切换到Feature分支
``` 
| `git branch -m <old> <new>` | Renames a branch | 重命名分支 | 
```bash
$ git branch -m BugFix BugFix_v2  # 重命名为BugFix_v2
``` 
| `git branch -d <branchname>` | Deletes a merged branch (cannot delete current HEAD) | 删除已合并的分支（不能删除当前HEAD分支） | 
```bash
$ git branch -d BugFix_v2  # 删除已合并的分支
``` 
| `git branch -D <branchname>` | Forces deletion of an unmerged branch | 强制删除未合并的分支 | 
```bash
$ git branch -D UnusedBranch  # 强制删除未合并分支
``` 

#### 4.1.3 遍历仓库（切换提交）
- **英文原文**：Use `git checkout <commit_ref>` to move `HEAD` to a previous commit (restores file states of that commit). `<commit_ref>` can be a branch name (e.g., `BugFix^`, where `^` means the previous commit of `BugFix`) or a SHA hash (from `git log`).  
- **中文译文**：用`git checkout <提交引用>`将`HEAD`移动到之前的提交（恢复该提交的文件状态）。`<提交引用>`可以是分支名（如`BugFix^`，`^`表示`BugFix`的上一个提交）或SHA哈希（从`git log`获取）。  
- 代码示例：
  ```bash
  # 切换到BugFix分支的上一个提交
  $ git checkout BugFix^
  # 输出：
  Note: checking out 'BugFix^'.
  You are in 'detached HEAD' state.  # 进入“分离HEAD”状态
  HEAD is now at 8327731 Added an introduction...

  # 切换到指定SHA的提交（从git log获取SHA前缀）
  $ git checkout 8327731
  ```

### 4.2 合并（Merging）
#### 4.2.1 合并核心场景
- **英文原文**：Merging combines changes from one branch to another. For example: ① Developer fixes a bug in `BugFix` branch; ② Merge `master` into `BugFix` (check for conflicts safely); ③ Merge `BugFix` into `master` (apply bug fix to main project).  
- **中文译文**：合并用于将一个分支的更改整合到另一个分支。例如：① 开发者在`BugFix`分支修复bug；② 将`master`合并到`BugFix`（安全检查冲突）；③ 将`BugFix`合并到`master`（将bug修复应用到主项目）。

#### 4.2.2 基础合并操作（示例）
1. **在`master`分支添加文件**  
   ```bash
   $ git checkout master  # 切换到master
   $ echo "This is master's file." > master.txt  # 创建文件
   $ git add master.txt && git commit -m "Added master.txt"  # 暂存+提交
   ```

2. **在`BugFix`分支添加文件**  
   ```bash
   $ git checkout BugFix  # 切换到BugFix
   $ echo "This is BugFix's file." > bugfix.txt  # 创建文件
   $ git add bugfix.txt && git commit -m "Added bugfix.txt"  # 暂存+提交
   ```

3. **将`master`合并到`BugFix`（检查冲突）**  
   ```bash
   $ git checkout BugFix  # 确保在BugFix分支
   $ git merge master -m "Merge master into BugFix"  # 合并并写提交信息
   # 输出：
   Merge made by the 'recursive' strategy.
    master.txt | 1 +
    1 file changed, 1 insertion(+)
    create mode 100644 master.txt
   ```

4. **将`BugFix`合并到`master`（快速合并）**  
   ```bash
   $ git checkout master  # 切换到master
   $ git merge BugFix  # 合并BugFix（无冲突则快速合并）
   # 输出：
   Fast-forward  # 快速合并（无新提交，仅移动master指针）
    bugfix.txt | 1 +
    1 file changed, 1 insertion(+)
    create mode 100644 bugfix.txt
   ```

#### 4.2.3 合并关键概念与命令
| 命令（Command） | 英文含义 | 中文含义 |
|-----------------|----------|----------|
| `git merge <branchname>` | Merges the specified branch into current branch (fast-forward if possible) | 将指定分支合并到当前分支（可能时快速合并） |
| `git merge --ff-only <branchname>` | Merges only if fast-forward is possible (fails if conflicts exist) | 仅允许快速合并（若有冲突则失败） |
| `git merge --no-ff <branchname>` | Forces a new merge commit (even if fast-forward is possible) | 强制创建新的合并提交（即使可快速合并） |
| `git merge --abort` | Stops the merge process if conflicts occur | 若发生冲突，终止合并过程 |
| `git cherry-pick <SHA>` | Merges only the specific commit (by SHA) into current branch | 仅将指定提交（通过SHA）合并到当前分支 |

#### 4.2.4 冲突处理（Conflict Handling）
- **英文原文**：If two branches modify the same part of a file, Git will prompt a conflict. You must resolve conflicts manually (edit the file to keep desired changes), then run `git add <conflicted_file>` and `git commit` to complete the merge.  
- **中文译文**：若两个分支修改了同一文件的同一部分，Git会提示冲突。需手动解决冲突（编辑文件保留需要的更改），然后运行`git add <冲突文件>`和`git commit`完成合并。

### 4.3 远程仓库管理（Working with Remote Repositories）
#### 4.3.1 远程仓库核心概念
- **英文原文**：Remote repositories are project versions hosted on the Internet/network. They enable collaboration—developers push local changes to remotes and pull others' changes to local.  
- **中文译文**：远程仓库是托管在互联网/网络上的项目版本，支持协作——开发者将本地更改推送到远程，同时从远程拉取他人的更改到本地。

#### 4.3.2 远程仓库操作命令
| 命令（Command） | 英文含义 | 中文含义 | 代码示例 |
|-----------------|----------|----------|----------|
| `git remote` | Lists shortnames of all remote repos | 列出所有远程仓库的简称 | 
```bash
$ git remote
# 输出：origin  # 克隆仓库默认远程简称为origin
``` 

| `git remote -v` | Lists remotes with their URLs (fetch/push) | 列出远程仓库及对应URL（拉取/推送） | 
```bash
$ git remote -v
# 输出：
origin  https://github.com/schacon/ticgit (fetch)
origin  https://github.com/schacon/ticgit (push)
``` 

| `git remote add <shortname> <URL>` | Adds a new remote repo with a shortname | 添加新远程仓库并指定简称 | 
```bash
# 添加简称为pb的远程仓库
$ git remote add pb https://github.com/paulboone/ticgit
``` 
| `git remote rm <shortname>` | Removes a remote repo by shortname | 按简称删除远程仓库 | ```bash
$ git remote rm pb  # 删除pb远程仓库
``` 
| `git remote rename <old> <new>` | Renames a remote repo's shortname | 重命名远程仓库的简称 | 
```bash
$ git remote rename pb pb_new  # 重命名为pb_new
``` 
| `git remote show <shortname>` | Shows detailed info about a remote repo | 显示远程仓库的详细信息 | 
```bash
$ git remote show origin  # 查看origin远程的详情
``` 

### 4.4 推送（Pushing）
#### 4.4.1 `git push`：将本地更改推送到远程
- **英文原文**：`git push <remote> <branch>` uploads local branch changes to the specified remote repo. It fails if you lack write access or someone pushed changes before you (you need to pull first).  
- **中文译文**：`git push <远程简称> <分支名>`将本地分支的更改上传到指定远程仓库。若你无写入权限，或他人先于你推送了更改（需先拉取），推送会失败。  
- 代码示例：
  ```bash
  # 将本地master分支推送到origin远程
  $ git push origin master
  ```

#### 4.4.2 `git push`常用选项
| 命令（Command） | 英文含义 | 中文含义 |
|-----------------|----------|----------|
| `git push <remote> <branch>` | Pushes the local branch to the remote | 将本地分支推送到远程 |
| `git push <remote> --force` | Forces push (even if it causes non-fast-forward merge; use carefully) | 强制推送（即使导致非快速合并，谨慎使用） |
| `git push <remote> --all` | Pushes all local branches to the remote | 将所有本地分支推送到远程 |
| `git push <remote> --tags` | Pushes all local tags to the remote | 将所有本地标签推送到远程 |

### 4.5 拉取（Pulling）
#### 4.5.1 `git pull`：从远程拉取并合并更改
- **英文原文**：`git pull <remote>` fetches remote changes and immediately merges them into the local current branch. It combines `git fetch` (downloads remote changes) and `git merge` (integrates into local).  
- **中文译文**：`git pull <远程简称>`从远程获取更改并立即合并到本地当前分支，相当于组合了`git fetch`（下载远程更改）和`git merge`（整合到本地）。  
- 代码示例：
  ```bash
  # 从origin远程拉取当前分支的更改并合并
  $ git pull origin
  ```

#### 4.5.2 `git pull`常用选项
| 命令（Command） | 英文含义 | 中文含义 |
|-----------------|----------|----------|
| `git pull <remote>` | Fetches and merges remote changes to local current branch | 拉取远程更改并合并到本地当前分支 |
| `git pull --no-commit <remote>` | Fetches remote changes but does not create a merge commit | 拉取远程更改但不创建合并提交 |
| `git pull --verbose` | Shows detailed output while pulling | 拉取时显示详细输出 |


## 五、核心考点总结（Key Exam Focus）
1. **Git基础工作流**  
   - 三大区域：Working Directory（编辑）→ Staging Area（`git add`暂存）→ Repository（`git commit -m`提交）；  
   - 初始化与克隆：`git init`（本地）、`git clone <URL>`（远程）；  
   - 状态与历史：`git status`（查看状态）、`git log`（查看提交历史）。

2. **分支与合并**  
   - 分支命令：`git branch <name>`（创建）、`git checkout <name>`（切换）、`git checkout -b <name>`（创建+切换）、`git branch -d`（删除已合并）/`-D`（强制删除）；  
   - 合并命令：`git merge <branch>`（基础合并）、`--ff-only`（仅快速合并）、`--no-ff`（强制新提交）、`--abort`（终止冲突）；  
   - 关键概念：`HEAD`指针、分离HEAD状态、快速合并（fast-forward）。

3. **远程仓库与协作**  
   - 远程操作：`git remote -v`（查看）、`git remote add <name> <URL>`（添加）、`git remote rm <name>`（删除）；  
   - 推拉命令：`git push <remote> <branch>`（推送）、`git pull <remote>`（拉取合并）；  
   - 冲突处理：手动编辑冲突文件→`git add`→`git commit`。

4. **常用命令细节**  
   - `git add`：`.`（当前目录）、`-A`（所有文件）；  
   - `git diff`：查看差异，`--no-pager`禁用分页；  
   - `git commit`：`-m`必须带有意义的信息；  
   - `git cherry-pick <SHA>`：合并单个指定提交。
