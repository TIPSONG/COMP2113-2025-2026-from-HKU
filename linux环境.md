# COMP2113 Module 1 Linux Environment 备考笔记（完整版，基于《Linux Environment.pdf》）
## 一、文档基础信息与学习目标
### 1.1 文档核心信息
- **课程关联**：COMP2113 Programming Technologies  
- **模块名称**：Module 1: Linux Environment  
- **预计学习时长**：4 hours  
- **核心环境**：Ubuntu Linux（文档明确本课程使用Ubuntu，Fedora、Debian等可兼容但以Ubuntu为主）  
- **文档页码范围**：共45页（p.1/45 ~ p.45/45）

### 1.2 学习目标（Learning Objectives）
- **英文原文**：At the end of this chapter, you should be able to: ① Start working in the Linux environment. ② Start using the Bash shell. ③ Display and understand the manuals of some simple shell commands.  
- **中文译文**：学完本章后，你应能：① 在Linux环境中开展操作；② 使用Bash Shell；③ 查看并理解简单Shell命令的手册。


## 二、Linux简介与远程访问（Introduction to Linux & Remote Access）
### 2.1 什么是Linux（What is Linux?）
- **英文原文**：Linux is an operating system like Microsoft Windows or Apple macOS (formerly known as OSX). It provides an interface for us to use the hardware components (e.g., CPU, hard disks) and comes with tools for common tasks (text editing, web browsing). There are many Linux distributions (e.g., Ubuntu, Fedora, Debian); they differ mainly in interfaces and tools.  
- **中文译文**：Linux是与Microsoft Windows、Apple macOS（前称OSX）类似的操作系统，它为用户提供操作硬件组件（如CPU、硬盘）的接口，并自带文本编辑、网页浏览等常用任务工具。Linux有多个发行版（如Ubuntu、Fedora、Debian），主要差异体现在界面和工具上。

### 2.2 为什么学习Linux（Why Learn Linux?）
| 英文原文 | 中文译文 |
|----------|----------|
| 1. Linux is open-source software. It can be customized for special hardware (e.g., supercomputers) and has higher security (source code inspectable). | 1. Linux是开源软件，可针对特殊硬件（如超级计算机）定制，且因源代码可审查，安全性更高。 |
| 2. A lot of software development tools (C, C++, Python, debuggers, timing tools) are available on Linux, which eases software development. | 2. Linux上提供大量软件开发工具（C、C++、Python、调试器、计时工具等），大幅简化开发流程。 |
| 3. Linux and most its software are free, enabling low-cost products (e.g., Android is based on Linux). | 3. Linux及多数相关软件可免费使用，可用于开发上网本、手机等低成本产品（如Android基于Linux开发）。 |

### 2.3 Linux远程访问（Remote Access via SSH）
- **英文原文**：Besides installing Linux locally, you can remote access the CS Ubuntu Linux server via SSH. If NOT in CS network, first connect to HKUVPN, then enter `ssh <CS username>@academy11.cs.hku.hk` in Windows Command Prompt or macOS Terminal.  
- **中文译文**：除本地安装Linux外，可通过SSH远程访问CS系的Ubuntu Linux服务器。若不在CS网络，需先连接HKUVPN，再在Windows命令提示符或macOS终端中输入`ssh <CS用户名>@academy11.cs.hku.hk`。  


## 三、Bash Shell基础（Bash Shell Basics）
### 3.1 Shell核心概念
- **英文原文**：A shell is a text-based program that accepts user commands and performs tasks. Common shells: Korn Shell, Bourne Shell, C Shell, Bash Shell. Commands vary by shell (e.g., Korn uses "print" to print strings, Bash uses "echo"). The default shell in this course is Bash Shell.  
- **中文译文**：Shell是基于文本的命令接收与执行程序，常见类型包括Korn Shell、Bourne Shell、C Shell、Bash Shell。不同Shell命令不统一（如Korn用"print"打印字符串，Bash用"echo"），本课程默认使用Bash Shell。

