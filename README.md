# Build  Chromium OS step by step

###  It is little hard for me, all the thing is unkown ,  even  developer-guide is very detailed
+ some fool mistake I had made,  finally I didn't complete the exercise
    +  creat workspace on smb and NFS 
    + 


### 0. deal with the network
+ Determine the purpose
    + penetrate the firewall
+ server 
    + ShadowsocksR can also achieve the purpose 
    + so I skip this step
+ client
    + git clone
    
    ```shell
    $ git clone https://github.com/shadowsocksr/shadowsocksr-libev.git
     ```

    + install dependent  library

    ```shell
    $ sudo apt-get install --no-install-recommends build-essential autoconf libtool libssl-dev libpcre3-dev asciidoc xmlto
    ```

    + ./configure  && make

    ``` shell
    $ cd shadowsocksr-libev &&  ./configure  && make
    ```

### Summary of 0

some trouble with libpcre libpcre-dev.
use apt install can't fix it.
In the end make libpcre by my self.

### 1. Get Chromium OS code and start building

+ Install depot_tools

    ```
    $ git clone https://chromium.googlesource.com/chromium/tools/depot_tools.git
    ```
 
+ add depot_tools to path

    ```
    $ export PATH=`pwd`/depot_tools:"$PATH"
    ```
 
+ Tweak your sudoers configuration
    It is not compatible with  cros_sdk.

    ```
    $ cd /tmp
    $ cat > ./sudo_editor <<EOF
    #!/bin/sh
    echo Defaults \!tty_tickets > \$1          # Entering your password in one shell affects all shells 
    echo Defaults timestamp_timeout=180 >> \$1 # Time between re-requesting your password, in minutes
    EOF
    $ chmod +x ./sudo_editor 
    $ sudo EDITOR=./sudo_editor visudo -f /etc/sudoers.d/relax_requirements
    ```
 
+ set e-mail & name for git

    ```
    $ git config --global user.email "you@example.com"
    $ git config --global user.name "Your Name"
    ```
 
+ Verify  architecture

    ```
    $ uname -m
    x86_64
    ```

+ Verify umask setting by touch file

    ```
    $ umask
    0002
    $ touch test && ll test
    -rw-rw-r--  test 
    ```

+ Get the source code

    + repo init

    ```
    $ repo init -u https://chromium.googlesource.com/chromiumos/manifest.git --repo-url     https://chromium.googlesource.com/external/repo.git -g minilayout
     ```
    + repo sync

    ```
    $ repo sync -j4
    ```
+ Google api key
    There are two ways to providing  api key into OS.
"at Build Time " or "at Runtime",
I think  providing keys at Runtime could be easily, so I choose this way.

    ```
    GOOGLE_API_KEY=your_api_key
    GOOGLE_DEFAULT_CLIENT_ID=your_client_id
    GOOGLE_DEFAULT_CLIENT_SECRET=your_client_secret
    ```
append them to /etc/chrome_dev.conf.

+ Create a chroot
so many toubles this section.
all toubles has was recorded in Mind Mapping.
 
    ```
    $ cros_sdk
    ```
+ build package

    ```
    $ export BOARD=x86-generic
    $ ./set_shared_user_password.sh
    $ ./build_packages --board=${BOARD}
    ```
