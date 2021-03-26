<!--
 * @Author: YichaoZhao
 * @Date: 2021-03-04 10:06:42
 * @LastEditTime: 2021-03-25 09:58:20
 * @LastEditors: YichaoZhao
 * @Description: In User Settings Edit#
 * @FilePath: /CodingLog/doc/MyCodingLog.md
-->

# 前言
我发誓，从今天开始写日志 2021-03-04

## 2021-03-04

### std::bind()
返回一个function object基于fn
    
    bind (Fn&& fn, Args&&... args);;

上个例子
    
    // bind example
    #include <iostream>     // std::cout
    #include <functional>   // std::bind

    // a function: (also works with function object: std::divides<double> my_divide;)
    double my_divide (double x, double y) {return x/y;}

    struct MyPair {
    double a,b;
    double multiply() {return a*b;}
    };

    int main () {
    using namespace std::placeholders;    // adds visibility of _1, _2, _3,...

    // binding functions:
    auto fn_five = std::bind (my_divide,10,2);               // returns 10/2
    std::cout << fn_five() << '\n';                          // 5

    auto fn_half = std::bind (my_divide,_1,2);               // returns x/2
    std::cout << fn_half(10) << '\n';                        // 5

    auto fn_invert = std::bind (my_divide,_2,_1);            // returns y/x
    std::cout << fn_invert(10,2) << '\n';                    // 0.2

    auto fn_rounding = std::bind<int> (my_divide,_1,_2);     // returns int(x/y)
    std::cout << fn_rounding(10,3) << '\n';                  // 3

    MyPair ten_two {10,2};

    // binding members:
    auto bound_member_fn = std::bind (&MyPair::multiply,_1); // returns x.multiply()
    std::cout << bound_member_fn(ten_two) << '\n';           // 20

    auto bound_member_data = std::bind (&MyPair::a,ten_two); // returns ten_two.a
    std::cout << bound_member_data() << '\n';                // 10

    return 0;
    }

    //
    auto f3 = std::bind (&MyPair::multiply, this);
    auto f3 = std::bind (&MyPair::multiply, &ten_two)

注意: 如果绑定类内的函数的话,那么需要指定具体实例化的类,在bind参数的第二个,可用具体的类名,也可以用this.


### 线程池
C++11 加入了线程库，从此告别了标准库不支持并发的历史。然而 c++ 对于多线程的支持还是比较低级，稍微高级一点的用法都需要自己去实现，譬如线程池、信号量等。线程池(thread pool)这个东西，在面试上多次被问到，一般的回答都是：“管理一个任务队列，一个线程队列，然后每次取一个任务分配给一个线程去做，循环往复。” 貌似没有问题吧。但是写起程序来的时候就出问题了。

//https://www.cnblogs.com/lzpong/p/6397997.html#autoid-0-0-0
//或https://github.com/lzpong/threadpool

**实现原理**
接着前面的废话说。“管理一个任务队列，一个线程队列，然后每次取一个任务分配给一个线程去做，循环往复。” 这个思路有神马问题？线程池一般要复用线程，所以如果是取一个 task 分配给某一个 thread，执行完之后再重新分配，在语言层面基本都是不支持的：一般语言的 thread 都是执行一个固定的 task 函数，执行完毕线程也就结束了(至少 c++ 是这样)。so 要如何实现 task 和 thread 的分配呢？

让每一个 thread 都去执行调度函数：循环获取一个 task，然后执行之。
idea 是不是很赞！保证了 thread 函数的唯一性，而且复用线程执行 task 。

### gtest(GoogleTest)

GoogleTest可以帮助我们更好地编写测试用例.

[Gtest](./Gtest.md)


## 2021-03-05

### 调试dashboard 的 gtest部分
因为没有修改配置文件中的ip导致无法生成coverage.info.具体原因待定.

[Gcov&Lcov](./Gcov&Lcov.md)


## 2021-03-06

周六休息

## 2021-03-07

周日休息

## 2021-03-08

今天学习一下Cmake

[Cmake](./Cmake.md)

下午5点 Gtest分享会
收获不大

## 2021-03-09

今日对HMI模块添加GoogleTest。

问题1： 将main函数放入了namespace中，会报错

## 2021-03-10

今日继续对HMI模块添加GoogleTest

今日使用scp远程传输文件,格式如下:

    scp <Sourcefile> <Targetfile>
    scp /media/fyy/143G/fyy/123 zyc@10.15.67.81:/home/zyc/Mtask/DashBoardForSubmit/

今日调试Gtest，遇到一个bug 没有解出来， 明日继续。

## 2021-03-11

今日继续解决昨日的bug
bug描述:使用gtest完成httplisten的时候,开多线程,gtest会显示失败
terminate called without an active exception
解决失败


C++ 获取当前文件路径:
    #include <stdio.h>
    #include <unistd.h>

    char *buffer;
    buffer = getcwd(NULL, 0);
    cout << "文件路径" << buffer << endl;
    //将需要调用的模块使用 strcat 作拼接;
    const char *model_path = strcat(buffer,"/models");


## 2021-03-11

### 今日首先学习C++的各种输入
1. cin>> 

用法1：　　
　　最基本，最常用的输入方式。
　　在输入过程中会过滤掉不可见字符，如空格、回车、TAB等。同时在遇到
　　如果不想略过空白字符，需要使用noskipws流控制
用法2：
　　接受一个字符串，遇到空格、TAB、回车都结束

2.cin.get()

用法1：
　　cin.get(字符串变量名)，可用来接受字符
　　cin.get(ch)或者ch = cin.get()
用法2：
　　cin.get(字符数组名，接收字符数目)，可用来接受一行字符串，可以接收空格
用法3：
　　cin.get（无参数），没有参数主要是用来舍弃输入流中的不需要的字符，或者舍弃回车，用来弥补cin.get(字符数组名，接收字符数目)的不足

## 2021-03-12

## 2021-03-13

## 2021-03-14
今天周日，休息

## 2021-03-15

今天上午完成emi的用例评审和技术评审报告.
今天下午准备完成emi模块开发.

## 2021-03-16 周二
今天去北京，
完成了 dashboard车端修复
完成emi的三期第一次评审
经验： 
1. 尽量脱离框架写一个Impl的类
2. 排期写的严谨些，这是产品经理们唯一能看懂的了。

## 2021-03-17 周三

今天完成了emi的二期评审
今天完成了dashboard离线手册的一部分
今天在持续开发emi
## 2021-03-18 周四

今天也在持续开发emi
今天完成了dashboard的提供全部topic的新功能的开发（使用http）

## 2021-03-19 周五

今天完成了emi大部分功能的开发
今天回保定


## 2021-03-20
今天周六，休息
## 2021-03-21
今天周日
学习了设计模式
## 2021-03-22
今天上午复习了排序算法
今天上午完成了emi的开发，下一步测试。

今天下午学习了一些C++的基本常识
[CppNote](./CppNote.md)

## 2021-03-23

今天上午完成了添加dashboard新功能的任务： 向前端发送软件版本号。
流程： 从中间件中通过dao接口获取版本号。

今天下午继续学习C++的基本常识
[CppNote](./CppNote.md)


## 2021-03-24 

今天上午将我的coding日志全部转到线上的github中了，之后可以降低对u盘的依赖了

今天上午准备完成对dashboard的更新，添加一个新的topic

今天开始认真看一下设计模式，开一个新的md

[DesignPatterns](./DesignPatterns.md)


## 2021-03-25

今天上午继续看设计模式