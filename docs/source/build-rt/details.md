
# Add a file to the distro

    mkdir a
    mkdir a/b
    date >a/b/c
    make BR2_ROOTFS_OVERLAY='a'

Outcome: the root of the generated filesystem now contains `/b/c`.

# Permanent storage filesystem

TODO

# Remove package

Currently impossible.

For simple cases, just remove the files from:

    rm output/target/usr/bin/hello

# random: nonblocking pool is initialized

TODO how to stop printing that

