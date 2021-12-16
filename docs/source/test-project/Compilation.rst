.. SPDX-License-Identifier: CC-BY-SA-2.0-UK

==========================
Build (On target over ssh)
==========================

Compile
-------

.. code-block:: shell

    make

Run
---
.. code-block:: bash

    sudo build/ctemp

Autorun on Startup
------------------
.. code-block:: none

    #!/bin/sh -e
    #
    # /etc/rc.local
    #

    /home/pi/linux_embed/build/ctemp
    exit 0

Makefile
--------

.. literalinclude:: ../../linux_embed/makefile
   :language: shell
