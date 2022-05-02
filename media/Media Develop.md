# Media Develop

## 直播

## 播放器

## 短视频

## 视频通话

## FFmpeg

### 编译

- 编译环境

  操作系统：Ubuntu Server 20.04 LTS 64bit

  ndk版本：ndk;24.0.8215888

  ffmpeg版本：5.1

  编译脚本

  ```sh
  #!/bin/bash
  for abi in arm64-v8a armeabi-v7a x86 x86-64
  do
  
  export NDK=/home/ubuntu/sdk/ndk/24.0.8215888
  export ABI=$abi
  export PREFIX=/home/ubuntu/output/$ABI
  # Only choose one of these, depending on your build machine...
  # export TOOLCHAIN=$NDK/toolchains/llvm/prebuilt/darwin-x86_64
  export TOOLCHAIN=$NDK/toolchains/llvm/prebuilt/linux-x86_64
  export SYSROOT=$TOOLCHAIN/sysroot
  
  # Only choose one of these, depending on your device...
  # export TARGET=aarch64-linux-android
  # export TARGET=armv7a-linux-androideabi
  # export TARGET=i686-linux-android
  # export TARGET=x86_64-linux-android
  case "$abi" in
          "arm64-v8a")
                  export TARGET=aarch64-linux-android
                  ;;
          "armeabi-v7a")i
                  export TARGET=armv7a-linux-androideabi
                  ;;
          "x86")
                  export TARGET=i686-linux-android
                  ;;
          "x86-64")
                  export TARGET=x86_64-linux-android
                  ;;
          *)
                  exit 1
                  ;;
  esac
  
  # Set this to your minSdkVersion.
  export API=21
  
  # Configure and build.
  export AR=$TOOLCHAIN/bin/llvm-ar
  export CC=$TOOLCHAIN/bin/$TARGET$API-clang
  export AS=$CC
  export CXX=$TOOLCHAIN/bin/$TARGET$API-clang++
  export LD=$TOOLCHAIN/bin/ld
  export RANLIB=$TOOLCHAIN/bin/llvm-ranlib
  export STRIP=$TOOLCHAIN/bin/llvm-strip
  ./configure \
          --disable-asm \
          --disable-doc \
          --prefix=$PREFIX \
          --enable-shared \
          --enable-static \
          --disable-programs \
          --cc=$CC \
          --cxx=$CXX \
          --strip=$STRIP \
          --enable-cross-compile
  
  make clean
  make -j8
  make install
  
  
  done
  ```

  