<!--
 * @Author: your name
 * @Date: 2021-03-08 13:56:19
 * @LastEditTime: 2021-03-11 19:42:21
 * @LastEditors: YichaoZhao
 * @Description: In User Settings Edit
 * @FilePath: /CodingLog/Cmake.md
-->

# Cmake

## "Hello World" with multiple source files

已知一个源文件 *main.cpp* 定义了一个main函数, 和一个伴随的 CMakeLists.txt文件.

main.cpp (C++ Hello World Example)

    #include <iostream>

    int main()
    {
        std::cout << "Hello World!\n";
        return 0;
    }

CMakeLists.txt

    cmake_minimum_required(VERSION 2.4) #设置对Cmake的最小需求版本 

    project(hello_world)  # 开始一个新的CMake项目, 这将触发许多内部Cmake逻辑,尤其是对默认C和C++编译器的检测.

    add_executable(app main.cpp) #使用 add_exectuable创建一个构建目标应用程序,该应用程序将使用当前设置的一些默认标志调用配置编译器,以从给定的源文件main.cpp编译可执行应用程序.

Command Line (In-Source-Build, **not recommended**)

    > cmake . # 在当前位置执行编译器检测,评估给定的CMakeLists.txt. 在当前目录生成编译环境.
    ...
    > cmake --build . # 编译的调用

Command Line (Out-of-Source, **recommended**)
为了保证代码的干净,推荐采用 Out-of-Source.

    > mkdir build
    > cd build
    > cmake ..
    > cmake --build .

Or CMake can also abstract your platforms shell's basic commands from above's example:

    > cmake -E make_directory build
    > cmake -E chdir build cmake .. 
    > cmake --build build 

## 添加include path

语法:

    include_directories([AFTER|BEFORE] [SYSTEM] dir1 [dir2 ...])

参数:
    
|参数|描述|
|--|--|
|**dirN**|一个或多个相对或绝对路径|
|**AFTER/BEFORE**|（可选）是否将给定目录添加到当前包含路径列表的开头或结尾； 默认行为由CMAKE_INCLUDE_DIRECTORIES_BEFORE定义|
|**SYSTEM**|（可选）告诉编译器在系统包含目录时使用给定目录，这可能会触发编译器的特殊处理|

如果文件结构如下:

    include\
        myHeader.h
    src\
        main.cpp
    CMakeLists.txt

那么CMake文件中应该这么写:

    include_directories(${PROJECT_SOURCE_DIR}/include)

添加 *include* 的directory 来 include 编译器寻找路径
它的子目录同样需要用这样的形式添加

## 添加一个版本号和一个配置的头文件

添加版本号

    cmake_minimum_required(VERSION 3.10)
    # set the project name and version
    project(Tutorial VERSION 1.0)

然后, 配置一个头文件来将版本号传递给源代码.

    configure_file(Tutorial.h.in TutorialConfig.h)

因为配置文件将会被写到二叉树中, 所以我们必须把这个二叉树的路径添加到Cmake文件中.

    target_include_directories(Tutorial PUBLIC
                           "${PROJECT_BINARY_DIR}"
                           )

创建一个 *Tutorial.h.in* 文件 在 source directory(源目录) 创建以下内容:

    // the configured options and settings for Tutorial
    #define Tutorial_VERSION_MAJOR @Tutorial_VERSION_MAJOR@
    #define Tutorial_VERSION_MINOR @Tutorial_VERSION_MINOR@

当CMake配置此头文件时，@ Tutorial_VERSION_MAJOR @和@ Tutorial_VERSION_MINOR @的值将被替换。

接下来，修改tutorial.cxx以包含已配置的头文件TutorialConfig.h。

    if (argc < 2) {
        // report version
        std::cout << argv[0] << " Version " << Tutorial_VERSION_MAJOR << "."
                << Tutorial_VERSION_MINOR << std::endl;
        std::cout << "Usage: " << argv[0] << " number" << std::endl;
        return 1;
    }

## 建立配置(build configuration)

在不同的主题下使用不同的Cmake配置

    CMAKE_MINIMUM_REQUIRED(VERSION 2.8.11)
    SET(PROJ_NAME "myproject")
    PROJECT(${PROJ_NAME})

    # Configuration types
    SET(CMAKE_CONFIGURATION_TYPES "Debug;Release" CACHE STRING "Configs" FORCE)
    IF(DEFINED CMAKE_BUILD_TYPE AND CMAKE_VERSION VERSION_GREATER "2.8")
    SET_PROPERTY(CACHE CMAKE_BUILD_TYPE PROPERTY STRINGS  ${CMAKE_CONFIGURATION_TYPES})
    ENDIF()

    SET(${PROJ_NAME}_PATH_INSTALL     "/opt/project"                     CACHE PATH "This directory contains installation Path")
    SET(CMAKE_DEBUG_POSTFIX "d")

    # Install
    #---------------------------------------------------#
    INSTALL(TARGETS ${PROJ_NAME}
        DESTINATION  "${${PROJ_NAME}_PATH_INSTALL}/lib/${CMAKE_BUILD_TYPE}/"
        )

## add library

用于添加自己的库

    add_library(MathFunctions mysqrt.cxx)

为了使用新的库,我们增加了一个新的函数 add_subdirectory(), 这个函数将会被顶层CmakeList.txt文件调用

    # add the MathFunctions library
    add_subdirectory(MathFunctions)

    # add the executable
    add_executable(Tutorial tutorial.cxx)

    target_link_libraries(Tutorial PUBLIC MathFunctions)

    # add the binary tree to the search path for include files
    # so that we will find TutorialConfig.h
    target_include_directories(Tutorial PUBLIC
                            "${PROJECT_BINARY_DIR}"
                            "${PROJECT_SOURCE_DIR}/MathFunctions"
                            )






## Get strated with CMake Tools on Linux
Cmake 是一个开源的，跨平台的工具。 它使用编译器和与平台无关的配置文件来生成特定与本地的编译器和平台的文件。




