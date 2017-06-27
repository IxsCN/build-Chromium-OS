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
