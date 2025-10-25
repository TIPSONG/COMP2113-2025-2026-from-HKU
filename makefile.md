# COMP2113A Make Tool & Makefile 备考笔记（基于《Makefile.pdf》）
## 一、文档基础信息与背景
### 1.1 文档核心信息
- **课程关联**：COMP2113A Programming Technologies  
- **授课单位**：The University of Hong Kong, School of Computing and Data Science  
- **核心内容**：Make工具的作用、Makefile的语法规则（目标、依赖、命令）、变量（普通变量/特殊变量）、伪目标（如`clean`/`tar`）  
- **适用场景**：多文件C++项目的自动化编译，避免手动编译的重复操作与不必要的 recompilation（重新编译）

### 1.2 背景：多文件C++程序的编译痛点
#### 1.2.1 单文件 vs 多文件编译对比
| 类型 | 编译方式 | 痛点（多文件场景） |
|------|----------|--------------------|
| 单文件（如`gcd_complete.cpp`） | 单条命令编译：<br>`g++ -o gcd_complete gcd_complete.cpp` | - |
| 多文件（如`gcd.cpp`/`gcd_main.cpp`/`gcd.h`） | 手动分三步：<br>1. `g++ -c gcd.cpp`（生成`gcd.o`）<br>2. `g++ -c gcd_main.cpp`（生成`gcd_main.o`）<br>3. `g++ gcd.o gcd_main.o -o gcd_main`（链接生成可执行文件） | 1. 文件多时（数十/数百个），手动输入命令繁琐；<br>2. 仅修改部分文件时，需手动判断哪些文件需重新编译，易遗漏或重复编译（浪费时间） |

#### 1.2.2 多文件依赖的复杂性
- **英文原文**：If `Country.h` is modified, we need to recompile `census.o` and `Country.o`, then link with `BigInteger.o` to update `census` (executable).  
- **中文译文**：若头文件`Country.h`被修改，依赖它的`census.o`（因`census.cpp`包含`Country.h`）和`Country.o`均需重新编译，再与未修改的`BigInteger.o`链接生成新的可执行文件`census`。  
- 核心问题：手动追踪“哪些文件依赖被修改的文件”难度大，需工具自动管理依赖与编译流程——**Make工具**应运而生。


## 二、Make工具核心作用（The Make Tool）
### 2.1 核心功能
- **英文原文**：Linux `make` is a tool that smartly recompiles and links files when some source files are changed. It avoids unnecessary recompilation and regenerates a file **only if needed**.  
- **中文译文**：Linux下的`make`工具可智能处理文件修改后的重新编译与链接，仅在文件“需要更新”时才重新生成（避免不必要的编译，节省时间）。

### 2.2 “需要更新”的判断标准
- **英文原文**：A file is not up to date if: ① it does not exist; ② its last modification time is older than the last modification time of its dependencies.  
- **中文译文**：文件需更新的两种情况：① 文件不存在（如首次编译）；② 文件的最后修改时间 **早于** 其依赖文件的最后修改时间（即依赖文件被修改过）。


## 三、Makefile基础（The Makefile）
### 3.1 关键前提：Makefile文件名与格式
- **英文原文**：The filename is important and needs to start with a capital letter 'M' (i.e., `Makefile`, not `makefile` or `MAKEFILE`).  
- **中文译文**：Makefile的文件名必须以大写字母`M`开头，即**`Makefile`**（小写`makefile`或全大写`MAKEFILE`无效）。

### 3.2 Makefile的核心规则（Rule）
#### 3.2.1 规则结构
- **英文原文**：A rule has two parts: ① Target (file to generate) + Dependency list (files the target depends on); ② Commands to generate the target (each command MUST start with a **tab**, not spaces).  
- **中文译文**：一条Makefile规则包含两部分：① 目标（需生成的文件）+ 依赖列表（目标依赖的文件）；② 生成目标的命令（每条命令必须以**Tab键**开头，不能用空格替代）。

