# 刷回官方固件

这里要说明一下301w的固件系统分区结构，由于301w是双存储模式，emmc中保存了路由器操作系统，9分区结构如下:

```bash
/dev/mmcblk0p1 --- "0:HLOS" --- 16MB --- 系统1 kernel 分区
/dev/mmcblk0p2 --- "0:HLOS_1" --- 16MB --- 系统2 kernel 分区
/dev/mmcblk0p3 --- "0:HLOS_2" --- 16MB --- 系统3 kernel 分区（空的，官方未刷入固件）
/dev/mmcblk0p4 --- "rootfs" --- 512MB --- 系统1 rootfs 分区
/dev/mmcblk0p5 --- "rootfs_1" --- 512MB --- 系统2 rootfs 分区
/dev/mmcblk0p6 --- "rootfs_2" --- 512MB --- 系统3 rootfs 分区（空的，官方未刷入固件）
/dev/mmcblk0p7 --- "0:WIFIFW" --- 4MB --- 无线的firmware分区
/dev/mmcblk0p8 --- "reserved" --- 16MB --- 保留分区（没用的）
/dev/mmcblk0p9 --- "rootfs_data" --- 2.1GB --- rootfs的数据分区，多个官方系统共用的数据分区，目前openwrt固件没有使用该分区
```
系统1和系统2的分区（kernel+rootfs）是互为备份的，理论上是一毛一样的。

前面提到的`sudo fw_setenv current_entry 1`即是切换到系统2，同理`sudo fw_setenv current_entry 0`即使切换到系统1

恢复官方固件可以在前面刷机教程提到的把备份的/dev/mmcblk0p1和/dev/mmcblk0p4 dd命令刷回301w的这个分区即可，如果之前没备份但是系统2还在也可以刷回去
例如`dd if=/dev/mmcblk0p2 of if=/dev/mmcblk0p1`这样就把系统2的kernel分区复制一份到系统1的kernel分区中了，同理rootfs`dd if=/dev/mmcblk0p5 of if=/dev/mmcblk0p4`

**但是注意一点，你刷系统1分区一定要切换到系统2下操作**
