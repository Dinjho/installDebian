### bebian修复引导失败 求助 :sob:！


#### 问题来源

> [ 双系统双硬盘 ] 
先安装WIN11,后安装debian12，grub引导Windows/debian正常，双系统运行良好。重装win11，问题出现：开机直接进入Windows，即使bios中debian的启动项在win之前

#### 解决过程 [未解决 :sob:]

> 参考[Grub EFI分区 —— 修复](https://forums.debiancn.org/t/topic/4805)仍未能解决【应该是我对这篇帖子没有理解到位...】

制作启动盘，选择安装界面时按C，进入grub命令模式。
输入命令——ls：列出磁盘分区，找到根目录所在分区(lvm/debian-root)
![ls](https://github.com/user-attachments/assets/3ef91341-73b5-40b9-92d2-63e18289c582)

```dos
输入
set root=(lvm/debian-root)
set prefix=(lvm/debian-boot)/grub
insmod normal
normal
```
移动光标进入系统 

<img align="center" src="https://github.com/user-attachments/assets/e6556012-d9ff-4ab1-9e02-672b05056f8a"  width="325" style="max-width:100%;" />


> **问题1：进入系统失败，只能进入emergency mode**

<img align="center" src="https://github.com/user-attachments/assets/5c9b38a3-7121-4343-bb7c-35b392b2eb9b"  width="625" style="max-width:100%;" />



输入密码进入emergency mode

```dos
输入
lsblk  #查看块设备     （或fdisk -l查看分区情况）
```

<img style="text-align: center;" src="https://github.com/user-attachments/assets/d51bd565-9a1c-4ee5-accc-c08063cfd134"  width="625" style="max-width:100%;" />

确认`/dev/nvme0n1p1/debian-root`就是ubuntu的根分区/
`/dev/nvme0n1p1/debian-boot`就是boot分区/boot

```dos
输入
sudo update-grub
sudo grub-install /dev/nvme0n1p1   #出现错误提示 找不到EFI目录
```
> **问题2：出现错误提示：找不到EFI目录！**

后面也参考了网上一些帖子，说是没有挂载正确，自己试了很多次，由于不太懂Linux，最终都没成功。

求助：应该如何挂载？为什么只能进入emergency mode？ :sob: :sob: :sob:



to do...