#### 3.2.2 规则示例（以`census`项目为例）
文档中`census`项目包含文件：`census.cpp`（含`main()`）、`BigInteger.cpp`、`Country.cpp`、`BigInteger.h`、`Country.h`，对应的Makefile规则如下：
```makefile
# 注释：以#开头，Make工具会忽略
# 规则1：生成census.o（目标），依赖census.cpp、BigInteger.h、Country.h
census.o: census.cpp BigInteger.h Country.h
	# 命令：生成census.o（-c表示仅编译不链接），注意开头是Tab
	g++ -c census.cpp

# 规则2：生成BigInteger.o，依赖BigInteger.cpp、BigInteger.h
BigInteger.o: BigInteger.cpp BigInteger.h
	g++ -c BigInteger.cpp

# 规则3：生成Country.o，依赖Country.cpp、BigInteger.h、Country.h
Country.o: Country.cpp BigInteger.h Country.h
	g++ -c Country.cpp

# 规则4：生成可执行文件census（目标），依赖3个.o文件
census: census.o BigInteger.o Country.o
	# 命令：链接所有.o文件，生成可执行文件census
	g++ census.o BigInteger.o Country.o -o census
```

#### 3.2.3 关键注意点
1. **Tab的强制性**：命令行必须以Tab开头（文档反复强调，空格会导致Make工具报错“命令未找到”）；  
2. **依赖列表的完整性**：目标依赖的所有文件（.cpp/.h）必须列全，否则修改未列的依赖文件时，Make不会重新编译目标；  
3. **注释格式**：仅支持单行注释，以`#`开头，从`#`到行尾均为注释。


## 四、Makefile变量（Variables）
### 4.1 普通变量：减少重复代码
- **英文原文**：Define variables to avoid retyping filenames. Use `$(variable)` to retrieve the variable value.  
- **中文译文**：定义变量可避免重复输入长文件名或命令，通过`$(变量名)`引用变量值。

#### 4.1.1 示例：`census`项目的变量优化
原规则中多次重复`census`（目标名）和`census.o BigInteger.o Country.o`（.o文件列表），用变量优化后：
```makefile
# 定义变量：TARGET=可执行文件名，OBJECTS=所有.o文件列表
TARGET = census
OBJECTS = census.o BigInteger.o Country.o

# 规则：生成可执行文件$(TARGET)，依赖$(OBJECTS)
$(TARGET): $(OBJECTS)
	# 引用变量：$(OBJECTS)替代所有.o文件，$(TARGET)替代可执行文件名
	g++ $(OBJECTS) -o $(TARGET)

# 其他.o文件规则不变（省略，同3.2.2）
census.o: census.cpp BigInteger.h Country.h
	g++ -c census.cpp
BigInteger.o: BigInteger.cpp BigInteger.h
	g++ -c BigInteger.cpp
Country.o: Country.cpp BigInteger.h Country.h
	g++ -c Country.cpp
```

### 4.2 特殊变量：Make工具自动定义
Make工具会自动生成3个特殊变量，用于简化命令（无需手动定义）：
| 特殊变量 | 英文含义 | 中文含义 | 示例（以`census: census.o BigInteger.o Country.o`为例） |
|----------|----------|----------|--------------------------------------------------------|
| `$@`     | The target of the rule | 当前规则的“目标” | `$@` = `census`（可执行文件） |
| `$^`     | The entire dependency list | 当前规则的“完整依赖列表” | `$^` = `census.o BigInteger.o Country.o` |
| `$<`     | The left-most item in the dependency list | 当前规则的“依赖列表中最左侧的文件” | `$<` = `census.o`（生成`census`时）；<br>`$<` = `census.cpp`（生成`census.o`时） |

