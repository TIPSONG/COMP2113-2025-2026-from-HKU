# COMP2113 Module 8 Pointers, Dynamic Memory & Linked Lists 备考笔记
## 一、文档基础信息与前置要求
### 1.1 文档核心信息
- **课程关联**：COMP2113 Programming Technologies  
- **模块名称**：Module 8: Pointers, Dynamic Memory & Linked Lists  
- **预计学习时长**：3 Hours  
- **核心内容**：  
  1. **指针（Pointers）**：内存地址、指针变量操作、指针与数组/结构体、指针实现引用传递；  
  2. **动态内存管理（Dynamic Memory Management）**：动态变量/数组的创建（`new`）与销毁（`delete`）、内存泄漏避免、动态数组扩展；  
  3. **链表（Linked Lists）**：节点结构、链表构建（头插/尾插）、遍历/插入/删除操作、与数组的差异；  
- **关键限定**：聚焦C++11标准，编译命令沿用 `g++ -pedantic-errors -std=c++11 源文件.cpp`，核心考察指针操作、动态内存安全管理、链表基础操作。

### 1.2 前置要求
- 掌握结构体（Module 7）、数组（Module 6）基础；  
- 理解“内存地址”概念，明确“指针变量存储内存地址”的核心逻辑。


## 二、Part I 指针（Pointers）
### 2.1 核心概念：内存地址与指针本质
- **内存地址**：计算机内存是连续编号的字节单元，每个变量占用一段内存，其起始字节的编号即为变量的**内存地址**。  
- **指针变量**：专门存储“内存地址”的变量，通过指针可间接访问/修改其指向的内存内容。

### 2.2 关键运算符：&（取地址）与*（解引用）
#### 2.2.1 取地址运算符&
- **功能**：获取变量的内存地址，仅用于**表达式中**（声明中为引用运算符）。  
- 示例：
  ```cpp
  #include <iostream>
  using namespace std;

  int main() {
      int x = 10;
      // &x：获取x的内存地址，输出为十六进制（如0x7ffeefbff4ac）
      cout << "Address of x: " << &x << endl;  
      return 0;
  }
  ```
- 注意：`&`的双重含义——表达式中是“取地址”，声明中是“引用”（如`void swap(int &a, int &b)`）。

#### 2.2.2 指针变量声明与初始化
- **语法**：`数据类型 * 指针名 = 地址值;`，指针类型必须与指向变量的类型匹配。  
- 示例：
  ```cpp
  #include <iostream>
  using namespace std;

  int main() {
      int x = 10;
      // 声明int型指针，指向x的地址
      int *p = &x;  
      // 错误：指针类型与变量类型不匹配（char*不能指向int变量）
      char *cp = &x;  

      // 同时声明多个指针：每个变量前都需加*
      int *p1, *p2;  
      return 0;
  }
  ```

#### 2.2.3 解引用运算符*
- **功能**：通过指针地址，访问/修改其指向的内存内容，`*指针名` 等价于“指针指向的变量”。  
- 示例（访问与修改）：
  ```cpp
  #include <iostream>
  using namespace std;

  int main() {
      int x = 10;
      int *p = &x;

      // 访问：*p等价于x，输出10
      cout << *p << endl;  
      // 修改：通过p修改x的值，x变为20
      *p = 20;  
      cout << x << endl;  // 输出20

      // 优先级注意：++优先级高于*，需加括号
      (*p)++;  // x变为21（正确：先解引用，再自增）
      *p++;    // 错误：先自增指针地址，再解引用（可能越界）
      return 0;
  }
  ```

### 2.3 结构体/类指针的成员访问（->运算符）
- **场景**：指针指向结构体/类时，用`->`访问成员（替代`(*指针).成员`，简化语法）。  
- 示例：
  ```cpp
  #include <iostream>
  #include <string>
  using namespace std;

  struct Student {
      string name;
      int id;
  };

  int main() {
      Student s = {"Alice", 3035123456};
      Student *sp = &s;

      // 三种等价访问方式
      cout << s.name << endl;        // 直接访问（结构体变量）
      cout << (*sp).name << endl;    // 解引用后访问
      cout << sp->name << endl;      // 指针专用访问（推荐）

      sp->id = 3035123457;  // 修改成员值
      cout << sp->id << endl;  // 输出3035123457
      return 0;
  }
  ```

### 2.4 危险指针：悬空指针与空指针
#### 2.4.1 悬空指针（Dangling Pointer）
- **定义**：未初始化、指向已释放内存或无效地址的指针，解引用会导致程序崩溃或不可预测行为。  
- 示例（错误）：
  ```cpp
  // 错误1：未初始化指针（悬空指针）
  int *dangling_ptr;
  cout << *dangling_ptr << endl;  // 危险：访问无效内存

  // 错误2：指向已释放内存（悬空指针）
  int *p = new int(10);
  delete p;  // 释放p指向的内存
  cout << *p << endl;  // 危险：p变为悬空指针
  ```

