<!--
 * @Author: YichaoZhao
 * @Date: 2021-03-24 10:03:35
 * @LastEditTime: 2021-03-24 20:58:47
 * @LastEditors: YichaoZhao
 * @Description: 
 * @FilePath: /CodingLog/doc/DesignPatterns.md
-->

# 设计模式

设计模式是为了解决某类重复出现的问题而出现的一套成功或有效的解决方案。

设计模式一般包括模式名称(Pattern Name), 问题(Problem), 解决方案(Solution), 效果(Consequence).

## 设计模式的作用和分类

狭义的设计模式一般分为3大类23种,其中,创建型模式关注对象的创建过程,结构型模式关注如何将现有类或对象组织在一起形成更强大的结构,行为型模式关注系统中对象之间的交互研究系统在运行时对象之间的相互通信协作,进一步明确对象的职责.

设计模式分类

|类型|种类|
|-|-|
|创建型模式|单例模式, 简单工厂模式, 抽象工厂模式, 工厂方法模式, 原型模式, 建造者模式|
|结构型模式|适配器模式, 桥接模式, 组合模式, 装饰模式, 外观模式, 享元模式, 代理模式|
|行为模式|职责链模式, 命令模式, 解释器模式, 迭代模式, 中介者模式, 备忘录模式, 观察者模式, 状态模式, 策略模式, 模板方法模式, 访问者模式|

## UML类图(Unified Model Language)

类图中的常见关系: 泛化(Generalization), 实现(Realization), 关联(Association), 聚合(Aggregation), 组合(Composition), 依赖(Dependency)

泛化: 空心三角和实线 类的继承 子类指向父类
实现: 空心三角和虚线 类和接口的关系 空心三角指向接口
关联: 关联是一种关系(has), 一个类可以调用另一个类的共有的属性和方法,在类中以成员变量的方式表示.比如老师有自己的学生,知道学生的姓名学号成绩,学生有自己老师,也知道老师的姓名和所教科目.关联分为单向关联(用带箭头的直线表示),双向关联(直线),自关联. 
聚合: 整体和部分的关系,部分离开整体后可以单独存在.菱形箭头表示. 菱形挨着整体
组合: 整体和部分的关系,部分离开整体不可以单独存在,实心菱形
依赖: 是一种使用关系,即一个类的实现需要另一个类的协助.常用于方法的局部变量,方法参数等. 带箭头的虚线,虚线指向协助类.


## 面向对象设计原则

设计模式需要遵循基本的软件设计原则. **可维护性(Maintainable)** 和 **可复用性(Reusable)** 是衡量软件质量的两个重要属性.

1. 单一职责原则
定义1: 一个对象应该只包含单一的职责,并且改职责被完整地封装在一个类中.
定义2: 就一个类而言,应该仅有一个引起它变化的原因.

首先需要知道两个原则:**高聚合**和**低耦合**
高聚合: 内聚是对软件系统中元素职责相关性和集中度的度量.如果元素具有高度相关的职责,除了这些职责内的任务,没有其他过多的工作,那么改元素就具有高内聚性.
低耦合: 耦合是软件结构中各类模块之间相互连接的一种度量, 耦合强弱取决于模块间接接口的复杂程度

2. 开闭原则
软件实体应该对扩展开放,对修改关闭.
开闭原则指软件实体应该在不修改原有代码的基础上进行扩展.软件设计过程中,需求可能会随时变化,需要根据需求扩展已有的设计.如果原有的设计符合开闭原则,那么扩展起来就比较安全(不会影响原有的功能,稳定)和方便(易于扩展).开闭原则的关键在于抽象化.可以为系统定义一个相对较为稳定的抽象层,将不同的实现行为放到具体的实现层中完成.
举个例子:要设计一个计算器的类,包含加减两个功能,很自然的想法是在类computer里声明并实现add和sub两个方法,那么如果要求再增加乘法功能,是不是哟啊在computer里增加mul的方法呢?这样就违背了开闭原则

3. 里氏代换原则
所有引用基类的地方必须能透明地使用其子类的对象.
在软件中, 如果用子类对象来替换基类对象,程序将不会产生任何异常和问题,反过来不成立

