+++
author = "xinyu"
title = "C++的数据类型和运算和编译"
date = "2024-03-26"
description = "C++的数据类型和运算"
categories = [
    "运算","数据类型","编译"
]
tags = [
    "运算","C++","编译","数据类型"
]
image = "1.jpg"

+++

![](2.jpg)

# C++的数据类型

C++ 提供了多种数据类型，包括基本数据类型和复合数据类型。以下是一些常见的 C++ 数据类型：

1. **基本数据类型**：
   - **整数类型**：包括 `int`、`short`、`long`、`long long` 等，分别表示不同范围的整数值。
   
   - **无符号整数类型**：包括 `unsigned int`、`unsigned short`、`unsigned long`、`unsigned long long` 等，用于表示非负整数。
   
- **字符类型**：`char`，用于表示单个字符。
   
   - **浮点类型**：包括 `float`、`double`、`long double`，用于表示浮点数。
   
     ```c++
     /*整数部分*/
     int a; short b; long c; long long d; ......
     /*无符号整数*/
     unsigned int a; ......
     /*字符型*/
     char a;
     /*浮点*/
     float a; double b; long double c; ......
     ```
   
     
   
2. **复合数据类型**：
   
   - **数组**：用于存储相同类型的多个元素。
   
  ### 一维数组
   
     一维数组是最简单的数组形式，可以通过以下方式声明和初始化：
   
     ```cpp
     // 声明一个整型数组
     int numbers[5];
     
     // 初始化数组
     int numbers[5] = {1, 2, 3, 4, 5};
     ```
   
     ### 多维数组
   
     多维数组是包含多个维度的数组，例如二维数组。在 C++ 中，可以这样声明和初始化二维数组：
   
     ```cpp
     // 声明一个二维整型数组
     int matrix[3][3];
     
     // 初始化二维数组
     int matrix[3][3] = {{1, 2, 3}, {4, 5, 6}, {7, 8, 9}};
     ```
   
     ### 访问数组元素
   
     可以使用下标（索引）访问数组中的元素，数组下标从 0 开始：
   
     ```cpp
     int numbers[5] = {1, 2, 3, 4, 5};
     int x = numbers[2]; // 访问第三个元素，值为 3
     ```
   
     ### 数组长度
   
     C++ 中的数组长度在声明时确定，无法动态改变。可以使用 `sizeof` 操作符来获取数组的长度：
   
     ```cpp
     int numbers[5];
     int length = sizeof(numbers) / sizeof(numbers[0]); // 获取数组长度为 5
     ```
   
   - **结构体**：允许将不同类型的数据组合在一起。
   
     ### 定义结构体
   
     可以使用 `struct` 关键字定义结构体，并在其中声明成员变量：
   
     ```cpp
     // 定义一个表示学生的结构体
     struct Student {
         int studentID;
         char name[50];
         int age;
     };
     ```
   
     ### 创建结构体变量
   
     定义结构体后，可以使用该结构体创建变量：
   
     ```cpp
     // 创建一个名为 "john" 的学生结构体变量
     Student john;
     ```
   
     ### 访问结构体成员
   
     可以使用成员访问操作符 `.` 来访问结构体的成员变量：
   
     ```cpp
     john.studentID = 12345;
     strcpy(john.name, "John Doe");
     john.age = 20;
     ```
   
     ### 结构体作为函数参数
   
     结构体可以作为函数的参数传递，也可以作为函数的返回值：
   
     ```cpp
     void printStudentInfo(Student s) {
         cout << "ID: " << s.studentID << ", Name: " << s.name << ", Age: " << s.age << endl;
     }
     ```
   
     ### 结构体数组
   
     可以创建结构体数组来存储多个结构体变量：
   
     ```cpp
     Student students[5]; // 创建包含5个学生的数组
     students[0].studentID = 1;
     // 其他成员赋值...
     ```
   
   - **枚举**：用于定义具名的整数常量集合。
   
     ### 定义枚举
   
     可以使用 `enum` 关键字定义枚举类型，并列出枚举常量：
   
     ```cpp
     // 定义一个枚举类型
     enum Weekday {
         Monday = 1,// 默认为0 可以显式地为枚举常量指定特定的值 后续的枚举常量会依次递增
         Tuesday,  // 2
         Wednesday, // 3
         Thursday, 
         Friday,
         Saturday,
         Sunday
     };
     ```
   
     在这个例子中，`Weekday` 是枚举类型的名称，`Monday`、`Tuesday`等是枚举常量，它们被赋予默认的整数值，从0开始递增。
   
     ### 使用枚举
   
     可以使用枚举常量来声明变量：
   
     ```cpp
     Weekday today = Wednesday; //today = 3 因为 Wednesday 枚举中为3
     ```
   
     ### 指定枚举常量的值
   
     可以显式指定枚举常量的值：
   
     ```cpp
     enum Color {
         Red = 1,
         Green = 2,
         Blue = 4
     };
     ```
   
     在这个例子中，`Red`的值为1，`Green`的值为2，`Blue`的值为4。
   
     ### 枚举和整数之间的转换
   
     枚举类型可以隐式地转换为整数，也可以将整数强制转换为枚举类型：
   
     ```cpp
     int day = today; // 将枚举值转换为整数
     Weekday tomorrow = static_cast<Weekday>(day + 1); // 将整数转换为枚举值
     ```
   
     枚举类型在编程中经常用于表示一组相关的常量，例如表示颜色、星期几、状态等。通过使用枚举，可以使代码更易读、更易维护，同时避免使用魔术数字来表示常量。
   
