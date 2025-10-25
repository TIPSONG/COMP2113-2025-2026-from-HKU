# COMP2113 Module 7 Structs, File I/O & Recursion 备考笔记（完整版，基于《Module 7 - Guidance Notes.pdf》）
## 一、文档基础信息与前置要求
### 1.1 文档核心信息
- **课程关联**：COMP2113 Programming Technologies  
- **模块名称**：Module 7: Structs, File I/O & Recursion  
- **预计学习时长**：3 Hours  
- **核心内容**：  
  1. **结构体（Structs）**：自定义复合数据类型，含成员变量/函数、嵌套结构体、结构体数组与函数传递；  
  2. **文件I/O**：文件读写（ofstream/ifstream）、追加模式、读至文件尾、字符串流（istringstream）、流格式化；  
  3. **递归（Recursion）**：递归定义、递归函数结构、示例（阶乘/斐波那契/汉诺塔）、栈溢出问题、递归与迭代对比；  
- **关键限定**：标有“Reference Only”的内容（如结构体大小对齐、类的详细实现）不纳入考核；类（Class）为可选内容，无需掌握代码实现。

### 1.2 前置要求（编译选项）
- **英文原文**：Use C++ 11 standard with `g++ -pedantic-errors -std=c++11 your_program.cpp`; `-pedantic-errors` ensures code conforms to ISO C/C++ standard (enforced in assignments).  
- **中文译文**：编译时必须指定C++11标准，命令为`g++ -pedantic-errors -std=c++11 源文件.cpp`，`-pedantic-errors`确保代码符合ISO标准，作业提交会强制检查。


## 二、Part I 结构体（Structs）
### 2.1 结构体核心概念
#### 2.1.1 什么是结构体？
- **英文原文**：A struct is a collection of variables (members) of different types, grouped under a single name. It organizes complex data, allowing related variables to be treated as a single unit (unlike parallel arrays).  
- **中文译文**：结构体是不同类型变量（成员）的集合，用单一名称分组，将相关变量视为一个整体（优于并行数组），C++中结构体还可包含成员函数（C语言结构体仅含成员变量）。

### 2.2 结构体的定义与初始化
#### 2.2.1 定义语法
- **英文原文**：Use `struct` keyword, followed by struct tag, member list in `{}`, and a semicolon.  
- **中文译文**：定义格式为“`struct 结构体名 { 成员列表; };`”，成员可含不同数据类型。  
- 示例：
  ```cpp
  // 定义学生结构体（含成员变量）
  struct Student {
      int id;         // 学号（int型）
      string name;    // 姓名（string型）
      char sex;       // 性别（char型）
      double gpa;     // 绩点（double型）
  };  // 必须加半角分号

  // 定义点结构体（后续用于嵌套/函数示例）
  struct Point {
      double x;  // x坐标
      double y;  // y坐标
  };
  ```

#### 2.2.2 初始化方式
- **英文原文**：Initialize with `{}` (order matches member order); omit size for arrays; fewer values → remaining members initialized to 0; assign with another struct of the same type.  
- **中文译文**：支持列表初始化、同类型结构体赋值，值不足时剩余成员默认初始化为0，示例：
  ```cpp
  #include <iostream>
  #include <string>
  using namespace std;

  struct Student {
      int id;
      string name;
      char sex;
      double gpa;
  };

  int main() {
      // 1. 列表初始化（成员顺序必须与定义一致）
      Student s1 = {3035123456, "Sze Ka Ka", 'F', 3.8};

      // 2. 部分初始化（剩余成员为0/空）
      Student s2 = {3035123457, "John"};  // sex='\0'，gpa=0.0

      // 3. 同类型结构体赋值
      Student s3 = s1;  // s3的id、name等与s1完全相同

      return 0;
  }
  ```

### 2.3 结构体成员访问与操作
#### 2.3.1 成员访问：点运算符（.）
- **英文原文**：Access members with the dot operator `.`; nested structs use `.` repeatedly.  
- **中文译文**：通过“结构体变量.成员名”访问成员，嵌套结构体需多级使用`.`。  
- 示例（嵌套结构体与成员修改）：
  ```cpp
  #include <iostream>
  using namespace std;

  // 嵌套结构体：三角形含3个点
  struct Point {
      double x;
      double y;
  };

  struct Triangle {
      Point p1, p2, p3;  // 成员为Point结构体
  };

  int main() {
      Triangle tr = {{1.0, 2.0}, {3.0, 4.0}, {5.0, 6.0}};

      // 访问嵌套成员：tr.p1.x（三角形的第一个点的x坐标）
      cout << "tr.p1.x = " << tr.p1.x << endl;  // 输出1.0

      // 修改成员值
      tr.p1.x += 2.0;  // tr.p1.x变为3.0
      cout << "Updated tr.p1.x = " << tr.p1.x << endl;  // 输出3.0

      return 0;
  }
  ```