#### 4.2.1 特殊变量优化示例
用特殊变量进一步简化`census`项目的Makefile：
```makefile
TARGET = census
OBJECTS = census.o BigInteger.o Country.o

# 规则1：生成可执行文件$@（即TARGET=census），依赖$^（即OBJECTS）
$(TARGET): $(OBJECTS)
	# $^替代所有.o文件，$@替代census，命令简化为：g++ 所有.o -o census
	g++ $^ -o $@

# 规则2：生成census.o，依赖census.cpp、BigInteger.h、Country.h
census.o: census.cpp BigInteger.h Country.h
	# $<替代最左侧依赖census.cpp，命令简化为：g++ -c census.cpp
	g++ -c $<

# 规则3：生成BigInteger.o
BigInteger.o: BigInteger.cpp BigInteger.h
	g++ -c $<

# 规则4：生成Country.o
Country.o: Country.cpp BigInteger.h Country.h
	g++ -c $<
```


## 五、伪目标（Phony Targets）
### 5.1 伪目标的作用
- **英文原文**：Fake targets that never exist. When you run `make <phony-target>`, the commands are executed (serves as shorthand for common operations like cleaning files or archiving).  
- **中文译文**：伪目标是“不存在的文件”，执行`make <伪目标名>`时，会直接运行对应的命令（常用于简化“清理文件”“打包源码”等操作）。

### 5.2 常见伪目标示例：`clean`与`tar`
#### 5.2.1 基础伪目标定义
```makefile
# 省略前面的编译规则（同4.2.1）
TARGET = census
OBJECTS = census.o BigInteger.o Country.o

$(TARGET): $(OBJECTS)
	g++ $^ -o $@

# 伪目标1：clean——删除可执行文件和所有.o文件（清理编译产物）
clean:
	rm $(TARGET) $(OBJECTS)  # 删除census、census.o、BigInteger.o、Country.o

# 伪目标2：tar——打包所有.cpp和.h文件（备份源码）
tar:
	tar -czvf census.tgz *.cpp *.h  # 生成census.tgz，包含所有源码文件
```

#### 5.2.2 伪目标的陷阱与解决：`.PHONY`
- **英文原文**：If a file named `clean`/`tar` exists in the current directory, `make` will think it’s up to date and skip the commands. Solve this by declaring `.PHONY: clean tar` (forces commands to run even if the target “file” exists).  
- **中文译文**：若当前目录存在名为`clean`或`tar`的文件，Make会认为“目标已更新”，跳过命令执行。解决方案：用`.PHONY`声明伪目标，强制执行命令。

#### 5.2.3 优化后的伪目标（含`.PHONY`）
```makefile
# 省略编译规则（同4.2.1）
TARGET = census
OBJECTS = census.o BigInteger.o Country.o

$(TARGET): $(OBJECTS)
	g++ $^ -o $@

# 伪目标：clean（清理）和tar（打包）
clean:
	rm $(TARGET) $(OBJECTS)
tar:
	tar -czvf census.tgz *.cpp *.h

# 关键：声明伪目标，避免文件冲突
.PHONY: clean tar
```

#### 5.2.4 伪目标使用命令
```bash
# 1. 执行clean：删除编译产物
$ make clean
# 2. 执行tar：打包源码
$ make tar
# 3. 执行默认目标（Makefile中第一条规则的目标，此处为census）
$ make  # 等价于make census
```


## 六、Make工具的执行流程（Using Make）
以`make census`（生成可执行文件`census`）为例，流程如下：
1. **检查目标依赖**：Make首先查看`census`的依赖列表（`census.o`/`BigInteger.o`/`Country.o`）；  
2. **递归检查依赖的目标**：  
   - 若`census.o`不存在或依赖文件（`census.cpp`/`BigInteger.h`/`Country.h`）被修改，执行`census.o`的规则（编译生成`census.o`）；  
   - 同理检查`BigInteger.o`和`Country.o`，按需重新编译；  
3. **生成最终目标**：所有依赖的`.o`文件就绪后，执行`census`的规则（链接所有`.o`生成可执行文件）；  
4. **跳过更新文件**：若目标（如`census.o`）已存在且修改时间晚于所有依赖，不执行任何操作。