里氏代换原则的指导意义在与:尽可能地使用基类类型来替换基类对象,程序将不会产生任何异常和问题,而在运行时再确定子类类型,然后用子类对象替换父类对象.设计时应将父类设计为抽象类或接口,子类继承父类并实现在父类中声明的方法;运行时子类实例(对象)替换父类实例(对象),可以很方便地扩展系统功能.

4. 依赖倒转原则.
依赖倒转原则: 高层模块不应该依赖底层模块,它们都应该依赖抽象.抽象不应该依赖细节,细节应该依赖于抽象.
什么是高层,什么是低层呢? 它们指的是继承中的基类子类关系,基类或者越抽象的类,层次越高,简单说,倒转依赖原则要求针对接口编程,不要针对实现编程.

依赖倒转原则要求在程序代码中传递参数, 或在关联关系中, 尽量引用层次高的出现层类,即使用接口或抽象类来声明变量, 参数类型声明, 方法返回类型声明, 以及数据类型转换等.而不要用具体的类来做这些事情.

5. 接口隔离原则
客户端不应该依赖那些它不需要的接口

6. 合成复用原则
优先使用对象组合, 而不是用过继承来达到复用的目的.
合成复用原则指导在软件设计时,优先使用关联, 聚合和组合的关系,尽量少使用泛化(继承)

7. 迪米特法则
每一个软件单位对其他单位都只有最少的知识, 而且局限于那些与本单位密切相关的软件单位

### 1.简单工厂模式

![SimpleFactor](../pic/DesignPatterns_SimpleFactory.png)

代码: 

    //抽象产品类AbstractProduct
    class AbstractProduct
    {
    public:
        //抽象方法：
    };
    
    //具体产品类Basketball
    class ConcreteProduct :public AbstractProduct
    {
    public:
        //具体实现方法
    };
    
    class Factory
    {
    public:
        shared_ptr<AbstractProduct> createProduct(string productName)
        {
            if (productName == "ProductA"){
                return make_shared<ProductA>();
            }
            else if (productName == "ProductB"){
                return make_shared<ProductB>();
            }
            ...
        }
    };


简单工厂模式存在者明显的不足,违反了开闭原则

### 2.工厂模式
简单工厂模式中，每新增一个具体产品，就需要修改工厂类内部的判断逻辑。为了不修改工厂类，遵循开闭原则，工厂方法模式中不再使用工厂类统一创建所有的具体产品，而是针对不同的产品设计了不同的工厂，每一个工厂只生产特定的产品。

工厂方法模式：定义一个用于创建对象的接口，但是让子类决定将哪一个类实例化。工厂方法模式让一个类的实例化延迟到其子类。

抽象工厂（AbstractFactory）：所有生产具体产品的工厂类的基类，提供工厂类的公共方法；
具体工厂（ConcreteFactory）：生产具体的产品
抽象产品（AbstractProduct）：所有产品的基类，提供产品类的公共方法
具体产品（ConcreteProduct）：具体的产品类

![Factor](../pic/DesignPatterns_Factory.png)

代码:

定义抽象产品类AbstractSportProduct，方法不提供实现

    //抽象产品类AbstractProduct
    class AbstractSportProduct
    {
    public:
        AbstractSportProduct(){
    
        }
        //抽象方法：
        void printName(){};
        void play(){};
    };

定义三个具体产品类

    //具体产品类Basketball
    class Basketball :public AbstractSportProduct
    {
    public:
        Basketball(){
            printName();
            play();
        }
        //具体实现方法
        void printName(){
            printf("Jungle get Basketball\n");
        }
        void play(){
            printf("Jungle play Basketball\n\n");
        }
    };
    
    //具体产品类Football
    class Football :public AbstractSportProduct
    {
    public:
        Football(){
            printName();
            play();
        }
        //具体实现方法
        void printName(){
            printf("Jungle get Football\n");
        }
        void play(){
            printf("Jungle play Football\n\n");
        }
    };
    
    //具体产品类Volleyball
    class Volleyball :public AbstractSportProduct
    {
    public:
        Volleyball(){
            printName();
            play();
        }
        //具体实现方法
        void printName(){
            printf("Jungle get Volleyball\n");
        }
        void play(){
            printf("Jungle play Volleyball\n\n");
        }
    };

