<!--
 * @Author: YichaoZhao
 * @Date: 2021-03-04 14:41:15
 * @LastEditTime: 2021-03-05 14:11:51
 * @LastEditors: YichaoZhao
 * @Description: In User Settings Edit
 * @FilePath: /CodingLog/Gtest.md
-->

# Gtest

GoogleTest可以帮助我们更好地编写测试用例.
Googletest是一个有Google的测试技术团队开发的测试框架,它考虑到了谷歌的特定需求和限制,无论你使用的是linux,windows还是mac,只要你编写C++代码,googletest都可以帮到你.它支持任何类型的测试,不只是单元测试.

一个好的测试体:

1. 测试应该是**独立**的且可**重复**的.Gtest通过在不同的对象上运行每个测试用例来隔离测试.当测试失败的时候,gtest允许你单独运行它,以便快速调试.
2. 测试应该well organized,并反应测试代码的结构.
3. 测试应该是可移植的且可复用的.
4. 当测试失败的时候,他们应该尽可能多的提供关于故障的信息.GoogleTest不会在第一个测试失败的时候停止,相反,它仅仅停止当前测试并仅需运行下一个.你还可以设置测试报告非致命故障,在次之后,当前测试将继续运行,这样,你可以在一个运行-编辑-编译周期中探测并解决多个bug.
5. 测试框架应该将测试者从家务活中解放出来.
6. 测试要快


## 基本概念

### Assertion(断言)
一个能够判断条件是否为真的statement.
一个Assertion的结果可以为sucess,nonfatal failure, 和fatal failure.

一个测试使用assertions来验证被测试代码的行为.如果测试崩溃了或者有一个failed assertion, 它fials, 否则success.

Assertion是类似于函数调用的macros. 你可以通过断言一个类或函数的行为来测试类.当一个断言失败,Googletest会打印这个assertion的源文件，所在位置的行数和失败信息。你同样可以提供一个自定义的失败信息。

Assertion有两种，一种叫 ASSERT_*（失败时产生fatal failure）, 另一种叫EXPEXT_*（失败时产生nonfatal failure）。

ASSERT_* 可能会产生资源泄露,由于一旦它fail就会退出函数,可能跳过一些 clean-up code.

下面提供一个自定义失败消息的例子:

    ASSERT_EQ(x.size(), y.size()) << "Vectors x and y are of unequal length";

    for (int i = 0; i < x.size(); ++i) {
    EXPECT_EQ(x[i], y[i]) << "Vectors x and y differ at index " << i;
    }

如上程序所示,任何可以被用 ostream输出的都可以跟在后面.

#### Basic Assertions
以下Assertions可以完成基本的true/false testing

|Fatal assertion| 	Nonfatal assertion| 	Verifies|
|--|--|--|
|ASSERT_TRUE(condition); 	|EXPECT_TRUE(condition); 	|condition is true|
|ASSERT_FALSE(condition); 	|EXPECT_FALSE(condition); 	|condition is false|

#### Binary Comparison
以下描述了比较两个值的Assertions
|Fatal assertion 	    |Nonfatal assertion 	    |Verifies|
|--|--|--|
|ASSERT_EQ(val1, val2);| 	EXPECT_EQ(val1, val2); 	|val1 == val2|
|ASSERT_NE(val1, val2);|	EXPECT_NE(val1, val2); 	|val1 != val2|
|ASSERT_LT(val1, val2);| 	EXPECT_LT(val1, val2); 	|val1 < val2|
|ASSERT_LE(val1, val2);| 	EXPECT_LE(val1, val2); 	|val1 <= val2|
|ASSERT_GT(val1, val2);| 	EXPECT_GT(val1, val2); 	|val1 > val2|
|ASSERT_GE(val1, val2);| 	EXPECT_GE(val1, val2); 	|val1 >= val2|

可以重载>,<,=号,但是不推荐.
ASSERT_EQ(actual, expected) is preferred to ASSERT_TRUE(actual == expected), since it tells you actual and expected's values on failure.

ASSERT_EQ能够比较两个指针是否相等,但是不能比较两个C的string。但是可以比较两个C++的STL的string。

比较指针的时候,用*_EQ(ptr,nullptr), 不能用 * _EQ(ptr,NULL)

浮点数的比较有其他说法.

#### String Comparison
下面的Assertionss比较C风格的string。

|Fatal assertion 	|Nonfatal assertion| 	Verifies|
|--|--|--|
|ASSERT_STREQ(str1,str2); 	|EXPECT_STREQ(str1,str2); 	|the two C strings have the same content|
|ASSERT_STRNE(str1,str2); 	|EXPECT_STRNE(str1,str2); 	|the two C strings have different contents|
|ASSERT_STRCASEEQ(str1,str2); 	|EXPECT_STRCASEEQ(str1,str2); 	|the two C strings have the same content, ignoring case|
|ASSERT_STRCASENE(str1,str2); 	|EXPECT_STRCASENE(str1,str2); 	|the two C strings have different contents, ignoring case|。