## 七、核心考点总结（Key Exam Focus）
1. **Makefile基础**  
   - 文件名必须为**`Makefile`**（大写M）；  
   - 规则结构：`目标: 依赖列表` + 缩进的命令（**必须用Tab，不能用空格**）；  
   - 依赖检查逻辑：目标是否存在？目标修改时间是否早于依赖？

2. **变量使用**  
   - 普通变量：`变量名=值`，引用方式`$(变量名)`（减少重复代码）；  
   - 特殊变量：`$@`（目标）、`$^`（完整依赖）、`$<`（最左侧依赖）的含义与替换示例。

3. **伪目标**  
   - 常见伪目标：`clean`（删除编译产物）、`tar`（打包源码）；  
   - 必须用`.PHONY: 伪目标名`声明，避免与目录中现有文件冲突；  
   - 执行命令：`make clean`、`make tar`。

4. **多文件编译流程**  
   - 编译步骤：`g++ -c 源文件.cpp`（生成`.o`）→ `g++ 所有.o -o 可执行文件`（链接）；  
   - Make工具的核心优势：自动追踪依赖，仅重新编译修改过的文件及其依赖。

5. **常见错误点**  
   - 命令行不用Tab：Make报错“*** missing separator. Stop.”；  
   - 依赖列表遗漏头文件（如`census.o`未列`Country.h`）：修改`Country.h`后，`census.o`不会重新编译；  
   - 伪目标未声明`.PHONY`：目录中存在`clean`文件时，`make clean`无效。

# COMP2113 Module 4a Makefile 备考笔记（完整版，基于《makefile_2.pdf》）
## 一、文档基础信息与学习目标
### 1.1 文档核心信息
- **课程关联**：COMP2113 Programming Technologies  
- **模块名称**：Module 4a: Makefile  
- **预计学习时长**：2 hours  
- **核心内容**：分离编译（Separate Compilation）的原理与优势、Make工具的使用、Makefile的规则与进阶技巧（变量、伪目标）  
- **关键限定**：聚焦多文件C++项目的自动化编译，强调课程要求的编译选项（`-pedantic-errors -std=c++11`）

### 1.2 学习目标（Learning Objectives）
- **英文原文**：At the end of this chapter, you should be able to: ① Understand how multiple programs could depend on each other. ② Understand why separate compilation is useful and how to use it. ③ Understand how to use the make tool.  
- **中文译文**：学完本章后，你应能：① 理解多文件程序间的依赖关系；② 理解分离编译的优势与使用方法；③ 掌握Make工具的使用。


## 二、分离编译（Separate Compilation）
### 2.1 背景：从单文件到多文件（Multiple Source Files）
#### 2.1.1 单文件局限性与多文件拆分
- **单文件示例**：`gcd_single.cpp`（含`main()`和`gcd`函数定义），编译命令简单但可维护性差：
  ```bash
  $ g++ gcd_single.cpp -o gcd_single  # 单文件编译
  ```
- **多文件拆分逻辑**（文档示例）：
  1. **头文件`gcd.h`**：声明`gcd`函数（供其他文件引用），含**包含守卫**（避免重复包含）；  
  2. **实现文件`gcd.cpp`**：定义`gcd`函数（函数具体逻辑）；  
  3. **主程序文件`gcd_main.cpp`**：含`main()`，通过`#include "gcd.h"`调用`gcd`函数。  

