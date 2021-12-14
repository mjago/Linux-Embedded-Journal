# Projects that use Buildroot:

- <https://en.wikipedia.org/wiki/OpenWrt>

## Games

Placed under `/usr/bin` and `/usr/games`.

Grouped under `packages/Config.in`:

    menu "Games"

Many / all are SDL based. It seems that SDL has an `fbdev` mode that dispenses X11.

## prdoom

## chocolate-doom

Doom clones.

This shows one running on uclinux blackfin SDL DirectFB: https://www.youtube.com/watch?v=fKyQOntPEFs

## ltris

## lbreakout2

From: http://lgames.sourceforge.net/about.php

Simple SDL based games `L` stands for Linux.

Should be able to run on framebuffer? But both on TTY and X11 they fail with:

    set_video_mode: cannot allocate screen: Couldn't set console screen info

Looks like this is caused by the call: <https://www.libsdl.org/release/SDL-1.2.15/docs/html/sdlsetvideomode.html>

`fbset` seems to do the same calls, and fails in the same way.

## opentyrian

Takes over screen and hangs.

## sl

Classic steam locomotive `sl` typo corrector. Text only.

## gnuchess

CLI chess.

## X11

http://unix.stackexchange.com/questions/70931/install-x11-on-my-own-linux-system

## GUI

- <http://unix.stackexchange.com/questions/70931/install-x11-on-my-own-linux-system/306116#306116>

## SDL without X11

- <http://stackoverflow.com/questions/1263710/minimal-linux-distrobution-with-sdl-support-and-no-xwindows>

## Web browser

- <http://unix.stackexchange.com/questions/17779/how-can-i-build-a-custom-distribution-for-running-a-simple-web-browser/306192#306192>

