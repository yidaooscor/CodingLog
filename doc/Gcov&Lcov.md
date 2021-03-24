<!--
 * @Author: YichaoZhao
 * @Date: 2021-03-05 10:46:33
 * @LastEditTime: 2021-03-05 16:54:07
 * @LastEditors: YichaoZhao
 * @Description: 
 * @FilePath: /CodingLog/Gcov&Lcov.md
-->

# Lcov
https://github.com/linux-test-project/lcov
覆盖率测试工具gcov的前段工具 Lcov

1. Gcov是进行代码运行的覆盖率统计的工具，它随着gcc的发布一起发布的，它的使用也很简单，需要在编译和链接的时候加上-fprofile-arcs -ftest-coverage生成二进制文件，gcov主要使用.gcno和.gcda两个文件，.gcno是由-ftest-coverage产生的，它包含了重建基本块图和相应的块的源码的行号的信息。.gcda是由加了-fprofile-arcs编译参数的编译后的文件运行所产生的，它包含了弧跳变的次数和其他的概要信息。gcda文件的生成需要先执行可执行文件才能生成。生成gcda文件之后执行命令gcov *.cpp就会在屏幕上打印出测试的覆盖率，并同时生成文件“ *cpp.gcov”,然后用vi打开就可以看见哪行被覆盖掉了。

2. lcov的安装很简单，下载源码执行make install就可以了，在生成的“*.cpp.gcov"文件中执行lcov --directory  .   --capture --output-file app.info生成info文件，再执行genhtml  -o  results  app.info就会生成result目录，生成的html文件就在result目录下。


## 1. Lcov是什么？

是GCOV图形化的前端工具
是Linux Test Project维护的开放源代码工具，最初被设计用来支持Linux内核覆盖率的度量
基于Html输出，并生成一棵完整的HTML树
输出包括概述、覆盖率百分比、图表，能快速浏览覆盖率数据
支持大项目，提供三个级别的视图：目录视图、文件视图、源码视图



通常的使用lcov的流程:
1. 根据预先的设计来开发你的软件
2. 写下单元测试用例来check 代码的功能
3. 使用这些写下的测试用例来check测试用例的代码覆盖率.
4. 生成报告并尝试最大化代码覆盖率


# Gcov
https://gcc.gnu.org/onlinedocs/gcc/Gcov.html
## Intro
gcov是一款测试覆盖率的程序.使用它和gcc一起来帮助你写出更高效, 运行更快的代码.并且帮助你发现你程序中没有测试的部分.
你可以将其用于性能评估,帮助你发现哪一部分最有效地影响了您的代码效率.
您还可以将gcov与其他配置文件工具一起使用gprof,以评估那些部分使用了最多的时间.

这个工具可以帮助你完成以下事情:

* 每行代码的执行频率
* 哪一行代码被实际执行了
* 每部分代码实际执行了多长时间

那这样的话你就可以使用这个工具完成以下任务:

* 根据工具提供的数据知道那些代码可以被优化
* 和testsuits一起使用, 来确保软件能被足够好地发行. Testsuites可以证明一个程序按照期望地被运行. Coverage program可以明确有多少程序被testsuite验证过了.然后,程序员就可以决定再增加哪一部分的test case.
* Gcov的特性要求最好不要把大量的代码写在同一行,因为Gcov统计时间是以行为单位

gcov会创建一个log文件叫做sourcefile.gcov 用来显示每一行代码执行了多长时间.

### invoking gcov

    gcov [option] files
    
    -a 