ignoring case 忽略大小写

#### Simple Test

* 创建一个test：
 1. 使用TEST() 宏来定义和命名一个测试函数. 类似一个正常的C++函数并且没有返回值.
 2. 在这个函数中,你可以用任何C++的语句和googletest的断言.
 3. 测试结果被Assertions决定.如果Assertions fails或者测试崩了,那么失败,否则成功.

    TEST(TestSuiteName, TestName) {
    ... test body ...
    }

* Test的参数:
    1. 第一个参数是test suit(测试组)的名字.
    2. 第二个参数是这个test的名字.
名字命名需要遵循C++的风格,并且不能有下划线.因为一个test的全名由test suit的名字和他自己的名字组成.不同test suit中的test可以重名.

下面再举一个例子:

    int Factorial(int n);  // Returns the factorial阶乘 of n

假设有上面这么一个函数:

    // Tests factorial of 0.
    TEST(FactorialTest, HandlesZeroInput) {
    EXPECT_EQ(Factorial(0), 1);
    }
    // Tests factorial of positive numbers.
    TEST(FactorialTest, HandlesPositiveInput) {
    EXPECT_EQ(Factorial(1), 1);
    EXPECT_EQ(Factorial(2), 2);
    EXPECT_EQ(Factorial(3), 6);
    EXPECT_EQ(Factorial(8), 40320);
    }

如上程序所示, 正数一个组,负数一个组,0一个组,很合理.

### Test Fixtures：使用the Same Data Configuration for Multipple Test

这个东西是用来测试相似数据的。

* 创建一个fixture：
1. 派生一个类从 ::testing::Test. 
2. 在类中声明任意你想用的objects
3. 如果必要,写一个构造函数或者一个SetUp()函数.注意是SetUp(),不是Setup().
4. 如果必要,再写一个析构函数或者TearDown()函数
5. 如果必要,再定义一些子程序.

如果用fixture, 函数应该为TEST_F()

    TEST_F(TestFixtureName, TestName) {
    ... test body ...
    }

第一个参数为test fixture的名字.

再使用TEST_F之前,你必须定义一个test_fixture.

对于每个test defined with TEST_F(), gtest将会创建一个新的test fixture再runtime,初始化用SetUp(), 结束调用TearDown().注意不同的 test在同一个test suit有不同的test fixture objects.

下面举个例子,下面测试一个FIFO queue 类的函数, 接口如下:

    template <typename E>  // E is the element type.
    class Queue {
    public:
    Queue();
    void Enqueue(const E& element);
    E* Dequeue();  // Returns NULL if the queue is empty.
    size_t size() const;
    ...
    };

首先,定义一个fixture class 

    class QueueTest : public ::testing::Test {
    protected:
    void SetUp() override {
        q1_.Enqueue(1);
        q2_.Enqueue(2);
        q2_.Enqueue(3);
    }

    // void TearDown() override {}

    Queue<int> q0_;
    Queue<int> q1_;
    Queue<int> q2_;
    };

下面,我们来写test:

    TEST_F(QueueTest, IsEmptyInitially) {
    EXPECT_EQ(q0_.size(), 0);
    }

    TEST_F(QueueTest, DequeueWorks) {
    int* n = q0_.Dequeue();
    EXPECT_EQ(n, nullptr);

    n = q1_.Dequeue();
    ASSERT_NE(n, nullptr);
    EXPECT_EQ(*n, 1);
    EXPECT_EQ(q1_.size(), 0);
    delete n;

    n = q2_.Dequeue();
    ASSERT_NE(n, nullptr);
    EXPECT_EQ(*n, 2);
    EXPECT_EQ(q2_.size(), 1);
    delete n;
    }

里边混用了ASSERR*_ 和 EXPEXT *_ Assertions. 经验为....

当这些tests运行的时候,下面的事情会发生:
    
    1. googletest 构造了一个QueueTest的Object(简称为 t1)
    2. t1.SetUp()初始化t1.
    3. 第一个test(IsEmptyInitially)运行在 t1上.
    4. t1.TearDown() cleans up
    5. t1被析构.
    6 以上的步骤将被重复再另一个QueueTest中

### Invoking the Test(调用测试)
TEST()和TEST_F()会隐式地被注册,所以不需要写注册表.
在定义完所有的tests后,你可以用 RUN_ALL_TESTS() 来运行,成功返回0,否则返回1. 注意: 这个RUN_ALL_TESTS()会运行所有的test甚至在不同的源文件中.

当调用时候, the RUN_ALL_TESTS() macro:
* 保存所有的googletest flags的状态
* 创建一个test fixture object for the first test.
* 初始化via SetUp()
* 运行test fixture
* clean up via TearDown()
* Delete the fixture
* 回复 flags
* 重复以上步骤.

