# COMP2113 Module 6 Arrays & Strings 备考笔记（完整版，基于《Module 6 - Guidance Notes.pdf》）
## 一、文档基础信息与学习目标
### 1.1 文档核心信息
- **课程关联**：COMP2113 Programming Technologies  
- **模块名称**：Module 6: Arrays & Strings  
- **预计学习时长**：3 Hours  
- **核心内容**：  
  1. **数组（Arrays）**：单维数组的声明/初始化、函数传递、搜索排序，二维数组的使用与函数参数；  
  2. **字符与字符数组（Char & Char Arrays）**：char类型的ASCII操作、字符处理函数、C字符串（null-terminated字符串）；  
  3. **C++字符串（C++ Strings）**：string类的初始化、拼接、比较、I/O，及常用成员函数（`length()`/`substr()`/`find()`等）；  
- **关键特色**：以“数据存储与处理”为核心，对比C风格字符数组与C++ string类的差异，强调实战场景（如搜索排序、字符串操作）。

### 1.2 学习目标（Learning Objectives）
- **英文原文**：掌握数组的声明与操作、字符数组的C字符串表示、C++ string类的成员函数，能实现数组搜索排序、字符串拼接/比较等任务。  
- **中文译文**：学完本章后，你应能：① 声明、初始化数组并处理元素（避免越界），实现数组的线性搜索与选择排序；② 利用ASCII码和`<cctype>`函数处理字符，理解C字符串的null终止特性；③ 使用C++ string类简化字符串操作，熟练调用常用成员函数。


## 二、Part I 数组（Arrays）
### 2.1 数组的核心概念
#### 2.1.1 什么是数组？
- **英文原文**：An array is a consecutive group of memory locations storing data of the **same type** (unlike Python lists/tuples which allow mixed types). It uses an index to access elements, starting from 0.  
- **中文译文**：数组是存储**同一类型**数据的连续内存空间（区别于Python列表/元组的混合类型），通过索引访问元素，索引从**0**开始。  
- 内存示意图（5个int的数组`score`）：  
  | 内存地址 | 1024-1027 | 1028-1031 | 1032-1035 | 1036-1039 | 1040-1043 |
  |----------|----------|----------|----------|----------|----------|
  | 数组元素 | `score[0]` | `score[1]` | `score[2]` | `score[3]` | `score[4]` |

#### 2.1.2 关键风险：数组越界（Index Out of Bound）
- **英文原文**：The compiler does NOT report errors for out-of-bound indexes. Accessing invalid indexes may modify other variables' memory, causing unexpected behavior.  
- **中文译文**：编译器不检查数组索引越界，访问无效索引会修改其他变量的内存，导致不可预测错误（如修改`average`变量的值）。  
- 示例（危险操作）：
  ```cpp
  int score[5];  // 索引0-4
  score[5] = 0;  // 越界！修改内存中score[4]之后的空间，可能影响其他变量
  ```

### 2.2 数组的声明与初始化
#### 2.2.1 声明语法
- **英文原文**：`base_type array_name[size];`，size为常量（数组大小固定，不可动态修改）。  
- **中文译文**：声明格式为“基础类型 数组名[大小];”，大小必须是常量（运行中不可改变）。  
- 示例：
  ```cpp
  int score[5];        // 5个int的数组（未初始化，值为垃圾值）
  double gpa[3];       // 3个double的数组
  char grade[8];       // 8个char的数组
  const int size = 10; // 常量指定大小（推荐，便于维护）
  int arr[size];       // 合法，size是常量
  ```

