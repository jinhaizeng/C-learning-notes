[TOC]
[markdown语法参考](https://www.zybuluo.com/EncyKe/note/120103#3-%E5%A4%96%E9%93%BE%E6%8E%A5)
# C++学习笔记小结
## C++新特性
[原文出处](https://www.ibm.com/developerworks/cn/aix/library/1307_lisl_c11/index.html)
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
### 转移语义的定义
右值引用是用来支持转移语义的。转移语义可以将资源 ( 堆，系统对象等 ) 从一个对象转移到另一个对象，这样能够减少不必要的临时对象的创建、拷贝以及销毁，能够大幅度提高 C++ 应用程序的性能。临时对象的维护 ( 创建和销毁 ) 对性能有严重影响。
转移语义是和拷贝语义相对的，可以类比文件的剪切与拷贝，当我们将文件从一个目录拷贝到另一个目录时，速度比剪切慢很多。
通过转移语义，临时对象中的资源能够转移其它的对象里。
在现有的 C++ 机制中，我们可以定义拷贝构造函数和赋值函数。要实现转移语义，需要定义转移构造函数，还可以定义转移赋值操作符。对于右值的拷贝和赋值会调用转移构造函数和转移赋值操作符。如果转移构造函数和转移拷贝操作符没有定义，那么就遵循现有的机制，拷贝构造函数和赋值操作符会被调用。
普通的函数和操作符也可以利用右值引用操作符实现转移语义。
### 实现转移构造函数和转移赋值函数
**这一块没有看懂，还要再搞一下，构造函数和转移函数这一块没有搞懂**

### 标准库函数std::move
move这一块也是有点乱

## [C++中std是什么意思？](https://blog.csdn.net/Calvin_zhou/article/details/78440145)