+ it take so much time, and in the end it done,but two package error
    + chromeos-base/factory
    
    ```
    factory-0.2.0-r372: symlink has no referent: "/build/x86-generic/tmp/portage/chromeos-base/factory-0.2.0-r372/work/factory-0.2.0/setup/netboot_firmware_settings.py"
    factory-0.2.0-r372: rsync error: some files/attrs were not transferred (see previous errors) (code 23) at main.c(1178) [sender=3.1.2]
    factory-0.2.0-r372: ERROR: Unexpected failure (exit code: 23). Abort.
    ```
    
    + net-misc/tlsdate
    
    ```
    make[1]: Entering directory '/build/x86-generic/tmp/portage/net-misc/tlsdate-0.0.5-r48/work/tlsdate-0.0.5'
    tlsdate-0.0.5-r48:   CC       src/conf.o
    tlsdate-0.0.5-r48:   CC       src/conf-unittest.o
    tlsdate-0.0.5-r48:   CC       src/platform-cros-util.o
    tlsdate-0.0.5-r48:   CC       src/platform-cros-util-unittest.o
    tlsdate-0.0.5-r48:   CC       src/src_tlsdated_unittest-tlsdated-unittest.o
    tlsdate-0.0.5-r48:   CC       src/src_tlsdated_unittest-conf.o
    tlsdate-0.0.5-r48:   CC       src/src_tlsdated_unittest-routeup.o
    tlsdate-0.0.5-r48: src/platform-cros-util-unittest.c: In function 'test_canonicalize_pac_parsing':
    tlsdate-0.0.5-r48: src/platform-cros-util-unittest.c:71:3: error: 'for' loop initial declarations are only allowed in C99 or C11 mode
    tlsdate-0.0.5-r48:    for (size_t i = 0; i < ARRAYSIZE(kCases); ++i) {
    tlsdate-0.0.5-r48:    ^
    tlsdate-0.0.5-r48: src/platform-cros-util-unittest.c:71:3: note: use option -std=c99, -std=gnu99, -std=c11 or -std=gnu11 to compile your code
    tlsdate-0.0.5-r48: src/platform-cros-util-unittest.c: In function 'test_canonicalize_pac_overflow':
    tlsdate-0.0.5-r48: src/platform-cros-util-unittest.c:95:3: error: 'for' loop initial declarations are only allowed in C99 or C11 mode
    tlsdate-0.0.5-r48:    for (size_t i = 0; i < ARRAYSIZE(kCases); ++i) {
    tlsdate-0.0.5-r48:    ^
    tlsdate-0.0.5-r48: make[1]: *** [Makefile:1222: src/platform-cros-util-unittest.o] Error 1
    tlsdate-0.0.5-r48: make[1]: *** Waiting for unfinished jobs....
    tlsdate-0.0.5-r48: make[1]: Leaving directory '/build/x86-generic/tmp/portage/net-misc/tlsdate-0.0.5-r48/work/tlsdate-0.0.5'
    tlsdate-0.0.5-r48: make: *** [Makefile:777: all] Error 2
    ```
+ try build image.
    guest that, if the package is not impotant, build image will work.

```
$ ./build_image --board=${BOARD} --noenable_rootfs_verification test
....
....
....
...
emerge: there are no ebuilds to satisfy "virtual/target-os" for /mnt/host/source/src/build/images/x86-generic/R61-9680.0.2017_06_25_0021-a1/rootfs/.

emerge: searching for similar names...
emerge: Maybe you meant any of these: virtual/target-os-dev, virtual/target-os-test, virtual/assets?

```
it seems not work, and also I get wrong understand of this. So i gave up and find a new laptop, ALL OVER AGAIN.