#### 2.2.2 初始化方式
##### 1. 列表初始化（Initializer List）
- **英文原文**：Use `{}` to initialize elements; size can be omitted (auto-detected). Fewer values → remaining elements initialized to 0.  
- **中文译文**：用`{}`初始化，可省略大小（自动匹配元素个数）；值不足时，剩余元素默认初始化为0。  
- 示例：
  ```cpp
  // 1. 指定大小，完整初始化
  int score[5] = {80, 100, 63, 84, 52};  // score[0]=80, score[1]=100,...

  // 2. 省略大小，自动匹配（size=3）
  int score[] = {80, 100, 52};  // score[0]=80, score[1]=100, score[2]=52

  // 3. 值不足，剩余为0
  int score[5] = {80, 100};  // score[0]=80, score[1]=100, score[2-4]=0

  // 4. 错误：值过多（编译器报错）
  int score[5] = {80, 100, 63, 84, 52, 96};  // 错误：元素数超过大小
  ```

##### 2. 循环初始化
- **英文原文**：Use a loop to assign values to each element (useful for dynamic initialization, e.g., user input).  
- **中文译文**：通过循环为每个元素赋值（适用于动态场景，如用户输入）。  
- 示例（初始化所有元素为0，再输入80个学生成绩）：
  ```cpp
  #include <iostream>
  using namespace std;

  int main() {
      const int STUDENT_NUM = 80;
      int score[STUDENT_NUM];

      // 1. 循环初始化所有元素为0
      for (int i = 0; i < STUDENT_NUM; ++i) {
          score[i] = 0;
      }

      // 2. 循环输入成绩
      cout << "Enter " << STUDENT_NUM << " scores:" << endl;
      for (int i = 0; i < STUDENT_NUM; ++i) {
          cin >> score[i];
      }

      return 0;
  }
  ```

#### 2.2.3 禁止操作：声明后整体赋值
- **英文原文**：It is illegal to assign an initializer list to an array after declaration (arrays are static).  
- **中文译文**：数组声明后，不能用`=`赋值整体列表（数组大小固定，不可动态修改）。  
- 错误示例：
  ```cpp
  int score[5];
  score = {80, 100, 63, 84, 52};  // 错误：声明后不能整体赋值
  score[] = {80, 100, 63, 84, 52};// 错误
  ```

### 2.3 数组与函数的传递
#### 2.3.1 传递数组元素（值传递/引用传递）
- **英文原文**：Array elements are passed like regular variables: pass-by-value (copy, no modification) or pass-by-reference (modify original).  
- **中文译文**：数组元素可像普通变量一样传递，支持值传递（复制值，不修改原元素）或引用传递（修改原元素）。  
- 示例：
  ```cpp
  #include <iostream>
  using namespace std;

  // 1. 值传递：不修改原元素
  int square_val(int x) {
      return x * x;
  }

  // 2. 引用传递：修改原元素
  void square_ref(int &x) {
      x *= x;
  }

  int main() {
      int a[4] = {0, 1, 2, 3};

      // 值传递：原数组不变
      for (int i = 0; i < 4; ++i) {
          a[i] = square_val(a[i]);  // a变为[0,1,4,9]
      }

      // 引用传递：直接修改原数组
      for (int i = 0; i < 4; ++i) {
          square_ref(a[i]);  // a变为[0,1,16,81]
      }

      return 0;
  }
  ```

#### 2.3.2 传递整个数组（类似引用传递）
- **英文原文**：An array parameter is passed like pass-by-reference: the function can modify the original array. The array name (no `[]`) is the argument; the function needs a size parameter (C++ doesn’t check bounds).  
- **中文译文**：传递整个数组时，函数参数本质是引用传递（可修改原数组）。实参仅需数组名（无`[]`），函数需额外传入数组大小（C++不检查数组边界）。  
- 语法与示例：
  ```cpp
  #include <iostream>
  using namespace std;

  // 函数声明：数组参数+大小参数（列数必须指定，单维数组可省略大小）
  void modifyArray(int arr[], int size);

  int main() {
      const int size = 5;
      int a[size] = {0, 1, 2, 3, 4};

      // 传递整个数组：实参仅数组名
      modifyArray(a, size);

      // 输出修改后的数组：[0,2,4,6,8]
      for (int i = 0; i < size; ++i) {
          cout << a[i] << " ";
      }
      return 0;
  }

  // 函数定义：数组参数arr[]接收整个数组
  void modifyArray(int arr[], int size) {
      // 修改原数组：每个元素×2
      for (int i = 0; i < size; ++i) {
          arr[i] *= 2;
      }
  }
  ```

