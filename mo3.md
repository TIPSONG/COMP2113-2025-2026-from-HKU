# COMP2113 Module 3 C++ Basics 备考笔记（完整版，基于《Module 3 - Guidance Notes.pdf》）
## 一、文档基础信息与核心注意事项
### 1.1 文档核心信息
- **课程关联**：COMP2113 Programming Technologies  
- **模块名称**：Module 3: C++ Basics  
- **预计学习时长**：3 Hours  
- **核心内容**：C++程序编译执行、基础操作（变量/常量/运算符/I/O）、控制流（分支/循环）  
- **关键限定**：聚焦C++语法，暂不涉及C语言的I/O操作；标有“Reference Only”或“Optional”的内容不纳入考核范围。

### 1.2 重要前置要求（C++标准与编译）
#### 1.2.1 强制C++标准编译命令
- **英文原文**：You can force the compiler to use a certain C++ standard (e.g., C++11) with `g++ -pedantic-errors -std=c++11 your_program.cpp -o your_program`. The `-pedantic-errors` flag ensures code conforms to ISO C/C++ standard.  
- **中文译文**：可通过指定编译命令强制编译器使用特定C++标准（如C++11），`-pedantic-errors`选项确保代码符合ISO C/C++标准，避免非标准语法。  
- 代码示例（编译`hello.cpp`）：
  ```bash
  # 编译命令：指定C++11标准，严格检查语法错误
  $ g++ -pedantic-errors -std=c++11 hello.cpp -o hello
  # 执行生成的可执行文件
  $ ./hello
  ```

#### 1.2.2 `using namespace std`的必要性
- **英文原文**：`cout` and `endl` are under the namespace `std`. If `using namespace std;` is removed, you must write `std::cout` and `std::endl` to avoid compilation errors.  
- **中文译文**：`cout`（标准输出）和`endl`（换行符）属于`std`命名空间。若删除`using namespace std;`，需在使用时添加`std::`前缀，否则编译器会报“未声明”错误。  
- 错误示例（删除`using namespace std;`后）：
  ```cpp
  #include <iostream>
  // 注释掉using namespace std;
  int main() {
      // 错误：cout未声明（缺少std::）
      cout << "Hello World!" << endl; 
      return 0;
  }
  ```
- 正确修正（二选一）：
  ```cpp
  // 方法1：添加using namespace std;
  #include <iostream>
  using namespace std;
  int main() {
      cout << "Hello World!" << endl;
      return 0;
  }

  // 方法2：使用std::前缀
  #include <iostream>
  int main() {
      std::cout << "Hello World!" << std::endl;
      return 0;
  }
  ```


## 二、Part I：程序编译与执行（Program Compilation & Execution）
### 2.1 第一个C++程序（Hello World）
#### 2.1.1 程序代码与结构解析
```cpp
// 注释：这是第一个C++程序（// 开头为单行注释）
#include <iostream>  // 包含输入输出流库（cout/endl定义在此）
using namespace std; // 启用std命名空间，避免重复写std::

int main() {  // 主函数：C++程序的入口（必须存在且唯一）
    cout << "Hello World!" << endl;  // 标准输出：打印字符串并换行
    return 0;  // 主函数返回0，表示程序正常结束（C++11后可省略，但建议保留）
}
```
- 结构说明：  
  1. `#include <iostream>`：预处理指令，告诉编译器引入`iostream`库（处理屏幕输出/键盘输入）；  
  2. `int main()`：主函数，程序从`main()`的第一句开始执行；  
  3. `return 0`：向操作系统返回0，标识程序成功执行（非0值表示异常）。

#### 2.1.2 编辑、编译与执行步骤
1. **编辑程序**：在Linux环境中用`vi`编辑（推荐），保存为`hello.cpp`：
   ```bash
   $ vi hello.cpp  # 打开vi编辑器，输入上述Hello World代码
   ```
