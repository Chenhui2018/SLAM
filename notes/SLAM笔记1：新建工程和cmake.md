# SLAM笔记1：新建工程和cmake

标签（空格分隔）： SLAM cmake

参考资料[^2.5TensorFlow运作方式入门]

本文是基于“TensorFlow中文社区”[^中文社区]的教程（2.5）而写的笔记。
本文的目录如下：
> * 1 新建hello SLAM
> * 2 cmake
> * 完整代码

---
## 1 新建hello SLAM
### 1.1选择的工作路径如下：
```
chan@chan-virtual-machine:~/projects/SLAM$
```
以后的SLAM相关的代码均在这个目录下。
### 1.2 CMD输入：
```
touch helloSLAM.cpp
gedit helloSLAM.cpp
```
### 1.3编辑以下代码：
```
#include <iostream>

int main()
{
	std::cout<<"Hello SLAM!"<<std::endl;
	return 0;
}
```
### 1.4 CMD输入：
```
g++ helloSLAM.cpp
./a.out
```
看到运行结果。

## 2 cmake 方式1 
### 2.1 CMD：
```
touch CMakeLists.txt
gedit CMakeLists.txt
```
### 1.2 输入cmake代码
```
# cmake version requirment
cmake_minimum_required(VERSION 2.8)

# project name.
project( helloSLAM )

# generate exectuable file.
add_executable( helloSLAM helloSLAM.cpp )
```
### 1.3 CMD
```
cmake .
make 
./helloSLAM
```
看到输出结果。

## 3 cmake 方式2
把代码和编译生成的东西分开。一般会将编译产生的文件放到某个文件夹下（一般指定名字为build）

### 3.1 清理
先将方式1中产生的所有文件全部删除。
只保留：
```
CMakeLists.txt helloSLAM.cpp
```
### 3.2 CMD
```
mkdir build
cd build
cmake ..
make
./hellowSLAM
```
看到输出结果。

## 4 cmake 方式3
生成lib文件并且调用。
### 4.1 清理
先将方式2中产生的所有文件全部删除。
只保留：
```
CMakeLists.txt helloSLAM.cpp
```
### 4.2 建立lib的头文件和源文件
```
touch libHelloSLAM.cpp
touch libHelloSLAM.h
gedit libHelloSLAM.cpp
```
编辑“libHelloSLAM.cpp”，输入：
```
#include <iostream>

int print_helloSLAM()
{
	std::cout<<"Hello SLAM!"<<std::endl;
	return 0;
}
```
CMD：
```
gedit libHelloSLAM.h
```
编辑“libHelloSLAM.h”，输入：
```
#pragma once

int print_helloSLAM();
```
### 4.3 修改CMakeLists.txt
CMD
```
gedit CMakeLists.txt
```
编辑“CMakeLists.txt”，输入：
```
# cmake version requirment
cmake_minimum_required( VERSION 2.8 )


# project name.
project( helloSLAM )

# generate library file.
add_library( helloSLAM libHelloSLAM.cpp )
```

### 4.4 编译
```
mkdir build_lib
cd build_lib
cmake ..
make
```
得到 **libhelloSLAM.a**
### 4.5 调用
CMD
```
gedit helloSLAM.cpp
```
编辑“helloSLAM.cpp”，输入：
```
#include "libHelloSLAM.h"

int main()
{
	print_helloSLAM();
	return 0;
}
```
修改CMakeLists.txt（和方式1一致）
```
# cmake version requirment
cmake_minimum_required(VERSION 2.8)

# project name.
project( helloSLAM )

# generate exectuable file.
add_executable( helloSLAM helloSLAM.cpp )

target_link_libraries(helloSLAM /home/chan/projects/SLAM/build_lib/libhelloSLAM.a)
```
编译可执行文件
CMD
```
cd .. # 到SLAM目录下
mkdir build
cd build
cmake ..
make
./hellowSLAM
```

## 5 cmake 语法提要
```
# 执行
cmake . # 表示在当前目录下执行 cmake
cmake .. # 表示在前一级目录下执行 cmake
make # 在当前目录下执行 make


# 语法
#1 设置 cmake 版本需求
cmake_minimum_required(VERSION 2.8)

#2 设置工程名
project( 工程名 )

#3 生成可执行文件
add_executable( 可执行文件名 cpp文件 )

#4 生成静态库
add_library( helloSLAM helloSLAM.cpp )

#5 生成共享库



```