### 2.4 数组的搜索与排序
#### 2.4.1 线性搜索（Linear Search）
- **英文原文**：Examine elements sequentially from first to last. Return index if found, -1 if not. Use `const` for the array parameter to prevent modification.  
- **中文译文**：从第一个元素到最后一个依次检查，找到则返回索引，未找到返回-1。数组参数加`const`防止意外修改。  
- 示例（找第一个匹配项）：
  ```cpp
  #include <iostream>
  using namespace std;

  // 线性搜索：const确保数组不被修改
  int linearSearch(const int arr[], int size, int key) {
      for (int i = 0; i < size; ++i) {
          if (arr[i] == key) {
              return i;  // 找到，返回索引
          }
      }
      return -1;  // 未找到
  }

  int main() {
      int a[9] = {-46, 7, 0, 23, 2048, -2, 1, 78, 99};
      int key = 78;
      int index = linearSearch(a, 9, key);

      if (index != -1) {
          cout << "Key " << key << " found at index " << index << endl;  // 输出7
      } else {
          cout << "Key not found" << endl;
      }
      return 0;
  }
  ```

##### 变种：找所有匹配项
- **英文原文**：Modify the function to start searching from a given position, call repeatedly until -1 is returned.  
- **中文译文**：修改函数，从指定位置开始搜索，重复调用直到返回-1，找到所有匹配项。  
- 示例：
  ```cpp
  // 从startPos开始搜索
  int linearSearchAll(const int arr[], int size, int key, int startPos) {
      for (int i = startPos; i < size; ++i) {
          if (arr[i] == key) {
              return i;
          }
      }
      return -1;
  }

  int main() {
      int a[9] = {-46, 78, 0, 23, 78, -2, 1, 78, 99};
      int key = 78;
      int currentPos = 0;
      int index;

      cout << "Key " << key << " found at indexes: ";
      while ((index = linearSearchAll(a, 9, key, currentPos)) != -1) {
          cout << index << " ";  // 输出3 7
          currentPos = index + 1;  // 下次从下一个位置开始
      }
      return 0;
  }
  ```

#### 2.4.2 选择排序（Selection Sort）
- **英文原文**：For each iteration i, find the smallest element in `arr[i..size-1]`, swap with `arr[i]`. After i iterations, `arr[0..i]` is sorted.  
- **中文译文**：每轮迭代i，找到`arr[i..size-1]`中的最小值，与`arr[i]`交换。i轮后，`arr[0..i]`已排序。  
- 示例（升序排序）：
  ```cpp
  #include <iostream>
  using namespace std;

  // 交换函数（引用传递）
  void swap(int &a, int &b) {
      int tmp = a;
      a = b;
      b = tmp;
  }

  // 选择排序：升序
  void selectionSort(int arr[], int size) {
      for (int i = 0; i < size; ++i) {
          int minIndex = i;  // 记录最小值索引
          // 找arr[i..size-1]的最小值
          for (int j = i + 1; j < size; ++j) {
              if (arr[j] < arr[minIndex]) {
                  minIndex = j;
              }
          }
          // 交换arr[i]和arr[minIndex]
          if (minIndex != i) {
              swap(arr[i], arr[minIndex]);
          }
      }
  }

  int main() {
      int a[6] = {-2, 7, 0, 23, 2048, -46};
      selectionSort(a, 6);

      // 输出排序后：-46 -2 0 7 23 2048
      for (int i = 0; i < 6; ++i) {
          cout << a[i] << " ";
      }
      return 0;
  }
  ```

