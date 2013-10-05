---
layout: post
title: cross-compile PCSX-Reloaded for ARM Cortex A9
tags:
- cross-compile
- MESS
- ARM
published: false
---
http://www.x.org/wiki/CrossCompilingXorg/
--> zlib cross

then libpng:
http://prdownloads.sourceforge.net/libpng/libpng-1.6.3.tar.gz
tar xzvf libpng-1.6.3.tar.gz
cd libpng-1.6.3
CC=arm-none-linux-gnueabi-gcc CPPFLAGS="-I/opt/zlib-arm/include" ./configure --prefix=/opt/libpng-arm --target=arm-linux --host=i686-linux
make

then expat
wget http://sourceforge.net/projects/expat/files/expat/2.1.0/expat-2.1.0.tar.gz
CC=arm-none-linux-gnueabi-gcc CPPFLAGS="-I/opt/zlib-arm/include" ./configure --prefix=/opt/expat-arm --target=arm-linux --host=i686-linux

apt-get install jhbuild
export PATH="/opt/CodeSourcery/Sourcery_CodeBench_Lite_for_ARM_GNU_Linux/bin:/opt/CodeSourcery/Sourcery_CodeBench_Lite_for_ARM_EABI/bin:$PATH"
export CROSS_COMPILE=arm-none-linux-gnueabi-
export DISCIMAGE=/opt/xorg-arm
jhbuild build xserver
--> hit 2 through, just need devel files

--> pcsx
CC=arm-none-linux-gnueabi-gcc CPPFLAGS="-I/opt/zlib-arm/include -I/opt/xorg-arm/usr/local/include" ./configure --target=arm-linux --host=i686-linux


