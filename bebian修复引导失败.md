### bebian修复引导失败 【已解决】



> update 1/20/2025 问题已解决 感谢[Zh-Rvw](https://forums.debiancn.org/t/topic/4805)


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



-------------update 2025/1/25-----------
> 按照大佬Zh-Rvw的回复：
  <img style="text-align: center;" src="[https://github.com/user-attachments/assets/d51bd565-9a1c-4ee5-accc-c08063cfd134](https://github.com/user-attachments/assets/a6d5c7cc-e3d6-4ab2-9849-617e535bbb11)"  width="625" style="max-width:100%;" />

具体方法如下：

- u盘启动，安装界面进入rescue mode。[非grub命令模式]
  
- 进入rescue mode后的界面跟安装debian的界面是一样的，一直next，直到出现提示需要mount系统的跟分区/,efi分区,直到出现命令行


- 输入blkid，找到/dev/nvme1n1p1 并记录UUID
  ```
  输入
  blkid
  ```
  > 为什么是/dev/nvme1n1p1？ 这个分区明明是Microsoft的EFI分区
   <img style="text-align: center;" src="https://github.com/user-attachments/assets/0d913965-d15c-4a43-95a9-eb5104f5159a"  width="625" style="max-width:100%;" />

- 修改/etc/fstab 文件
  ```
  输入
  nano /etc/fstab
  ```
  <img src="https://github.com/user-attachments/assets/57866999-16d9-4d0c-abe7-420c8651fb8f" width="625" style="max-width:100%;" />

> 图中黄线提示/boot/efi在/dev/nvme1n1p1
  修改红线提示的UUID为前面记录的UUID

- 重新挂载并运行grub指令
  ```
  sudo mount -a （重新挂载分区）
  update-grub
  grub-install /dev/nvme0n1p1
  ```
- 重启 进入系统
> 以防万一，进系统后我又执行了`sudo update-grub sudo` `grub-install /dev/nvme0n1p1`指令
  <img src="https://github.com/user-attachments/assets/d6967400-dd5a-4505-9c25-729300f8987c" width="625" style="max-width:100%;" />

大功告成！再次感谢 [Zh-Rvw](https://forums.debiancn.org/t/topic/4805)



--------------------to do----------------------------------------

疑问

第一次尝试修复Grub引导的时候，错误提示“找不到EFI目录” 但我在Debian的根目录下查看/boot/efi目录是一直存在的 cd到efi查看其文件也都存在 所以这个错误提示困扰了我好久。

现在再明白这个"找不到EFI"指的是找不到Microsoft的EFI，而非Debian的efi。因为重装win11，原来的EFI肯定是找不到的。按照您的建议修改/etc/fstab 文件，其中的/boot/efi行内容修改成了/dev/nvme1n1p1的UUID，这个分区是Microsoft的EFI分区。

不知道我这样理解对不对哦 :sweat:





