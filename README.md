[![Build Status](https://travis-ci.org/csgo-data/mms_sample_ext.svg)](https://travis-ci.org/csgo-data/mms_sample_ext)

Sample Metamod:Source Plugin for CSGO that also implements a SourceMod Extension

For more info, read ["Writing Extensions from Metamod:Source Plugins"](https://wiki.alliedmods.net/Writing_Extensions_from_Metamod:Source_Plugins)

This is the same `mms_sample_ext` from the `sourcemod` repo, except I made it work with CMake and only really care about it compiling on Linux and OSX.

### Cloning

This repo has submodules, meaning it depends on other git projects. And some of those submodules have submodules of their own. To ensure you get everything, make sure you clone recursively:

```bash
$ git clone --recursive https://github.com/csgo-data/mms_sample_ext.git
```

If you already cloned the repo, but did not do so resursively (via `--recursive`), then you can run the following:

```bash
$ git submodule update --init --recursive
```

### Building

If you're on a 64-bit system, this gets cross-compiled to 32-bit because that's how Valve rolls, for now. If this is the case, you'll need the following packages on Ubuntu/Debian:

```
$ sudo apt-get install cmake g++-multilib lib32z1 lib32z1-dev libc6-i386 libc6-dev-i386
```

When you're ready, use CMake to build it:

```bash
$ mkdir build
$ cd build
$ cmake ..
```
