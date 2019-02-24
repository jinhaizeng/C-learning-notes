# C++学习笔记小结
## C++新特性
右值引用，其实现了转移语义以及精确传递。他的主要目的有两个方面：
1. 消除两个对象交互时不必要的对象拷贝，节省运算存储资源，提高效率
2. 能够更简洁明确地定义泛类函数
### 左值与右值的定义
C++和C中所有的表达式和变量要么是左值，要么是右值。
左值：非临时对象，可以在多条语句中使用的对象（所有的变量都满足这个定义）
右值：临时的对象，只在当前的语句中有效
### 左值和右值的语法符号
左值的声明符号为```&```，右值的声明符号为```&&```
示例程序1：
```c++
void process_value(int& i) { 
 std::cout << "LValue processed: " << i << std::endl; 
} 
 
void process_value(int&& i) { 
 std::cout << "RValue processed: " << i << std::endl; 
} 
 
int main() { 
 int a = 0; 
 process_value(a); 
 process_value(1); 
}
```
运行结果：
```C++
LValue processed: 0 
RValue processed: 1
```
Process_value 函数被重载，分别接受左值和右值。由输出结果可以看出，临时对象是作为右值处理的。
但是如果临时对象通过一个接受右值的函数传递给另一个函数时，就会变成左值，因为这个临时对象在传递过程中，变成了命名对象。
示例程序2：
```C++
void process_value(int& i) { 
 std::cout << "LValue processed: " << i << std::endl; 
} 
 
void process_value(int&& i) { 
 std::cout << "RValue processed: " << i << std::endl; 
} 
 
void forward_value(int&& i) { 
 process_value(i); 
} 
 
int main() { 
 int a = 0; 
 process_value(a); 
 process_value(1); 
 forward_value(2); 
}
```
运行结果：
```C++
LValue processed: 0 
RValue processed: 1 
LValue processed: 2
```
虽然 2 这个立即数在函数 forward_value 接收时是右值，但到了 process_value 接收时，变成了左值。