## Sherpa-Onnx

[k2-fsa/sherpa-onnx: Speech-to-text, text-to-speech, speaker recognition, and VAD using next-gen Kaldi with onnxruntime without Internet connection. Support embedded systems, Android, iOS, Raspberry Pi, RISC-V, x86_64 servers, websocket server/client, C/C++, Python, Kotlin, C#, Go, NodeJS, Java, Swift, Dart, JavaScript, Flutter, Object Pascal, Lazarus, Rust (github.com)](https://github.com/k2-fsa/sherpa-onnx) 

在荔枝派以离线的方式 通过 CPU 运行 `Sherpa-Onnx` 

目标架构是 RISCV 编译架构是 X86_64 所以采用交叉编译的方式



### 交叉编译

[colab/sherpa-onnx/sherpa_onnx_RISC_V.ipynb at master · k2-fsa/colab (github.com)](https://github.com/k2-fsa/colab/blob/master/sherpa-onnx/sherpa_onnx_RISC_V.ipynb) 

可以直接使用 `colab` 执行，也可以选择在 Linux 系统中完成

接下来称编译主机为主机A，荔枝派为主机B

Tip： `colab` 是 Google 的免费云端服务器，使用 Linux 操作系统



#### ToolChain 主机A

```shell
mkdir -p $HOME/toolchain

# 因为荔枝派使用了平头哥的玄铁 CPU 后续的模型也是运行在 CPU 上的
# 所以这里下载针对玄铁 CPU 的工具链
wget -q https://occ-oss-prod.oss-cn-hangzhou.aliyuncs.com/resource//1663142514282/Xuantie-900-gcc-linux-5.10.4-glibc-x86_64-V2.6.1-20220906.tar.gz

tar xf ./Xuantie-900-gcc-linux-5.10.4-glibc-x86_64-V2.6.1-20220906.tar.gz --strip-components 1 -C $HOME/toolchain

# 在当前的 shell 对话中更新环境
# Tip: 如果切换 shell 会话则需要重新执行 EXPORT 命令
export PATH=$HOME/toolchain/bin:$PATH

# =>1
riscv64-unknown-linux-gnu-gcc --version

#############################
riscv64-unknown-linux-gnu-gcc (Xuantie-900 linux-5.10.4 glibc gcc Toolchain V2.6.1 B-20220906) 10.2.0
Copyright (C) 2020 Free Software Foundation, Inc.
This is free software; see the source for copying conditions.  There is NO
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.
#############################

# =>2
riscv64-unknown-linux-gnu-g++ --version

#############################
riscv64-unknown-linux-gnu-g++ (Xuantie-900 linux-5.10.4 glibc gcc Toolchain V2.6.1 B-20220906) 10.2.0
Copyright (C) 2020 Free Software Foundation, Inc.
This is free software; see the source for copying conditions.  There is NO
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.
```



#### Build Sherpa-Onnx 主机A

```shell
sudo apt-get install -y libtool

export PATH=$HOME/toolchain/bin:$PATH

git clone https://github.com/k2-fsa/sherpa-onnx

# 如果编译出现问题  可以考虑切换到仓库的稳定版本
# git checkout acf0975

cd sherpa-onnx
./build-riscv64-linux-gnu.sh
```



#### Check FileFormat 主机A

```shell
file build-riscv64-linux-gnu/install/bin/sherpa-onnx

build-riscv64-linux-gnu/install/bin/sherpa-onnx: ELF 64-bit LSB executable, UCB RISC-V, RVC, double-float ABI, version 1 (GNU/Linux), dynamically linked, interpreter /lib/ld-linux-riscv64-lp64d.so.1, for GNU/Linux 4.15.0, stripped
```



#### Check Dynamic Section 主机A

```shell
readelf -d build-riscv64-linux-gnu/install/bin/sherpa-onnx

Dynamic section at offset 0x13bdc0 contains 31 entries:
  标记        类型                         名称/值
 0x0000000000000001 (NEEDED)             共享库：[libpthread.so.0]
 0x0000000000000001 (NEEDED)             共享库：[libonnxruntime.so.1.14.1]
 0x0000000000000001 (NEEDED)             共享库：[libm.so.6]
 0x0000000000000001 (NEEDED)             共享库：[libstdc++.so.6]
 0x0000000000000001 (NEEDED)             共享库：[libgcc_s.so.1]
 0x0000000000000001 (NEEDED)             共享库：[libc.so.6]
 0x000000000000000f (RPATH)              Library rpath: [$ORIGIN:$ORIGIN/../lib:$ORIGIN/../../../sherpa_onnx/lib]
 0x0000000000000020 (PREINIT_ARRAY)      0x142758
 0x0000000000000021 (PREINIT_ARRAYSZ)    0x8
 0x0000000000000019 (INIT_ARRAY)         0x142760
 0x000000000000001b (INIT_ARRAYSZ)       16 (bytes)
 0x000000000000001a (FINI_ARRAY)         0x142770
 0x000000000000001c (FINI_ARRAYSZ)       8 (bytes)
 0x0000000000000004 (HASH)               0x10280
 0x000000006ffffef5 (GNU_HASH)           0x10980
 0x0000000000000005 (STRTAB)             0x12838
 0x0000000000000006 (SYMTAB)             0x110e0
 0x000000000000000a (STRSZ)              8741 (bytes)
 0x000000000000000b (SYMENT)             24 (bytes)
 0x0000000000000015 (DEBUG)              0x0
 0x0000000000000003 (PLTGOT)             0x14d430
 0x0000000000000002 (PLTRELSZ)           4968 (bytes)
 0x0000000000000014 (PLTREL)             RELA
 0x0000000000000017 (JMPREL)             0x17950
 0x0000000000000007 (RELA)               0x14e30
 0x0000000000000008 (RELASZ)             16008 (bytes)
 0x0000000000000009 (RELAENT)            24 (bytes)
 0x000000006ffffffe (VERNEED)            0x14c50
 0x000000006fffffff (VERNEEDNUM)         6
 0x000000006ffffff0 (VERSYM)             0x14a5e
 0x0000000000000000 (NULL)               0x0
```



#### Tar 主机A

```shell
# 将交叉编译好的文件进行打包
cd ./build-riscv64-linux-gnu
tar cjvf install.tar.bz2 ./install

# 移动到方便操作的路径下
mv install.tar.bz2 ../..
```



### 荔枝派安装

#### 解压 主机B

```
tar xvf install.tar.bz2
cd install/
```



#### 准备链接文件 主机A

```shell
find $HOME/toolchain/ -name ld-linux-riscv64xthead-lp64d.so.1
```

找到目标文件后，通过 `Xftp` 上传到荔枝派中 `/install/lib`

```shell
# 此时在主机 B 的环境中检查
ls -la

-rw-r--r-- 1 sipeed sipeed  1243928 Oct 16 15:31 ld-linux-riscv64xthead-lp64d.so.1
-rw-r--r-- 1 sipeed sipeed   261840 Oct 16 15:08 libespeak-ng.so
-rw-r--r-- 1 sipeed sipeed    72408 Oct 16 15:08 libkaldi-decoder-core.so
-rw-r--r-- 1 sipeed sipeed    68232 Oct 16 15:08 libkaldi-native-fbank-core.so
-rw-r--r-- 1 sipeed sipeed 13277720 Oct 16 14:14 libonnxruntime.so
-rw-r--r-- 1 sipeed sipeed 13277720 Oct 16 14:14 libonnxruntime.so.1.14.1
lrwxrwxrwx 1 sipeed sipeed       23 Oct 16 15:08 libpiper_phonemize.so -> libpiper_phonemize.so.1
lrwxrwxrwx 1 sipeed sipeed       27 Oct 16 15:08 libpiper_phonemize.so.1 -> libpiper_phonemize.so.1.2.0
-rw-r--r-- 1 sipeed sipeed   404456 Oct 16 15:08 libpiper_phonemize.so.1.2.0
-rw-r--r-- 1 sipeed sipeed  1306496 Oct 16 15:08 libsherpa-onnx-core.so
lrwxrwxrwx 1 sipeed sipeed       23 Oct 16 15:08 libsherpa-onnx-fst.so -> libsherpa-onnx-fst.so.6
-rw-r--r-- 1 sipeed sipeed  1394904 Oct 16 15:08 libsherpa-onnx-fst.so.6
-rw-r--r-- 1 sipeed sipeed   769200 Oct 16 15:08 libsherpa-onnx-kaldifst-core.so
-rw-r--r-- 1 sipeed sipeed   206712 Oct 16 15:08 libucd.so
drwxr-xr-x 2 sipeed sipeed     4096 Oct 16 15:08 pkgconfig

# 更新 Linux 的链接环境
export LD_LIBRARY_PATH=$PWD/lib:$LD_LIBRARY_PATH
```



#### 执行链接脚本 主机B

```shell
./bin/sherpa-onnx
```



### 执行效果



#### 下载模型 主机B

[Release asr-models · k2-fsa/sherpa-onnx (github.com)](https://github.com/k2-fsa/sherpa-onnx/releases/tag/asr-models) 

针对荔枝派选择小模型 [sherpa-onnx-streaming-zipformer-small-bilingual-zh-en-2023-02-16.tar.bz2](https://github.com/k2-fsa/sherpa-onnx/releases/download/asr-models/sherpa-onnx-streaming-zipformer-small-bilingual-zh-en-2023-02-16.tar.bz2) 

```shell
tar xvf sherpa-onnx-streaming-zipformer-small-bilingual-zh-en-2023-02-16.tar.bz2

# 把模型文件拷贝到执行文件夹中
cd install/
cp bin/sherpa-onnx sherpa-onnx-streaming-zipformer-small-bilingual-zh-en-2023-02-16.tar.bz2
cp bin/sherpa-onnx-alsa sherpa-onnx-streaming-zipformer-small-bilingual-zh-en-2023-02-16.tar.bz2

cd 96/
```



#### 运行测试用例 主机B

执行模型里写入的音频测试文件 `0.wav`

```shell
# Tip: 复制的时候一行行复制 不要把换行符复制进去
../sherpa-onnx
--encoder=./encoder-epoch-99-avg-1.int8.onnx
--decoder=./decoder-epoch-99-avg-1.onnx
--joiner=./joiner-epoch-99-avg-1.int8.onnx
--tokens=./tokens.txt
--num-threads=4
../test_wavs/0.wav
```

如果是荔枝派运行，由于 CPU 算力有限，一般线程设置在 3 个可以做到 RTF < 1

在此基础上，如果是内存版本 8 + 32 G 的荔枝派，可以设置在 4 个线程，3 个线程的效果很一般



#### 查看语音 IO 设备 主机B

```shell
# 列出当前使用的所有音频设备  选择 card 和 device 编号
arecord -l

# 根据信息可以看出是 card-0 device-1
**** List of CAPTURE Hardware Devices ****
xcb_connection_has_error() returned true
card 0: LightSoundCard [Light-Sound-Card], device 1: light-i2s-dai-ES7210 ADC 0 es7210.6-0040-1 [light-i2s-dai-ES7210 ADC 0 es7210.6-0040-1]
  Subdevices: 1/1
  Subdevice #0: subdevice #0

# 根据 IO 设备号选择
../sherpa-onnx-alsa
--encoder=./encoder-epoch-99-avg-1.int8.onnx
--decoder=./decoder-epoch-99-avg-1.onnx
--joiner=./joiner-epoch-99-avg-1.int8.onnx
--tokens=./tokens.txt
--num-threads=4
plughw:0,1
```



```
../sherpa-onnx-alsa --encoder=./encoder-epoch-99-avg-1.int8.onnx --decoder=./decoder-epoch-99-avg-1.onnx --joiner=./joiner-epoch-99-avg-1.int8.onnx --tokens=./tokens.txt --num-threads=4 plughw:0,1
```



```
sherpa-onnx-streaming-zipformer-small-bilingual-zh-en-2023-02-16/
```