**注意:**
一定不要忽略 RUN_ALL_TESTS()的返回值,.
另外,最多调用一次RUN_ALL_TESTS()

大多数的使用者不需要定义他们自己的main函数,而是链接gtest_main. 


## 单元测试示例:
GoogleTest和GoogleMock是谷歌公司于2008年发布的两套用于单元测试的应用框架。以下基于gtest-1.2.1以及gmock-1.0.0

### 单元测试概述
执行单元测试，就是为了证明这一段代码的行为和我们期望的一致。

单元测试（Unit Test）是开发者编写的一小段代码，用于检验被测代码的一个很小的，很明确的功能是否正确，通过编写单元测试可以在编码阶段发现程序编码错误，甚至是程序设计错误。

对于单元测试框架，目前最为大家熟知的是JUnit及其针对各种语言衍生产品，C++对应的就是CppUnit。但是CppUnit不好用，于是Google开发了一套Google Test。

### 应用GoogleTest编写单元测试代码。

#### 定义单元测试

定义TEST

#### 实现单元测试

    // Configure.h 
    #pragma once 

    #include <string> 
    #include <vector> 

    class Configure 
    { 
    private: 
        std::vector<std::string> vItems; 

    public: 
        int addItem(std::string str); 

        std::string getItem(int index); 

        int getSize(); 
    }; 


    // Configure.cpp 
    #include "Configure.h" 

    #include <algorithm> 

    /** 
    * @brief Add an item to configuration store. Duplicate item will be ignored 
    * @param str item to be stored 
    * @return the index of added configuration item 
    */ 
    int Configure::addItem(std::string str) 
    { 
    std::vector<std::string>::const_iterator vi=std::find(vItems.begin(), vItems.end(), str); 
        if (vi != vItems.end()) 
            return vi - vItems.begin(); 

        vItems.push_back(str); 
        return vItems.size() - 1; 
    } 

    /** 
    * @brief Return the configure item at specified index. 
    * If the index is out of range, "" will be returned 
    * @param index the index of item 
    * @return the item at specified index 
    */ 
    std::string Configure::getItem(int index) 
    { 
        if (index >= vItems.size()) 
            return ""; 
        else 
            return vItems.at(index); 
    } 

    /// Retrieve the information about how many configuration items we have had 
    int Configure::getSize() 
    { 
        return vItems.size(); 
    } 

    // ConfigureTest.cpp 
    #include <gtest/gtest.h> 

    #include "Configure.h" 

    TEST(ConfigureTest, addItem) 
    { 
        // do some initialization 
        Configure* pc = new Configure(); 
        
        // validate the pointer is not null 
        ASSERT_TRUE(pc != NULL); 


        // call the method we want to test 
        pc->addItem("A"); 
        pc->addItem("B"); 
        pc->addItem("A"); 


        // validate the result after operation 
        EXPECT_EQ(pc->getSize(), 2); 
        EXPECT_STREQ(pc->getItem(0).c_str(), "A"); 
        EXPECT_STREQ(pc->getItem(1).c_str(), "B"); 
        EXPECT_STREQ(pc->getItem(10).c_str(), ""); 


        delete pc; 
    }



#### 运行单元测试

在实现完单元测试的测试逻辑后，可以通过RUN_ALL_TEST()来运行它们。

运行GoogleTest编写的单元测试的一种比较可行的方法是：

为每一个被测试的class分别创建一个测试文件，并在该文件中编写针对这一class的单元测试；编写一个mian。cpp文件，并在其中包含所有单元测试

    #include <gtest/gtest.h> 


    int main(int argc, char** argv) { 
        testing::InitGoogleTest(&argc, argv); 


        // Runs all tests using Google Test. 
        return RUN_ALL_TESTS(); 
    }

最后，将所有测试代码以及main.cpp编译并链接到目标程序中.
此外,在运行可执行目标程序时,可以使用--**gtest_filter**来执行要执行的测试用例.例如

    ./foo_test //没有指定 filter ，运行所有测试；
    ./foo_test --gtest_filter=* //指定 filter 为 * ，运行所有测试；
    ./foo_test --gtest_filter=FooTest.* //运行测试用例 FooTest 的所有测试；
    ./foo_test --gtest_filter=*Null*:*Constructor* 运行所有全名（即测试用例名 + “ . ” + 测试名，如 GlobalConfigurationTest.noConfigureFileTest ）含有"Null" 或 "Constructor" 的测试；
    ./foo_test --gtest_filter=FooTest.*-FooTest.Bar 运行测试用例 FooTest 的所有测试，但不包括 FooTest.Bar。这一特性在包含大量测试用例的项目中会十分有用。


## 打造自己的单元测试框架