2. **编译程序**：使用指定C++标准的`g++`命令：
   ```bash
   $ g++ -pedantic-errors -std=c++11 hello.cpp -o hello
   # 若编译成功，当前目录会生成可执行文件hello（无后缀）
   ```
3. **执行程序**：通过`./可执行文件名`运行：
   ```bash
   $ ./hello
   Hello World!  # 输出结果
   ```

### 2.2 调试提示（Hints on Debugging）
- **英文原文**：  
  1. Compiler-reported error lines may be incorrect (error may be before the reported line).  
  2. Error nature may be incorrect (compiler guesses your intent).  
  3. Fix the first error first and recompile (subsequent errors are often wrong).  
- **中文译文**：  
  1. 编译器提示的错误行号可能不准确（错误可能在提示行之前）；  
  2. 错误类型描述可能不准确（编译器仅基于语法推测）；  
  3. 若存在多个错误，优先修复第一个错误并重新编译（后续错误多由第一个错误引发，修复后可能消失）。

### 2.3 Windows离线开发选项（Offline Options）
- **英文原文**：Windows users can use IDEs like Dev-C++ (https://www.bloodshed.net/) or Code::Blocks (https://www.codeblocks.org/) for simple C++ programs, but advanced features (input/output redirection) are not supported. **Test programs on Linux before submission**.  
- **中文译文**：Windows用户可使用Dev-C++或Code::Blocks编写简单C++程序，但不支持输入输出重定向等高级功能。**提交作业前必须在Linux平台测试程序**，避免因系统差异导致运行错误。


## 三、Part II：基础操作（Basic Operations）
### 3.1 变量（Variables）
#### 3.1.1 变量声明与初始化
- **核心规则**：C++是**静态类型语言**，变量声明时必须指定数据类型；未初始化的变量会存储“垃圾值”（不可预测），使用前必须赋值或初始化。  
- 声明与初始化示例：
  ```cpp
  #include <iostream>
  using namespace std;

  int main() {
      // 1. 仅声明（未初始化，值为垃圾值，不推荐直接使用）
      int width;
      // 2. 声明并初始化（推荐）
      int height = 4;
      // 3. 同时声明多个同类型变量（部分初始化，未初始化的仍为垃圾值）
      int area = width * height, length = 5;  // 风险：width未初始化，area值不可预测

      // 正确用法：先初始化所有变量
      width = 5;
      area = width * height;  // 此时area = 5*4 = 20（正确）
      cout << "Area: " << area << endl;
      return 0;
  }
  ```

#### 3.1.2 变量名（标识符）规则
- **英文原文**：  
  1. Must start with a letter (A-Z/a-z) or underscore (_).  
  2. Subsequent characters: letters, digits (0-9), or underscore.  
  3. Case-sensitive (e.g., `radius` vs `Radius` are different).  
  4. Cannot be a reserved keyword (e.g., `int`, `if`, `else`).  
- **中文译文**：  
  1. 必须以字母（A-Z/a-z）或下划线（_）开头；  
  2. 后续字符可包含字母、数字（0-9）或下划线；  
  3. 区分大小写（如`radius`和`Radius`是不同变量）；  
  4. 不能是C++保留关键字（如`int`、`if`、`else`）。  
- 示例（合法vs非法标识符）：
  | 合法标识符 | 非法标识符 | 原因 |
  |------------|------------|------|
  | `a_man`、`_oOOo_`、`ABCx123` | `2008`、`year1-student`、`an integer` | 以数字开头/含减号/含空格 |
  | `Days_of_Week`、`cos` | `const`、`friend`、`delete` | 是C++保留关键字 |

#### 3.1.3 C++保留关键字（Reserved Keywords）
- 无需死记，常用关键字包括：`int`、`double`、`bool`、`if`、`else`、`for`、`while`、`return`、`const`、`using`、`namespace`等（完整列表见文档P19）。

### 3.2 数据类型（Data Types）
#### 3.2.1 基础数据类型属性（32位系统）
| 数据类型 | 描述（Description） | 大小（Size） | 范围（Range） |
|----------|--------------------|--------------|---------------|
| `char`   | 字符或小整数 | 1字节 | 0 ~ 255（无符号）；-128 ~ 127（有符号） |
| `bool`   | 布尔值 | 1字节 | `true`（1）或`false`（0） |
| `int`    | 整数 | 4字节 | -2147483648 ~ 2147483647 |
| `double` | 双精度浮点数 | 8字节 | ±1.7e-308 ~ ±1.7e+308（约15位有效数字） |
- 注意：大小和范围因系统（32位/64位）而异，上述为常见值。

#### 3.2.2 字符串基础（Strings - The Very Basics）
- **核心规则**：`string`不是C++基础数据类型，而是标准库中的类，使用前必须包含`<string>`头文件（即使部分编译器隐式包含，也建议显式添加）。  
- 示例：
  ```cpp
  #include <iostream>
  #include <string>  // 必须包含string头文件
  using namespace std;

  int main() {
      string greeting = "Hi", name = "COMP2113";  // 字符串变量初始化
      cout << greeting << " " << name << endl;  // 输出：Hi COMP2113
      
      greeting = "Good morning";  // 修改字符串值
      cout << greeting << " " << name << endl;  // 输出：Good morning COMP2113
      return 0;
  }
  ```

### 3.3 常量（Constants）
#### 3.3.1 字面常量（Literal Constants）
- 直接写在代码中的固定值，无需声明，类型由值推断：
  | 类型 | 示例 | 说明 |
  |------|------|------|
  | 整数 | `65`（十进制）、`0101`（八进制，即十进制65）、`0x41`（十六进制，即十进制65） | 不同进制表示 |
  | 浮点数 | `3.14`、`6e23`（科学计数法，即6×10²³）、`1.6e-19` | 小数或科学计数法 |
  | 字符 | `'A'`、`'\n'`（换行符）、`'\x41'`（十六进制ASCII码，即'A'） | 用单引号`' '`包裹 |
  | 字符串 | `"Hello"`、`""`（空字符串） | 用双引号`" "`包裹（与字符区分） |
  | 布尔 | `true`、`false` | 对应整数1和0 |

#### 3.3.2 `const`修饰的常量变量
- **核心规则**：用`const`修饰变量后，其值不可修改（只读），编译时会检查，避免意外更改。  
- 示例（错误与正确用法）：
  ```cpp
  #include <iostream>
  using namespace std;

  int main() {
      // 正确：声明const常量并初始化（必须初始化，否则报错）
      const double PI = 3.14159265359;
      double r = 5.0;
      double area = PI * r * r;  // 合法：读取PI的值
      
      // 错误：尝试修改const变量的值，编译报错
      PI = 3.14;  // 编译错误：assignment of read-only variable 'PI'
      
      cout << "Area: " << area << endl;
      return 0;
  }
  ```

### 3.4 运算符（Operators）
#### 3.4.1 算术运算符（Arithmetic Operators）
| 运算符 | 功能 | 示例 | 注意事项 |
|--------|------|------|----------|
| `+`    | 加法 | `3 + 5 = 8` | - |
| `-`    | 减法 | `10 - 4 = 6` | - |
| `*`    | 乘法 | `2 * 6 = 12` | 注意与指针的区别，此处为算术乘法 |
| `/`    | 除法 | `7 / 2 = 3`（整数除法，截断小数）；`7.0 / 2 = 3.5`（浮点数除法） | 整数/整数结果为整数；除数不能为0（运行时错误） |
| `%`    | 取模（求余数） | `13 % 3 = 1` | 仅适用于整数，不能用于`double` |

- 示例（整数除法与取模）：
  ```cpp
  #include <iostream>
  using namespace std;

  int main() {
      int a = 7, b = 2;
      double c = 7.0, d = 2;
      
      cout << a / b << endl;  // 输出3（整数除法，截断小数）
      cout << c / d << endl;  // 输出3.5（浮点数除法）
      cout << 13 % 3 << endl; // 输出1（13 = 3*4 + 1）
      
      // 错误：取模用于double，编译报错
      // cout << 7.0 % 2 << endl;
      return 0;
  }
  ```

#### 3.4.2 关系运算符（Relational Operators）
- 用于比较两个值，结果为`bool`类型（`true`=1，`false`=0）：

| 运算符 | 功能 | 示例（`a=1`, `b=2`） | 结果 |
|--------|------|----------------------|------|
| `>`    | 大于 | `a > b` | `false`（0） |
| `>=`   | 大于等于 | `a >= b` | `false`（0） |
| `<`    | 小于 | `a < b` | `true`（1） |
| `<=`   | 小于等于 | `a <= b` | `true`（1） |
| `==`   | 等于 | `a == b` | `false`（0） |
| `!=`   | 不等于 | `a != b` | `true`（1） |

#### 3.4.3 逻辑运算符（Logical Operators）
- 用于组合布尔表达式，遵循“短路求值”（仅计算必要的表达式）：

| 运算符 | 功能 | 真值表（A,B为布尔值） | 短路规则 |
|--------|------|------------------------|----------|
| `&&`   | 逻辑与（AND） | A=true且B=true → true；否则false | A=false时，不计算B |
| `||`   | 逻辑或（OR） | A=true或B=true → true；否则false | A=true时，不计算B |
| `!`    | 逻辑非（NOT） | A=true → false；A=false → true | - |

- 示例（短路求值）：
  ```cpp
  #include <iostream>
  using namespace std;

  int main() {
      int gals = 0, gifts = 10;
      // 短路求值：gals==0为true，不计算gifts/gals（避免除0错误）
      bool omg = (gals == 0) || ((gifts / gals) < 2);
      cout << omg << endl;  // 输出1（true）
      
      // 短路求值：gals!=0为false，不计算gifts/gals（避免除0错误）
      bool cool = (gals != 0) && ((gifts / gals) >= 2);
      cout << cool << endl; // 输出0（false）
      return 0;
  }
  ```

#### 3.4.4 自增/自减运算符（Increment/Decrement）
- **前缀（`++i`/`--i`）**：先自增/自减，再使用值；  
- **后缀（`i++`/`i--`）**：先使用值，再自增/自减。  
- 示例：
  ```cpp
  #include <iostream>
  using namespace std;

  int main() {
      int i = 0, c;
      
      // 前缀++：先i+1（i=1），再赋值给c（c=1）
      c = ++i;
      cout << "c=" << c << ", i=" << i << endl;  // 输出c=1, i=1
      
      // 后缀++：先赋值给c（c=1），再i+1（i=2）
      c = i++;
      cout << "c=" << c << ", i=" << i << endl;  // 输出c=1, i=2
      return 0;
  }
  ```

#### 3.4.5 赋值运算符（Assignment Operators）
- 复合赋值运算符简化`变量 = 变量 运算符 值`的写法：

| 运算符 | 示例 | 等价于 |
|--------|------|--------|
| `=`    | `x = 5` | - |
| `+=`   | `x += 3` | `x = x + 3` |
| `-=`   | `x -= 2` | `x = x - 2` |
| `*=`   | `x *= 4` | `x = x * 4` |
| `/=`   | `x /= 2` | `x = x / 2` |
| `%=`   | `x %= 3` | `x = x % 3` |

### 3.5 类型转换（Type Conversions）
#### 3.5.1 隐式转换（Implicit Conversion）
- 编译器自动完成，遵循“低精度→高精度”规则（避免数据丢失）：
  ```cpp
  #include <iostream>
  using namespace std;

  int main() {
      // 1. 整数→浮点数（低→高，安全）
      double result1 = 3.0 / 2;  // 2（int）→2.0（double），结果3.5
      // 2. 浮点数→整数（高→低，截断小数，可能丢失数据）
      int result2 = 2.8;  // 2.8（double）→2（int），编译器可能警告
      
      cout << result1 << endl;  // 输出3.5
      cout << result2 << endl;  // 输出2
      return 0;
  }
  ```

#### 3.5.2 显式转换（Explicit Conversion，强制类型转换）
- 手动指定转换类型，避免编译器警告，格式：`(目标类型)值`：
  ```cpp
  #include <iostream>
  using namespace std;

  int main() {
      // 显式转换：2.8→2（int），编译器无警告
      int x = (int)2.8;
      cout << x << endl;  // 输出2
      
      // 错误：无意义的转换（字符串→整数），编译报错
      // int y = (int)"abc";
      return 0;
  }
  ```

### 3.6 基础I/O（Input/Output）
#### 3.6.1 标准输出（`cout`）
- **功能**：向标准输出设备（默认屏幕）输出数据，使用`<<`（插入运算符），可链式调用。  
- 转义序列（特殊字符）：
  | 转义序列 | 功能 |
  |----------|------|
  | `\n`     | 换行 |
  | `\t`     | 水平制表符 |
  | `\\`     | 输出反斜杠 |
  | `\'`     | 输出单引号 |
  | `\"`     | 输出双引号 |

- 示例：
  ```cpp
  #include <iostream>
  using namespace std;

  int main() {
      int a = 1, b = 2;
      // 链式输出：多个值用<<连接
      cout << "a=" << a << "\t" << "b=" << b << endl;  // 输出a=1    b=2（\t制表）
      cout << "Line1\nLine2" << endl;  // 输出两行：Line1和Line2
      return 0;
  }
  ```

#### 3.6.2 标准输入（`cin`）
- **功能**：从标准输入设备（默认键盘）读取数据，使用`>>`（提取运算符），需按`Enter`确认输入。  
- 示例（读取多个值）：
  ```cpp
  #include <iostream>
  using namespace std;

  int main() {
      int age;
      double height, weight;
      
      // 提示用户输入
      cout << "Please input your age, height and weight: ";
      // 读取三个值（空格/换行分隔均可）
      cin >> age >> height >> weight;
      
      // 输出结果
      cout << "\nYour age: " << age << endl;
      cout << "Your height: " << height << endl;
      cout << "Your weight: " << weight << endl;
      return 0;
  }
  ```
- 运行效果：
  ```
  Please input your age, height and weight: 20 175.5 60
  Your age: 20
  Your height: 175.5
  Your weight: 60
  ```

#### 3.6.3 文件重定向（File Redirection）
- **功能**：将文件内容作为标准输入，避免重复手动输入（测试调试常用）。  
- 示例（假设`info.txt`内容为`20 170 58`）：
  ```bash
  # 1. 编译程序（假设程序名为simple_input.cpp）
  $ g++ -pedantic-errors -std=c++11 simple_input.cpp -o simple_input
  # 2. 文件重定向：将info.txt作为输入
  $ ./simple_input < info.txt
  # 输出结果（无需手动输入）：
  # Please input your age, height and weight: 
  # Your age: 20
  # Your height: 170
  # Your weight: 58
  ```


## 四、Part III：控制流（Flow of Control）
### 4.1 分支（Branching）
#### 4.1.1 `if`语句（单分支）
- **语法**：条件为`true`时执行语句（或代码块`{}`）。  
- 示例（判断是否及格）：
  ```cpp
  #include <iostream>
  using namespace std;

  int main() {
      int mark = 75;
      // 条件为true（75>=60），执行cout
      if (mark >= 60) {
          cout << "Passed!" << endl;
          cout << "Well done!" << endl;  // 代码块内的语句均执行
      }
      return 0;
  }
  ```

#### 4.1.2 `if-else`语句（双分支）
- **语法**：条件为`true`执行`if`块，否则执行`else`块。  
- 示例（比较两个整数的最大值）：
  ```cpp
  #include <iostream>
  using namespace std;

  int main() {
      int a, b, max;
      cout << "Input two integers: ";
      cin >> a >> b;
      
      if (a > b) {
          max = a;  // a大时执行
      } else {
          max = b;  // b大或相等时执行
      }
      
      cout << "Max: " << max << endl;
      return 0;
  }
  ```

#### 4.1.3 嵌套`if-else`（多分支）
- **语法**：`if-else`内部嵌套`if-else`，注意“悬空`else`”问题（`else`默认匹配最近的未配对`if`）。  
- 示例（判断年龄和身高是否符合条件）：
  ```cpp
  #include <iostream>
  using namespace std;

  int main() {
      int age = 20, height = 180;
      // 嵌套if-else：先判断年龄，再判断身高
      if (age >= 18 && age <= 25) {
          if (height >= 180) {
              cout << "Yes I do!" << endl;
          } else {
              cout << "I am sorry" << endl;
          }
      } else {
          cout << "Bye bye" << endl;
      }
      return 0;
  }
  ```

#### 4.1.4 `switch`语句（多分支，基于值匹配）
- **语法**：根据“控制表达式”（必须为整数/字符/布尔）的值匹配`case`，需用`break`终止当前分支（否则“贯穿”），`default`处理未匹配的情况。  
- 示例（根据成绩等级输出绩点）：
  ```cpp
  #include <iostream>
  using namespace std;

  int main() {
      char grade;
      cout << "Input your grade (A-F): ";
      cin >> grade;
      
      switch (grade) {
          case 'A':
              cout << "Grade point: 4.0" << endl;
              break;  // 终止分支，避免贯穿
          case 'B':
              cout << "Grade point: 3.0" << endl;
              break;
          case 'C':
              cout << "Grade point: 2.0" << endl;
              break;
          case 'D':
              cout << "Grade point: 1.0" << endl;
              break;
          case 'F':
              cout << "Grade point: 0.0" << endl;
              break;
          default:
              cout << "Invalid grade!" << endl;  // 未匹配时执行
      }
      return 0;
  }
  ```
- 注意：若省略`break`，会从匹配的`case`开始向下执行所有`case`（如输入`B`会输出3.0、2.0、1.0、0.0和无效提示）。

#### 4.1.5 三元运算符（`?:`，`if-else`简写）
- **语法**：`condition ? expr1 : expr2`（条件为`true`返回`expr1`，否则返回`expr2`）。  
- 示例（简化及格判断）：
  ```cpp
  #include <iostream>
  using namespace std;

  int main() {
      int mark = 55;
      // 等价于if(mark>=60) cout<<"Passed"; else cout<<"Failed";
      cout << (mark >= 60 ? "Passed" : "Failed") << endl;  // 输出Failed
      return 0;
  }
  ```

### 4.2 循环（Looping）
#### 4.2.1 `while`循环（条件控制循环）
- **语法**：先判断条件，为`true`时执行循环体（需确保循环内更新条件变量，避免死循环）。  
- 三种常见类型：
  1. **计数器控制**（已知循环次数）：
     ```cpp
     #include <iostream>
     using namespace std;

     int main() {
         int i = 1;  // 计数器初始化
         // 条件：i<=5（循环5次）
         while (i <= 5) {
             cout << i << " ";
             i++;  // 更新计数器（必须，否则死循环）
         }
         return 0;  // 输出：1 2 3 4 5
     }
     ```

  2. **哨兵控制**（未知循环次数，用特殊值终止）：
     ```cpp
     #include <iostream>
     using namespace std;

     int main() {
         int x = 0, total = 0;
         cout << "Enter a negative number to end: ";
         // 哨兵：输入负数终止循环
         while (x >= 0) {
             total += x;
             cout << "Total: " << total << ", next number? ";
             cin >> x;  // 更新x（输入负数时循环终止）
         }
         cout << "Final total: " << total << endl;
         return 0;
     }
     ```

  3. **标记控制**（用`bool`变量控制循环）：
     ```cpp
     #include <iostream>
     using namespace std;

     int main() {
         int num = 23, guess;
         bool isGuessed = false;  // 标记：是否猜中
         while (!isGuessed) {
             cout << "Guess (0-99): ";
             cin >> guess;
             if (guess == num) {
                 cout << "Correct!" << endl;
                 isGuessed = true;  // 更新标记，终止循环
             } else if (guess < num) {
                 cout << "Too small!" << endl;
             } else {
                 cout << "Too large!" << endl;
             }
         }
         return 0;
     }
     ```

#### 4.2.2 `for`循环（计数器控制，结构紧凑）
- **语法**：`for(初始化; 条件; 更新)`，初始化仅执行一次，条件为`true`执行循环体，每次循环后执行更新。  
- 示例（输出1~20，每行一个）：
  ```cpp
  #include <iostream>
  using namespace std;

  int main() {
      // 初始化i=1；条件i<=20；更新i++
      for (int i = 1; i <= 20; ++i) {
          cout << i << endl;
      }
      return 0;
  }
  ```
- 示例（计算1~20的奇数和）：
  ```cpp
  #include <iostream>
  using namespace std;

  int main() {
      int sum = 0;
      // i从1开始，每次+2（仅奇数）
      for (int i = 1; i <= 20; i += 2) {
          sum += i;
      }
      cout << "Sum of odd numbers: " << sum << endl;  // 输出100
      return 0;
  }
  ```

#### 4.2.3 `break`与`continue`
- **`break`**：立即终止循环，跳转到循环后的语句：
  ```cpp
  #include <iostream>
  using namespace std;

  int main() {
      for (int i = 0; i < 20; ++i) {
          if (i == 15) {
              break;  // i=15时终止循环
          }
          cout << i << " ";  // 输出0~14
      }
      return 0;
  }
  ```

- **`continue`**：终止当前迭代，跳转到下一次迭代的条件判断：
  ```cpp
  #include <iostream>
  using namespace std;

  int main() {
      for (int i = 0; i < 20; ++i) {
          if (i % 2 == 0) {
              continue;  // 跳过偶数，仅执行奇数的cout
          }
          cout << i << " ";  // 输出1 3 5 ... 19
      }
      return 0;
  }
  ```


## 五、核心考点总结（Key Exam Focus）
1. **编译与执行**  
   - 强制C++标准的编译命令：`g++ -pedantic-errors -std=c++11 源文件.cpp -o 可执行文件`；  
   - `using namespace std`的作用（避免`std::`前缀）及删除后的错误修正；  
   - 调试原则：优先修复第一个错误，错误行号可能不准确。

2. **变量与常量**  
   - 变量声明必须指定类型，未初始化的变量有垃圾值；  
   - 标识符规则（开头字母/下划线，区分大小写，非关键字）；  
   - `const`常量的用法（必须初始化，值不可修改）。

3. **运算符与类型转换**  
   - 整数除法（截断小数）与取模（仅整数）；  
   - 逻辑运算符的短路求值（避免除0错误）；  
   - 自增/自减的前缀与后缀区别；  
   - 显式类型转换（`(int)2.8`）与隐式转换的规则。

4. **基础I/O**  
   - `cout`（`<<`、转义序列）与`cin`（`>>`）的用法；  
   - 文件重定向（`./程序 < 输入文件`）的测试场景。

5. **控制流**  
   - `if-else`的嵌套与悬空`else`问题（用`{}`避免）；  
   - `switch`的`break`必要性（防止贯穿）与`default`的作用；  
   - `while`循环的条件更新（避免死循环）；  
   - `for`循环的结构（初始化/条件/更新）；  
   - `break`（终止循环）与`continue`（跳过当前迭代）的区别。