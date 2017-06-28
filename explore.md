## 嗯 并没有结束

+ chromeos-base/factory

```
factory-0.2.0-r372: symlink has no referent: "/build/x86-generic/tmp/portage/chromeos-base/factory-0.2.0-r372/work/factory-0.2.0/setup/netboot_firmware_settings.py"
factory-0.2.0-r372: rsync error: some files/attrs were not transferred (see previous errors) (code 23) at main.c(1178) [sender=3.1.2]
factory-0.2.0-r372: ERROR: Unexpected failure (exit code: 23). Abort.
```

+ try this
  
  + [Google Chromium OS Factory Software Platform](https://chromium.googlesource.com/chromiumos/platform/factory/+/master/README.md)
  + NG
   
+ wait for Answer
[build fail in chromeos-base/factory](https://stackoverflow.com/questions/44763650/build-fail-in-chromeos-base-factory)
  + it works 
  + But why? need follow this group [Builds fail in chromeos-base/factory](https://groups.google.com/a/chromium.org/forum/#!searchin/chromium-os-dev/base$2Ffactory/chromium-os-dev/-rR3wIhyGRI/ZK9f6jc8AQAJ)

+ build image

```
./build_image --board=${BOARD} --noenable_rootfs_verification test
```

wait .... (no error, nice)
Done.

```
INFO    : Kernel partition image emitted: /mnt/host/source/src/build/images/x86-generic/R61-9690.0.2017_06_27_2334-a1/hd_vmlinuz.image
INFO    : Root filesystem hash emitted: /mnt/host/source/src/build/images/x86-generic/R61-9690.0.2017_06_27_2334-a1/rootfs.hash
INFO    : Kernel image A   size is 5600256 bytes.
INFO    : Kernel image B   size is 5600256 bytes.
INFO    : Kernel partition A size is 16777216 bytes.
INFO    : Kernel partition B size is 16777216 bytes.
INFO    : Appending rootfs.hash (16519168 bytes) to the root fs
INFO    : Extracting the kernel command line from /mnt/host/source/src/build/images/x86-generic/R61-9690.0.2017_06_27_2334-a1/vmlinuz.image
INFO    : Unmounting image from /mnt/host/source/src/build/images/x86-generic/R61-9690.0.2017_06_27_2334-a1/stateful_dir and /mnt/host/source/src/build/images/x86-generic/R61-9690.0.2017_06_27_2334-a1/rootfs_dir
Cleaning up /usr/local symlinks for /mnt/host/source/src/build/images/x86-generic/R61-9690.0.2017_06_27_2334-a1/stateful_dir/dev_image
INFO    : Done. Image(s) created in /mnt/host/source/src/build/images/x86-generic/R61-9690.0.2017_06_27_2334-a1

INFO    : Test image created as chromiumos_test_image.bin
INFO    : To copy the image to a USB key, use:
INFO    :   cros flash usb:// ../build/images/x86-generic/R61-9690.0.2017_06_27_2334-a1/chromiumos_test_image.bin
INFO    : To convert it to a VM image, use:
INFO    :   ./image_to_vm.sh --from=../build/images/x86-generic/R61-9690.0.2017_06_27_2334-a1 --board=x86-generic --test

INFO    : Elapsed time (build_image): 28m43s
```

```
(cr) ~/trunk/src/scripts $ ls -la ../build/images/x86-generic/R61-9690.0.2017_06_27_2334-a1/
total 2947896
drwxr-xr-x 3 marixs chronos       4096 6月  28 00:03 .
drwxr-xr-x 3 marixs chronos       4096 6月  28 00:03 ..
-rw-r--r-- 1 marixs chronos        399 6月  28 00:03 boot.config
-rw-r--r-- 1 marixs chronos        210 6月  27 23:51 boot.desc
-rw-r--r-- 1 marixs chronos 3602874368 6月  28 00:03 chromiumos_test_image.bin
-rw-r--r-- 1 marixs chronos        586 6月  28 00:03 config.txt
drwxr-xr-x 2 marixs chronos       4096 6月  28 00:02 esp
-rw-r--r-- 1 root      root          1671 6月  28 00:02 id_rsa
-rw-r--r-- 1 root      root           399 6月  28 00:02 id_rsa.pub
-rw-r--r-- 1 marixs chronos    1381156 6月  27 23:51 license_credits.html
-rwxr-xr-x 1 marixs chronos       5748 6月  27 23:34 mount_image.sh
-rwxr-xr-x 1 marixs chronos       4871 6月  27 23:34 pack_partitions.sh
-rw-r--r-- 1 marixs chronos      13390 6月  27 23:34 partition_script.sh
-rwxr-xr-x 1 marixs chronos       4830 6月  27 23:34 umount_image.sh
-rwxr-xr-x 1 marixs chronos       5095 6月  27 23:34 unpack_partitions.sh
-rw-r--r-- 1 marixs chronos    5507152 6月  28 00:03 vmlinuz.bin

```

chromiumos_test_image.bin is the image.

+ transform format 

```
./image_to_vm.sh --board=${BOARD} --test_image
```
wait...
Done.
```
65536 bytes (66 kB) copied, 0.000228664 s, 287 MB/s
INFO    : Kernel partition image emitted: /mnt/host/source/src/build/images/x86-generic/R61-9690.0.2017_06_27_2334-a1/hd_vmlinuz.image
INFO    : Root filesystem hash emitted: /mnt/host/source/src/build/images/x86-generic/R61-9690.0.2017_06_27_2334-a1/rootfs.hash
INFO    : Kernel image A   size is 5600256 bytes.
INFO    : Kernel image B   size is 5600256 bytes.
INFO    : Kernel partition A size is 16777216 bytes.
INFO    : Kernel partition B size is 16777216 bytes.
INFO    : Appending rootfs.hash (16519168 bytes) to the root fs
INFO    : Extracting the kernel command line from /mnt/host/source/src/build/images/x86-generic/R61-9690.0.2017_06_27_2334-a1/vmlinuz.image
INFO    : Unmounting image from /mnt/host/source/src/build/images/x86-generic/R61-9690.0.2017_06_27_2334-a1/stateful_dir and /mnt/host/source/src/build/images/x86-generic/R61-9690.0.2017_06_27_2334-a1/rootfs_dir
Cleaning up /usr/local symlinks for /mnt/host/source/src/build/images/x86-generic/R61-9690.0.2017_06_27_2334-a1/stateful_dir/dev_image
partx: /dev/loop0: error deleting partition 5
Creating final image
Created image at /mnt/host/source/src/build/images/x86-generic/R61-9690.0.2017_06_27_2334-a1
If you have qemu-kvm installed, you can start the image by:
sudo kvm -m 1024 -vga cirrus -pidfile /tmp/kvm.pid -net nic,model=virtio -net user,hostfwd=tcp:127.0.0.1:9222-:22 \
-hda /mnt/host/source/src/build/images/x86-generic/R61-9690.0.2017_06_27_2334-a1/chromiumos_qemu_image.bin
```
+ copy it

```
scp /mnt/host/source/src/build/images/x86-generic/R61-9690.0.2017_06_27_2334-a1/chromiumos_qemu_image.bin 'toor@192.168.2.220:/media/toor/wd/'
```
wait...
> sleep ....
done..
生成的是kvm的镜像....
我需要启动盘，重新来过
```
 $ cros flash usb:///dev/sdb1 ${BOARD}/latest

/dev/sdb1 is not a removable device.

Do you want to continue? (yes/No)? yes
cros flash usb:///dev/sdb1 ${BOARD}/latest

/dev/sdb1 is not a removable device.

 [###############################################################-----] 90%

```
/dev/sdb1 是我的u盘，但是奇怪的是为什么会提示 not a removable devise呢？
先不管等等再说。
等待.....
```
00:18:49: NOTICE: cros flash completed successfully.
```
ok，去试试。
不能从u盘启动...为什么？
> If a device path is specified, Cros Flash will check if the device is indeed removable. If no path is given, user will be prompted to choose from a list of removable devices. Note that auto-mounting of USB devices should be turned off as it may corrupt the disk image while it's being written.

恩 坑了自己，因为u盘自动mount了。重新来过
```
cros flash usb:// ${BOARD}/latest
Removable device(s) found. Please select/confirm to continue:
  [0]: SanDisk Ultra Fit 14.3G (/dev/sdb)
Please choose an option [0-0]: 0
[###############################################################-----] 90%
```

感觉这次写入快了很多，心理作用吗？
```
 [######################################################################] 100%
00:33:08: NOTICE: cros flash completed successfully.
```
这次应该可以了，写入成功后u盘出现三个分区。分别是ROOT-A,OEM,STATE.
现在去试试。
失败!!!!
1. 虚拟机会卡在booting the kernel。
2. 用booting the kernel 一闪而过之后 一直黑屏。
...睡觉，先不折腾了。