### 3.2 基础Shell命令（含代码示例）
#### 3.2.1 `date`：查看日期时间
- **英文原文**：To check current datetime, type `date`. The `$` sign is the shell prompt (no need to type).  
- **中文译文**：输入`date`查看当前日期时间，`$`为Shell提示符，无需输入。  
- 代码示例：
  ```bash
  $ date
  # 输出示例：Fri Jul 27 15:58:42 HKT 2018
  ```

#### 3.2.2 `ls`：列出文件/目录
- **英文原文**：`ls` lists files/directories in the current directory. `ls -l` uses long format (shows size, owner, modification time); `ls -a` shows hidden files (starting with `.`).  
- **中文译文**：`ls`列出当前目录的文件/目录；`ls -l`以长格式显示（含大小、所有者、修改时间）；`ls -a`显示隐藏文件（以`.`开头）。  
- 代码示例：
  ```bash
  # 基础列出
  $ ls
  # 长格式列出
  $ ls -l
  # 显示隐藏文件
  $ ls -a
  ```

#### 3.2.3 `cd`：切换目录
- **英文原文**：`cd dir` changes to directory "dir"; `cd ..` goes to the parent directory; `cd ~` goes to your home directory; `cd ~username` goes to another user’s home directory.  
- **中文译文**：`cd 目录名`进入指定目录；`cd ..`回到父目录；`cd ~`回到自身家目录；`cd ~用户名`进入其他用户家目录。  
- 代码示例：
  ```bash
  # 进入engg1340目录
  $ cd engg1340
  # 回到父目录
  $ cd ..
  # 回到家目录
  $ cd ~
  ```

#### 3.2.4 `pwd`：查看当前目录路径
- **英文原文**：`pwd` prints the current working directory, helping you confirm your location in the Linux filesystem.  
- **中文译文**：`pwd`打印当前工作目录路径，帮助确认在Linux文件系统中的位置。  
- 代码示例：
  ```bash
  $ pwd
  # 输出示例：/home/research/ra/1801/tmchan
  ```

#### 3.2.5 `man`：查看命令手册
- **英文原文**：`man x` shows the manual page of command "x". Press `↑↓` to scroll, `Q` to quit. From the manual, you can learn command functions (e.g., `man ls` shows "ls - list directory contents").  
- **中文译文**：`man 命令名`查看命令的手册页，按`↑↓`滚动内容，按`Q`退出。从手册中可了解命令功能（如`man ls`显示“ls - 列出目录内容”）。  
- 代码示例：
  ```bash
  # 查看ls命令手册
  $ man ls
  ```


## 四、目录与文件管理（Directory & File Management）
### 4.1 路径类型（Path Types）
#### 4.1.1 绝对路径（Full Path）
- **英文原文**：Full path starts with `/` and includes all directories from the root to the target. E.g., `/home/kit/Desktop/file4` is the full path for file4 under `/home/kit/Desktop`.  
- **中文译文**：绝对路径以`/`开头，包含从根目录到目标文件/目录的所有层级，如`/home/kit/Desktop/file4`是`/home/kit/Desktop`下file4的绝对路径。

#### 4.1.2 相对路径（Relative Path）
- **英文原文**：Relative path is relative to the current directory and does not start with `/`. E.g., if current directory is `/home`, "file3" is the relative path for `/home/file3`; if current directory is `/home/kit`, "Desktop/file4" is the relative path for `/home/kit/Desktop/file4`.  
- **中文译文**：相对路径相对于当前目录，不以`/`开头。例如：当前目录为`/home`时，"file3"是`/home/file3`的相对路径；当前目录为`/home/kit`时，"Desktop/file4"是`/home/kit/Desktop/file4`的相对路径。

### 4.2 目录操作命令
#### 4.2.1 `mkdir`：创建目录
- **英文原文**：`mkdir dir` creates a single directory; `mkdir dir1 dir2` creates multiple directories; `mkdir "dir name"` creates a directory with spaces.  
- **中文译文**：`mkdir 目录名`创建单个目录；`mkdir 目录1 目录2`创建多个目录；`mkdir "带空格的目录名"`创建含空格的目录。  
- 代码示例：
  ```bash
  # 创建单个目录
  $ mkdir lab
  # 创建多个目录
  $ mkdir lab1 lab2
  # 创建含空格的目录
  $ mkdir "lab 1"
  ```

