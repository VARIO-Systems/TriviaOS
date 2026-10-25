<p align=center>
  <b>TriviaOS</b>
</p>

---

<p align=center>
  <a href="https://github.com/reactos/reactos">
    <img alt="Based on ReactOS 0.4.17" src="https://img.shields.io/badge/based%20on-ReactOS%200.4.17-0688CB.svg"></a>
  <a href="COPYING">
    <img alt="License" src="https://img.shields.io/badge/license-GNU_GPL_2.0-0688CB.svg"></a>
  <img alt="Development status" src="https://img.shields.io/badge/status-alpha-E44E4A.svg">
</p>

## Quick Links
[Short overview (TRIVIAOS.md)](TRIVIAOS.md) &bull;
[Source tree layout](#source-tree-layout) &bull;
[Building](#building) &bull;
[Contributing](CONTRIBUTING.md) &bull;
[Coding style](CODING_STYLE.md) &bull;
[Installation notes](INSTALL) &bull;
[License](COPYING) &bull;
[Upstream project](https://reactos.org/)

## What is TriviaOS?

TriviaOS is an open-source fork built directly on top of the **ReactOS 0.4.17** source base. Like its upstream, it is a from-scratch reimplementation of the Microsoft&reg; Windows&trade; NT architecture and aims to run applications and drivers written for the Windows NT family (NT4, 2000, XP, 2003) natively, without emulation layers such as Wine.

Like ReactOS, TriviaOS currently targets compatibility with Windows Server 2003 (NT 5.2), and keeps an eye toward later NT releases. The whole kernel, driver, subsystem and user-mode code base is ReactOS code; TriviaOS development happens as changes layered on top of that base.

### Base and upstream relationship

| Item | Value |
| --- | --- |
| Fork base | ReactOS **0.4.17** (`KERNEL_VERSION` in [version.cmake](sdk/include/reactos/version.cmake)) |
| Base revision | `7ede3531eee` (ReactOS development branch, `0.4.17-dev`) |
| Target platform | Windows Server 2003 (NT 5.2), `WINVER 0x502` |
| Origin remote | https://github.com/VARIO-Systems/TriviaOS.git |
| Upstream remote | https://github.com/reactos/reactos.git |

Upstream changes are picked up by merging the ReactOS repository into this tree:

```
git fetch upstream
git merge upstream/master
```

TriviaOS is an independent project. It is **not** affiliated with, sponsored by or endorsed by the ReactOS project. "ReactOS" is a trademark of its respective owners; all credit for the operating system base belongs to the ReactOS developers (see [CREDITS](CREDITS)).

### Product quality warning

**TriviaOS inherits an Alpha quality operating system.** The code base is under heavy development and different things may not work well; it can corrupt the data present on your hard disk. It is HIGHLY recommended to test TriviaOS in a virtual machine or on a computer with no sensitive or critical data.

## Supported architectures

| Architecture | CMake `ARCH` | Notes |
| --- | --- | --- |
| IA-32 (x86) | `i386` | Primary target, most complete |
| x86-64 | `amd64` | Supported |
| ARM (32-bit) | `arm` | Work in progress, upstream base |
| ARM64 | `arm64` | Work in progress, upstream base |

## Source tree layout

| Path | Content |
| --- | --- |
| [ntoskrnl/](ntoskrnl) | Windows NT kernel: Executive (`Ex`), Memory Manager (`Mm`), I/O (`Io`), Object Manager (`Ob`), Process Manager (`Ps`), Security (`Se`), Kernel (`Ke`), Cache (`Cc`), LPC, Power, WMI |
| [hal/](hal) | Hardware Abstraction Layer (HAL) |
| [win32ss/](win32ss) | Win32 subsystem kernel-mode part (`win32k`, GDI/USER) |
| [subsystems/](subsystems) | Native subsystems: CSR (`csrss`), VDM (`ntvdm`, `wow16`) |
| [boot/](boot) | Boot loaders (FreeLoader), boot data, registry hives, `txtsetup` |
| [base/](base) | Base system user mode: setup (`usetup`), services, shell (`cmd`), applications, CTF |
| [dll/](dll) | System DLLs and control panels |
| [drivers/](drivers) | Kernel-mode drivers (bus, filesystems, input, network, storage, ...) |
| [sdk/](sdk) | Public headers, import libraries, build tools and CMake glue |
| [media/](media) | INF files, NLS data, documentation in [media/doc](media/doc) |
| [modules/](modules) | Optional modules: extra apps, and the test suites ([modules/rostests](modules/rostests)) |
| [CMakeLists.txt](CMakeLists.txt) | Top-level build description |
| [configure.cmd](configure.cmd) / [configure.sh](configure.sh) | Configure helpers for Windows / Unix |
| [CMakePresets.json](CMakePresets.json) | Ready-made configure presets (MinGW, MSVC, clang, per architecture) |

## Building

To build the system it is strongly advised to use the _ReactOS Build Environment (RosBE)_, which ships a matching CMake/compiler set. Up-to-date versions for Windows and for Unix/GNU-Linux are available from the upstream download page: ["Build Environment"](https://reactos.org/wiki/Build_Environment).

Alternatively Microsoft Visual C++ 2019+ (or LLVM/Clang) can be used; building with MSVC is covered here: ["Visual Studio or Microsoft Visual C++"](https://reactos.org/wiki/CMake#Visual_Studio_or_Microsoft_Visual_C.2B.2B).

### Binaries

Configure the build in the directory that should hold the build files, using `configure.cmd` or `configure.sh` depending on your system, or use one of the CMake presets:

```
cmake --preset mingw-i386-debug
cmake --build output-MinGW-i386
```

Then build a single module with `ninja <modulename>` or everything with plain `ninja`.

### Bootable images

| Target | Output |
| --- | --- |
| `ninja bootcd` | `bootcd.iso` — bootable installer CD, also usable from USB |
| `ninja livecd` | `liveimg.iso` — RAM-disk based live system |
| `ninja bootcdregtest` | `bootcdregtest.iso` — boot CD running the automated test suite |

## Installing

By default TriviaOS can only be installed on a machine that has a FAT16 or FAT32 partition as the active (bootable) partition. The partition on which TriviaOS is to be installed (which may or may not be the bootable partition) must also be formatted as FAT16 or FAT32. Setup can format the partitions if needed.

BtrFS can be used as well, but consider this an experimental feature; regressions that are not triggered on a FAT setup may be observed.

To install, burn the boot CD image (or write it to a USB drive), boot from it, and follow the instructions. See [INSTALL](INSTALL) for more details.

## Testing

The test suites live in [modules/rostests](modules/rostests) and include the Win32/NT API tests, kernel-mode tests (`kmtests`), driver tests and the Wine test suite. `ninja bootcdregtest` produces an image that boots straight into the automated test run.

If you discover a bug, please search the existing issue trackers first — the bug may already be reported upstream at [JIRA](https://jira.reactos.org/issues/).

## Contributing

Contributions to TriviaOS are welcome. Please read [CONTRIBUTING.md](CONTRIBUTING.md) for the general workflow, respect our [coding style](CODING_STYLE.md), and use the commit template in [.gitmessage](.gitmessage) (`git config commit.template .gitmessage`).

__Legal notice__: If you have seen proprietary Microsoft Windows source code (including but not limited to the leaked Windows NT 3.5, NT 4 and 2000 source code, and the Windows Research Kernel), your contribution won't be accepted because of potential copyright violation. Contributions must follow clean-room rules; third-party code (Wine, BtrFS, mbedTLS, ...) is synced from its upstream project, see [media/doc](media/doc).

## License

The code of TriviaOS is licensed under [GNU GPL 2.0](COPYING), inherited from the ReactOS base. Bundled components may be covered by additional licenses, see [COPYING.ARM](COPYING.ARM), [COPYING.LIB](COPYING.LIB) and [COPYING3](COPYING3).

## More information

- Short project overview: [TRIVIAOS.md](TRIVIAOS.md)
- Sparse developer notes: [media/doc](media/doc)
- Upstream project: [reactos.org](https://reactos.org/)
