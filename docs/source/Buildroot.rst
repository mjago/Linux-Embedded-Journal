.. SPDX-License-Identifier: CC-BY-SA-2.0-UK

=========
Buildroot
=========

Introduction
------------

The `Buildroot <https://buildroot.org>`_ project claims to be a
simple, efficient and easy-to-use tool to generate embedded Linux
systems through a cross-compilation process on a host machine. It has
a simple structure that makes it easy to understand and extend. It
relies only on the well-known Makefile language.

Buildroot takes care of:

- cross compilation. In other words, compiles GCC for you. Several architectures supported.
- bootloading. Several bootloaders supported.
- root filesystem generation.
- tons of packages, e.g. X.org. Packages have a dependency system, but no versioning.

Lots of software supported.


Details
-------

.. toctree::
   :caption: Table of Contents
   :numbered:
   :maxdepth: 2

   build-rt/details
