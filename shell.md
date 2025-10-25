# COMP2113 Module 2 Shell Script 备考笔记（完整版，基于《Shell Script.pdf》）
## 一、文档基础信息与学习目标
### 1.1 文档核心信息
- **课程关联**：COMP2113 Programming Technologies  
- **模块名称**：Module 2: Shell Script  
- **预计学习时长**：2 hours  
- **核心环境**：Bash Shell（文档明确基于Bash编写脚本，路径为`/bin/bash`）  
- **文档页码范围**：共19页（p.1/19 ~ p.19/19）

### 1.2 学习目标（Learning Objectives）
- **英文原文**：At the end of this chapter, you should be able to: ① Understand what shell scripts are. ② Write Bash shell script with a sequence of shell commands.  
- **中文译文**：学完本章后，你应能：① 理解Shell脚本的定义与特性；② 编写包含一系列Shell命令的Bash脚本。


## 二、Shell Script基础（Shell Script Basics）
### 2.1 什么是Shell Script（What is Shell Script）
- **英文原文**：Shell script (.sh) is a computer program designed to be run by the Linux shell. It is an interpreted language (not compiled) – no need to compile into binary before execution; the shell parses and interprets it every time it runs. We save a sequence of shell commands in a .sh file to form a shell script.  
- **中文译文**：Shell脚本（后缀`.sh`）是为Linux Shell设计的程序，属于**解释型语言**（非编译型）——执行前无需编译为二进制文件，每次运行时由Shell解析并解释。将一系列Shell命令保存到`.sh`文件中，即可形成Shell脚本。

### 2.2 关键注意点：换行符问题（EOL Issue）
- **英文原文**：Editing scripts in Windows and importing to Linux may cause execution failure (Windows uses CRLF, Linux uses LF). Create/edit scripts in Linux (e.g., vi in SSH) or set Windows text editor to UNIX format (LF).  
- **中文译文**：在Windows编辑脚本后导入Linux可能导致执行失败（Windows换行符为CRLF，Linux为LF）。建议在Linux中创建/编辑脚本（如SSH中的vi编辑器），或在Windows文本编辑器中设置换行符为UNIX格式（LF）。

### 2.3 脚本创建与运行（Create & Run a Script）
#### 2.3.1 示例1：简单Hello脚本（hello.sh）
1. **创建脚本**  
   - 英文原文：Use `vi hello.sh` to create the script; start with `#!/bin/bash` (indicates the Bash path), then add `echo` commands.  
   - 中文译文：用`vi hello.sh`创建脚本，开头需写`#!/bin/bash`（指定Bash解释器路径），再添加`echo`命令（用于打印字符串或变量值）。  
   - 代码示例：
     ```bash
     $ vi hello.sh
     # 脚本内容
     #!/bin/bash
     echo "Hello world!"
     echo "Welcome to ENGG1340"
     ```

2. **添加执行权限**  
   - 英文原文：Make the script executable by the user with `chmod u+x hello.sh` (u = user, +x = add execute permission).  
   - 中文译文：用`chmod u+x hello.sh`给用户添加执行权限（u=所有者，+x=添加执行权限）。  
   - 代码示例：
     ```bash
     $ chmod u+x hello.sh
     ```

3. **运行脚本**  
   - 英文原文：Run the script with `./hello.sh` (`.` means current directory).  
   - 中文译文：用`./hello.sh`运行脚本（`.`表示当前目录）。  
   - 代码示例与输出：
     ```bash
     $ ./hello.sh
     Hello world!
     Welcome to ENGG1340
     ```

#### 2.3.2 示例2：`echo -n`去除尾随行符
- **英文原文**：Use `echo -n` to avoid trailing newline. For example, `echo -n "Hello world!"` will append the next `echo` output to the same line.  
- 中文译文：`echo -n`可去除输出后的尾随行符，例如`echo -n "Hello world!"`会使下一个`echo`的输出追加到同一行。  
- 代码示例与输出：
  ```bash
  # 脚本ex1_1.sh内容
  #!/bin/bash
  echo -n "Hello world!"
  echo "Welcome to ENGG1340!"
  echo "bye"

  # 运行与输出
  $ ./ex1_1.sh
  Hello world!Welcome to ENGG1340!
  bye
  ```