#### 2.4.2 空指针（Null Pointer）
- **定义**：用`nullptr`（C++11）或`0`初始化的指针，明确指向“无有效地址”，解引用会崩溃，但可通过判断避免错误。  
- 示例（正确）：
  ```cpp
  #include <iostream>
  using namespace std;

  int main() {
      // 空指针：指向无有效地址
      int *null_ptr = nullptr;  

      // 安全使用：先判断是否为空
      if (null_ptr != nullptr) {
          cout << *null_ptr << endl;
      } else {
          cout << "Pointer is null." << endl;  // 输出此句
      }
      return 0;
  }
  ```

### 2.5 指针与数组的关系
- **核心结论**：**数组名是指向数组首元素的常量指针**（不可修改地址），指针可像数组名一样访问数组元素。  
- 示例：
  ```cpp
  #include <iostream>
  using namespace std;

  int main() {
      int arr[5] = {0, 2, 4, 6, 8};
      // p指向arr[0]（数组名arr等价于&arr[0]）
      int *p = arr;  

      // 四种等价访问arr[2]的方式
      cout << arr[2] << endl;    // 数组方式：4
      cout << p[2] << endl;      // 指针数组方式：4
      cout << *(arr + 2) << endl;// 数组指针运算：4
      cout << *(p + 2) << endl;  // 指针运算：4

      // 错误：数组名是常量指针，不能修改地址
      arr = p + 1;  
      return 0;
  }
  ```

### 2.6 指针实现引用传递（函数参数）
- **功能**：通过传递变量地址，使函数间接修改实参（替代引用参数，效果一致）。  
- 示例（swap函数）：
  ```cpp
  #include <iostream>
  using namespace std;

  // 指针参数：接收实参的地址
  void swap(int *a, int *b) {
      int temp = *a;
      // 解引用修改实参值
      *a = *b;  
      *b = temp;
  }

  int main() {
      int x = 2, y = 3;
      // 传递x和y的地址
      swap(&x, &y);  
      cout << x << " " << y << endl;  // 输出3 2
      return 0;
  }
  ```


## 三、Part II 动态内存管理（Dynamic Memory Management）
### 3.1 静态变量vs动态变量
| 特性                | 静态变量（如`int x;`） | 动态变量（如`int *p = new int;`） |
|---------------------|------------------------|----------------------------------|
| 创建时机            | 编译时分配内存         | 运行时分配内存                  |
| 生命周期            | 由作用域决定（出域销毁） | 手动销毁前一直存在              |
| 访问方式            | 直接通过变量名         | 通过指针间接访问                |
| 数量灵活性          | 固定（编译时确定）     | 灵活（运行时动态调整）          |

### 3.2 动态变量操作：new与delete
#### 3.2.1 创建动态变量（new）
- **语法**：`指针 = new 数据类型(初始化值);`，内存分配在“堆区”，需手动释放。  
- 示例：
  ```cpp
  #include <iostream>
  using namespace std;

  int main() {
      // 创建int型动态变量，初始值为42
      int *p = new int(42);  
      // 创建string型动态变量，初始值为"hello"
      string *sp = new string("hello");  

      cout << *p << endl;    // 输出42
      cout << *sp << endl;   // 输出hello
      return 0;
  }
  ```

#### 3.2.2 销毁动态变量（delete）
- **语法**：`delete 指针;`，释放堆区内存，避免内存泄漏（堆区内存不会自动回收）。  
- 示例（正确配对）：
  ```cpp
  #include <iostream>
  using namespace std;

  int main() {
      int *p = new int(42);
      delete p;  // 释放p指向的内存
      p = nullptr;  // 重置为nullptr，避免悬空指针

      // 错误1：重复删除（崩溃）
      delete p;  
      // 错误2：未删除（内存泄漏）
      int *q = new int(10);  
      return 0;
  }
  ```

### 3.3 动态数组：new[]与delete[]
- **场景**：运行时确定数组大小（如用户输入长度），语法为`指针 = new 数据类型[长度];`。  
- 示例：
  ```cpp
  #include <iostream>
  using namespace std;

  int main() {
      int n;
      cout << "Enter array size: ";
      cin >> n;

      // 创建int型动态数组，长度为n（运行时确定）
      int *arr = new int[n];  

      // 赋值
      for (int i = 0; i < n; ++i) {
          arr[i] = i * 2;
      }

      // 输出
      for (int i = 0; i < n; ++i) {
          cout << arr[i] << " ";
      }

      // 销毁动态数组：必须用delete[]（不能用delete）
      delete[] arr;  
      arr = nullptr;
      return 0;
  }
  ```