#### 4.2.2 `rmdir`：删除空目录
- **英文原文**：`rmdir dir` deletes an empty directory (only works if the directory has no files/subdirectories).  
- **中文译文**：`rmdir 目录名`删除空目录（仅当目录无文件/子目录时有效）。  
- 代码示例：
  ```bash
  $ rmdir lab
  ```

#### 4.2.3 `rm -rf`：删除非空目录
- **英文原文**：For non-empty directories, use `rm -rf dir` ( `-r` = recursive delete of subdirectories/files; `-f` = no confirmation prompt).  
- **中文译文**：删除非空目录需用`rm -rf 目录名`（`-r`表示递归删除子目录和文件；`-f`表示无确认提示）。  
- 代码示例：
  ```bash
  # 删除含文件的lab1目录
  $ rm -rf lab1
  ```

#### 4.2.4 `mv`：重命名目录
- **英文原文**：`mv dir1 dir2` renames directory "dir1" to "dir2" (the 1st and 2nd arguments are both directories).  
- **中文译文**：`mv 原目录名 新目录名`将目录“原目录名”重命名为“新目录名”（两个参数均为目录）。  
- 代码示例：
  ```bash
  # 将lab重命名为lab_renamed
  $ mv lab lab_renamed
  ```

### 4.3 文件操作命令
#### 4.3.1 `touch`：创建空文件
- **英文原文**：`touch file` creates an empty file named "file".  
- **中文译文**：`touch 文件名`创建名为“文件名”的空文件。  
- 代码示例：
  ```bash
  $ touch file1.txt
  ```

#### 4.3.2 `cat`：查看文件内容
- **英文原文**：`cat file` displays the content of "file".  
- **中文译文**：`cat 文件名`显示文件内容。  
- 代码示例：
  ```bash
  $ cat file1.txt
  # 输出示例：
  # Hello, this is the content of file1.txt
  # Bye Bye!
  ```

#### 4.3.3 `cp`：复制文件/目录
- **英文原文**：`cp file1 file2` copies "file1" to "file2"; `cp -r dir1 dir2` copies "dir1" to "dir2" ( `-r` = recursive copy for subdirectories).  
- **中文译文**：`cp 文件1 文件2`将“文件1”复制为“文件2”；`cp -r 目录1 目录2`将“目录1”复制为“目录2”（`-r`表示递归复制子目录）。  
- 代码示例：
  ```bash
  # 复制文件
  $ cp file1.txt file2.txt
  # 复制目录
  $ cp -r lab1 lab2
  ```

#### 4.3.4 `mv`：移动/重命名文件
- **英文原文**：`mv file dir` moves "file" to directory "dir"; `mv file1 file2` renames "file1" to "file2". Use `mv myfile* lab` to move all files starting with "myfile" to "lab".  
- **中文译文**：`mv 文件 目录`将文件移动到目录；`mv 原文件名 新文件名`将“原文件名”重命名为“新文件名”；用`mv myfile* lab`可将所有以“myfile”开头的文件移动到“lab”目录。  
- 代码示例：
  ```bash
  # 移动文件到mydir目录
  $ mv hello.txt mydir
  # 重命名文件
  $ mv file1.txt newfile.txt
  # 批量移动文件
  $ mv myfile* lab
  ```

#### 4.3.5 `rm`：删除文件
- **英文原文**：`rm file` deletes a file; do not use `rm` for directories (use `rm -rf` for non-empty directories).  
- **中文译文**：`rm 文件名`删除文件；不要用`rm`删除目录（非空目录需用`rm -rf`）。  
- 代码示例：
  ```bash
  $ rm file1.txt
  ```


## 五、vi编辑器使用（Use of vi Editor）
### 5.1 核心模式（Core Modes）
- **英文原文**：vi has two modes: ① Insert mode – edit text (enter via pressing `i`; "INSERT" shows at bottom left); ② Command mode – execute commands (enter via pressing `Esc`; no "INSERT" prompt). vi starts in Command mode.  
- **中文译文**：vi有两种模式：① 插入模式（编辑文本，按`i`进入，左下角显示“INSERT”）；② 命令模式（执行保存、退出等命令，按`Esc`进入，无“INSERT”提示）。vi初始启动时处于命令模式。

