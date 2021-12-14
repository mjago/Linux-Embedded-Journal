
# config

## Configuration file

All options are stored in `.config` before build. The `.config` file fully specifies the entire system.

Make and environment variables *don't* in general override options:

    # Does not work.
    #make defconfig BR2_SOME_OPT=y

Whenever you do `make`, `make oldconfig` gets run. `make oldconfig` removes any new options you've added to the file, unless they are specified under `package/Config.in`.

Some configs are not put on the `.config`, while others are commented out. TODO: commented out means dependencies and have been met, removed not?

## Don't ask for password at login

<http://unix.stackexchange.com/questions/299408/how-to-login-automatically-without-typing-root-in-buildroot-x86-64-qemu>

## Important configurations

    LINUX_KERNEL_VERSION="4.5.3"
    BR2_GCC_VERSION_4_9_X=y