### 2.5 二维数组（Two-Dimensional Arrays）
#### 2.5.1 声明与初始化
- **英文原文**：A 2D array is an array of arrays, declared as `base_type arr[rows][cols]`. Initialize by filling rows first; omit row size (auto-detected) but not column size.  
- **中文译文**：二维数组是“数组的数组”，声明格式`基础类型 数组名[行数][列数]`。初始化时按行填充，可省略行数（自动匹配），但**列数必须指定**。  
- 示例：
  ```cpp
  #include <iostream>
  using namespace std;

  int main() {
      // 1. 完整初始化（3行4列）
      int arr1[3][4] = {
          {1, 2, 3, 4},   // 第0行
          {5, 6, 7, 8},   // 第1行
          {9, 10, 11, 12} // 第2行
      };

      // 2. 省略行括号，按行填充
      int arr2[3][4] = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12};

      // 3. 省略行数（自动识别为2行3列）
      int arr3[][3] = {{1,2,3}, {4,5,6}};

      // 4. 部分初始化，剩余为0
      int arr4[2][3] = {{1}, {4,5}};  // arr4[0] = [1,0,0], arr4[1] = [4,5,0]

      return 0;
  }
  ```

#### 2.5.2 二维数组作为函数参数
- **英文原文**：When passing a 2D array to a function, the **column size must be specified** (row size can be omitted).  
- **中文译文**：二维数组作为函数参数时，**列数必须指定**（行数可省略），函数需传入行数。  
- 示例（打印二维数组）：
  ```cpp
  #include <iostream>
  using namespace std;

  // 函数声明：列数必须为5，行数省略
  void print2DArray(const int arr[][5], int rows);

  int main() {
      const int rows = 3, cols = 5;
      int arr[rows][cols] = {{0,1,2,3,4}, {5,6,7,8,9}, {10,11,12,13,14}};
      print2DArray(arr, rows);  // 传递数组名和行数
      return 0;
  }

  // 函数定义：列数固定为5
  void print2DArray(const int arr[][5], int rows) {
      for (int i = 0; i < rows; ++i) {  // 遍历行
          for (int j = 0; j < 5; ++j) {  // 遍历列
              cout << arr[i][j] << " ";
          }
          cout << endl;
      }
  }
  ```


## 三、Part II 字符与字符数组（Char & Char Arrays）
### 3.1 char类型与ASCII码
#### 3.1.1 char类型的本质
- **英文原文**：char occupies 1 byte, representing characters via ASCII codes (0-127). A char can be initialized with a character literal (e.g., 'a') or its ASCII value (e.g., 97).  
- **中文译文**：char类型占1字节，通过ASCII码（0-127）表示字符，可用字符常量（如'a'）或ASCII值（如97）初始化。  
- 示例（ASCII操作）：
  ```cpp
  #include <iostream>
  using namespace std;

  int main() {
      // 1. 字符初始化
      char c1 = 'A';    // ASCII 65
      char c2 = 98;     // ASCII 98 → 'b'
      cout << c1 << " " << c2 << endl;  // 输出A b

      // 2. 算术操作（基于ASCII码）
      cout << 'z' - 'a' << endl;  // 25（小写字母ASCII差）
      cout << c1 + 32 << endl;    // 97 → 'a'（大写转小写）

      // 3. 字符比较（基于ASCII码）
      if (c1 < c2) {
          cout << c1 << " is less than " << c2 << endl;  // 成立，A(65) < b(98)
      }

      return 0;
  }
  ```

#### 3.1.2 字符处理函数（<cctype>头文件）
- **英文原文**：<cctype> provides functions to check/convert characters (return non-zero for true, 0 for false).  
- **中文译文**：<cctype>头文件提供字符检查和转换函数（返回非0表示true，0表示false），常用函数如下：

| 函数（Function） | 功能（Description） | 示例 |
|------------------|--------------------|------|
| `isdigit(int c)`  | 检查是否为数字（0-9） | `isdigit('5')` → 非0 |
| `isalpha(int c)`  | 检查是否为字母（a-z/A-Z） | `isalpha('A')` → 非0 |
| `islower(int c)`  | 检查是否为小写字母 | `islower('b')` → 非0 |
| `isupper(int c)`  | 检查是否为大写字母 | `isupper('c')` → 0 |
| `tolower(int c)`  | 转换为小写（非字母不变） | `tolower('A')` → 'a' |
| `toupper(int c)`  | 转换为大写（非字母不变） | `toupper('b')` → 'B' |