#### 2.3.3 示例3：编译运行C++程序的脚本（ex1_2.sh）
- **英文原文**：Write a script to compile (g++), run (./add), and display the result of a C++ program; use redirection (`< input.txt`, `> output.txt`).  
- 中文译文：编写脚本编译（g++）、运行（./add）C++程序并显示结果，使用重定向（`< input.txt`读取输入，`> output.txt`写入输出）。  
- 前置文件：
  - `add.cpp`（C++程序，读取两个整数并求和）：
    ```cpp
    #include <iostream>
    int main() {
      int a, b;
      std::cin >> a >> b;
      std::cout << a + b;
    }
    ```
  - `input.txt`（输入数据）：`3 4`  
- 脚本代码与输出：
  ```bash
  # 脚本ex1_2.sh内容
  #!/bin/bash
  # Compile the code
  g++ add.cpp -o add
  # Run the code and display result
  ./add < input.txt > output.txt
  cat output.txt

  # 运行与输出
  $ ./ex1_2.sh
  7
  ```


## 三、变量使用（Using Variables）
### 3.1 变量核心规则
- **英文原文**：Shell scripts have only one variable type: **string**. Variable names are case-sensitive, and can only contain letters (a-z/A-Z), numbers (0-9), or underscores (_). No space allowed before/after `=`.  
- **中文译文**：Shell脚本仅有一种变量类型——**字符串**。变量名区分大小写，且只能包含字母（a-z/A-Z）、数字（0-9）或下划线（_）。变量赋值时`=`前后**不允许有空格**。

### 3.2 定义与访问变量（Defining & Accessing Variables）
- **英文原文**：Define a variable with `var="value"` (no space around `=`); access its value with `$var` (dollar sign required).  
- 中文译文：用`var="值"`定义变量（`=`前后无空格）；用`$var`访问变量值（需加美元符号）。  
- 代码示例与输出：
  ```bash
  # 脚本内容
  #!/bin/bash
  pet="dog"  # 定义变量（无空格）
  echo $pet   # 访问变量（加$）

  # 输出
  $ ./ex2_1.sh
  dog
  ```

### 3.3 读取用户输入（Read User Input）
- **英文原文**：Use the `read` command to read user input and assign to a variable (no `$` when assigning). Use `$var` to retrieve the value.  
- 中文译文：用`read`命令读取用户输入并赋值给变量（赋值时变量名前不加`$`），用`$var`获取值。  
- 代码示例与输出：
  ```bash
  # 脚本ex2_2.sh内容
  #!/bin/bash
  echo "What is your name?"
  read name  # 读取输入赋值给name（无$）
  echo "Hello, $name"  # 获取值（加$）

  # 运行与输出
  $ ./ex2_2.sh
  What is your name?
  Kit
  Hello, Kit
  ```

### 3.4 引号使用（Quoting: Single ' ' & Double " "）
#### 3.4.1 引号类型与差异
- **英文原文**：Three ways to specify strings: ① Unquoted (only for single-word strings); ② Single quote (''): no variable substitution; ③ Double quote (""): supports variable substitution (`$`), escape (`\`), and command substitution (`` ` ``).  
- 中文译文：指定字符串的三种方式：① 无引号（仅适用于单单词字符串）；② 单引号（''）：不支持变量替换；③ 双引号（""）：支持变量替换（`$`）、转义（`\`）和命令替换（`` ` ``）。

#### 3.4.2 示例：单双引号对比
- 代码示例与输出：
  ```bash
  # 脚本ex2_3.sh内容
  #!/bin/bash
  name="Apple"
  echo 'Hello, $name'  # 单引号：无变量替换
  echo "Hello, $name"  # 双引号：变量替换
  echo "\$name = $name"  # 双引号：\转义$

  # 输出
  $ ./ex2_3.sh
  Hello, $name
  Hello, Apple
  $name = Apple
  ```

