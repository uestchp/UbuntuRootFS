# Ubuntu Root Filesystem

运行`make`即可获取到Ubuntu Core Minimal系统。

**注意事项：**  
* 由于使用到了Host的设备节点，所以在使用make获取文件系统的时候，2-3次最好重启一下系统，否则会遇到/dev设备节点无法挂载等等各种不可预知的问题。  
* **强烈建议使用虚拟机折腾，由于会涉及到本机的/dev目录下的操作，误操作可能导致本机出现各种不可预知的问题，血的教训**

类似仓库：
* https://github.com/hypriot/os-rootfs
* [drtyhlpr/rpi23-gen-image](https://github.com/drtyhlpr/rpi23-gen-image)  
  **这个仓库可以重点分析，这是树莓派Debian系统合成方法，经测试，系统需要用Ubuntu 16.04版本，否则其中的一些package是装不上的。**

## Support System

* Ubuntu 16.04(xenial)
* Debian wheezy
* Debian Stretch

## Refers

* [Debian for ARM](http://www.cnblogs.com/zengjfgit/p/6413894.html)
* [Installing Ubuntu Rootfs on NXP i.MX6 boards](https://community.nxp.com/docs/DOC-330147)
* [ubuntu 下出现E: Sub-process /usr/bin/dpkg returned an error code](http://blog.csdn.net/yusiguyuan/article/details/24269129)
* [Thread: The system has no more ptys.](https://ubuntuforums.org/showthread.php?t=1190892)
* [6.2.2. Mounting and Populating /dev](http://www.linuxfromscratch.org/lfs/view/stable/chapter06/kernfs.html#ch-system-bindmount)
* [gcc-linaro-arm-linux-gnueabihf-4.9-2014.09_linux.tar.xz](https://releases.linaro.org/archive/14.09/components/toolchain/binaries/)
* [Make: how to continue after a command fails?](https://stackoverflow.com/questions/2670130/make-how-to-continue-after-a-command-fails)
* [Can not write log, openpty() failed (/dev/pts not mounted?)](http://mqjing.blogspot.tw/2013/07/chroot-pts-w-can-not-write-log-openpty.html)
* [Installing Ubuntu Rootfs on NXP i.MX6 boards](https://community.nxp.com/docs/DOC-330147)
* [Debian on the i.MX6 sabre sd platform in a few commands](https://community.nxp.com/docs/DOC-95044)

## pre-install package

在这里面加入需要预安装的`packages`：
* [Trusty pre-install package](customize/bin/trusty/install_packages)
* [Xenial pre-install package](customize/bin/xenial/install_packages)

## Init Type

Trusty是14.04的代号，Xenial是16.04的代号，两者的init程序是不一样的：
* [Trusty Upstart Init Ref](http://manpages.ubuntu.com/manpages/trusty/man5/init.5.html)
* [Xenial Systemd Init Ref](http://www.ruanyifeng.com/blog/2016/03/systemd-tutorial-commands.html)

## Custom Ubuntu Core

* 参考文档：https://docs.ubuntu.com/core/en/
* Build a custom Ubuntu Core image: https://docs.ubuntu.com/core/en/guides/build-device/image-building

## SD image

* Shell cmd: `sudo make sdimg`
* 查看打包后的方法：
  ```Shell
  aplex@aplex:~/zengjf/UbuntuRootFS/images$ sudo losetup -f --show sdcard.img
  /dev/loop0
  aplex@aplex:~/zengjf/UbuntuRootFS/images$ sudo kpartx -av /dev/loop0
  add map loop0p1 (253:0): 0 32768 linear 7:0 16384
  add map loop0p2 (253:1): 0 1399932 linear 7:0 49152
  aplex@aplex:~/zengjf/UbuntuRootFS$ ls /dev/mapper/
  control  loop0p1  loop0p2
  aplex@aplex:~/zengjf/UbuntuRootFS$ sudo mount /dev/mapper/loop0p2 /mnt
  aplex@aplex:~/zengjf/UbuntuRootFS$ ls /mnt
  bin  boot  dev  etc  home  lib  lost+found  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
  aplex@aplex:~/zengjf/UbuntuRootFS$
  ```