### 5.2 完整操作流程
#### 5.2.1 打开/创建文件
- **英文原文**：Type `vi filename` to open a file. If the file does not exist, vi creates a new one.  
- **中文译文**：输入`vi 文件名`打开文件，文件不存在则新建。  
- 代码示例：
  ```bash
  # 打开file2.txt（不存在则创建）
  $ vi file2.txt
  ```

#### 5.2.2 编辑文本（插入模式）
- **英文原文**：Press `i` to switch to Insert mode, then edit text freely.  
- **中文译文**：按`i`切换到插入模式，之后可自由编辑文本。  
- 操作示例：  
  在插入模式下输入：  
  `Hi, my name is kiu.`  
  `Nice to meet you :)`  
  `bye bye`

#### 5.2.3 保存与退出（命令模式）
- **英文原文**：Press `Esc` to enter Command mode, then use commands: ① `:wq` – save and quit; ② `:w` – save without quitting; ③ `:w filename` – save as a new file; ④ `:q` – quit (only if no unsaved changes); ⑤ `:q!` – quit without saving.  
- **中文译文**：按`Esc`进入命令模式，再执行以下命令：① `:wq` – 保存并退出；② `:w` – 保存不退出；③ `:w 新文件名` – 另存为新文件；④ `:q` – 退出（仅无未保存更改时可用）；⑤ `:q!` – 不保存强制退出。  
- 代码示例（命令模式下输入）：
  ```bash
  # 保存并退出
  :wq
  # 另存为new_file.txt
  :w new_file.txt
  # 不保存强制退出
  :q!
  ```

### 5.3 vi速查命令（附录9.1）
- **英文原文**：Useful vi commands (Appendix 9.1): `ZZ` = save & quit; `ZQ` = quit without saving; `dd` = delete a line; `yy` = copy a line; `p` = paste; `h/j/k/l` = move left/down/up/right.  
- **中文译文**：常用vi速查命令（附录9.1）：`ZZ`=保存并退出；`ZQ`=不保存退出；`dd`=删除一行；`yy`=复制一行；`p`=粘贴；`h/j/k/l`=左/下/上/右移动光标。  


## 六、文件传输（Transfer Files to Linux Server）
### 6.1 方法1：用`cat`命令创建并写入内容
- **英文原文**：Use `cat > filename` to create a file and paste text. Press `Ctrl + D` to save, `Ctrl + C` to exit without saving.  
- **中文译文**：输入`cat > 文件名`创建文件并粘贴内容，按`Ctrl + D`保存，按`Ctrl + C`取消操作不保存。  
- 代码示例：
  ```bash
  # 创建hello.txt并输入内容
  $ cat > hello.txt
  Hi! I am using cat to make this file.
  I know Bash shell now!
  # 按Ctrl + D保存退出
  # 查看文件
  $ cat hello.txt
  ```

### 6.2 方法2：用vi编辑器创建文件
- **英文原文**：Refer to the "Use of vi editor" section: `vi filename` creates a file, edit in Insert mode, save with `:wq`.  
- **中文译文**：参考“vi编辑器使用”章节：`vi 文件名`创建文件，在插入模式编辑，用`:wq`保存。  
- 代码示例：
  ```bash
  $ vi new_file.txt
  # 按i进入插入模式编辑，按Esc+`:wq`保存退出
  ```

### 6.3 方法3：SCP/SFTP（推荐，文档重点）
#### 6.3.1 核心要求
- **英文原文**：SFTP does not allow direct access from outside CS network – connect to HKUVPN first. Recommended tools: WinSCP (Windows), FileZilla (macOS).  
- **中文译文**：CS外部网络无法直接访问SFTP服务器，需先连接HKUVPN。推荐工具：Windows用WinSCP，macOS用FileZilla。

