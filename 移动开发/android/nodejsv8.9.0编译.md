
# mac 编译最新版nodejs v8.9.0
1. 生成 android 构建环境
```bash
./android-configure $ANDROID_NDK_HOME
```
2. 安装支持zlib库的 python，并copy到android_toolschain 目录下

3. 替换 out/Makefile 相关内容
```makefile
CC.target ?= /Users/zhufeng/test/1/node-v8.9.0/android-toolchain/bin/arm-linux-androideabi-clang
CFLAGS.target ?= $(CPPFLAGS) $(CFLAGS)
CXX.target ?= /Users/zhufeng/test/1/node-v8.9.0/android-toolchain/bin/arm-linux-androideabi-clang++
CXXFLAGS.target ?= $(CPPFLAGS) $(CXXFLAGS) -std=c++11 -stdlib=libc++ -isystem /Users/zhufeng/test/node-v8.9.0/android-toolchain/include/c++/4.9.x/
LINK.target ?= /Users/zhufeng/test/1/node-v8.9.0/android-toolchain/bin/arm-linux-androideabi-clang++
LDFLAGS.target ?= $(LDFLAGS)
AR.target ?= /Users/zhufeng/test/1/node-v8.9.0/android-toolchain/bin/arm-linux-androideabi-ar

# C++ apps need to be linked with g++.
LINK ?= $(CXX.target)

# TODO(evan): move all cross-compilation logic to gyp-time so we don't need
# to replicate this environment fallback in make as well.
CC.host ?= gcc
CFLAGS.host ?= $(CPPFLAGS_host) $(CFLAGS_host)
CXX.host ?= g++
CXXFLAGS.host ?= $(CPPFLAGS_host) $(CXXFLAGS_host)
LINK.host ?=g++
LDFLAGS.host ?=
AR.host ?= ar
```
4. 搜索 out/*.mk 删除一下定义
```
-D_FILE_OFFSET_BITS=64'
```

5. 搜索 out/*.mk 替换 "-std=gnu10x"为
```
-std=c++11
```

6. 添加导入库文件, out/node.target.mk
```
 -static-libstdc++
```

