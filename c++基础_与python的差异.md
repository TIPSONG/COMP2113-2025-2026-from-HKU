# COMP2113A Python vs. C++ 语法差异备考笔记（基于《Python_vs_C++.pdf》）
## 一、文档基础信息
- **课程关联**：COMP2113A Programming Technologies  
- **授课单位**：The University of Hong Kong, School of Computing and Data Science  
- **核心内容**：Python（高级解释型语言）与C++（编译型语言）的核心语法差异对比  
- **文档关键范围**：聚焦🔶5-7至🔶5-58中“语法差异”相关内容，排除无关的HTML5、Java等语言及重复课程标识内容


## 二、核心语法差异对比（Core Syntax Differences）
### 1. 语言基础属性（Language Basics）
- **核心差异**：Python是高级解释型语言，语法简洁；C++是编译型语言，语法更复杂，常用于系统/软件开发。  
  - **Python**：High-level, interpreted language with simple syntax。  
  - **C++**：Compiled language with more complex syntax, often used for system/software development。


### 2. 变量声明（Variable Declaration）
- **核心差异**：Python无需指定数据类型，无语句结尾符号；C++必须指定数据类型，语句结尾需加半角分号（;）。  

#### Python 代码示例
```python
# 整数变量：直接赋值，无需声明类型
x = 10
# 字符串变量：同样无需指定类型
name = "Alice"
```
- 关键说明：声明时无需 indicate data type，解释器自动推断变量类型；赋值语句结尾无分号，与C++区分。

#### C++ 代码示例
```cpp
// 整数变量：必须显式声明int类型，语句结尾加;
int x = 10;
// 字符串变量：必须显式声明string类型，语句结尾加;
string name = "Alice";
```
- 关键说明：Must indicate data type during variable declaration，且每句结尾必须加 semi-colon (;)、、。


### 3. Python List/Tuple vs. C++ Array
- **核心差异**：Python的List（列表）和Tuple（元组）支持不同类型元素；C++的Array（数组）所有元素必须是同一类型。  

#### Python 代码示例
```python
# List（列表）：元素可混合整数、字符串
my_list = [1, 2, 'a']
# Tuple（元组）：元素可混合布尔值、整数、字符串
my_tuple = (True, 2, "Hello")
```
- 关键说明：Elements can be of different types，List用[]定义，Tuple用()定义，两者均支持多类型元素混合、、。

#### C++ 代码示例
```cpp
// 整数数组：所有元素必须是int类型，声明时指定长度3
int arr1[3] = {1, 2, 3};
// 字符数组：所有元素必须是char类型，声明时指定长度4
char arr2[4] = {'a', 'b', 'c', 'd'};
```
- 关键说明：Elements must be of the same type，数组声明时需指定“元素类型+长度”，初始化值需与类型匹配、、。


### 4. 代码块定义（Indentation vs. Braces）
- **核心差异**：Python用缩进（通常4个空格）定义代码块；C++用大括号（{}）定义代码块，缩进仅影响可读性。  

#### Python 代码示例
```python
# if语句代码块：冒号后用4个空格缩进
if x > 0:
    print("Positive")  # 缩进内属于if代码块
    print("Inside if block")  # 同属一个代码块
print("Outside if block")  # 无缩进，不属于if代码块
```
- 关键说明：Uses indentation to define code blocks，冒号（:）后需换行并统一缩进，缩进不一致会导致语法错误、。

#### C++ 代码示例
```cpp
#include <iostream>
using namespace std;

int main() {
    // if语句代码块：用{}包裹
    if (x > 0) {
        cout << "Positive" << endl;  // {}内属于if代码块
        cout << "Inside if block" << endl;
    }  // {}结束，代码块终止
    cout << "Outside if block" << endl;  // 不属于if代码块
    return 0;
}
```
- 关键说明：Uses braces {} to define blocks，代码块逻辑由{}决定，与缩进无关、。


### 5. 条件控制（Control Structure: if）
- **核心差异**：Python的if后接条件+冒号（:），无需括号；C++的if后接括号包裹的条件，代码块用{}。  

#### Python 代码示例
```python
x = 5
# 条件无括号，elif简化else if，冒号后缩进
if x > 0:
    print("Positive")
elif x == 0:
    print("Zero")
else:
    print("Negative")
```
- 关键说明：if条件无需括号，elif为“else if”的简化写法，分支逻辑通过缩进区分、、。

#### C++ 代码示例
```cpp
#include <iostream>
using namespace std;

int main() {
    int x = 5;
    // 条件必须用()包裹，else if连写，代码块用{}
    if (x > 0) {
        cout << "Positive" << endl;
    } else if (x == 0) {
        cout << "Zero" << endl;
    } else {
        cout << "Negative" << endl;
    }
    return 0;
}
```
- 关键说明：if条件需用()包裹，else if不可简写为“elif”，cout语句需加endl（可选）和分号、、。


### 6. 循环（Loop: for）
- **核心差异**：Python的for循环基于可迭代对象（如range()）；C++的for循环需显式声明“初始化+条件+更新”，用()包裹。  

