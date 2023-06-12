# ubuntu20.04 系统迁移

## 双系统从SSD到新SSD

参考链接：

https://juejin.cn/post/6952523655838433311

1. 用U盘制作ubuntu20.04启动盘

2. 进入bios选择U盘启动，grub第二项，进去后再选择try ubuntu

3. 用gpart格式化新ssd，分四个区：/boot(20还是否需要有待考证)；swap；/；/home

4. 分区格式和原分区一致；swap是linux swap格式，其余都是ext4格式

5. 使用`dd`命令来进行字节级别的迁移（dd会把UUID一起复制）：

   若，根目录所在的分区是/dev/nvme0n1p5，新硬盘划分的是/dev/nvme1n1p2,则：

   ````shell
   sudo dd if=/dev/nvme0n1p5 of=/dev/nvme1n1p2
   ````

6. 依次dd /；/home；/boot。swap不用复制

7. 更新新的UUID

8. 挂载新的/目录(这里是挂载到/root上)：

   ````shell
   sudo mount /dev/nvme1n1p2 /root
   ````

9. 更新/etc/fstab文件，更新UUID

   ````
   sudo gedit /root/etc/fstab
   ````

10. 直接推荐使用 boot-repair 工具:

    ````
    sudo add-apt-repository ppa:yannubuntu/boot-repair
    sudo apt install -y boot-repair
    
    boot-repair
    ````

11. 选择高级选项，手动指定新的引导盘，然后按提示修复引导
12. 修复完毕，重启再进原系统

------

注意：

迁移后出现NVIDIA驱动没了需要重新安装（最后猜测可能是BIOS的快速启动忘关了）