- 多文件代码示例：
  ```cpp
  // 1. gcd.h（头文件：函数声明+包含守卫）
  #ifndef GCD_H  // 包含守卫：若未定义GCD_H
  #define GCD_H  // 定义GCD_H
  int gcd(int a, int b);  // 函数声明
  #endif  // 结束包含守卫

  // 2. gcd.cpp（实现文件：函数定义）
  #include <iostream>
  #include "gcd.h"  // 引用头文件
  using namespace std;
  int gcd(int a, int b) {  // 函数定义
    while (a != b) {
      if (a > b) a -= b;
      else b -= a;
    }
    return a;
  }

  // 3. gcd_main.cpp（主程序文件：调用函数）
  #include <iostream>
  #include "gcd.h"  // 引用头文件（无需知道函数实现）
  using namespace std;
  int main() {
    int a, b, c;
    cout << "Please input two positive numbers: ";
    cin >> a >> b;
    c = gcd(a, b);  // 调用gcd函数
    cout << "GCD is " << c << endl;
    return 0;
  }
  ```

#### 2.1.2 多文件编译命令（非自动化）
- **英文原文**：To compile the main program, you need to provide both the function definition file and the main program file.  
- **中文译文**：编译多文件程序时，需同时指定所有源文件，或分两步（生成目标文件→链接）：
  ```bash
  # 方法1：直接编译所有源文件（一步完成，适合文件少）
  $ g++ gcd_main.cpp gcd.cpp -o gcd

  # 方法2：分离编译（生成.o目标文件→链接，适合文件多）
  $ g++ -c gcd.cpp  # 生成gcd.o（仅编译不链接）
  $ g++ -c gcd_main.cpp  # 生成gcd_main.o
  $ g++ gcd.o gcd_main.o -o gcd  # 链接所有.o生成可执行文件
  ```

