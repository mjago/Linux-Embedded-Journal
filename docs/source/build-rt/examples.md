# Examples

## Basic

Run two commands, get a Linux distro built from source.

1.  Examples
    1. [Add a new package in tree](https://github.com/cirosantilli/buildroot/tree/in-tree-package-2016.05)
    1. [Package with a Kernel module in tree](https://github.com/cirosantilli/buildroot/tree/kernel-module-2016.05)
    1. [Out of tree package](https://github.com/cirosantilli/buildroot/tree/out-of-tree-2016.05)


## QEMU example

Tested on Ubuntu 16.04.

    git clone git://git.buildroot.net/buildroot
    cd buildroot
    git checkout 2016.05
    make qemu_x86_64_defconfig
    ## Can't use -jN, use `BR2_JLEVEL=2` instead.
    make BR2_JLEVEL=2
    ## Wait.
    ## cat board/qemu/x86_64/readme.txt
    qemu-system-x86_64 -M pc -kernel output/images/bzImage -drive file=output/images/rootfs.ext2,if=virtio,format=raw -append root=/dev/vda -net nic,model=virtio -net user

## QEMU x86 nographic

https://www.hiroom2.com/2016/05/20/ubuntu-16-04-build-buildroot-and-run-qemu/

`qemu -nographic -append 'console=ttyS0'`

At Buildroot 7e85734709e0da78cc399c1b85655528e2d5f209 requires:

    sed -i -e 's/BR2_TARGET_GENERIC_GETTY_PORT="tty1"/BR2_TARGET_GENERIC_GETTY_PORT="ttyS0"/g' .config

TODO: but why does it work on: https://github.com/cirosantilli/linux-kernel-module-cheat/tree/b8f190cc24b4f7474894b68a5510a8f3d767843d even without changing it? Buildroot version difference?

## ARM

Full QEMU command documented under `board/qemu/*/readme.txt`.

The obvious x86 analog just works, beauty.

The only thing is that you have to move the x86 output away if you have one:

    mv output output.x86~
    make qemu_arm_versatile_defconfig
    make
    qemu-system-arm -M versatilepb -kernel output/images/zImage -dtb output/images/versatile-pb.dtb -drive file=output/images/rootfs.ext2,if=scsi,format=raw -append "root=/dev/sda console=ttyAMA0,115200" -serial stdio -net nic,model=rtl8139 -net user -nographic

Then in QEMU:

    cat /proc/cpuinfo

Vexpress requires a more recent QEMU, 2.0.0 does not work, but the 2.7.0 built with QEMU did.

TODO: Ctrl + C kills the emulator itself. Why? Not like that in X86.

- <https://github.com/cloudius-systems/osv/issues/49>
- <https://unix.stackexchange.com/questions/167165/how-to-pass-ctrl-c-in-qemu>

    
