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

