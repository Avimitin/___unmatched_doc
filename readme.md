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