#### 2.3.2 结构体数组
- **英文原文**：An array of structs stores multiple struct variables; access via `数组名[索引].成员名`.  
- **中文译文**：结构体数组存储多个结构体变量，通过“数组索引.成员名”访问，适用于批量存储同类数据（如多个学生记录）。  
- 示例（学生数组）：
  ```cpp
  #include <iostream>
  #include <string>
  using namespace std;

  struct Student {
      string name;
      int id;
      double gpa;
  };

  int main() {
      const int MAX_STUDENTS = 2;
      Student students[MAX_STUDENTS];  // 结构体数组

      // 初始化数组元素
      students[0] = {"Alice", 3035123456, 3.9};
      students[1].name = "Bob";
      students[1].id = 3035123457;
      students[1].gpa = 3.5;

      // 遍历数组
      for (int i = 0; i < MAX_STUDENTS; ++i) {
          cout << "Student " << i+1 << ": " 
               << students[i].name << ", " 
               << students[i].id << ", " 
               << students[i].gpa << endl;
      }
      return 0;
  }
  ```

### 2.4 结构体与函数
#### 2.4.1 结构体作为函数参数
- **英文原文**：Structs can be passed by value (copies the entire struct) or by reference (more efficient, modifies original).  
- **中文译文**：结构体可值传递（拷贝整个结构体，效率低）或引用传递（仅传递地址，效率高，可修改原结构体）。  
- 示例（值传递 vs 引用传递）：
  ```cpp
  #include <iostream>
  using namespace std;

  struct Point {
      double x;
      double y;
  };

  // 1. 值传递：不修改原结构体
  void printPoint(Point p) {
      cout << "(" << p.x << ", " << p.y << ")" << endl;
  }

  // 2. 引用传递：修改原结构体
  void translatePoint(Point &p, double dx, double dy) {
      p.x += dx;  // 修改原结构体的x
      p.y += dy;  // 修改原结构体的y
  }

  int main() {
      Point p = {1.0, 2.0};
      printPoint(p);  // 输出(1, 2)

      translatePoint(p, 3.0, 4.0);  // 原结构体p的x=4.0，y=6.0
      printPoint(p);  // 输出(4, 6)

      return 0;
  }
  ```

#### 2.4.2 结构体的成员函数（C++特有）
- **英文原文**：C++ structs can have member functions (access members directly without parameters); define inside the struct or use `::` outside.  
- **中文译文**：C++结构体可包含成员函数（直接访问成员，无需传参），可在结构体内定义，或在外部用作用域解析符`::`定义。  
- 示例（圆结构体的成员函数）：
  ```cpp
  #include <iostream>
  using namespace std;

  struct Circle {
      double x, y;  // 圆心坐标
      double r;     // 半径

      // 1. 结构体内定义成员函数：计算面积
      double Area() {
          const double PI = 3.14159;
          return PI * r * r;
      }

      // 2. 成员函数声明（外部定义）
      void Enlarge(double addR);
  };

  // 外部定义成员函数：扩大半径
  void Circle::Enlarge(double addR) {
      r += addR;  // 直接访问成员r
  }

  int main() {
      Circle c = {0, 0, 2.0};
      cout << "Original Area: " << c.Area() << endl;  // 输出12.56636

      c.Enlarge(3.0);  // 半径变为5.0
      cout << "Enlarged Area: " << c.Area() << endl;  // 输出78.53975

      return 0;
  }
  ```

### 2.5 可选内容：类（Class）
- **英文原文**：Class is an ADT (Abstract Data Type) with public (interface) and private (implementation) sections; struct members are public by default, class members are private by default.  
- **中文译文**：类是抽象数据类型，含公有（接口，外部可访问）和私有（实现，仅成员函数可访问）部分；结构体默认公有，类默认私有。无需掌握代码实现，了解与结构体的区别即可。


