title: 让虚拟机支持安装64位操作系统的处理方法 
date: 2014-08-09 10:59:39
categories: 虚拟机
tags: [虚拟机，64位]
---
尝试虚拟的操作系统是CentOS-5.4-x86_64，因此在系统选项中选择了Other Linux 64bit，尝试系统后系统报错：  

This CPU is VT-capable, but VT is not enabled (check your BIOS settings).
You have configured this virtual machine as a 64-bit guest operating system. However, this host's CPU is not capable of running 64-bit virtual machines or this virtual machine has 64-bit support disabled.
For more detailed information, see http://www.vmware.com/info?id=152

<!--more-->
点击确定，屏幕显示错误信息为：
```
Your CPU does not support long mode. Use a 32bit distribution.
```

对于这种情况，需要设置本机的BIOS，将【Inter Virtualization Technology】选项设置为：**ENABLE**，然后重启系统，就可以了.

**注意**：这里指的是本机的BIOS，而非虚拟机的BIOS，如果BIOS设置了没有类似的设置，那么系统就无法虚拟64位的系统了。