定义抽象工厂类AbstractFactory，方法为纯虚方法

    //抽象工厂类
    class AbstractFactory
    {
    public:
        virtual shared_ptr<AbstractSportProduct> getSportProduct() = 0;
    };

定义三个具体工厂类

    /具体工厂类BasketballFactory
    class BasketballFactory :public AbstractFactory
    {
    public:
        BasketballFactory(){
            printf("BasketballFactory\n");
        }
        shared_ptr<AbstractSportProduct> getSportProduct(){
            printf("basketball");
            return make_shared<Basketball>();
        }
    };
    
    //具体工厂类FootballFactory
    class FootballFactory :public AbstractFactory
    {
    public:
        FootballFactory(){
            printf("FootballFactory\n");
        }
        shared_ptr<AbstractSportProduct> getSportProduct(){
            return make_shared<Football>();
        }
    };
    
    //具体工厂类VolleyballFactory
    class VolleyballFactory :public AbstractFactory
    {
    public:
        VolleyballFactory(){
            printf("VolleyballFactory\n");
        }
        shared_ptr<AbstractSportProduct> getSportProduct(){
            return  make_shared<Volleyball>();
        }
    };


客户端使用方法

    #include <iostream>
    #include "FactoryMethod.h"
    
    int main()
    {
        printf("工厂方法模式\n");
        
        //定义工厂类对象和产品类对象
        shared_ptr<AbstractFactory> fac;
        shared_ptr<AbstractSportProduct> product;
    
        fac = make_shared<BasketballFactory>();
        product = fac->getSportProduct();
    
        fac = make_shared<FootballFactory>();
        product = fac->getSportProduct();
    
        fac = make_shared<VolleyballFactory>();
        product = fac->getSportProduct();	
    
        system("pause");
        return 0;
    }

工厂模式总结

优点:

* 工厂方法用于创建客户所需产品，同时向客户隐藏某个具体产品类将被实例化的细节，用户只需关心所需产品对应的工厂；
* 工厂自主决定创建何种产品，并且创建过程封装在具体工厂对象内部，多态性设计是工厂方法模式的关键；
* 新加入产品时，无需修改原有代码，增强了系统的可扩展性，符合开闭原则。

缺点:

* 添加新产品时需要同时添加新的产品工厂，系统中类的数量成对增加，增加了系统的复杂度，更多的类需要编译和运行，增加了系统的额外开销；
* 工厂和产品都引入了抽象层，客户端代码中均使用的抽象层（AbstractFactory和AbstractSportProduct ），增加了系统的抽象层次和理解难度。

### 3.抽象工厂模式

提供一个创建一系列相关或相互依赖对象的接口,而无需指定它们具体的类.
简而言之,一个工厂可以提供创建多种相关产品的接口, 而无需像工厂方法一样,为每一个产品都提供一个具体工厂.

* 抽象工厂（AbstractFactory）：所有生产具体产品的工厂类的基类，提供工厂类的公共方法；
* 具体工厂（ConcreteFactory）：生产具体的产品
* 抽象产品（AbstractProduct）：所有产品的基类，提供产品类的公共方法
* 具体产品（ConcreteProduct）：具体的产品类

![AbstractFactor](../pic/DesignPatterns_AbstractFactory.png)


案例:
Jungle想要进行户外运动，它可以选择打篮球和踢足球。但这次Jungle不想弄脏原本穿的T恤，所以Jungle还需要穿球衣，打篮球就穿篮球衣，踢足球就穿足球衣。篮球保管室可以提供篮球和篮球衣，足球保管室可以提供足球和足球衣。Jungle只要根据心情去某个保管室，就可以换上球衣、拿上球，然后就可以愉快地玩耍了。

#### 1. 产品类:

##### 1.1 产品类ball

