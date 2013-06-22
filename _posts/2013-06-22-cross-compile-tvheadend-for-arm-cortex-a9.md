---
layout: post
title: cross-compile Tvheadend for ARM Cortex A9
tags:
- cross-compile
- Tvheadend
- ARM
status: publish
type: post
published: true
---
This will be the first in a series of posts as I get
[Tvheadend](https://tvheadend.org/projects/tvheadend),
[libhdhomerun](https://github.com/jzerbe/libhdhomerun), and
[dvbhdhomerun](https://github.com/jzerbe/dvbhdhomerun) running on my
[XIOS DS Media Play](http://www.pivosgroup.com/xios.html).
I am doing all of the building on a Debian 7 VM.

setup CROSS_COMPILE env
------------
I attempted to roll a cross-compile environment in Cygwin for several hours
and gave up. Everything was so much easier on a Debian 7 VM.

1. <http://www.mentor.com/embedded-software/sourcery-tools/sourcery-codebench/editions/lite-edition/arm-gnu-linux>
2. Run the installer. Something like: `./arm-2013.05-24-arm-none-linux-gnueabi.bin`
3. In `~/.profile`, stick in:
`PATH="/root/CodeSourcery/Sourcery_CodeBench_Lite_for_ARM_GNU_Linux/bin:$PATH"`
... or wherever you pointed the installer to.

zlib + OpenSSL to match ARM buildroot
------------
I based my approach on:
<http://stackoverflow.com/questions/11841919/cross-compile-openssh-for-arm/14541123#14541123>

    cd /opt
    wget http://zlib.net/zlib-1.2.5.tar.gz
    tar xzvf zlib-1.2.5.tar.gz
    cd zlib-1.2.5
    CC=arm-none-linux-gnueabi-gcc
    ./configure --prefix=/opt/zlib-arm
    make
    make install

    cd /opt
    wget http://www.openssl.org/source/openssl-1.0.0a.tar.gz
    tar xzvf openssl-1.0.0a.tar.gz
    cd openssl-1.0.0a
    export cross=arm-none-linux-gnueabi-
    ./Configure dist --prefix=/opt/openssl-arm
    make CC="${cross}gcc" AR="${cross}ar r" RANLIB="${cross}ranlib"
    make install

build/package Tvheadend
------------
Standard instructions: <https://github.com/tvheadend/tvheadend/blob/master/README.md>

    cd /opt
    git clone --depth 1 https://github.com/tvheadend/tvheadend.git
    cd tvheadend
    ln -s /opt/openssl-arm/include/openssl/ openssl
    ln -s /opt/zlib-arm/include/zlib.h zlib.h
    ln -s /opt/zlib-arm/include/zconf.h zconf.h
    ./configure --cc=arm-none-linux-gnueabi-gcc --host=armle-unknown-linux --target=armle-unknown-linux --build=i686-pc-linux --disable-avahi --release

    cd /opt/tvheadend
    cp build.linux/tvheadend .
    cd ..
    zip -r tvheadend.zip tvheadend