- 示例：
  ```cpp
  #include <iostream>
  #include <cctype>  // 必须包含
  using namespace std;

  int main() {
      char c = '7';
      cout << c << (isdigit(c) ? " is " : " is not ") << "a digit" << endl;  // 是数字

      c = 'B';
      cout << c << (isalpha(c) ? " is " : " is not ") << "a letter" << endl;  // 是字母
      cout << (char)toupper('b') << endl;  // 转换为B

      return 0;
  }
  ```

### 3.2 C字符串（C-Strings / Null-Terminated Strings）
#### 3.2.1 核心定义
- **英文原文**：A C-string is a char array ending with the **null character ('\0')** (ASCII 0). It indicates the end of the string; without it, cout will print garbage.  
- **中文译文**：C字符串是**以'\0'（ASCII 0）结尾**的字符数组，'\0'标识字符串结束，缺少则输出乱码。  
- 内存示意图（字符串"John"）：  
  | 数组元素 | `name[0]` | `name[1]` | `name[2]` | `name[3]` | `name[4]` |
  |----------|-----------|-----------|-----------|-----------|-----------|
  | 内容     | 'J'       | 'o'       | 'h'       | 'n'       | '\0'      |
  | 说明     | 字符J     | 字符o     | 字符h     | 字符n     | 结束标识  |

#### 3.2.2 声明与初始化
- **英文原文**：Initialize with a string literal (auto-adds '\0') or explicit '\0'. The array size is **string length + 1** (for '\0').  
- **中文译文**：可用字符串常量（自动添加'\0'）或显式添加'\0'初始化，数组大小需为“字符串长度+1”（预留'\0'空间）。  
- 示例：
  ```cpp
  #include <iostream>
  using namespace std;

  int main() {
      // 1. 字符串常量初始化（auto-add '\0'，size=5）
      char name1[16] = "John";  // name1[4] = '\0'

      // 2. 显式初始化（需手动加'\0'）
      char name2[5] = {'J', 'o', 'h', 'n', '\0'};

      // 3. 省略大小（auto-detect size=5）
      char name3[] = "John";

      // 4. 错误：无'\0'，输出乱码
      char name4[4] = "John";  // 无空间存'\0'，cout输出John后接乱码

      cout << name1 << endl;  // 正常输出John
      cout << name4 << endl;  // 输出John+乱码

      return 0;
  }
  ```

#### 3.2.3 禁止操作：声明后整体赋值
- **英文原文**：Like regular arrays, a C-string cannot be assigned with `=` after declaration (must modify elements or use <cstring> functions like `strcpy()`).  
- **中文译文**：与普通数组类似，C字符串声明后不能用`=`整体赋值，需逐元素修改或用`<cstring>`函数（如`strcpy()`）。  
- 错误示例：
  ```cpp
  char name[16];
  name = "John";  // 错误：不能整体赋值
  name[] = "John";// 错误
  ```


## 四、Part III C++字符串（C++ Strings）
### 4.1 string类的基础使用
#### 4.1.1 核心优势
- **英文原文**：The C++ string class wraps char arrays, handling '\0' automatically. It supports assignment, concatenation, and comparison via simple operators.  
- **中文译文**：C++ string类封装了字符数组，自动处理'\0'，支持赋值、拼接、比较等简化操作，无需关注底层实现。  
- 前置要求：必须包含`<string>`头文件。