## 三、Part II 文件I/O（File I/O）
### 3.1 核心概念：流（Streams）
- **英文原文**：C++ uses streams for I/O: `cout` (screen output), `cin` (keyboard input), `ofstream` (file output), `ifstream` (file input); include `<fstream>` for file operations.  
- **中文译文**：C++通过流实现I/O：`cout`（屏幕输出）、`cin`（键盘输入）、`ofstream`（文件输出）、`ifstream`（文件输入）；文件操作需包含`<fstream>`头文件。

### 3.2 写文件（ofstream）
#### 3.2.1 基本流程
1. 声明`ofstream`对象；  
2. 打开文件（`open()`或构造函数）；  
3. 检查文件打开错误（`fail()`）；  
4. 用`<<`写文件；  
5. 关闭文件（`close()`）。

#### 3.2.2 示例（写文件与追加模式）
```cpp
#include <iostream>
#include <fstream>
#include <cstdlib>  // for exit()
#include <string>
using namespace std;

int main() {
    // 1. 声明输出流对象并打开文件（或 ofstream fout; fout.open("data1.txt");）
    ofstream fout("data1.txt");

    // 2. 检查文件打开错误
    if (fout.fail()) {
        cout << "Error opening file!" << endl;
        exit(1);  // 终止程序
    }

    // 3. 写文件（类似cout）
    string name = "Peter";
    int age = 30;
    double weight = 130.5;
    fout << name << " " << age << " " << weight << endl;

    // 4. 追加模式写文件（ios::app）
    ofstream fout_append("data1.txt", ios::app);
    if (fout_append.fail()) {
        cout << "Error opening file for append!" << endl;
        exit(1);
    }
    fout_append << "John 25 129.3" << endl;

    // 5. 关闭文件
    fout.close();
    fout_append.close();
    return 0;
}
```
- **输出结果**：`data1.txt`内容为  
  `Peter 30 130.5`  
  `John 25 129.3`

### 3.3 读文件（ifstream）
#### 3.3.1 基本流程
1. 声明`ifstream`对象；  
2. 打开文件；  
3. 检查错误；  
4. 用`>>`或`getline()`读文件；  
5. 关闭文件。

#### 3.3.2 示例1：读至文件尾（EOF）
```cpp
#include <iostream>
#include <fstream>
#include <cstdlib>
#include <string>
using namespace std;

int main() {
    ifstream fin("data1.txt");
    if (fin.fail()) {
        cout << "Error opening file!" << endl;
        exit(1);
    }

    // 读至文件尾：fin >> 表达式返回false时到达EOF
    string name;
    int age;
    double weight;
    while (fin >> name >> age >> weight) {
        cout << "Name: " << name << ", Age: " << age << ", Weight: " << weight << endl;
    }

    fin.close();
    return 0;
}
```
- **输出结果**：  
  `Name: Peter, Age: 30, Weight: 130.5`  
  `Name: John, Age: 25, Weight: 129.3`

#### 3.3.3 示例2：逐行读文件（getline()）
```cpp
#include <iostream>
#include <fstream>
#include <cstdlib>
#include <string>
using namespace std;

int main() {
    ifstream fin("data1.txt");
    if (fin.fail()) {
        cout << "Error opening file!" << endl;
        exit(1);
    }

    // 逐行读：getline(流对象, 字符串变量)
    string line;
    while (getline(fin, line)) {
        cout << "Line: " << line << endl;
    }

    fin.close();
    return 0;
}
```
- **输出结果**：  
  `Line: Peter 30 130.5`  
  `Line: John 25 129.3`

### 3.4 字符串流（istringstream）
- **英文原文**：`istringstream` treats a string as a stream; extract data with `>>`; include `<sstream>`.  
- **中文译文**：`istringstream`将字符串视为流，用`>>`提取数据（如拆分空格分隔的字符串），需包含`<sstream>`头文件。  
- 示例（拆分字符串）：
  ```cpp
  #include <iostream>
  #include <sstream>
  #include <string>
  using namespace std;

  int main() {
      string line = "Apple Banana Cherry";
      istringstream iss(line);  // 将字符串转为流

      string word;
      // 提取流中的单词（空格分隔）
      while (iss >> word) {
          cout << "Word: " << word << endl;
      }
      return 0;
  }
  ```
- **输出结果**：  
  `Word: Apple`  
  `Word: Banana`  
  `Word: Cherry`