#### Python 代码示例
```python
# 遍历0-4：range(5)生成0、1、2、3、4
for i in range(5):
    print(i)  # 输出结果：每行一个数字，从0到4
```
- 关键说明：基于range()生成的序列迭代，无需手动初始化或更新循环变量，解释器自动处理、、。

#### C++ 代码示例
```cpp
#include <iostream>
using namespace std;

int main() {
    // for循环结构：for(初始化; 条件; 更新)，用()包裹
    for (int i = 0; i < 5; i++) {
        cout << i << endl;  // 输出结果：每行一个数字，从0到4
    }
    return 0;
}
```
- 关键说明：需显式声明循环变量类型（如int），三部分用分号分隔，i++表示每次循环后自增1、、。


### 7. 循环（Loop: while）
- **核心差异**：Python的while后接条件+冒号（:）；C++的while后接括号包裹的条件，循环变量需显式声明类型。  

#### Python 代码示例
```python
# 初始化循环变量：无需声明类型
x = 0
# 条件无括号，冒号后缩进，手动更新变量
while x < 5:
    print(x)  # 输出结果：0、1、2、3、4
    x += 1  # 手动更新：x = x + 1，避免无限循环
```
- 关键说明：条件无需括号，需手动更新循环变量，否则会触发无限循环、、。

#### C++ 代码示例
```cpp
#include <iostream>
using namespace std;

int main() {
    // 初始化循环变量：必须声明int类型
    int x = 0;
    // 条件用()包裹，手动更新变量x++
    while (x < 5) {
        cout << x << endl;  // 输出结果：0、1、2、3、4
        x++;  // 等价于x = x + 1，手动更新变量
    }
    return 0;
}
```
- 关键说明：循环变量需显式声明类型，条件用()包裹，x++与x += 1功能一致、、。


### 8. 函数定义（Functions）
- **核心差异**：Python用def定义函数，无需指定返回类型和参数类型；C++需显式指定返回类型和参数类型。  

#### Python 代码示例
```python
# 定义加法函数：def开头，参数无类型，无需声明返回类型
def add(a, b):
    return a + b  # 自动返回结果，类型由传入值推断

# 调用函数：传入整数，返回整数
result = add(3, 5)
print(result)  # 输出结果：8
```
- 关键说明：用def关键字定义，函数名后接括号（参数无类型）和冒号，return后直接返回值、、。

#### C++ 代码示例
```cpp
#include <iostream>
using namespace std;

// 加法函数：显式指定返回类型int，参数类型int
int add(int a, int b) {
    return a + b;  // 返回int类型，语句结尾加;
}

// 无返回值函数：返回类型为void，无需return
void multiple(int a, int b) {
    cout << a * b << endl;  // 直接输出结果，语句结尾加;
}

int main() {
    int result = add(3, 5);
    cout << result << endl;  // 输出加法结果：8
    multiple(3, 5);          // 输出乘法结果：15
    return 0;
}
```
- 关键说明：函数定义格式为“返回类型 函数名(参数类型 参数)”，无返回值需指定void，参数类型必须与传入值一致、、。


### 9. 注释（Comments）
- **核心差异**：Python用#表示单行注释；C++用//表示单行注释，/* ... */表示多行注释。  

#### Python 代码示例
```python
# 这是单行注释：每行注释需加#
# 这是第二行单行注释，无多行注释专用语法
def add(a, b):
    return a + b  # 这是行尾注释：解释函数功能
```
- 关键说明：所有注释均以#开头，无多行注释专用语法，行尾注释需与代码间隔空格、、。

#### C++ 代码示例
```cpp
#include <iostream>
using namespace std;

// 这是单行注释：用//开头
/* 这是多行注释：
   用/*开头，*/结尾，
   可跨多行 */
int add(int a, int b) {
    return a + b;  // 这是行尾注释：解释返回值
}
```
- 关键说明：单行注释用//，多行注释用/* ... */包裹，多行注释不可嵌套、、、、、。


## 三、核心考点总结（Key Exam Focus）
1. **C++ 语法必注意项**  
   - 必须显式声明变量和函数参数的数据类型；  
   - 所有语句（变量声明、cout、return等）结尾必须加半角分号（;）；  
   - 条件（if）和循环（for/while）的表达式必须用括号（()）包裹；  
   - 代码块（函数、条件、循环内部）必须用大括号（{}）定义、、、、、、。

2. **Python 语法必注意项**  
   - 变量声明无需指定类型，解释器自动推断；  
   - 代码块必须用统一缩进（推荐4个空格）定义，冒号（:）后需换行缩进；  
   - 注释仅支持#，无/* ... */多行注释语法、、、。

3. **常见语法混淆规避**  
   - 勿将C++的{}用于Python代码块，也勿将Python的缩进逻辑用于C++；  
   - 勿遗漏C++的分号或Python的冒号，两者均会导致语法错误；  
   - 勿在Python函数中指定返回/参数类型，也勿在C++函数中省略类型声明。