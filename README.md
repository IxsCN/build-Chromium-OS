# build-Chromium-OS
step by step

#Build  Chromium OS 

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
 $ repo init -u https://chromium.googlesource.com/chromiumos/manifest.git --repo-url https://chromium.googlesource.com/external/repo.git -g minilayout
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
it will take