#### 6.3.2 WinSCP配置步骤
- **英文原文**：1. Download WinSCP from https://winscp.net/; 2. Open WinSCP → "New Site": File protocol = SCP/SFTP, Host name = academy11.cs.hku.hk, Username = [your CS account], Password = [your CS password], Port = (empty); 3. (Optional, if not in CS network) "Advanced" → "Tunnel": Check "Connect through SSH tunnel", Host name = gatekeeper.cs.hku.hk, Username/Password = [your CS account/password]; 4. Click "Save" → "Login"; 5. Drag-and-drop files between local computer and server.  
- **中文译文**：1. 从https://winscp.net/下载WinSCP；2. 打开WinSCP→“新建站点”：文件协议=SCP/SFTP，主机名=academy11.cs.hku.hk，用户名=[你的CS账户]，密码=[你的CS密码]，端口=（留空）；3.（可选，非CS网络）“高级”→“隧道”：勾选“通过SSH隧道连接”，隧道主机名=gatekeeper.cs.hku.hk，用户名/密码=[你的CS账户/密码]；4. 点击“保存”→“登录”；5. 拖拽文件实现本地与服务器传输。

#### 6.3.3 FileZilla配置步骤
- **英文原文**：For macOS: 1. Download FileZilla from https://filezilla-project.org/ (uncheck sponsored software); 2. Open FileZilla → "Quickconnect": Host = sftp://academy.cs.hku.hk, Username = [your CS account], Password = [your CS password], Port = (empty); 3. Click "Quickconnect"; 4. Drag-and-drop files.  
- **中文译文**：macOS用户：1. 从https://filezilla-project.org/下载FileZilla（取消赞助软件勾选）；2. 打开FileZilla→“快速连接”：主机=sftp://academy.cs.hku.hk，用户名=[你的CS账户]，密码=[你的CS密码]，端口=（留空）；3. 点击“快速连接”；4. 拖拽文件传输。  


## 七、文件权限（File Permission）
### 7.1 权限核心概念
#### 7.1.1 三类所有者（Three Owner Types）
- **英文原文**：Each file/directory has 3 owners: ① User (the owner of the file); ② Group (users in the same group, same permissions); ③ Other (users who are not the owner or in the group).  
- **中文译文**：每个文件/目录有三类所有者：① User（文件所有者）；② Group（同组用户，权限相同）；③ Other（非所有者且不在同组的用户）。

#### 7.1.2 三种权限（Three Permission Types）
- **英文原文**：Each owner type has 3 permissions: ① Read (`r`) – open file/list directory content; ② Write (`w`) – modify file/add/remove files in directory; ③ Execute (`x`) – run program.  
- **中文译文**：每类所有者有三种权限：① Read（`r`）– 打开文件/列出目录内容；② Write（`w`）– 修改文件/在目录中增删文件；③ Execute（`x`）– 运行程序。

### 7.2 权限标识解读（Permission Indicator）
#### 7.2.1 10位标识结构
- **英文原文**：The permission indicator (e.g., `-rw-------`) is 10 characters: [Type] + [User rwx] + [Group rwx] + [Other rwx]. Type: `-` = normal file, `d` = directory.  
- **中文译文**：权限标识（如`-rw-------`）共10位，结构为：[类型] + [User权限] + [Group权限] + [Other权限]。类型：`-`表示普通文件，`d`表示目录。

#### 7.2.2 `ls -l`输出解读
- 示例输出：
  ```bash
  $ ls -l
  total 10
  -rw------- 1 cklai ra 50 Jul 30 17:07 file1.txt
  drwx------ 2 cklai ra 2 Jul 30 17:00 lab/
  ```
- 字段解读：
  | 字段 | 英文解释 | 中文解释 |
  |------|----------|----------|
  | `total 10` | Total space of files in the directory (KB) | 目录中文件占用总空间（KB） |
  | `-rw-------` | Permission indicator | 权限标识（file1.txt为普通文件，User: rw-，Group: ---, Other: ---） |
  | `1` | Number of hard links | 硬链接数量 |
  | `cklai` | Owner of the file/directory | 文件/目录所有者 |
  | `ra` | Group of the owner | 所有者所属组 |
  | `50` | File size (bytes) | 文件大小（字节） |
  | `Jul 30 17:07` | Last modification time | 最后修改时间 |
  | `file1.txt` | File/directory name | 文件/目录名 |

