# TriviaOS

**TriviaOS is an open-source operating system built on top of ReactOS 0.4.17.**

TriviaOS is not a from-scratch project: it is a fork of the [ReactOS](https://reactos.org/) source
tree at version **0.4.17** (`0.4.17-dev`, base commit `7ede3531eee`). Everything below the project
name — kernel, HAL, Win32 subsystem, drivers, setup and user-mode components — is ReactOS code and
stays under ReactOS development. TriviaOS work is layered on top of that base.

## Base

| Item | Value |
| --- | --- |
| Upstream | ReactOS 0.4.17 (`KERNEL_VERSION` in [version.cmake](sdk/include/reactos/version.cmake)) |
| Base commit | `7ede3531eee` |
| Compatibility target | Windows Server 2003 (NT 5.2), `WINVER 0x502` |
| Architectures | `i386`, `amd64`, `arm`, `arm64` |
| License | GNU GPL 2.0 (see [COPYING](COPYING)) |

Updating from upstream:

```
git fetch upstream
git merge upstream/master
```

## What it does

TriviaOS runs Windows NT applications and drivers natively — no emulation and no Wine layer,
because the NT architecture is implemented directly. Current compatibility target is NT 5.2
(Windows Server 2003), inherited from the ReactOS base.

## Build in short

Use the _ReactOS Build Environment (RosBE)_ or MSVC 2019+/LLVM, then:

```
configure.cmd                 # or ./configure.sh
ninja bootcd                  # -> bootcd.iso (installer / live boot)
ninja livecd                  # -> liveimg.iso (RAM-disk live system)
```

Concrete build steps, layout of the tree and installation notes: [README.md](README.md).

## Reference test machine

TriviaOS is validated on one fixed reference laptop in addition to virtual machines:

**Toshiba Satellite A200-1O6** — Intel Core 2 Duo T7100 (2 × 1.8 GHz), ATI Mobility Radeon HD 2400
(128 MB), 2 GB DDR2-667, 160 GB HDD, 15.4" WXGA (1280 × 800). Full specification:
[README.md](README.md#test-hardware).

## Status

Alpha. TriviaOS inherits an operating system that is under heavy development — expect broken
features and possible data loss. Test in a virtual machine.

## Credits and licensing

The entire operating system base is the work of the ReactOS project (see [CREDITS](CREDITS)) and is
licensed under the GNU GPL 2.0. TriviaOS is an independent fork and is **not** affiliated with,
sponsored or endorsed by the ReactOS project; "ReactOS" is a trademark of its respective owners.
