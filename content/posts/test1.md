---
title: "Test1"
date: 2020-11-30T20:14:45+08:00
draft: true
---

# CmakeLists
###### tags: `linux2020`
## How to compile the program?
```
$ gcc yourProgram.c -o executableFileName
$ g++ yourprogram.cpp -o executableFileName
```
## How to compile the program with Library?
If you wanna link the lib to your program in linux, e.g. libmath, you should add **-lm** in the command. **l** means lib and **m** means math.
```
$ gcc doMath.c -o doMath -lm
```
Advanced example, e.g. linking openCV with your cpp file:
```
$ g++ -L/usr/local/lib -o Contour ContourTest.cpp -lopencv_core -lopencv_imgproc -lopencv_highgui -lopencv_imgcodecs
```
**-L** can change your library path.
## How to write a simple CMakeLists?
Assume that you only have **doMath.cpp** without any header file.
```
$ touch CMakeLists.txt
$ vim CMakeLists.txt
```
```bash=
# Minimum cmake version which required.
cmake_minimum_required( VERSION 2.8)

# Your project name, e.g. DoMath.
project(DoMath)

# Generate executable file, e.g. doMath.
add_executable( doMath doMath.cpp )
```
```
$ cmake .
$ make 
$ ./doMath
```
That's all, easy~ right?
But what if you have more files then this~ For example, you own the whole program folder like below:
```
FloatOptmizer/
            |---src/
            |    |---FloatLoader.cc
            |    |---FloatGenerator.cc   
            |    |---demo.cc
            |---header/
            |    |---FloatLoader.h
            |    |---FloatGenerator.h   
            |    |---demo.h
            |---build/
            CMakeLists.txt
```
Look at the **line 8**, set CMAKE_BUILD_TYPE to **Debug type** is important, because the default CMAKE_BUILD_TYPE is **None type** which will showing nothing when you compile(cmake) your program. Change it to debug type can print the error or warning information if any exception occurred. Notice line 10 and line 12, the different between **static library** and **shared lilbrary**. You should know the library have three types: **static, shared, dynamic-loaded**. [Reference](https://blog.xuite.net/tzeng015/twblog/113272198-Library%E5%8F%AF%E5%88%86%E6%88%90%E4%B8%89%E7%A8%AE%EF%BC%8Cstatic%E3%80%81shared%E8%88%87dynamically+loaded%E3%80%82)
```bash=
# Minimum cmake version which required.
cmake_minimum_required( VERSION 2.8)

# Your project name, e.g. FloatOptmizer.
project( FloatOptmizer )

# Setting build type to "Debug" which is important.
set( CMAKE_BUILD_TYPE "Debug" )

# Add your code file as STATIC library.
add_library( floatloader src/FloatLoader.cc )

# Add your code file as SHARED library.
add_library( floatloader_shared SHARED src/FloatLoader.cc src/FloatGenerator.cc ) 

# Generate executable file.
add_executable( demo src/demo.cc )

# Link your DYNAMIC library.
target_link_libraries( demo floatloader_shared )
```
```
$ cd build
$ cmake ..
$ make 
$ ./demo
```
The cmake compiling info. :
```mediawiki=
-- The C compiler identification is GNU 7.5.0
-- The CXX compiler identification is GNU 7.5.0
-- Check for working C compiler: /usr/bin/cc
-- Check for working C compiler: /usr/bin/cc -- works
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Detecting C compile features
-- Detecting C compile features - done
-- Check for working CXX compiler: /usr/bin/c++
-- Check for working CXX compiler: /usr/bin/c++ -- works
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Configuring done
-- Generating done
-- Build files have been written to: /home/huang/float_process/build
```
The make compiling info. :
```mediawiki=
Scanning dependencies of target floatloader_shared
[ 14%] Building CXX object CMakeFiles/floatloader_shared.dir/src/FloatLoader.cc.o
[ 28%] Building CXX object CMakeFiles/floatloader_shared.dir/src/FloatGenerator.cc.o
[ 42%] Linking CXX shared library libfloatloader_shared.so
[ 42%] Built target floatloader_shared
Scanning dependencies of target demo
[ 57%] Building CXX object CMakeFiles/demo.dir/src/demo.cc.o
[ 71%] Linking CXX executable demo
[ 71%] Built target demo
Scanning dependencies of target floatloader
[ 85%] Building CXX object CMakeFiles/floatloader.dir/src/FloatLoader.cc.o
[100%] Linking CXX static library libfloatloader.a
[100%] Built target floatloader
```