### 7.3 修改权限：`chmod`命令
#### 7.3.1 命令格式
- **英文原文**：`chmod [who][operator][permissions] filename`; `who` = u(User)/g(Group)/o(Other)/a(All); `operator` = +(add)/-(remove)/=(set); `permissions` = r/w/x.  
- **中文译文**：`chmod [所有者类型][操作符][权限] 文件名`；`who`指u（所有者）、g（同组用户）、o（其他用户）、a（所有用户）；`operator`指+（添加）、-（移除）、=（设置）；`permissions`指r（读）、w（写）、x（执行）。

#### 7.3.2 操作示例
- 代码示例：
  ```bash
  # 1. 给Other添加Write权限
  $ touch file.txt
  $ ls -l file.txt  # 初始：-rw-------
  $ chmod o+w file.txt
  $ ls -l file.txt  # 结果：-rw-----w-

  # 2. 给Other添加Read和Execute权限
  $ chmod o+rx file.txt
  $ ls -l file.txt  # 结果：-rw----rwx（*表示可执行）

  # 3. 给All移除Write权限
  $ chmod a-w file.txt
  $ ls -l file.txt  # 结果：-r--r--r-x
  ```


## 八、其他有用命令（Other Useful Commands）
### 8.1 `grep`：搜索文件内容
- **英文原文**：`grep 'pattern' file` returns lines containing "pattern"; use `-E` for regular expressions. E.g., `grep 'ke' example1.txt` returns lines with "ke" (e.g., "4 Chicken 50", "1 Coke 5.5").  
- **中文译文**：`grep '匹配模式' 文件名`返回含“匹配模式”的行，正则匹配需加`-E`。例如`grep 'ke' example1.txt`返回含“ke”的行（如“4 Chicken 50”、“1 Coke 5.5”）。  
- 代码示例：
  ```bash
  $ grep 'ke' example1.txt
  ```

### 8.2 `wc`：统计文件内容
- **英文原文**：`wc file` counts lines, words, bytes; `-l` = lines only, `-w` = words only. E.g., `wc example1.txt` returns "6 18 71 example1.txt" (6 lines, 18 words, 71 bytes).  
- **中文译文**：`wc 文件名`统计行数、单词数、字节数；`-l`仅统计行数，`-w`仅统计单词数。例如`wc example1.txt`返回“6 18 71 example1.txt”（6行、18词、71字节）。  
- 代码示例：
  ```bash
  $ wc example1.txt
  $ wc -l example1.txt  # 仅行数
  $ wc -w example1.txt  # 仅单词数
  ```

### 8.3 `sort`：排序文件内容
- **英文原文**：`sort file` sorts alphabetically by default; `-n` = numeric sort, `-r` = reverse, `-k N` = sort by Nth field, `-t 'del'` = specify delimiter. E.g., `sort -k3 -n example1.txt` sorts by the 3rd field numerically.  
- **中文译文**：`sort 文件名`默认按字母排序；`-n`按数值排序，`-r`倒序，`-k N`按第N列排序，`-t '分隔符'`指定分隔符。例如`sort -k3 -n example1.txt`按第3列数值排序。  
- 代码示例：
  ```bash
  # 数值排序
  $ sort -n example1.txt
  # 按第3列数值排序
  $ sort -k3 -n example1.txt
  ```

### 8.4 `cut`：提取文件列
- **英文原文**：`cut -d 'del' -f N file` extracts the Nth field (delimiter = 'del', field ID starts at 1). E.g., `cut -d ' ' -f 1,3 example1.txt` extracts the 1st and 3rd columns.  
- **中文译文**：`cut -d '分隔符' -f N 文件名`提取第N列（分隔符为“分隔符”，列号从1开始）。例如`cut -d ' ' -f 1,3 example1.txt`提取第1、3列。  
- 代码示例：
  ```bash
  $ cut -d ' ' -f 1,3 example1.txt
  ```

### 8.5 `uniq`：移除相邻重复行
- **英文原文**：`uniq file` removes adjacent duplicate lines (only adjacent ones). E.g., if `example2.txt` has duplicate "Apple" lines, `uniq example2.txt` keeps one "Apple".  
- **中文译文**：`uniq 文件名`移除相邻重复行（仅相邻行）。例如`example2.txt`中有重复的“Apple”行，`uniq example2.txt`仅保留一行“Apple”。  
- 代码示例：
  ```bash
  # 查看原始文件
  $ cat example2.txt
  Apple
  Apple
  Apple pie
  # 移除相邻重复行
  $ uniq example2.txt
  Apple
  Apple pie
  ```

