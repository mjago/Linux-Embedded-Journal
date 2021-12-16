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

.. code-block:: none

   TARGET_EXEC ?= ctemp
   BUILD_DIR ?= ./build
   SRC_DIRS := ./ctrltemp
   
   SRCS := $(shell find $(SRC_DIRS) -name '*.c')
   OBJS := $(SRCS:%=$(BUILD_DIR)/%.o)
   DEPS := $(OBJS:.o=.d)
   
   INC_DIRS := $(shell find $(SRC_DIRS) -type d)
   INC_FLAGS := $(addprefix -I,$(INC_DIRS))
   
   CPPFLAGS ?= $(INC_FLAGS) -MMD -MP
   
   $(BUILD_DIR)/$(TARGET_EXEC): $(OBJS)
   	$(CC) $(OBJS) -o $@ -lm -I/usr/local/include -L/usr/local/lib -lwiringPi -lpaho-mqtt3c $(LDFLAGS)
   
   # assembly
   $(BUILD_DIR)/%.s.o: %.s
   	$(MKDIR_P) $(dir $@)
   	$(AS) $(ASFLAGS) -c $< -o $@
   
   # c source
   $(BUILD_DIR)/%.c.o: %.c
   	$(MKDIR_P) $(dir $@)
   	$(CC) $(CPPFLAGS) $(CFLAGS) -c $< -o $@
   
   .PHONY: clean
   
   clean:
   	$(RM) -r $(BUILD_DIR)
   
   -include $(DEPS)
   
   MKDIR_P ?= mkdir -p
   
