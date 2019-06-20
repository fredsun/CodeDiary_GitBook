---
description: FFmpeg的Android初探
---

# FFmpeg Android

## 

**FFmpeg准备**

1. 更新apt-get, 需要sudo

   ```text
   apt-get update
   apt-get install yasm
   apt-get install pkg-config
   ```

2. 安装ndk r19c

   ```text
   https://dl.google.com/android/repository/android-ndk-r19c-linux-x86_64.zip
   unzip android-ndk-r19c-linux-x86_64.zip
   ```

**FFmpeg 准备**

1. 下载FFmpeg 4.1.3

   ```text
   https://github.com/FFmpeg/FFmpeg/archive/n4.1.3.tar.gz
   tar -zxvf n4.1.3.tar.gz
   ```

2. 编译FFmpeg

   * 4.1 修改 configure

     ```text
     SLIBNAME_WITH_MAJOR='$(SLIBNAME).$(LIBMAJOR)'  
     LIB_INSTALL_EXTRA_CMD='$$(RANLIB)"$(LIBDIR)/$(LIBNAME)"'  
     SLIB_INSTALL_NAME='$(SLIBNAME_WITH_VERSION)'  
     SLIB_INSTALL_LINKS='$(SLIBNAME_WITH_MAJOR)$(SLIBNAME)'
     ```

     修改为

     ```text
     SLIBNAME_WITH_MAJOR='$(SLIBPREF)$(FULLNAME)-$(LIBMAJOR)$(SLIBSUF)'  
     LIB_INSTALL_EXTRA_CMD='$$(RANLIB)"$(LIBDIR)/$(LIBNAME)"'  
     SLIB_INSTALL_NAME='$(SLIBNAME_WITH_MAJOR)'  
     SLIB_INSTALL_LINKS='$(SLIBNAME)'
     ```

   * 4.2 新建build.sh

     \`\`\`

     **!/bin/bash**

     NDK=/home/Documents/NDK/android-ndk-r15c

     ADDI\_LDFLAGS="-fPIE -pie"

     ADDI\_CFLAGS="-fPIE -pie -march=armv7-a -mfloat-abi=softfp -mfpu=neon"

     CPU=armv7-a

     ARCH=arm

     HOST=arm-linux

     SYSROOT=$NDK/toolchains/llvm/prebuilt/linux-x86\_64/sysroot

     CROSS\_PREFIX=$NDK/toolchains/llvm/prebuilt/linux-x86\_64/bin/armv7a-linux-androideabi21-

     PREFIX=$\(pwd\)/android/$CPU

     x264=$\(pwd\)/x264/android/$CPU

   configure\(\) { ./configure  --prefix=$PREFIX  --disable-encoders  --disable-decoders  --disable-avdevice  --disable-static  --disable-doc  --disable-ffplay  --disable-network  --disable-doc  --disable-symver  --enable-neon  --enable-shared  --enable-libx264  --enable-gpl  --enable-pic  --enable-jni  --enable-pthreads  --enable-mediacodec  --enable-encoder=aac  --enable-encoder=gif  --enable-encoder=libopenjpeg  --enable-encoder=libmp3lame  --enable-encoder=libwavpack  --enable-encoder=libx264  --enable-encoder=mpeg4  --enable-encoder=pcm\_s16le  --enable-encoder=png  --enable-encoder=srt  --enable-encoder=subrip  --enable-encoder=yuv4  --enable-encoder=text  --enable-decoder=aac  --enable-decoder=aac\_latm  --enable-decoder=libopenjpeg  --enable-decoder=mp3  --enable-decoder=mpeg4\_mediacodec  --enable-decoder=pcm\_s16le  --enable-decoder=flac  --enable-decoder=flv  --enable-decoder=gif  --enable-decoder=png  --enable-decoder=srt  --enable-decoder=xsub  --enable-decoder=yuv4  --enable-decoder=vp8\_mediacodec  --enable-decoder=h264\_mediacodec  --enable-decoder=hevc\_mediacodec  --enable-ffmpeg  --enable-bsf=aac\_adtstoasc  --enable-bsf=h264\_mp4toannexb  --enable-bsf=hevc\_mp4toannexb  --enable-bsf=mpeg4\_unpack\_bframes  --enable-cross-compile  --cross-prefix=$CROSS\_PREFIX  --target-os=android  --arch=$ARCH  --sysroot=$SYSROOT  --extra-cflags="-I$x264/include $ADDI\_CFLAGS"  --extra-ldflags="-L$x264/lib $ADDI\_LDFLAGS" }

   build\(\) { make clean

   configure make -j4 make install }

   build

```text
  查看所有编译配置选项：./configure --help
  查看支持的解码器：./configure --list-decoders
  查看支持的编码器：./configure --list-encoders
  查看支持的硬件加速：./configure --list-hwaccels
  1. build之前需要执行 ```./configure
```

1. 赋予脚本执行权限：`chmod +x build.sh`
2. 执行脚本开始编译：`./build.sh`

**报错汇总**

**nasm/yasm not found or too old. Use --disable-x86asm for a crippled build**

* 未安装yasm

**权限问题**

```text
mkdir: cannot create directory ‘/usr/local/share/man/man1’: Permission denied
doc/Makefile:125: recipe for target 'install-man' failed
make: *** [install-man] Error 1
```

* 解决: 以为root权限编译

**ffbuild/config.mak: No such file or directory参考github**

* 执行.sh前未执行`./configure`