### 3.5 流格式化（Output Manipulators）
- **英文原文**：Use manipulators to format output (e.g., fixed decimal, column width); include `<iomanip>` for parameterized manipulators (e.g., `setw`, `setprecision`).  
- **中文译文**：用操纵符格式化输出（如固定小数、列宽），带参数的操纵符（`setw`、`setprecision`）需包含`<iomanip>`。

#### 3.5.1 常用操纵符
| 操纵符（Manipulator） | 功能（Function） | 示例 |
|-----------------------|------------------|------|
| `fixed`               | 固定小数格式（非科学计数法） | `cout << fixed << 0.000123` → `0.000123` |
| `scientific`          | 科学计数法 | `cout << scientific << 0.000123` → `1.230000e-04` |
| `setprecision(n)`      | 固定格式下保留n位小数；默认格式下保留n位有效数字 | `cout << fixed << setprecision(2) << 1.234` → `1.23` |
| `setw(n)`             | 设置下一个输出的列宽（右对齐，默认空格填充） | `cout << setw(5) << 12` → `  12`（占5列） |
| `setfill(c)`          | 配合`setw`，用字符c填充空白 | `cout << setfill('*') << setw(5) << 12` → `***12` |
| `left`/`right`        | 左对齐/右对齐（配合`setw`） | `cout << left << setw(5) << 12` → `12   ` |

#### 3.5.2 示例（格式化输出表格）
```cpp
#include <iostream>
#include <iomanip>
#include <string>
using namespace std;

int main() {
    cout << fixed << setprecision(2);  // 固定2位小数
    cout << setfill('-');  // 填充字符为'-'

    // 表头
    cout << left << setw(10) << "Name" 
         << left << setw(5) << "Age" 
         << left << setw(8) << "Weight" << endl;
    cout << setw(23) << "-" << endl;  // 分隔线

    // 数据行
    cout << left << setw(10) << "Peter" 
         << left << setw(5) << 30 
         << left << setw(8) << 130.5 << endl;
    cout << left << setw(10) << "John" 
         << left << setw(5) << 25 
         << left << setw(8) << 129.3 << endl;

    return 0;
}
```
- **输出结果**：  
  `Name      Age   Weight  `  
  `-----------------------`  
  `Peter     30    130.50  `  
  `John      25    129.30  `


## 四、Part III 递归（Recursion）
### 4.1 核心概念：递归定义
- **英文原文**：A recursive problem is defined in terms of a smaller version of itself, with two parts: ① Base case (terminates recursion); ② General case (reduces to base case).  
- **中文译文**：递归问题通过“更小的自身”定义，需满足两个条件：① 基例（终止递归）；② 递归例（分解为更小的子问题，最终到达基例）。  
- 示例：阶乘的递归定义  
  基例：`0! = 1`  
  递归例：`n! = n × (n-1)!`（n > 0）

### 4.2 递归函数的结构与流程
#### 4.2.1 递归函数的通用结构
```cpp
返回类型 递归函数(参数) {
    if (基例条件) {
        return 基例值;  // 终止递归
    } else {
        // 递归例：调用自身处理更小的子问题
        return 递归操作(递归函数(更小参数));
    }
}
```

#### 4.2.2 示例1：阶乘（Factorial）
```cpp
#include <iostream>
using namespace std;

// 递归函数：计算n!
int factorial(int n) {
    // 基例：0! = 1
    if (n == 0) {
        return 1;
    } 
    // 递归例：n! = n * (n-1)!
    else {
        return n * factorial(n - 1);
    }
}

int main() {
    int n = 5;
    cout << n << "! = " << factorial(n) << endl;  // 输出5! = 120
    return 0;
}
```
- **执行流程**：  
  `factorial(5)` → `5 × factorial(4)` → `5 × 4 × factorial(3)` → `5×4×3×factorial(2)` → `5×4×3×2×factorial(1)` → `5×4×3×2×1×factorial(0)` → `5×4×3×2×1×1 = 120`

#### 4.2.3 示例2：斐波那契数列（Fibonacci）
- 递归定义：  
  基例：`F(0)=0`，`F(1)=1`  
  递归例：`F(n) = F(n-1) + F(n-2)`（n > 1）
- 代码示例：
  ```cpp
  #include <iostream>
  using namespace std;

  int fib(int n) {
      if (n < 2) {
          return n;  // 基例：F(0)=0，F(1)=1
      } else {
          return fib(n-1) + fib(n-2);  // 递归例
      }
  }

  int main() {
      for (int i = 0; i < 10; ++i) {
          cout << fib(i) << " ";  // 输出0 1 1 2 3 5 8 13 21 34
      }
      return 0;
  }
  ```