### 3.4 动态数组扩展（核心实战：phonebook示例）
- **场景**：动态数组容量不足时，扩展容量（核心步骤：创建新数组→拷贝旧数据→释放旧数组→更新指针和大小）。  
- 示例（grow_phonebook函数实现）：
  ```cpp
  #include <iostream>
  using namespace std;

  // 假设PhoneRec是存储电话记录的结构体
  struct PhoneRec {
      string name;
      string phone;
  };

  // 扩展动态数组：pb为原数组指针（引用传递），pb_size为原大小（引用传递），n为扩展增量
  void grow_phonebook(PhoneRec *&pb, int &pb_size, int n) {
      // 步骤1：创建新动态数组，大小=原大小+增量
      PhoneRec *new_pb = new PhoneRec[pb_size + n];

      // 步骤2：拷贝旧数组数据到新数组
      for (int i = 0; i < pb_size; ++i) {
          new_pb[i] = pb[i];
      }

      // 步骤3：释放旧数组内存
      delete[] pb;

      // 步骤4：更新指针和大小
      pb = new_pb;
      pb_size += n;
  }

  int main() {
      int size = 3;
      PhoneRec *phonebook = new PhoneRec[size];

      // 扩展数组：容量从3增加到6
      grow_phonebook(phonebook, size, 3);  
      cout << "New size: " << size << endl;  // 输出6

      delete[] phonebook;
      return 0;
  }
  ```

### 3.5 常见动态内存错误
1. **内存泄漏**：未用`delete`释放动态内存，堆区内存耗尽（最常见错误）；  
2. **重复删除**：对同一指针多次调用`delete`，导致程序崩溃；  
3. **悬空指针**：删除指针后未重置为`nullptr`，后续误操作其指向的无效内存；  
4. **动态数组用delete销毁**：用`delete`替代`delete[]`，导致部分内存未释放。


## 四、Part III 链表（Linked Lists）
### 4.1 数组vs链表：数据访问模式对比
| 特性                | 数组（Array）          | 链表（Linked List）              |
|---------------------|------------------------|----------------------------------|
| 内存存储            | 连续内存单元           | 非连续内存单元（节点通过指针连接） |
| 访问方式            | 随机访问（直接通过索引） | 顺序访问（从表头遍历）            |
| 插入/删除效率       | 低（需移动大量元素）    | 高（仅修改指针，无需移动元素）     |
| 空间效率            | 无额外开销             | 需额外存储指针（节点开销）        |
| 适用场景            | 频繁访问、少量插入删除  | 频繁插入删除、访问顺序化          |

### 4.2 链表节点结构（核心基础）
- **定义**：用结构体封装“数据”和“下一个节点的指针”，构成链表的基本单元。  
- 示例：
  ```cpp
  #include <iostream>
  using namespace std;

  // 链表节点结构体：存储int数据和下一个节点指针
  struct Node {
      int info;  // 数据域
      Node *next; // 指针域：指向后一个Node
  };

  // 链表由表头指针标识（指向第一个节点，空链表为nullptr）
  Node *head = nullptr;
  ```

### 4.3 链表核心操作
#### 4.3.1 遍历链表（Traversal）
- **逻辑**：从表头`head`开始，用临时指针`current`依次访问每个节点，直到`current == nullptr`（表尾）。  
- 示例：
  ```cpp
  void print_list(Node *head) {
      // 临时指针：避免修改head（表头不能动）
      Node *current = head;  
      while (current != nullptr) {
          // 访问当前节点数据
          cout << current->info << " -> ";  
          // 移动到下一个节点
          current = current->next;  
      }
      cout << "NULL" << endl;
  }

  // 调用示例：print_list(head);
  ```

#### 4.3.2 构建链表（头插法vs尾插法）
##### 1. 头插法（Head Insertion）
- **逻辑**：新节点插入到表头，成为新的`head`，操作高效（O(1)），但输入顺序与链表顺序相反。  
- 示例：
  ```cpp
  // 头插：将num插入链表头部
  void head_insert(Node *&head, int num) {
      // 步骤1：创建新节点
      Node *new_node = new Node;
      new_node->info = num;
      // 步骤2：新节点指向原表头
      new_node->next = head;  
      // 步骤3：更新表头为新节点
      head = new_node;  
  }

  // 调用示例：输入23、56、14，链表为14->56->23->NULL
  int main() {
      Node *head = nullptr;
      head_insert(head, 23);
      head_insert(head, 56);
      head_insert(head, 14);
      print_list(head);  // 输出14 -> 56 -> 23 -> NULL
      return 0;
  }
  ```