抽象产品类AbstractBall, 球类的基类，定义抽象方法play

    //抽象产品类AbstractBall
    class AbstractBall
    {
    public:
    	AbstractBall(){}
    	//抽象方法：
    	void play(){};
    };

具体产品类， 分别为Basketball和Football，具体实现方法play

    //具体产品类Basketball
    class Basketball :public AbstractBall
    {
    public:
        Basketball(){
            play();
        }
        //具体实现方法
        void play(){
            printf("Jungle play Basketball\n\n");
        }
    };
    
    //具体产品类Football
    class Football :public AbstractBall
    {
    public:
        Football(){
            play();
        }
        //具体实现方法
        void play(){
            printf("Jungle play Football\n\n");
        }
    };

##### 1.2 产品类shirt

抽象产品类AbstractShirt：球衣类的基类，定义抽象方法wearShirt

    //抽象产品类AbstractShirt
    class AbstractShirt
    {
    public:
        AbstractShirt(){}
        //抽象方法：
        void wearShirt(){};
    };

具体产品类BasketballShirt和FootballShirt，具体实现方法wearShirt

    //具体产品类BasketballShirt
    class BasketballShirt :public AbstractShirt
    {
    public:
        BasketballShirt(){
            wearShirt();
        }
        //具体实现方法
        void wearShirt(){
            printf("Jungle wear Basketball Shirt\n\n");
        }
    };
    
    //具体产品类FootballShirt
    class FootballShirt :public AbstractShirt
    {
    public:
        FootballShirt(){
            wearShirt();
        }
        //具体实现方法
        void wearShirt(){
            printf("Jungle wear Football Shirt\n\n");
        }
    };

#### 2. 定义工厂类

定义抽象工厂AbstractFactory，声明两个方法getBall和getShirt

    //抽象工厂类
    class AbstractFactory
    {
    public:
        virtual AbstractBall *getBall() = 0;
        virtual AbstractShirt *getShirt() = 0;
    };

定义具体工厂BasketballFactory和FootballFactory，重新具体实现两个方法getBall和getShirt

    //具体工厂类BasketballFactory
    class BasketballFactory :public AbstractFactory
    {
    public:
        BasketballFactory(){
            printf("BasketballFactory\n");
        }
        AbstractBall *getBall(){
            printf("Jungle get basketball\n");
            return new Basketball();
        }
        AbstractShirt *getShirt(){
            printf("Jungle get basketball shirt\n");
            return new BasketballShirt();
        }
    };
    
    //具体工厂类BasketballFactory
    class FootballFactory :public AbstractFactory
    {
    public:
        FootballFactory(){
            printf("FootballFactory\n");
        }
        AbstractBall *getBall(){
            printf("Jungle get football\n");
            return new Football();
        }
        AbstractShirt *getShirt(){
            printf("Jungle get football shirt\n");
            return new FootballShirt();
        }
    };

#### 3. 客户端

    #include <iostream>
    #include "AbstractFactory.h"
    
    int main()
    {
        printf("抽象工厂模式\n");
        
        //定义工厂类对象和产品类对象
        AbstractFactory *fac = NULL;
        AbstractBall *ball = NULL;
        AbstractShirt *shirt = NULL;
    
        fac = new BasketballFactory();
        ball = fac->getBall();
        shirt = fac->getShirt();
    
        fac = new FootballFactory();
        ball = fac->getBall();
        shirt = fac->getShirt();
    
        system("pause");
        return 0;
    }

## 4. 建造者模式

将一个复杂对象构建与它的表示分离, 使得同样的构建过程可以创建不同的表示.

同样的构建过程可以创建不同的表示?? 想象一下, 建造一栋房子, 建造过程无非都是打地基,筑墙,安装门窗等过程,但不同的客户可能希望不同的风格或过程,最终呈现出来的房子就是不同的风格了.

* 抽象建造者（AbstractBuilder）：创建一个Product对象的各个部件指定的抽象接口；
* 具体建造者（ConcreteBuilder）：实现AbstractBuilder的接口，实现各个部件的具体构造方法和装配方法，并返回创建结果。
* 产品（Product）：具体的产品对象
* 指挥者（Director）： 构建一个使用Builder接口的对象，安排复杂对象的构建过程，客户端一般只需要与Director交互，指定建造者类型，然后通过构造函数或者setter方法将具体建造者对象传入Director。它主要作用是：隔离客户与对象的生产过程，并负责控制产品对象的生产过程。