#### 4.1.2 初始化与赋值
- **英文原文**：A string object can be initialized with a C-string, string literal, or another string. Assignment with `=` is allowed after declaration.  
- **中文译文**：string对象可通过C字符串、字符串常量或其他string初始化，声明后可用`=`赋值。  
- 示例：
  ```cpp
  #include <iostream>
  #include <string>  // 必须包含
  using namespace std;

  int main() {
      // 1. 初始化
      string s1 = "Hello";          // 字符串常量
      string s2 = s1;               // 另一个string
      char c_str[] = "World";       // C字符串
      string s3 = c_str;            // C字符串

      // 2. 声明后赋值
      string s4;
      s4 = "C++";  // 合法：string支持赋值

      cout << s1 << " " << s2 << " " << s3 << " " << s4 << endl;  // Hello Hello World C++

      return 0;
  }
  ```

#### 4.1.3 字符串拼接（+）
- **英文原文**：Concatenate strings with `+`; at least one operand must be a string object (cannot concatenate two string literals directly).  
- **中文译文**：用`+`拼接字符串，**至少一个操作数必须是string对象**（不能直接拼接两个字符串常量）。  
- 示例：
  ```cpp
  #include <iostream>
  #include <string>
  using namespace std;

  int main() {
      string s1 = "I love ";
      string s2 = "cats";

      // 1. string + string
      string s3 = s1 + s2;  // "I love cats"

      // 2. string + 字面量
      string s4 = s1 + "dogs";  // "I love dogs"

      // 3. 字面量 + string
      string s5 = "I hate " + s2;  // "I hate cats"

      // 4. 错误：两个字面量直接加
      string s6 = "I love " + "dinosaurs";  // 错误

      cout << s3 << endl;  // I love cats
      return 0;
  }
  ```

#### 4.1.4 字符串比较（关系运算符）
- **英文原文**：Strings are compared lexicographically (dictionary order) with `==`, `!=`, `<`, `>`, etc. Comparison is case-sensitive.  
- **中文译文**：用`==`、`!=`、`<`、`>`等运算符按字典序比较字符串，区分大小写。  
- 示例：
  ```cpp
  #include <iostream>
  #include <string>
  using namespace std;

  int main() {
      string s1 = "Apple", s2 = "apple", s3 = "apples";

      cout << (s1 == s2) << endl;  // 0（false，A(65) < a(97)）
      cout << (s1 < s2) << endl;   // 1（true）
      cout << (s2 < s3) << endl;   // 1（true，s2长度短且前缀匹配）
      cout << (s3 != s1) << endl;  // 1（true）

      return 0;
  }
  ```

#### 4.1.5 字符串I/O
##### 1. cin/cout（默认分隔空格）
- **英文原文**：`cin >> string` ignores leading whitespace and stops at whitespace (cannot read strings with spaces). `cout << string` prints the string directly.  
- **中文译文**：`cin >> 字符串`忽略前导空格，遇到空格停止（无法读取含空格的字符串）；`cout << 字符串`直接打印。  
- 示例：
  ```cpp
  #include <iostream>
  #include <string>
  using namespace std;

  int main() {
      string s1, s2;
      cout << "Enter a sentence: ";
      cin >> s1 >> s2;  // 输入"I love C++"，s1="I"，s2="love"

      cout << "s1: " << s1 << ", s2: " << s2 << endl;  // s1:I, s2:love
      return 0;
  }
  ```

##### 2. getline（读取整行，含空格）
- **英文原文**：`getline(cin, string)` reads a line until newline (includes spaces). Use it to read entire lines.  
- **中文译文**：`getline(cin, 字符串)`读取整行（直到换行符），包含空格，用于读取完整句子。  
- 示例：
  ```cpp
  #include <iostream>
  #include <string>
  using namespace std;

  int main() {
      string s;
      cout << "Enter a sentence: ";
      getline(cin, s);  // 输入"I love C++"，s="I love C++"

      cout << "You entered: " << s << endl;  // 输出完整句子
      return 0;
  }
  ```

### 4.2 string类的常用成员函数
#### 4.2.1 基础函数（length()、empty()）
| 函数 | 功能 | 示例 |
|------|------|------|
| `length()` | 返回字符串长度（字符数，不含'\0'） | `string s="Hello"; s.length()` → 5 |
| `empty()` | 判断是否为空（空返回true，否则false） | `string s; s.empty()` → true |