### 2.2 编译流程（Compilation Process）
#### 2.2.1 四阶段详解
- **英文原文**：The compilation process consists of 4 stages: ① Pre-processing (process # directives); ② Compilation (expanded code → assembly code); ③ Assembly (assembly code → object code `.o`); ④ Linker (object codes + libraries → executable).  
- **中文译文**：编译流程分四阶段，多文件场景下前3阶段对每个文件独立执行，最后阶段统一链接：
  1. **预处理（Pre-processing）**：处理`#include`/`#define`等指令，生成扩展后的源码（如`gcd.cpp`→`gcd.i`）；  
  2. **编译（Compilation）**：将扩展源码转换为机器相关的汇编代码（如`gcd.i`→`gcd.s`）；  
  3. **汇编（Assembly）**：将汇编代码转换为机器码，生成目标文件（`.o`，如`gcd.s`→`gcd.o`）；  
  4. **链接（Linking）**：将所有目标文件与系统库链接，生成可执行文件（如`gcd.o`+`gcd_main.o`→`gcd`）。  

- 多文件编译流程图示（文档示例）：  
  `gcd.cpp`→`gcd.i`→`gcd.s`→`gcd.o`  
                          ↘  
  `gcd_main.cpp`→`gcd_main.i`→`gcd_main.s`→`gcd_main.o` → `gcd`（可执行文件）

### 2.3 分离编译的优势（Advantages of Separate Compilation）
- **英文原文**：  
  1. Allows separate writing/compile/testing of source files (eases development).  
  2. Only modified files need recompilation (saves time, e.g., Linux OS full compile takes 8h, modified files take less).  
  3. Hides implementation (provide object codes without source files).  
- **中文译文**：  
  1. **独立开发与测试**：开发者可单独编写、编译和测试单个文件，简化开发流程；  
  2. **节省编译时间**：仅修改过的文件需重新编译，无需全量编译（如Linux系统全量编译需8小时，修改少量文件仅需几分钟）；  
  3. **隐藏实现细节**：可仅提供目标文件（`.o`）而不暴露源码，用户通过链接使用功能（保护知识产权）。


## 三、Make工具与Makefile基础（Using the Make Tool）
### 3.1 Make工具核心作用
- **英文原文**：Linux `make` is a tool that smartly recompiles/links files when source files change. It avoids unnecessary recompilation and regenerates files **only if needed** (file not exists or older than dependencies).  
- **中文译文**：Make工具可智能管理多文件编译，仅在文件“需要更新”时重新生成（避免冗余操作），“需要更新”的判断标准与之前一致：① 文件不存在；② 文件修改时间早于依赖文件。

### 3.2 Makefile关键规则
#### 3.2.1 Makefile文件名与规则结构
- **英文原文**：Makefile filename must be `Makefile` or `makefile` (no extension). A rule has 2 parts: ① Target + Dependency list; ② Commands (each starts with a **tab**, not spaces).  
- **中文译文**：Makefile文件名必须为`Makefile`（大写M）或`makefile`（小写m，无后缀）；一条规则包含两部分：  
  1. **目标（Target）**：需生成的文件（如`.o`或可执行文件），后接冒号`:`；  
  2. **依赖列表（Dependency list）**：目标依赖的文件（如`.cpp`/`.h`）；  
  3. **命令（Commands）**：生成目标的指令（每条必须以**Tab键**开头，空格无效）。

#### 3.2.2 基础Makefile示例（文档gcd项目）
```makefile
# 注释：以#开头，Make工具忽略
# 规则1：生成gcd.o，依赖gcd.cpp和gcd.h
gcd.o: gcd.cpp gcd.h
	# 命令：编译生成gcd.o，加课程要求的编译选项
	g++ -pedantic-errors -std=c++11 -c gcd.cpp

# 规则2：生成gcd_main.o，依赖gcd_main.cpp和gcd.h
gcd_main.o: gcd_main.cpp gcd.h
	g++ -pedantic-errors -std=c++11 -c gcd_main.cpp

# 规则3：生成可执行文件gcd_main，依赖两个.o文件
gcd_main: gcd.o gcd_main.o
	g++ -pedantic-errors -std=c++11 gcd.o gcd_main.o -o gcd_main
```

#### 3.3 Make工具执行流程（以`make gcd_main`为例）
1. **递归检查依赖**：  
   - Make首先查看`gcd_main`的依赖（`gcd.o`/`gcd_main.o`）；  
   - 检查`gcd.o`：若`gcd.o`不存在，或`gcd.cpp`/`gcd.h`修改时间晚于`gcd.o`，执行`gcd.o`的规则（重新编译）；  
   - 同理检查`gcd_main.o`，按需重新编译。  
2. **生成目标**：  
   - 所有`.o`文件就绪后，对比`gcd_main`与依赖的修改时间：若`gcd_main`不存在或更旧，执行链接命令生成可执行文件。  
3. **示例：修改`gcd_main.cpp`后的执行**：  
   ```bash
   $ make gcd_main
   # 仅重新编译gcd_main.o（因gcd_main.cpp被修改）
   g++ -pedantic-errors -std=c++11 -c gcd_main.cpp
   # 重新链接生成gcd_main
   g++ -pedantic-errors -std=c++11 gcd.o gcd_main.o -o gcd_main
   ```


## 四、Makefile进阶技巧（Tricks with Make）
### 4.1 变量（Variables）：减少重复代码
#### 4.1.1 普通变量：自定义常用值
- **英文原文**：Define variables to avoid re-typing filenames/options. Use `$(variable)` to retrieve values.  
- **中文译文**：定义变量存储重复使用的文件名或编译选项，通过`$(变量名)`引用，简化修改（如统一修改编译选项）。  
- 示例（含课程要求的`FLAGS`变量）：
  ```makefile
  # 定义变量：编译选项（课程要求）、目标名、目标文件列表
  FLAGS = -pedantic-errors -std=c++11
  TARGET = gcd_main
  OBJECTS = gcd.o gcd_main.o

  # 规则1：生成gcd.o
  gcd.o: gcd.cpp gcd.h
	g++ $(FLAGS) -c gcd.cpp  # 引用FLAGS变量

  # 规则2：生成gcd_main.o
  gcd_main.o: gcd_main.cpp gcd.h
	g++ $(FLAGS) -c gcd_main.cpp

  # 规则3：生成可执行文件（引用TARGET和OBJECTS变量）
  $(TARGET): $(OBJECTS)
	g++ $(FLAGS) $(OBJECTS) -o $(TARGET)
  ```

#### 4.1.2 特殊变量：Make自动定义
- **英文原文**：Three special variables are auto-defined: ① `$@` = Target; ② `$^` = Full dependency list; ③ `$<` = Left-most dependency.  
- **中文译文**：Make自动生成3个特殊变量，进一步简化命令：
  | 特殊变量 | 英文含义 | 中文含义 | 示例（`gcd_main: gcd.o gcd_main.o`） |
  |----------|----------|----------|------------------------------------|
  | `$@`     | The target | 当前规则的目标 | `$@` = `gcd_main` |
  | `$^`     | The full dependency list | 完整依赖列表 | `$^` = `gcd.o gcd_main.o` |
  | `$<`     | Left-most dependency | 最左侧依赖 | `$<` = `gcd.o`（生成`gcd_main`时）；<br>`$<` = `gcd.cpp`（生成`gcd.o`时） |

- 特殊变量优化后的Makefile：
  ```makefile
  FLAGS = -pedantic-errors -std=c++11
  TARGET = gcd_main
  OBJECTS = gcd.o gcd_main.o

  # 规则1：$<替代gcd.cpp，$@替代gcd.o
  gcd.o: gcd.cpp gcd.h
	g++ $(FLAGS) -c $< -o $@  # 等价于g++ $(FLAGS) -c gcd.cpp -o gcd.o

  # 规则2：$<替代gcd_main.cpp
  gcd_main.o: gcd_main.cpp gcd.h
	g++ $(FLAGS) -c $< -o $@

  # 规则3：$^替代OBJECTS，$@替代TARGET
  $(TARGET): $(OBJECTS)
	g++ $(FLAGS) $^ -o $@  # 等价于g++ $(FLAGS) gcd.o gcd_main.o -o gcd_main
  ```

### 4.2 伪目标（Phony Targets）：简化常用操作
#### 4.2.1 伪目标的作用
- **英文原文**：Phony targets are fake files. Running `make <phony-target>` executes commands (shorthand for operations like cleaning/archiving).  
- **中文译文**：伪目标是“不存在的文件”，用于封装常用操作（如清理编译产物、打包源码），执行`make 伪目标名`即可触发命令。

#### 4.2.2 常见伪目标示例：`clean`与`tar`
```makefile
# 省略前面的编译规则（同4.1.2）
FLAGS = -pedantic-errors -std=c++11
TARGET = gcd_main
OBJECTS = gcd.o gcd_main.o

$(TARGET): $(OBJECTS)
	g++ $(FLAGS) $^ -o $@
gcd.o: gcd.cpp gcd.h
	g++ $(FLAGS) -c $< -o $@
gcd_main.o: gcd_main.cpp gcd.h
	g++ $(FLAGS) -c $< -o $@

# 伪目标1：clean——删除可执行文件、.o文件和压缩包
clean:
	rm -f $(TARGET) $(OBJECTS) gcd.tgz  # -f避免文件不存在时报错

# 伪目标2：tar——打包所有源码文件（.cpp/.h）为gcd.tgz
tar:
	tar -czvf gcd.tgz *.cpp *.h  # -c创建、-z压缩、-v显示过程、-f指定文件名

# 关键：声明伪目标，避免与目录中现有文件冲突
.PHONY: clean tar
```

#### 4.2.3 伪目标的陷阱与解决（`.PHONY`）
- **英文原文**：If a file named `clean`/`tar` exists, `make` skips commands. Solve by `.PHONY: clean tar` (forces commands to run).  
- **中文译文**：若目录中存在名为`clean`或`tar`的文件，Make会认为“目标已更新”，跳过命令。解决方案：用`.PHONY`声明伪目标，强制执行命令。

#### 4.2.4 伪目标使用命令
```bash
# 1. 清理编译产物（删除gcd_main、.o文件、gcd.tgz）
$ make clean

# 2. 打包源码（生成gcd.tgz，包含所有.cpp和.h）
$ make tar

# 3. 生成默认目标（Makefile中第一条规则的目标，此处为gcd_main）
$ make  # 等价于make gcd_main
```


## 五、完整Makefile示例与执行效果
### 5.1 完整Makefile（含所有技巧）
```makefile
# 课程要求的编译选项：严格语法检查+C++11标准
FLAGS = -pedantic-errors -std=c++11
# 目标可执行文件名
TARGET = gcd_main
# 所有目标文件列表
OBJECTS = gcd.o gcd_main.o

# 规则1：生成可执行文件（默认目标，第一条规则）
$(TARGET): $(OBJECTS)
	g++ $(FLAGS) $^ -o $@

# 规则2：生成gcd.o
gcd.o: gcd.cpp gcd.h
	g++ $(FLAGS) -c $< -o $@

# 规则3：生成gcd_main.o
gcd_main.o: gcd_main.cpp gcd.h
	g++ $(FLAGS) -c $< -o $@

# 伪目标：清理编译产物
clean:
	rm -f $(TARGET) $(OBJECTS) gcd.tgz

# 伪目标：打包源码
tar:
	tar -czvf gcd.tgz *.cpp *.h

# 声明伪目标，避免文件冲突
.PHONY: clean tar
```

### 5.2 执行效果（文档Sample Run）
```bash
# 初始文件
$ ls
Makefile gcd.cpp gcd.h gcd_main.cpp

# 1. 生成可执行文件gcd_main
$ make gcd_main
g++ -pedantic-errors -std=c++11 -c gcd.cpp -o gcd.o
g++ -pedantic-errors -std=c++11 -c gcd_main.cpp -o gcd_main.o
g++ -pedantic-errors -std=c++11 gcd.o gcd_main.o -o gcd_main

# 2. 查看生成的文件
$ ls
Makefile gcd_main gcd.cpp gcd.h gcd.o gcd_main.cpp gcd_main.o

# 3. 打包源码
$ make tar
tar -czvf gcd.tgz *.cpp *.h
gcd.cpp
gcd_main.cpp
gcd.h

# 4. 清理编译产物
$ make clean
rm -f gcd_main gcd.o gcd_main.o gcd.tgz

# 5. 清理后剩余文件
$ ls
Makefile gcd.cpp gcd.h gcd_main.cpp
```


## 六、核心考点总结（Key Exam Focus）
1. **分离编译基础**  
   - 编译四阶段：预处理（#指令）→编译（源码→汇编）→汇编（汇编→.o）→链接（.o→可执行文件）；  
   - 分离编译命令：`g++ -c 源文件.cpp`（生成.o）、`g++ 所有.o -o 可执行文件`（链接）；  
   - 优势：独立开发、节省时间、隐藏实现。

2. **Makefile基础规则**  
   - 文件名：必须为`Makefile`或`makefile`（无后缀）；  
   - 规则结构：`目标: 依赖列表` + Tab开头的命令（空格无效，报错“missing separator”）；  
   - 执行流程：递归检查依赖→按需编译→链接生成目标。

3. **变量使用**  
   - 普通变量：`FLAGS = -pedantic-errors -std=c++11`（课程必加）、`TARGET`（目标名）、`OBJECTS`（.o列表），引用方式`$(变量名)`；  
   - 特殊变量：`$@`（目标）、`$^`（完整依赖）、`$<`（最左侧依赖），必须掌握替换示例。

4. **伪目标与`.PHONY`**  
   - 常见伪目标：`clean`（删除产物）、`tar`（打包源码）；  
   - 必须用`.PHONY: clean tar`声明，避免与目录中现有文件冲突；  
   - 执行命令：`make clean`、`make tar`。

5. **课程特殊要求**  
   - 编译选项必须包含`-pedantic-errors`（严格语法检查）和`-std=c++11`（C++11标准），否则可能编译失败；  
   - 头文件必须加包含守卫（`#ifndef/#define/#endif`），避免重复包含。