### 8.6 `diff`：比较文件差异
- **英文原文**：`diff file1 file2` shows how to transform file1 to file2 (uses `a`=add, `c`=change, `d`=delete). E.g., `diff fileA.txt fileB.txt` outputs steps like "1a2 > eee" (add "eee" after line 1 of fileA).  
- **中文译文**：`diff 文件1 文件2`显示将文件1转换为文件2的步骤（`a`=添加、`c`=修改、`d`=删除）。例如`diff fileA.txt fileB.txt`输出“1a2 > eee”（在文件A第1行后添加“eee”）。  
- 代码示例：
  ```bash
  $ diff fileA.txt fileB.txt
  ```

### 8.7 `spell`：检查拼写错误
- **英文原文**：`spell file` displays misspelled words. If `spell` is missing: `su` (root) → `yum install aspell` → `exit` (back to user). E.g., `spell example3.txt` returns "beautiffful", "happpy".  
- **中文译文**：`spell 文件名`显示拼写错误词。若`spell`缺失：`su`切换到root→`yum install aspell`安装→`exit`回到普通用户。例如`spell example3.txt`返回“beautiffful”、“happpy”。  
- 代码示例：
  ```bash
  $ spell example3.txt
  # 若缺失，安装步骤
  $ su
  $ yum install aspell
  $ exit
  ```


## 九、标准I/O、重定向与管道（Standard I/O, Redirection & Pipe）
### 9.1 标准I/O（Standard I/O）
- **英文原文**：Shell uses file descriptors: 0 = Standard Input (stdin, default keyboard), 1 = Standard Output (stdout, default screen), 2 = Standard Error (stderr, default screen).  
- **中文译文**：Shell使用文件描述符：0=标准输入（stdin，默认键盘），1=标准输出（stdout，默认屏幕），2=标准错误（stderr，默认屏幕）。

### 9.2 重定向（Redirection）
| 命令 | 英文原文 | 中文译文 | 代码示例 |
|------|----------|----------|----------|
| `cmd > file` | Redirect stdout to "file" (overwrite) | 将标准输出重定向到文件（覆盖） | ```bash
$ ls -l > files.txt
| `cmd >> file` | Append stdout to "file" (no overwrite) | 将标准输出追加到文件（不覆盖） | ```bash
$ wc data.txt >> files.txt
| `cmd 2> file` | Redirect stderr to "file" | 将标准错误重定向到文件 | ```bash
$ grep 'abc' nofile.txt 2> error.txt
| `cmd < file` | Use "file" as stdin | 以文件作为标准输入 | ```bash
$ ./add < input.txt  # add为C++程序，输入来自input.txt


### 9.3 管道（Pipe: `|`）
- **英文原文**：Pipe redirects stdout of cmd1 to stdin of cmd2 (no intermediate files). E.g., `ls -l | grep "Jan 26"` searches lines with "Jan 26" in `ls -l` output.  
- **中文译文**：管道（`|`）将命令1的标准输出直接作为命令2的标准输入（无需中间文件）。例如`ls -l | grep "Jan 26"`在`ls -l`的输出中搜索含“Jan 26”的行。  
- 代码示例：
  ```bash
  # 1. 搜索ls -l中含"Jan 26"的行
  $ ls -l | grep "Jan 26"

  # 2. 排序后提取列并写入文件
  $ sort -k3 -n data.txt | cut -d' ' -f2,3 > result.txt
  ```


## 十、搜索命令（Searching）
### 10.1 `find`：搜索文件/目录
#### 10.1.1 命令格式
- **英文原文**：`find [path] -name "filename" -type [f/d]`; `path` = `.` (current directory); `-type f` = files, `-type d` = directories.  
- **中文译文**：`find [路径] -name "文件名" -type [f/d]`；`path`为`.`（当前目录）；`-type f`搜索文件，`-type d`搜索目录。

#### 10.1.2 操作示例
- 代码示例：
  ```bash
  # 1. 搜索当前目录及子目录中的hello.txt文件
  $ find . -name "hello.txt" -type f

  # 2. 搜索以"hello."开头的文件
  $ find . -name "hello.*" -type f

  # 3. 搜索目录hello
  $ find . -name "hello" -type d
  ```