3. **指针类型**：
   
- **指针**：用于存储变量的地址。
   
4. **其他类型**：
   
   - **布尔类型**：`bool`，表示逻辑值，只能取 `true` 或 `false`。
   
     ```cpp
     bool isCodingFun = true;
     cout << isCodingFun << endl; // true = 1  endl 等同于'\n'也就是换行符 
     bool isFishTasty = false;
     cout << isFishTasty; // false = 0
     ```
   
   - **空类型**：`void`，用于表示没有值的返回类型。
   
     ### 1. 函数返回类型
   
     空类型 `void` 通常用作函数的返回类型，表示该函数不返回任何值。例如：
   
     ```cpp
     void printMessage() {
         cout << "Hello, World!" << endl;
     }
     ```
   
     ### 2. 函数参数类型
   
     函数参数也可以是空类型，表示该函数不接受任何参数。例如：
   
     ```cpp
     void doSomething() {
         // 执行某些操作
     }
     ```
   
     ### 3. 指针类型
   
     空指针（`void*`）是一个特殊的指针类型，可以指向任何类型的数据。空指针通常用于在不确定指向的数据类型时使用。例如：
   
     ```cpp
     void* ptr;
     int num = 10;
     ptr = &num; // void* 指针指向整型变量
     ```
   
     ### 4. 函数指针
   
     空类型也可以用于函数指针，表示指向不返回任何值的函数。例如：
   
     ```cpp
     void (*funcPtr)(); // 函数指针，指向不返回任何值的函数
     
     void myFunction() {
         cout << "This is a function." << endl;
     }
     
     funcPtr = myFunction;
     funcPtr(); // 调用 myFunction
     ```

这些数据类型可以用于定义变量、函数参数和返回值，以及在编程中进行数据处理和存储。

# 与   或   异或   非   C++运算

### 逻辑运算符：

1. **与运算（AND）：`&&`**

   - 用于连接两个条件，只有当两个条件都为真时，整个表达式才为真。

   - 示例：`if (x > 0 && y < 10) { /* do something */ }`

     ```cpp
     int x = 5;
     int y = 3;
     if (x > 0 && y > 0) {
         // 如果 x 和 y 都大于 0，则执行以下代码
         std::cout << "Both x and y are greater than 0." << std::endl;
     }
     ```

2. **或运算（OR）：`||`**

   - 用于连接两个条件，只要其中一个条件为真，整个表达式就为真。

   - 示例：`if (x == 0 || y == 0) { /* do something */ }`

     ```cpp
     int x = 5;
     int y = 3;
     if (x > 0 || y > 0) {
         // 如果 x 或 y 中至少有一个大于 0，则执行以下代码
         std::cout << "At least one of x and y is greater than 0." << std::endl;
     }
     ```

3. **非运算（NOT）：`!`**

   - 用于对一个条件取反，如果条件为真，则取反后为假；如果条件为假，则取反后为真。

   - 示例：`if (!flag) { /* do something */ }`

     ```cpp
     bool isTrue = true;
     if (!isTrue) {
         // 如果 isTrue 为假，则执行以下代码
         std::cout << "isTrue is false." << std::endl;
     }
     ```

4. **逻辑异或运算符 ：`^`**

   - 逻辑异或运算符 `^` 用于对两个布尔表达式进行逻辑异或操作。

   - 当两个操作数一个为 `true` 一个为 `false` 时，逻辑异或的结果为 `true`；否则结果为 `false`。

   - 逻辑异或操作符 `^` 通常用于布尔类型的操作，用来判断两个条件是否不同。

   - 示例：

     ```cpp
     bool a = true;
     bool b = false;
     bool result = a ^ b; // result 的值为 true
     ```