- 示例：
  ```cpp
  string s1 = "Stay hungry, stay foolish";
  string s2 = "";

  cout << s1.length() << endl;  // 25
  cout << s2.empty() << endl;   // 1（true）
  ```

#### 4.2.2 子串与搜索（substr()、find()、rfind()）
| 函数 | 功能 | 示例 |
|------|------|------|
| `substr(pos, n)` | 返回从pos开始，长度为n的子串；n省略则取到末尾 | `s="Hello"; s.substr(1,3)` → "ell" |
| `find(str, pos)` | 从pos开始找str的**第一个**匹配，返回索引；未找到返回`string::npos`（-1） | `s="Hello World"; s.find("World")` → 6 |
| `rfind(str, pos)` | 从pos开始找str的**最后一个**匹配，返回索引；未找到返回`string::npos` | `s="Hello World"; s.rfind("l")` → 9 |

- 示例：
  ```cpp
  string s = "Outside it is cloudy and warm.";

  // substr：从6开始取6个字符 → "cloudy"
  cout << s.substr(6, 6) << endl;

  // find：找"is"的第一个位置 → 11
  cout << s.find("is") << endl;

  // rfind：找'l'的最后一个位置 → 14
  cout << s.rfind('l') << endl;

  // 未找到：返回string::npos
  if (s.find("the") == string::npos) {
      cout << "Not found" << endl;
  }
  ```

#### 4.2.3 修改函数（insert()、erase()、replace()）
| 函数 | 功能 | 示例 |
|------|------|------|
| `insert(pos, str)` | 在pos位置插入str | `s="Stay hungry"; s.insert(5, "very ")` → "Stay very hungry" |
| `erase(pos, n)` | 从pos开始删除n个字符；n省略则删到末尾 | `s="C++ programming."; s.erase(11,4)` → "C++ program." |
| `replace(pos, n, str)` | 从pos开始，用str替换n个字符 | `s="Stay hungry"; s.replace(5,5,"funny")` → "Stay funny" |

- 示例：
  ```cpp
  string s1 = "Cloudy and warm.";
  string s2 = "Angel is taking programming.";

  // insert：在10位置插入"very " → "Cloudy and very warm."
  cout << s1.insert(10, "very ") << endl;

  // replace：从0开始，用"Nelson"替换5个字符 → "Nelson is taking programming."
  cout << s2.replace(0, 5, "Nelson") << endl;
  ```


## 五、核心考点总结（Key Exam Focus）
1. **数组基础**  
   - 索引从0开始，越界不报错但危险；  
   - 初始化：列表初始化可省略大小，值不足则补0；声明后不能整体赋值；  
   - 传递：元素支持值/引用传递，整个数组传递需传入大小，本质是引用传递。

2. **数组操作**  
   - 线性搜索：顺序查找，返回索引或-1；变种可找所有匹配项；  
   - 选择排序：每轮找最小值交换，n轮排序n个元素；  
   - 二维数组：声明`arr[rows][cols]`，传递时列数必须指定。

3. **字符与C字符串**  
   - char类型：基于ASCII码操作，<cctype>函数（isdigit、tolower等）；  
   - C字符串：必须以'\0'结尾，数组大小=长度+1；声明后不能整体赋值，无'\0'输出乱码。

4. **C++ string类**  
   - 基础：包含`<string>`，支持赋值、拼接（至少一个string对象）、字典序比较；  
   - I/O：cin/cout分隔空格，getline读整行；  
   - 成员函数：`length()`/`empty()`（基础）、`substr()`/`find()`（子串搜索）、`insert()`/`erase()`/`replace()`（修改），未找到返回`string::npos`。

5. **关键区别**  
   - C字符串 vs C++ string：前者需手动处理'\0'，后者自动处理；前者不能赋值，后者支持`=`；  
   - 数组传递 vs 普通变量传递：数组传递本质是引用，普通变量默认值传递。