##### 2. 尾插法（Tail Insertion）
- **逻辑**：新节点插入到表尾，需维护`tail`指针（指向最后一个节点），输入顺序与链表顺序一致（O(1)）。  
- 示例：
  ```cpp
  // 尾插：将num插入链表尾部
  void tail_insert(Node *&head, Node *&tail, int num) {
      // 步骤1：创建新节点（表尾节点next为nullptr）
      Node *new_node = new Node;
      new_node->info = num;
      new_node->next = nullptr;

      // 步骤2：空链表时，head和tail都指向新节点
      if (head == nullptr) {
          head = new_node;
          tail = new_node;
      } else {
          // 步骤3：非空链表，tail->next指向新节点，更新tail
          tail->next = new_node;
          tail = new_node;
      }
  }

  // 调用示例：输入23、56、14，链表为23->56->14->NULL
  int main() {
      Node *head = nullptr, *tail = nullptr;
      tail_insert(head, tail, 23);
      tail_insert(head, tail, 56);
      tail_insert(head, tail, 14);
      print_list(head);  // 输出23 -> 56 -> 14 -> NULL
      return 0;
  }
  ```

#### 4.3.3 插入节点（指定位置）
- **逻辑**：找到插入位置的前驱节点`after`，新节点指向`after->next`，`after->next`指向新节点。  
- 示例：
  ```cpp
  // 在after节点后插入num（假设after非nullptr）
  void insert_after(Node *after, int num) {
      // 步骤1：创建新节点
      Node *new_node = new Node;
      new_node->info = num;
      // 步骤2：新节点指向after的下一个节点
      new_node->next = after->next;  
      // 步骤3：after指向新节点
      after->next = new_node;  
  }

  // 调用示例：在值为56的节点后插入38
  Node *find(Node *head, int target) {
      Node *current = head;
      while (current != nullptr) {
          if (current->info == target) {
              return current;  // 找到目标节点
          }
          current = current->next;
      }
      return nullptr;  // 未找到
  }

  // 主函数中调用：
  Node *after = find(head, 56);
  if (after != nullptr) {
      insert_after(after, 38);  // 链表变为23->56->38->14->NULL
  }
  ```

#### 4.3.4 删除节点（指定位置）
- **逻辑**：找到前驱节点`after`，用临时指针`temp`指向待删除节点，`after->next`跳过待删除节点，`delete temp`释放内存。  
- 示例（删除after后的节点）：
  ```cpp
  // 删除after节点后的节点（假设after非nullptr且after->next非nullptr）
  void delete_after(Node *after) {
      // 步骤1：temp指向待删除节点
      Node *temp = after->next;  
      // 步骤2：after跳过待删除节点
      after->next = temp->next;  
      // 步骤3：释放待删除节点内存
      delete temp;  
  }

  // 调用示例：删除56后的38节点
  Node *after = find(head, 56);
  if (after != nullptr && after->next != nullptr) {
      delete_after(after);  // 链表恢复为23->56->14->NULL
  }
  ```

#### 4.3.5 销毁链表（避免内存泄漏）
- **逻辑**：迭代删除表头节点，直到`head == nullptr`。  
- 示例：
  ```cpp
  void delete_list(Node *&head) {
      while (head != nullptr) {
          // 临时指针指向当前表头
          Node *temp = head;  
          // 表头移动到下一个节点
          head = head->next;  
          // 释放当前表头内存
          delete temp;  
      }
  }

  // 调用示例：程序结束前销毁链表
  int main() {
      Node *head = nullptr;
      // ... 构建链表 ...
      delete_list(head);  // 释放所有节点内存
      return 0;
  }
  ```


## 五、核心考点总结（Key Exam Focus）
1. **指针基础**  
   - `&`（取地址）与`*`（解引用）的双重含义，指针声明时的类型匹配；  
   - 结构体/类指针的成员访问（`->`运算符），解引用与自增/自减的优先级（需加括号`(*p)++`）；  
   - 悬空指针与空指针的区别，空指针的安全使用（先判断再解引用）。

2. **动态内存管理**  
   - `new`/`delete`（动态变量）、`new[]`/`delete[]`（动态数组）的配对使用，避免内存泄漏；  
   - 动态数组扩展的核心步骤（创建新数组→拷贝→释放旧数组→更新指针）；  
   - 常见错误：重复删除、悬空指针、动态数组用`delete`销毁。

3. **链表操作**  
   - 节点结构体定义（数据域+指针域），表头`head`的作用（不可修改）；  
   - 遍历链表的逻辑（临时指针`current`，终止条件`current == nullptr`）；  
   - 构建链表（头插法/尾插法）、插入/删除节点的步骤，销毁链表的必要性；  
   - 数组与链表的优缺点对比（访问方式、插入删除效率、空间开销）。

4. **实战应用**  
   - 指针实现引用传递（如`swap`函数）；  
   - 动态数组扩展（`grow_phonebook`函数）；  
   - 链表的排序/反转（核心思路：拆分为单个节点，重建有序链表/反转指针方向）。
