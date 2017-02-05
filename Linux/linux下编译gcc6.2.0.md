linux下编译gcc6.2.0
======

在archlinx的下gcc已经更新到6.2.1了，win10的WSL下还是gcc4.8。官方源没有比较新的版本，于是自己编译使用。

## GCC6的几个新特性
GCC 6 现在的默认值是 C++ 14. GCC 6 现在包括 C++ Concepts.
C++运行时库现在支持特殊的数学函数 (ISO/IEC 29124:2010)
支持 C++17 的实验功能

## 准备

可以去gnu官网下载gcc6.2.0的源码，但国内访问速度比较慢。可以进中科大的镜像站去下载。

## 下载并解压

```
wget http://mirrors.ustc.edu.cn/gnu/gcc/gcc-6.2.0/gcc-6.2.0.tar.bz2
tar -xjvf gcc-6.2.0.tar.bz2
```

解压之后进入源码目录，运行下面命令下载依赖包

`./contrib/download_prerequisites    #必须在源码根目录下运行此命令`

编译gcc前需安装build-essential,bison,flex,texinfo。
生成Makefile

在源码目录下建立一个build目录(也可以在别的目录下)，然后进入build目录运行configure脚本生成Makefile文件。

```
mkdir build && cd build
../configure --prefix=/usr/local/gcc6 --enable-checking=release --enable-languages=c,c++ --enable-threads=posix --disable-multilib
# --prefix=/usr/local/gcc6  指定安装路径
# --enable-languages=c,c++  支持的编程语言
# --enable-threads=posix    使用POSIX/Unix98作为线程支持库
# --disable-multilib        取消多目标库编译(取消32位库编译)
```

下面是archlinux自带gcc的编译配置命令。

```
--prefix=/usr --libdir=/usr/lib --libexecdir=/usr/lib --mandir=/usr/share/man --infodir=/usr/share/info --with-bugurl=https://bugs.archlinux.org/ --enable-languages=c,c++,ada,fortran,go,lto,objc,obj-c++ --enable-shared --enable-threads=posix --enable-libmpx --with-system-zlib --with-isl --enable-__cxa_atexit --disable-libunwind-exceptions --enable-clocale=gnu --disable-libstdcxx-pch --disable-libssp --enable-gnu-unique-object --enable-linker-build-id --enable-lto --enable-plugin --enable-install-libiberty --with-linker-hash-style=gnu --enable-gnu-indirect-function --disable-multilib --disable-werror --enable-checking=release
```

## 编译安装

上一步生成Makefile没有问题后，就可以直接编译安装了。

```
make -j8    #使用8个线程并行编译
make install    #安装(可能需要root权限)
```

## 生成软链接

```
cd /usr/local/bin
ln -s /usr/local/gcc6/bin/gcc gcc6
```