## 5. 原型模式

使用原型实例指定待创建对象的类型,并且通过复制这个原型来创建新的对象.

原型模式的工作原理是将一个原型对象传给要发动创建的对象(即客户端对象), 这个要发动创建的对象通过请求原型对象复制自己来实现创建过程. 从工厂方法角度而言, 创建新对象的工厂就是原型类自己.软件系统中有些对象的创建过程比较复杂, 且有时需要频繁创建, 原型模式通过给出一个原型对象来指明所要创建的对象类型,然后用复制这个原型兑现改的办法创建出更多同类型的对象,这就是原型模式的意图所在.

原型模式结构:

* 抽象原型类（AbstractPrototype）：声明克隆clone自身的接口
* 具体原型类（ConcretePrototype）：实现clone接口
* 客户端（Client）：客户端中声明一个抽象原型类，根据客户需求clone具体原型类对象实例

## 6. 单例模式

单例模式:
确保只有一个类只有一个实例, 并且提供一个全局访问点来访问这个唯一实例.

单例模式需要注意多线程的时候需要加锁

## 7. 适配器模式

当组合需要使用的类型不兼容时, 也需要类似于变压器一样的适配器来协调这些不兼容者, 这就是适配器模式.


## 8. 桥接模式

将抽象的部分与它的实现部分解耦,使得两者都能够独立变化.

* Abstraction（抽象类）：定义抽象类的接口（抽象接口），由聚合关系可知，抽象类中包含一个Implementor类型的对象，它与Implementor之间有关联关系，既可以包含抽象业务方法，也可以包含具体业务方法；
* Implementor（实现类接口）：定义实现类的接口，这个接口可以与Abstraction类的接口不同。一般而言，实现类接口只定义基本操作，而抽象类的接口还可能会做更多复杂的操作。
* RefinedAbstraction（扩充抽象类）：具体类，实现在抽象类中定义的接口，可以调用在Implementor中定义的方法；
* ConcreteImplementor（具体实现类）：具体实现了Implementor接口，在不同的具体实现类中实现不同的具体操作。运行时ConcreteImplementor将替换父类。


桥接模式总结:

优点： 

* 分离抽象接口与实现部分，使用对象间的关联关系使抽象与实现解耦；
* 桥接模式可以取代多层继承关系，多层继承违背单一职责原则，不利于代码复用；
* 桥接模式提高了系统可扩展性，某个维度需要扩展只需增加实现类接口或者具体实现类，而且不影响另一个维度，符合开闭原则。

缺点:

* 桥接模式难以理解，因为关联关系建立在抽象层，需要一开始就设计抽象层；
* 如何准确识别系统中的两个维度是应用桥接模式的难点。

## 9. 组合模式

组合多个对象形成树形结构以表示具有部分-整体关系的层次结构。组合模式让客户端可以统一对待单个对象和组合对象。

略

## 10. 装饰模式

动态地给一个对象增加一些额外的职责.就扩展而言, 装饰器模式提供了一种比使用子类更加灵活的替代方案.
装饰模式是一种用于代替集成技术的技术


优点:
对于扩展一个类的新功能，装饰模式比继承更加灵活；
动态扩展一个对象的功能；
可以对一个对象进行多次装饰（如上述例子第二个手机和第三个手机）；
具体构件类和具体装饰类可以独立变化和扩展，符合开闭原则。

缺点:
装饰模式中会增加很多小的对象，对象的区别主要在于各种装饰的连接方式不同，而并不是职责不同，大量小对象的产生会占用较多的系统资源；
装饰模式比继承模式更灵活，但也更容易出错，更难于排错。

## 11. 外观模式

为子系统中的一组接口提供一个统一的入口。外观模式定义了一个高层接口，这个接口使得这一子系统更加容易使用。

## 12. 享元模式
