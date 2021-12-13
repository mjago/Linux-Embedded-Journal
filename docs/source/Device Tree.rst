Device Tree
===========

Introduction
------------

The `Devicetree <https://en.wikipedia.org/wiki/Devicetree>`_ is a data structure for describing hardware. Rather
than hard coding every detail of a device into an operating system,
many aspects of the hardware can be described in a data structure that
is passed to the operating system at boot time. The devicetree is used
by OpenFirmware, OpenPOWER Abstraction Layer (OPAL), Power
Architecture Platform Requirements (PAPR) and in the standalone
Flattened Device Tree (FDT) form.

Specification
-------------

The `Devicetree Specification <https://www.devicetree.org/specifications/>`_ provides a full technical description of
the devicetree data format and best practices.

Details
-------

.. toctree::
   :caption: Table of Contents
   :numbered:

   device-tree/chapter1-introduction
   device-tree/chapter2-devicetree-basics.rst
   device-tree/chapter3-devicenodes.rst
   device-tree/chapter4-device-bindings.rst
   device-tree/chapter5-flattened-format.rst
   device-tree/chapter6-source-language.rst
