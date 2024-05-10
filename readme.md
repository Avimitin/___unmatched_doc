## unmatched openocd 远程调试指南

1. 将 Micro USB A 数据线将 unmatched UART 口和电脑相连
2. 下载仓库内的 openocd.cfg 文件
3. 下载 openocd 程序，执行命令 `openocd -f openocd.cfg`
4. 下载 riscv64 gdb，执行
5. 在 gdb 内执行 `:target extended-remote :3333` 连接上 gdb server

## Eclipse 内

1. 项目内选择 debugger 页面
2. GDB Debugger 一栏选择 riscv64 的 gdb
3. 在 Debugger Option 内的 Connection 栏目，填入端口 3333

## 远程调试

此处假设有两台设备，其中设备 A 是一台 x86-64 Linux，IP 为 192.168.114.51，
设备 B 是一台 riscv64gc Linux 机器，IP 为 192.168.114.52。

测试程序使用目录内的 test.cpp 文件。

首先可以在设备 A 用 cross compile 交叉编译 test.cpp，也可以在设备 B 上 native 编译。

- 交叉编译

```console
$ uname -m
x86_64
$ riscv64-linux-gnu-g++ -g test.cpp -o test
```

- 原生编译

```console
$ uname -m
riscv64
$ g++ -g test.cpp -o test
```

如果是交叉编译，把编译出来的 elf 文件用 scp 或者 rsync 发送到设备 B 上。
如果是原生编译可以直接运行 gdb server。

```bash
# 发送到家目录下
scp test 192.168.115.52:~/
```

登录设备 B，运行 gdbserver

```bash
ssh 192.168.115.52
# 监听本机端口，接受所有设备的访问
gdbserver :3333 ./test
```

在设备 A 启动 gdb multi-arch 或者 gdb riscv64

```bash
riscv64-linux-gnu-gdb
```

在 gdb 的 console 内，输入设备 B 的 IP 和端口连接远端服务器：

```gdb
gdb> target extended-remote 192.168.115.52:3333
```

一些简单的 GDB 调试演示：

```gdb
gdb> break test.cpp:6
gdb> continue
gdb> print i
0
gdb> continue
gdb> print n[0]
100
```

如果要退出 debug，在设备 A 的 gdb console 输入：

```gdb
gdb> monitor exit
```