### 10.2 `grep`与正则表达式（grep & Regular Expressions）
#### 10.2.1 常用正则符号
| 符号 | 英文原文 | 中文译文 | 代码示例 |
|------|----------|----------|----------|
| `.` | Match any single character | 匹配任意单个字符 | ```bash
$ grep -E '.ell' example.txt  # 匹配Hello、shell等
| `^` | Match start of line | 匹配行首 | ```bash
$ grep -E '^apple' example.txt  # 匹配以apple开头的行
| `$` | Match end of line | 匹配行尾 | ```bash
$ grep -E 'apple$' example.txt  # 匹配以apple结尾的行
| `?` | Match 0 or 1 occurrence of previous character | 匹配前一字符0或1次 | ```bash
$ grep -E '^ap?' example.txt  # 匹配ap、a等
| `+` | Match 1 or more occurrences of previous character | 匹配前一字符1次及以上 | ```bash
$ grep -E '^ap+' example.txt  # 匹配ap、app等
| `*` | Match 0 or more occurrences of previous character | 匹配前一字符0次及以上 | ```bash
$ grep -E '^ap*' example.txt  # 匹配a、ap、app等
| `[]` | Match any character in the set | 匹配集合中的任意字符 | ```bash
$ grep -E '[Aa]pple' example.txt  # 匹配Apple、apple
| `{n,m}` | Match n to m occurrences of previous pattern | 匹配前一模式n到m次 | ```bash
$ grep -E '^[0-9]{1,2}' example.txt  # 匹配1-2位数字开头的行


## 十一、问题解决与参考资源（Troubleshooting & References）
### 11.1 无法跟随笔记时的解决方法
- **英文原文**：If you cannot follow the notes: ① Ask lecturer/TA during online self-learning labs; ② Read reference materials; ③ Post questions on Moodle forums (one per lab); ④ Join online consultations (send email in advance to schedule).  
- **中文译文**：若无法理解笔记：① 在在线自学实验课咨询讲师/助教；② 阅读参考资料；③ 在Moodle对应实验论坛发帖提问；④ 参加在线咨询（需提前发邮件预约）。

### 11.2 参考资源
- **英文原文**：1. vi guide: https://www.cs.colostate.edu/helpdocs/vi.html; 2. Linux tutorials: Linux.org, Command-line bootcamp; 3. Books: *A Practical Guide to Linux Commands, Editors, and Shell Programming* (Mark Sobell); 4. Command references: http://ss64.com/bash/.  
- **中文译文**：1. vi指南：https://www.cs.colostate.edu/helpdocs/vi.html；2. Linux教程：Linux.org、Command-line bootcamp；3. 书籍：《A Practical Guide to Linux Commands, Editors, and Shell Programming》（Mark Sobell）；4. 命令参考：http://ss64.com/bash/。


## 十二、核心考点总结（Key Exam Focus）
1. **Shell基础**：`ls`（`-l`/`-a`）、`cd`（`~`/`..`）、`man`的用法；远程访问SSH命令与HKUVPN必要性。  
2. **目录/文件操作**：`mkdir`（含空格目录）、`rm -rf`（非空目录）、`mv`（移动/重命名）、`cp -r`（目录复制）。  
3. **vi编辑器**：模式切换（`i`/`Esc`）、保存退出命令（`:wq`/`:q!`/`:w filename`）、速查命令（`dd`/`yy`/`p`）。  
4. **文件权限**：10位权限标识解读、`chmod`符号模式（如`o+w`、`a-r`）。  
5. **重定向与管道**：`>`/`>>`/`2>`（重定向）、`|`（管道组合命令）。  
6. **搜索与正则**：`find`（`-name`/`-type`）、`grep`常用正则（`.`/`^`/`$`/`[]`/`{n,m}`）。  
7. **其他命令**：`uniq`（相邻去重）、`diff`（文件对比）、`spell`（拼写检查及安装）。  
8. **文件传输**：`cat > 文件名`、SFTP的WinSCP/FileZilla配置（主机/用户名/端口）。