+ fix error of net-misc/tlsdate. According to the log, it seems easy to fix, edit the file src/platform-cros-util-unittest.c, and Declare variables i before for loop, just like this.
    + first mark the package (net-misc/tlsdate) as active. and sync down source sources
    
    ```
    $ PACKAGE_NAME="net-misc/tlsdate"
    $ cros_workon --board=${BOARD} start ${PACKAGE_NAME}
    $ repo sync
    ```
    
    + second Create a branch 
    
    ```
    $ repo start marixs
    ```
    
    + and then fix the error. may two ways to fix it.
        + edit the src/platform-cros-util-unittest.c
        + edit Makefile, and add -std=c11
        
    I choose first solution, because i don't have deep understand for this project, edit Makefile may be the bad idea.
    like this:
    
    ```
    for (size_t i = 0; i < ARRAYSIZE(kCases); ++i) {
    ```

    to

    ```
    size_t i=0;
    for (i = 0; i < ARRAYSIZE(kCases); ++i) {
    ```
    
    + make the package
    
    ```
    $ cros_workon_make --board=${BOARD} ${PACKAGE_NAME} --test
    ```
    
    + install the changes
    
    ```
    $ cros_workon_make --board=${BOARD} ${PACKAGE_NAME} --install
    ```
     and something wrong about license, it show the URL [http://www.chromium.org/chromium-os/licensing-for-chromiumos-package-owners](http://www.chromium.org/chromium-os/licensing-for-chromiumos-package-owners)
     and searched the "Chromium OS dev", find this [Licensing for Chromium OS Developers](http://www.chromium.org/chromium-os/licensing-for-chromiumos-package-owners)
     so i copyed "Google-TOS" license.
     
     ```
     $ cp  ~/trunk/src/third_party/chromiumos-overlay/licenses/Google-TOS ~/trunk/src/third_party/chromiumos-overlay/licenses/copyright-attribution/net-misc/tlsdate
     $ cros_workon_make --board=${BOARD} ${PACKAGE_NAME} --install
     ....
     ....
     ....
     >>> Installing (1 of 1) net-misc/tlsdate-9999::chromiumos to /build/x86-generic/
     * Removing /usr/lib*/*.la
     * Removing /etc/init.d
     * Removing /etc/conf.d
     * Removing /etc/logrotate.d
     * Removing /etc/sandbox.d
     * Removing /usr/share/bash-completion
     * Removing /usr/share/man
     * Removing /usr/share/info
     * Removing /usr/share/doc
     * Running stacked hooks for pre_pkg_preinst
     *    wrap_old_config_scripts ...                                                                                               [ ok ]

     * Messages for package net-misc/tlsdate-9999 merged to /build/x86-generic/:

     * For inplace build you need to modify the sandbox
     * Set SANDBOX_WRITE=/mnt/host/source in your env.
    >>> Auto-cleaning packages...

    >>> Using system located in ROOT tree /build/x86-generic/

    >>> No outdated packages were found on your system.

     ```
     
+ fix error of chromeos-base/factory.
    + searched: I found this post. [Builds fail in chromeos-base/factory](https://groups.google.com/a/chromium.org/forum/#!searchin/chromium-os-dev/base$2Ffactory/chromium-os-dev/-rR3wIhyGRI/ZK9f6jc8AQAJ),  hopeless (‘⊙д-) .

    + like last error (net-misc/tlsdate).
    
    ```
    $ PACKAGE_NAME="chromeos-base/factory"
    $ cros_workon --board=${BOARD} start ${PACKAGE_NAME}
    $ repo sync
    $ cros_workon_make --board=${BOARD} ${PACKAGE_NAME} --test
    ...
    ...
    ...
    ...
    Re-run failed tests sequentially:
    *** PASS [5.73 s] go/src/overlord/test/overlord_e2e_unittest.py
    *** FAIL [1.57 s] py/goofy/goofy_unittest.py (return:1)
    *** FAIL [3.98 s] py/hwid/v3/builder_unittest.py (return:1)
    *** FAIL [4.08 s] py/hwid/v3/hwid_utils_unittest.py (return:1)
    *** FAIL [8.50 s] py/instalog/plugins/http_e2e_unittest.py (return:1)
    *** FAIL [12.57 s] py/instalog/plugins/input_http_unittest.py (return:1)
    *** FAIL [2.41 s] py/shopfloor/factory_server_par_unittest.py (return:1)
    *** FAIL [0.21 s] py/tools/build_board_unittest.py (return:1)
    *** FAIL [1.17 s] py/utils/sys_utils_unittest.py (return:1)
    ```
    
    + from the result, compile is PASS,so guess the unit test script is not good
    + by read Makefile, I find unittest blacklist fuc,so have a try.
    
    ```
    /mnt/host/source/src/platform/factory/devtools/mk $ vi unittests.blacklist 
    ```
    
    + return OK, install
    
    ```
    $ cros_workon_make --b
oard=${BOARD} chromeos-base/factory --install
    ...
    ...
    ...
    Invalid '-' operator in non-incremental variable 'INPUT_DEVICES': '-*'
    Invalid '-' operator in non-incremental variable 'TTY_CONSOLE': '-tty2'
    Invalid '-' operator in non-incremental variable 'INPUT_DEVICES': '-*'
    Invalid '-' operator in non-incremental variable 'TTY_CONSOLE': '-tty2'
    Invalid '-' operator in non-incremental variable 'INPUT_DEVICES': '-*'
    Invalid '-' operator in non-incremental variable 'TTY_CONSOLE': '-tty2'
    ```
    
    + try fail. back to this alert "factory-0.2.0-r372: symlink has no referent: "/build/x86-generic/tmp/portage/chromeos-base/factory-0.2.0-r372/work/factory-0.2.0/setup/netboot_firmware_settings.py""
    
    ```
    $ ls -l | grep netboot_firmware_settings.py
    lrwxrwxrwx 1 sunxiaoyu chronos    43 6月  25 19:58 netboot_firmware_settings.py -> ../../dev/host/netboot_firmware_settings.py

    ```
    + ~ and found that file is empty, it may project's bug. and i found this in Google [netboot_firmware_settings.py: Fix lint errors by Drew Davenport · 11 days ago](https://chromium.googlesource.com/chromiumos/platform/dev-util/) ~
    + ~ so I'm sure that, the softlink is wrong. Fix it. ~
    + so download file from [here](https://chromium.googlesource.com/chromiumos/platform/dev-util/+/c0bcabb6682eb0ad4597ee32e270d66c5633c340/host/netboot_firmware_settings.py) , and try relplace it.
