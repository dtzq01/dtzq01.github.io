---
layout: post
title: "编译后配置rpath, 链接指定的库文件"
categories: linux build
tags: compile
---
### 背景
系统中针对同样的库，存在多个版本，可执行文件需要指定特定的版本的.so才能执行。

### 方法
1. 使用patchelf --print-rpath $filename查看文件的run path。
2. 将需要的特定的版本的.so运行路径指定给patchelf --set-rpath $so_path $filename。

```
code@code:~/code/ins_dir/sbin$ patchelf --print-rpath ./iptables
/home/code/code/ipt/../ins_dir/lib
code@code:~/code/ins_dir/sbin$ patchelf --set-rpath /lib/ ./iptables
code@code:~/code/ins_dir/sbin$ patchelf --print-rpath ./iptables
/lib/
code@code:~/code/ins_dir/sbin$ ldd ./iptables
        linux-vdso.so.1 (0x00007ffe1e390000)
        libip4tc.so.2 => /lib/libip4tc.so.2 (0x00007fa5d7786000)
        libip6tc.so.2 => /lib/libip6tc.so.2 (0x00007fa5d757e000)
        libxtables.so.12 => /lib/libxtables.so.12 (0x00007fa5d736d000)
        libc.so.6 => /lib/x86_64-linux-gnu/libc.so.6 (0x00007fa5d6f7c000)
        libdl.so.2 => /lib/x86_64-linux-gnu/libdl.so.2 (0x00007fa5d6d78000)
        /lib64/ld-linux-x86-64.so.2 (0x00007fa5d7c0c000)
code@code:~/code/ins_dir/sbin$ 
```