#### 4.2.4 示例3：汉诺塔（Tower of Hanoi）
- **问题描述**：3根柱子（A、B、C），n个圆盘从A移到C，规则：① 一次移1个；② 大盘不压小盘。  
- **递归算法**：  
  基例（n=1）：直接从A移到C；  
  递归例（n>1）：① 把n-1个圆盘从A移到B（C为中间柱）；② 把第n个圆盘从A移到C；③ 把n-1个圆盘从B移到C（A为中间柱）。
- 代码示例：
  ```cpp
  #include <iostream>
  using namespace std;

  // 递归函数：n个圆盘从src移到des，tmp为中间柱
  void hanoi(int n, char src, char des, char tmp) {
      if (n == 1) {
          // 基例：1个圆盘直接移
          cout << "Move disk from " << src << " to " << des << endl;
      } else {
          // 1. n-1个从src→tmp
          hanoi(n-1, src, tmp, des);
          // 2. 第n个从src→des
          cout << "Move disk from " << src << " to " << des << endl;
          // 3. n-1个从tmp→des
          hanoi(n-1, tmp, des, src);
      }
  }

  int main() {
      int n = 3;
      hanoi(n, 'A', 'C', 'B');  // 3个圆盘从A移到C
      return 0;
  }
  ```
- **输出结果**（7步）：  
  `Move disk from A to C`  
  `Move disk from A to B`  
  `Move disk from C to B`  
  `Move disk from A to C`  
  `Move disk from B to A`  
  `Move disk from B to C`  
  `Move disk from A to C`

### 4.3 递归的问题与对比
#### 4.3.1 栈溢出（Stack Overflow）
- **英文原文**：Each recursive call uses stack memory; excessive depth (e.g., infinite recursion) causes stack overflow.  
- **中文译文**：每次递归调用占用栈内存，递归深度过大（如无基例导致无限递归）会引发栈溢出。  
- **示例（无限递归）**：
  ```cpp
  int infiniteRecur(int n) {
      // 无基例，会无限调用infiniteRecur(n)
      return n + infiniteRecur(n);
  }
  ```

#### 4.3.2 递归 vs 迭代
| 维度（Dimension） | 递归（Recursion） | 迭代（Iteration） |
|-------------------|-------------------|-------------------|
| 代码复杂度 | 简洁，易理解（如汉诺塔） | 较繁琐，需手动管理循环 |
| 效率 | 低（函数调用开销大） | 高（无函数调用开销） |
| 内存 | 耗内存（栈存储递归调用） | 省内存（仅需循环变量） |
| 适用场景 | 问题天然递归（如树形结构、分治） | 简单循环任务（如求和、遍历） |


## 五、核心考点总结（Key Exam Focus）
1. **结构体（Structs）**  
   - 定义与初始化：`struct 名 { 成员; };`，列表初始化需匹配成员顺序，同类型可赋值；  
   - 成员访问：点运算符`.`，嵌套结构体需多级访问（`tr.p1.x`）；  
   - 函数传递：值传递拷贝整个结构体，引用传递高效且可修改原结构体；  
   - 成员函数：C++结构体可含成员函数，外部定义用`::`，直接访问成员变量。

2. **文件I/O**  
   - 写文件：`ofstream`，追加用`ios::app`，检查`fail()`避免打开错误；  
   - 读文件：`ifstream`，`while (fin >> x)`读至EOF，`getline()`逐行读；  
   - 字符串流：`istringstream`拆分字符串，需`<sstream>`；  
   - 流格式化：`fixed`/`scientific`控制浮点数，`setw`/`setprecision`控制格式，需`<iomanip>`。

3. **递归**  
   - 必要条件：必须有基例（终止）和递归例（分解为子问题）；  
   - 示例掌握：阶乘、斐波那契、汉诺塔的递归实现；  
   - 注意事项：避免无限递归（栈溢出），理解递归与迭代的优缺点；  
   - 流程追踪：能手动追踪递归函数的调用顺序（如`factorial(5)`的执行步骤）。

4. **常见错误**  
   - 结构体：初始化成员顺序错误，函数传递时漏传结构体大小（数组结构体）；  
   - 文件I/O：忘记关闭文件，未检查`fail()`导致文件打开失败；  
   - 递归：无基例导致无限递归，递归参数未减小导致无法到达基例。