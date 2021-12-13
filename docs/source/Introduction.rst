Introduction
============

In order to investigate the use of **Embedded Linux** for project design, I
am building a test project that utilises lots of interface techniques, and
also run an MQTT server (Mosquitto) which is accessible over Wifi.
The interfaces protocols used are:

* I2c
* SPI
* One-wire
* GPIO Output
* GPIO Input
* Anything else I think of

The target board is a low-cost **Raspberry Pi V3 A+**.

Initially, I will develop the :doc:`Test Project`. in the C
language on a stock raspi-lite system using the Nano editor over SSH
(building on the target system with GCC).

Next I'll investigate several methods of Linux Distro development tools, in order
to prepare a suitable stripped-down system and Kernel to run the test project on.

Next I'll investigate techniques for determining how to determine the dependencies
required to build the test project, and how to cross-compile the test project on
the host PC.

I will then investigate writing custom Linux Kernel driver/s to support the test
project. This will also require knowledge of DeviceTree and how to configure.

RAM
---

I quickly discovered that 8GB of RAM is not ideal for building Linux
systems / Cross-Compilers (particularly LLVM within Yoe Distro). Since
this is not sufficient RAM, I needed to extend **my Swap File** to 8GB
for a successful build. Since my disk drive is a **Solid State Drive**
I would rather not provide the extra wear by treating it as RAM, and
so have upgraded my RAM size to 24GB.

Journal
-------

I'm maintaining a journal of my investigations into **Embedded
Linux**. The journal is generated using the `Sphinx Documentation Generator <https://www.sphinx-doc.org>`_, and hosted on the internet using the `Read the Docs <https://readthedocs.org>`_ hosting option. **Sphinx** is able to parse both **.rst** (`Restructured Text <https://docutils.sourceforge.io/rst.html>`_) and **.md** (`Markdown <https://www.markdownguide.org/>`_) markup formats, and plays nicely with **Read the Docs**, and together with an `Online Github Repository <https://github.com/mjago/Linux-Embedded-Journal>`_, I am able to set up a *continuous-build-documentation-system* that builds and updates automatically as new information is added.

Having the journal online makes it easy to reference any aspect of the journal, and may also prove useful to anybody else interested in **Embedded Linux**.
