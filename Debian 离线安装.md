Debian 离线安装



- 下载[完整包](https://cdimage.debian.org/debian-cd/current/amd64/iso-dvd/)（约4G）离线安装（在线安装大概率出现网络问题）

- 制作U盘启动盘（Rfuse）

- 分区：选择Manual手动分区根据[官方手册](https://www.debian.org/releases/stable/armel/apcs03.zh-cn.html)分出以下4个

  ![image-20250113162037315](C:/Users/D1njh/AppData/Roaming/Typora/typora-user-images/image-20250113162037315.png)

​	（其中/boot的分区类型为xfs）