### 位运算符：

1. **按位与（AND）：`&`**

   - 对两个数的每一位进行与操作，只有两个数对应位均为1时，结果位才为1。

   - 示例：`result = a & b;`

     ```cpp
     int x = 5;    // 二进制表示为 00000101
     int y = 3;    // 二进制表示为 00000011
     int result = x & y;  // 结果为 00000001，即 1
     ```

2. **按位或（OR）：`|`**

   - 对两个数的每一位进行或操作，只要两个数对应位有一个为1，结果位就为1。

   - 示例：`result = a | b;`

     ```cpp
     int x = 5;    // 二进制表示为 00000101
     int y = 3;    // 二进制表示为 00000011
     int result = x | y;  // 结果为 00000111，即 7
     ```

3. **按位异或（XOR）：`^`**

   - 对两个数的每一位进行异或操作，如果两个数对应位不同，结果位为1；相同则为0。

   - 示例：`result = x ^ y;`

     ```cpp
     int x = 5;    // 二进制表示为 00000101
     int y = 3;    // 二进制表示为 00000011
     int result = x ^ y;  // 结果为 00000110，即 6
     ```

4. **按位非（NOT）：`~`**

   - 对一个数的每一位取反，0 变为1，1 变为0。

   - 示例：`result = ~x;`

     ```cpp
     int x = 5;    // 二进制表示为 00000101
     int result = ~x;  // 结果为 11111010，即 -6（根据补码表示）
     ```

5. ### 位左移运算符 `<<`：

   - **语法**：`x << n`，其中 `x` 是要进行位左移的数，`n` 是要左移的位数。

   - **功能**：将 `x` 的二进制表示向左移动 `n` 位，右侧空出的位用0填充。

   - 示例：

     ```cpp
     int x = 5;    // 二进制表示为 00000101
     int result = x << 3;  // 左移3位，结果为 00101000，即 40
     ```

6. ### 位右移运算符 `>>`：

   - **语法**：`x >> n`，其中 `x` 是要进行位右移的数，`n` 是要右移的位数。

   - **功能**：将 `x` 的二进制表示向右移动 `n` 位，左侧空出的位根据符号位或补零规则进行填充。

   - 示例：

     ```cpp
     int x = 20;    // 二进制表示为 00010100
     int result = x >> 2;  // 右移2位，结果为 00000101，即 5
     ```

# 加减乘除取余自增自减

```cpp
#include <iostream>
using namespace std;
int main(){
	int x=5;
	int y= 2;
	
	cout << x + y << endl; // 7
	cout << x - y << endl; // 3
	cout << x * y << endl; // 10
	cout << x / y << endl; // 2
	cout << x % y << endl; // 1
	cout << x++ << endl; // 5
	cout << ++x << endl; // 7
	cout << y-- << endl; // 2
	cout << --y << endl; // 0
}
```

# 打开编译

```cpp
touch main.cpp
=============================================
#include <iostream> // C++ 标准库
using namespace std; // 简化C++代码 可省略 std::

int main()
{
    int a;
    cin >> a;
    cout << a << endl;
    return 0;
}
==============================================
在编译命令中添加 -lstdc++ 参数以链接标准 C++ 库，如下所示：
gcc main.cpp -o main -lstdc++
./main
```

# 字符串

```cpp
#include <iostream>
#include <string>
using namespace std; 

int main()
{
    string firstName = "John";
    string lastName = "Doe";
    string fullName = firstName + lastName;
    cout << fullName << endl; // JohnDoe
    
    fullName = firstName + " " + lastName;
    cout << fullName << endl; // John Doe
    
    fullName = firstName.append(lastName);
    cout << fullName << endl; // JohnDoe
    
    string txt = "ABCDEFGHIJKLMNOPORSTUVWXYZ"; // 看字符串长度
    cout << "The length of the txt string is:" << txt.length() << endl; 
    // The length of the txt string is:26
    cout << "The length of the txt string is:" << txt.size() << endl;
    // The length of the txt string is:26
    
    string myString = "Hello";
    cout << myString[0] << endl; // H

	mystring[0] = 'J';
    cout << myString << endl; // Jello
    
    return 0;
}
```

# 数学库

```cpp
#include <iostream>
#include <cmath>
```





























