Release note:

- 2017 March 10  加入CPU task group 调度相关的fix
https://pan.baidu.com/s/1pKOTHNX

md5sum kernel-3.10.0-327.13.1.el7.2.ppc64le.rpm
bfd9213878c86fa2785548f3b3c6d25f  kernel-3.10.0-327.13.1.el7.2.ppc64le.rpm

CPU task group调度有个的bug会导致跑SPARK/YARN call load时机器会重启。这个不是powerpc所特有的问题。
upstream的commit:
094f469172e0 sched/fair: Initialize throttle_count for new task-groups lazily
754bd598be9b sched/fair: Do not announce throttled next buddy in dequeue_task_fair()
Note:  这个功能还未在实际load上验证，但是7.3的内核（包含这个fix)已经被验证过。


- 2016 Dec 26 [改动]i40e驱动为kmod方式，i40e的内部版本升级为1.5.10k 
https://pan.baidu.com/s/1kVsO75d

$ md5sum kmod-i40e-1.5.10_k-2.el7.ppc64le.rpm
8a9b4dc5f4a6bd26ac67438050782b8b  kmod-i40e-1.5.10_k-2.el7.ppc64le.rpm 
[改动]
lshw更新devicetree: Parse board serial number
https://pan.baidu.com/s/1kVsO75d


- 2016 Nov 14 更新i40e网卡
1.因为kernel升级到3.10.0-327.13.1.el7.1，需要rebuild i40e网卡。
更新的i40e在这里 https://pan.baidu.com/s/1kVsO75d
[root@localhost ppc64le]# md5sum i40e-1.4.25-1.ppc64le.rpm 
40c06cbc13455be8a809db0d1db1a6c6 i40e-1.4.25-1.ppc64le.rpm


- 2016 Oct 5 
1. 网卡后2个端口的disable [改动] eth0、1、2、3 禁用2.3口 删除ifconfig
-a中的网口 echo "0002:01:00.2" >  /sys/bus/pci/drivers/i40e/unbind 删除
lspci的网口: echo 1 > /sys/bus/pci/devices/0002:01:00.2/remove
请注意0002:01:00.2 是该网卡的pciid
请把上面2个命令加入一个自定义的systemd的service 
[测试]
tlinux(centos72)启动后，看ifconfig和lspci的结果，eth2，3的内容应该消失了

2.更新lshw rpm [改动] 当enclosure设备的procfs entry不存在时不会coredump 
[测试]
插上lsi卡
rpm -ivh lshw.rpm
运行lshw不会coredump 

链接: https://pan.baidu.com/s/1kVsO75d
4680642251f41256c0d27a91c3171d9e  lshw-B.02.18.42-2.ppc64le.rpm


更新的kernel.rpm 
链接: https://pan.baidu.com/s/1kVsO75d

md5sum 3ea3b9c6b3f7da561ed9d7fe6e2a19a3
kernel-3.10.0-327.13.1.el7.1.ppc64le.rpm 

kernel的[改动]有：
1. enclosure设备的sysfs entry的内核部分改动 [hj, 此部分ibm因缺乏硬件lsi卡未验证功能]
支持在server上sysfs entry查看bmc的firmware version的改动，类似ipmitool mc info的功能

[测试]
        rpm -Uvh kernel***.rpm
        reboot    
        modprobe ipmi_devintf ipmi_msghandler ipmi_powernv
        check the sysfs entry, e.g. # cat
        /sys/devices/platform/ipmi_bmc.0933.32/manufacturer_id 0x002a7c # cat
        /sys/devices/platform/ipmi_bmc.0933.32/firmware_revision 1.13

        check the sysfs entry of enclosure device，e.g.
        cat /sys/devices/pci0001:00/0001:00:00.0/0001:01:00.0/0001:02:01.0/0001:03:00.0/host0/port-0:0/expander-0:0/port-0:0:0/end_device-0:0:0/target0:0:0/0:0:0:0/enclosure_device
        lshw 看disk的slot
