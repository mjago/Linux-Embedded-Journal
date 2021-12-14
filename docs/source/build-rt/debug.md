# Debugging tools

## Basics

The basics: always compile with:

- debugging symbols
- gdb and gdbserver on target. Requires toolchain with thread support, e.g. glibc.
- host cross gdb
- strace
- QEMU: buildroot can even compile QEMU!

## sshd

<http://stackoverflow.com/a/39301480/895245>

## nc

## netcat

Not enabled on BusyBox by default, see: `package/busybox/default.config`

But we have ping (TODO from where?), so whatever.

