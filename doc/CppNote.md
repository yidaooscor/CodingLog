<!--
 * @Author: YichaoZhao
 * @Date: 2021-03-22 17:24:54
 * @LastEditTime: 2021-03-23 16:00:34
 * @LastEditors: YichaoZhao
 * @Description: 
 * @FilePath: /CodingLog/CppNote.md
-->

# CppNote

1. 继承、虚继承、菱形继承
2. 虚函数表
3. 指针在16位机、32位机、64位机分别占用多少个字节 ： 16位机--2字节、32位机--4字节、64位机--8字节。
4. 引用一个全局变量： 如果在本文中，则直接使用。如果不在，则需要引用其头文件或者使用关键字 extern
5. 在main函数执行之前，会执行全局函数的构造

## Union
一种奇特的结构，它的长度为其之中最长的单位的长度。
这里所谓的共享不是指把多个成员同时装入一个联合变量内， 而是指该联合变量可被赋予任一成员值，但每次只能赋一种值， 赋入新值则冲去旧值。

### 类的成员函数重载，覆盖和隐藏
重载：
覆盖：父类函数中的完全相同的函数被子类覆盖
隐藏：父类中的同名（不一定是相同的参数和返回值）

### static
改关键字可以解决函数执行完后变量值被释放的问题。

### 内存泄露
内存泄露指的是程序中动态分配了内存，但是在程序结束时没有释放这部分内存，从而造成那一部分内存不可以的情况。
检测内存泄露的方法：
1. 看代码new之后是否有delete
2. 让程序长时间运行，看任务管理器对应程序内存是不是一直向上增长。
3. 使用常用内存泄露检测工具检测

### 各种指针

* 野指针
野指针只想一个已删除的对象或无意义地址的指针。野指针不好判断,人们一般不会用错空指针,但是容易判断错误野指针.

* 悬垂指针

请看如下程序

    int *p=NULL;
    void fun()
    {int i=10;p= & i;}
    void main()
    {
        fun();
    
        cout<<"第一次：*p = "<<*p<<endl;
        cout<<"第二次：*p = "<<*p<<endl;
    
    }

p被销毁了，所以出现了垂悬指针

解决方法：引入智能指针。


* 哑指针：
传统的C/C++指针


### const
* 常量指针和指针常量
常量指针：是一个指向常量的指针
指针常量：是一个不能修改所指地址的指针

* const char * ,char const *, char * const的区别
char const * cp // cp是一个常指针，指向char的数据类型
const char * co  //cp是一个指向char类型常量的指针



作用：
1. 可以用于全局变量的定义：为该变量分配静态存储区。程序运行结束前不会被释放。
2. 声明和定义静态成员函数：表示该函数为静态函数，只能在本文件中被调用。 
3. 定义静态局部变量：只被初始化一次，只有程序运行结束才会释放。区别是作用域的范围。


### 虚函数，纯虚函数

为什么基类析构函数是虚函数？
编译器总是根据类型来调用成员函数。但是一个派生类的指针可以安全地转换为一个基类的指针。这样删除一个基类的指针的时候，C++不管这个类是基类还是派生类，调用的都是基类的析构函数而不是派生类的。如果你以来与派生类的析构函数的代码来释放资源，而没有重载析构函数，那么会出现资源泄露。

构造函数不可以为虚函数：
虚函数采用一种虚调用的方法。虚调用是一种可以在只有部分信息的情况下工作的机制。如果创建一个对象，则需要知道对象的准确类型，因此构造函数不能为虚函数。

虚函数是有开销的

* 虚函数表
虚函数是通过一张虚函数表来实现的.就像一个地图一样,指明了实际所应该调用的函数地址.
这个我们着重看一下这张虚函数表. C++的编译器保证了虚函数表的指针存在于对象实例中最前面的位置.因此,我们可以通过对象实例的地址得到这张虚函数表,然后通过遍历其中函数指针,并调用相应的函数.

### 面向对象的三大基本特性
1. 封装：将客观失误抽象成类
2. 继承：子类继承父类的方法和属性
3. 多态：允许将子类类型的指针赋值给父类类型的指针。

### 引用和指针
二者有什么区别，为什么引用比指针要安全：
1. 引用在创建的时候必须初始化，保证引用的对象是有效的，所以不存在null引用，而指针在定义的时候不必初始化，所以，指针则可以是null。


### 拷贝构造函数
1. 深拷贝、 浅拷贝
该问题发生在类中有指针和引用的情况下，浅拷贝会将类中的指针复制一份，导致指针在两个中相同，这样运行析构函数的时候必然会有问题

### C++类

* C++空类默认成员函数
默认构造函数, 析构函数, 复制构造函数, 赋值函数




### 其他
1. C++不是类型安全的函数
2. 有哪几种情况只能用构造函数初始化列表而不能用赋值初始化? const成员, 引用成员.