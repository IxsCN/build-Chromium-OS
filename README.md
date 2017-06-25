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