### 3.5 命令替换（Command Substitution）
- **英文原文**：Use backquotes (`` ` ``) to store the output of a shell command in a variable. For example, `` a=`cat file.txt` `` stores the content of file.txt in `a`.  
- 中文译文：用反引号（`` ` ``）将Shell命令的输出存储到变量中，例如`` a=`cat file.txt` ``将file.txt的内容存入变量`a`。  
- 前置文件：`file.txt`内容为`Apple\nBanana\nCherry`  
- 代码示例与输出：
  ```bash
  # 脚本ex2_4.sh内容
  #!/bin/bash
  # 存储file.txt内容到a
  a="`cat file.txt`"
  echo $a

  # 存储file.txt行数到b（wc -l统计行数，cut提取数字）
  b="`wc -l file.txt | cut -d' ' -f1`"
  echo "There are $b lines in file"

  # 输出
  $ ./ex2_4.sh
  Apple Banana Cherry
  There are 3 lines in file
  ```

### 3.6 字符串操作（Operations on Strings）
#### 3.6.1 字符串长度（Get Length）
- **英文原文**：Use `${#var}` to get the length of the string stored in `var`.  
- 中文译文：用`${#变量名}`获取变量中字符串的长度。  
- 代码示例与输出：
  ```bash
  # 脚本ex2_5_1.sh内容
  #!/bin/bash
  a="Apple"
  echo ${#a}  # 获取长度

  # 输出
  $ ./ex2_5_1.sh
  5
  ```

#### 3.6.2 子串提取（Substring）
- **英文原文**：Use `${var:pos:len}` to get a substring of `var` (starts at position `pos`, length `len`; index starts at 0).  
- 中文译文：用`${变量名:起始位置:长度}`提取子串（起始位置`pos`，长度`len`，索引从0开始）。  
- 代码示例与输出：
  ```bash
  # 脚本ex2_5_2.sh内容
  #!/bin/bash
  a="Apple Pie"  # 索引：A(0),p(1),p(2),l(3),e(4), (5),P(6),i(7),e(8)
  echo ${a:6:3}  # 从6开始，取3个字符：P,i,e → Pie

  # 输出
  $ ./ex2_5_2.sh
  Pie
  ```

#### 3.6.3 字符串替换（Replace Part of String）
- **英文原文**：Use `${var/from/to}` to replace the **first occurrence** of `from` with `to` in `var`.  
- 中文译文：用`${变量名/原字符串/目标字符串}`替换变量中`原字符串`的**第一次出现**为`目标字符串`。  
- 代码示例与输出：
  ```bash
  # 脚本ex2_5_3.sh内容
  #!/bin/bash
  a="Apple Pie"
  from="Pie"
  to="juice"
  echo ${a/$from/$to}  # 替换Pie为juice

  # 输出
  $ ./ex2_5_3.sh
  Apple juice
  ```

#### 3.6.4 数值操作（Treat as Number）
- **英文原文**：Use the `let` command for mathematical operations (supports +, -, *, /, %). Shell variables are strings, but `let` treats them as numbers.  
- 中文译文：用`let`命令执行数值运算（支持+、-、*、/、%）。Shell变量本质是字符串，但`let`会将其视为数值。  
- 代码示例与输出：
  ```bash
  # 脚本ex2_5_4.sh内容
  #!/bin/bash
  a=10
  let "b = $a + 9"  # 10+9=19
  echo $b
  let "c = $a * $a"  # 10*10=100
  echo $c
  let "d = $a % 9"   # 10%9=1
  echo $d

  # 输出
  $ ./ex2_5_4.sh
  19
  100
  1
  ```

### 3.7 命令行参数（Command Line Arguments）
- **英文原文**：Command line arguments are labeled `$0`, `$1`, ..., `$9` ( `$0` = script name). Arguments after `$9` are `${10}`, `${11}`, ... ( `$10` is interpreted as `$1` + "0" without braces). The number of arguments is `$#`.  
- 中文译文：命令行参数用`$0`、`$1`…`$9`表示（`$0`=脚本名），`$9`之后的参数需用`${10}`、`${11}`…（无大括号时`$10`会被解析为`$1`+“0”）。参数总数用`$#`表示。  
- 代码示例与输出：
  ```bash
  # 脚本ex2_6.sh内容
  #!/bin/bash
  echo "Script name: $0"
  echo "1st argument: $1"
  echo "2nd argument: $2"
  echo "3rd argument: $3"
  echo "4th argument: $4"
  echo "Total arguments: $#"

  # 运行（输入4个参数）与输出
  $ ./ex2_6.sh we are the world
  Script name: ./ex2_6.sh
  1st argument: we
  2nd argument: are
  3rd argument: the
  4th argument: world
  Total arguments: 4
  ```


## 四、控制流（Flow of Control）
### 4.1 If-else 语句（If-else and Condition）
#### 4.1.1 基础语法
- **英文原文**：Basic syntax of if-statement:  
  ```bash
  if [ condition ]  # Space between [ ] and condition is REQUIRED
  then
    # action
  elif [ condition2 ]
  then
    # action2
  else
    # action3
  fi
  ```  
  Missing spaces will cause syntax errors. The body is enclosed by `then` and `fi`; `elif` is "else-if".  
- 中文译文：if语句基础语法如上，`[ condition ]`中`[`与`]`前后的**空格必须存在**，缺失会导致语法错误。语句体用`then`和`fi`包裹，`elif`表示“else-if”。

#### 4.1.2 三种条件类型（Condition Types）
##### 1. 字符串比较（String Comparisons）
- **英文原文**：String comparisons (enclose variables in "" to handle spaces):  

| String Comparison | Description (English) | Description (中文) |
|--------------------|------------------------|--------------------|
| `[ "$string" ]`    | True if length of $string is non-zero | 若$string长度非0则为真 |
| `[ "$s1" == "$s2" ]` | True if $s1 equals $s2 | 若$s1等于$s2则为真 |
| `[ "$s1" != "$s2" ]` | True if $s1 differs from $s2 | 若$s1不等于$s2则为真 |
| `[ "$s1" \> "$s2" ]` | True if $s1 is sorted after $s2 | 若$s1排序在$s2之后则为真 |
| `[ "$s1" \< "$s2" ]` | True if $s1 is sorted before $s2 | 若$s1排序在$s2之前则为真 |

##### 2. 文件/目录检查（File/Directory Checking）
- **英文原文**：File/directory checks (convenient to verify existence, type, permissions):  

| File Check | Description (English) | Description (中文) |
|-------------|------------------------|--------------------|
| `[ -e $file ]` | True if $file exists | 若$file存在则为真 |
| `[ -f $file ]` | True if $file is a regular file | 若$file是普通文件则为真 |
| `[ -d $file ]` | True if $file is a directory | 若$file是目录则为真 |
| `[ -s $file ]` | True if $file size > 0 | 若$file大小>0则为真 |
| `[ -r $file ]` | True if $file is readable | 若$file可读则为真 |
| `[ -w $file ]` | True if $file is writable | 若$file可写则为真 |
| `[ -x $file ]` | True if $file is executable | 若$file可执行则为真 |

##### 3. 数值比较（Number Comparison）
- **英文原文**：Number comparisons (shell variables are strings, but these expressions treat them as numbers):  

| Number Comparison | Description (English) | Description (中文) |
|--------------------|------------------------|--------------------|
| `[ $a -eq $b ]`    | True if $a == $b | 若$a等于$b则为真 |
| `[ $a -ne $b ]`    | True if $a != $b | 若$a不等于$b则为真 |
| `[ $a -lt $b ]`    | True if $a < $b | 若$a小于$b则为真 |
| `[ $a -le $b ]`    | True if $a <= $b | 若$a小于等于$b则为真 |
| `[ $a -gt $b ]`    | True if $a > $b | 若$a大于$b则为真 |
| `[ $a -ge $b ]`    | True if $a >= $b | 若$a大于等于$b则为真 |

#### 4.1.3 If-else 示例（Examples）
##### 示例1：检查文件存在（Check File Existence）
- **英文原文**：Script to check if `hello.cpp` exists in current directory:  
- 中文译文：检查当前目录中`hello.cpp`是否存在的脚本：  
- 代码示例与输出：
  ```bash
  # 脚本内容
  #!/bin/bash
  if [ -e hello.cpp ]
  then
    echo "hello.cpp exists"
  else
    echo "hello.cpp not found!"
  fi

  # 若hello.cpp不存在，输出：
  $ ./ex3_1_1.sh
  hello.cpp not found!
  ```

##### 示例2：检查编译成功（Check Successful Compilation）
- **英文原文**：If compilation succeeds, the executable is generated. Use `[ -e $executable ]` to verify:  
- 中文译文：若编译成功，会生成可执行文件，用`[ -e 可执行文件名 ]`验证：  
- 代码示例与输出：
  ```bash
  # 脚本内容
  #!/bin/bash
  if [ -e hello.cpp ]
  then
    echo "hello.cpp exists"
    g++ hello.cpp -o hello  # 编译生成hello
    if [ -e hello ]         # 检查可执行文件是否存在
    then
      ./hello  # 运行可执行文件
    fi
  else
    echo "hello.cpp not found!"
  fi

  # 若编译成功且hello.cpp输出"Hello World!"，输出：
  $ ./ex3_1_2.sh
  hello.cpp exists
  Hello World!
  ```

##### 示例3：处理编译错误（Handle Compilation Error）
- **英文原文**：Redirect compilation errors to a file with `2> error.txt`, then display errors if compilation fails:  
- 中文译文：用`2> error.txt`将编译错误重定向到文件，若编译失败则显示错误：  
- 代码示例与输出：
  ```bash
  # 脚本内容
  #!/bin/bash
  if [ -e hello.cpp ]
  then
    echo "hello.cpp exists"
    # 编译错误重定向到error.txt
    g++ hello.cpp -o hello 2> error.txt
    if [ -e hello ]
    then
      ./hello
    else
      echo "Compilation failed!"
      echo "Here are the error messages:"
      cat error.txt  # 显示错误内容
    fi
  else
    echo "hello.cpp not found!"
  fi

  # 若hello.cpp有语法错误，输出：
  $ ./ex3_1_3.sh
  hello.cpp exists
  Compilation failed!
  Here are the error messages:
  hello.cpp: In function ‘int main()’:
  hello.cpp:5:5: error: expected ‘;’ before ‘return’
     return 0;
     ^~~~~~
  ```

### 4.2 For 循环（For-loop）
#### 4.2.1 基础语法
- **英文原文**：For-loop syntax (operates on a list; body enclosed by `do` and `done`):  
  ```bash
  for var in $list  # $list is split by spaces
  do
    # action (use $var for current item)
  done
  ```  
  The list is split by spaces; each item becomes `$var` in each iteration.  
- 中文译文：for循环语法如上，基于列表迭代，语句体用`do`和`done`包裹。列表按空格分割，每次迭代中`$var`表示当前列表项。

#### 4.2.2 示例1：遍历数字列表（Loop Through Numbers）
- 代码示例与输出：
  ```bash
  # 脚本内容
  #!/bin/bash
  list="1 2 3 4 5"  # 空格分割的列表
  for i in $list
  do
    echo "This is iteration $i"
  done

  # 输出
  $ ./ex3_2_1.sh
  This is iteration 1
  This is iteration 2
  This is iteration 3
  This is iteration 4
  This is iteration 5
  ```

#### 4.2.3 示例2：遍历文件（Loop Through Files）
- **英文原文**：Script to backup all .cpp files (use `ls *.cpp` to get the file list via command substitution):  
- 中文译文：备份所有`.cpp`文件的脚本（用`ls *.cpp`通过命令替换获取文件列表）：  
- 代码示例与输出：
  ```bash
  # 脚本backup.sh内容
  #!/bin/bash
  # 命令替换：获取所有.cpp文件列表
  list="`ls *.cpp`"
  for fileName in $list
  do
    # 备份为fileName.backup
    cp $fileName "$fileName.backup"
  done

  # 运行前文件：a.cpp、b.cpp、c.cpp
  # 运行脚本
  $ ./backup.sh
  # 运行后文件：a.cpp、a.cpp.backup、b.cpp、b.cpp.backup、c.cpp、c.cpp.backup
  ```


## 五、实用技巧（Useful Techniques in Shell Script）
### 5.1 隐藏不需要的输出（Hide Unwanted Output）
- **英文原文**：Redirect command output/errors to `/dev/null` (system "dustbin") with `1>/dev/null 2>&1`:  
  - `1>/dev/null`: Redirect stdout (1) to /dev/null.  
  - `2>&1`: Redirect stderr (2) to the same location as stdout.  
- 中文译文：用`1>/dev/null 2>&1`将命令的输出/错误重定向到`/dev/null`（系统“垃圾桶”）：  
  - `1>/dev/null`：将标准输出（1）重定向到`/dev/null`。  
  - `2>&1`：将标准错误（2）重定向到标准输出的位置（即`/dev/null`）。  
- 代码示例与输出：
  ```bash
  # 脚本ex4_1.sh内容
  #!/bin/bash
  # 复制文件，隐藏输出/错误
  cp file123 fileabc 1>/dev/null 2>&1
  if [ -e fileabc ]
  then
    echo "Copy successful"
  else
    echo "$0: Oops. Copy failed :("
  fi

  # 若file123不存在，输出：
  $ ./ex4_1.sh
  ./ex4_1.sh: Oops. Copy failed :(
  ```

### 5.2 输出到标准错误（Output to Standard Error）
- **英文原文**：Use `>&2` at the end of `echo` to output to stderr (useful for error messages):  
- 中文译文：在`echo`命令末尾加`>&2`，将内容输出到标准错误（适用于错误提示）：  
- 代码示例与输出：
  ```bash
  # 脚本ex4_2.sh内容
  #!/bin/bash
  cp file123 fileabc 1>/dev/null 2>&1
  if [ -e fileabc ]
  then
    echo "Copy successful"
  else
    # 输出到标准错误
    echo "$0: Oops. Copy failed :(" >&2
  fi

  # 重定向stderr到error.txt并运行
  $ ./ex4_2.sh 2> error.txt
  # 屏幕无输出，错误写入error.txt
  $ cat error.txt
  ./ex4_2.sh: Oops. Copy failed :(
  ```


## 六、核心考点总结（Key Exam Focus）
1. **脚本基础**  
   - 开头必须写`#!/bin/bash`（指定Bash解释器），执行前需用`chmod u+x 脚本名`添加权限。  
   - 换行符问题：Windows（CRLF）需转为Linux（LF），否则脚本可能无法运行。

2. **变量操作**  
   - 赋值规则：`var="值"`（`=`前后无空格），访问用`$var`；  
   - 单双引号差异：单引号无变量替换，双引号支持`$`/`\`/`` ` ``；  
   - 命令替换：用`` `命令` ``存储命令输出；  
   - 字符串操作：`${#var}`（长度）、`${var:pos:len}`（子串）、`${var/from/to}`（替换）；  
   - 命令行参数：`$0`（脚本名）、`$1-$9`（参数）、`${10+}`（10以上参数）、`$#`（参数总数）。

3. **控制流语法**  
   - if-else：`[ condition ]`前后必须有空格，条件分字符串/文件/数值三类（表格需熟记）；  
   - for循环：`for var in $list`（列表按空格分割），遍历文件用`list="`ls *.cpp`"`。

4. **重定向技巧**  
   - 隐藏输出：`1>/dev/null 2>&1`；  
   - 输出到stderr：`echo "error" >&2`。

5. **实战场景**  
   - 编译运行C++脚本：结合`g++`、重定向（`<`/`>`）、if检查可执行文件；  
   - 文件备份脚本：for循环遍历`*.cpp`，`cp $file $file.backup`。