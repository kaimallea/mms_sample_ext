[![Build Status](https://travis-ci.org/csgo-data/mms_sample_ext.svg)](https://travis-ci.org/csgo-data/mms_sample_ext)

Sample Metamod:Source Plugin for CSGO that also implements a SourceMod Extension

For more info, read ["Writing Extensions from Metamod:Source Plugins"](https://wiki.alliedmods.net/Writing_Extensions_from_Metamod:Source_Plugins)

This is the same `mms_sample_ext` from the `sourcemod` repo, except I made it work with CMake and only really care about it compiling on Linux and OSX.

### Building

If you're on a 64-bit system, this gets cross-compiled to 32-bit because that's how Valve rolls, for now.

```bash
$ mkdir build
$ cd build
$ cmake ..
```
