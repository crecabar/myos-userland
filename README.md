# myos-userland

First-party userspace commands and tools for MyOS.

## Purpose

`myos-userland` contains the **first-party userspace programs** that make MyOS usable as a small Unix-like operating system.

These programs run as ordinary MyOS ELF processes and consume the same public interfaces that third-party ports use.

## Architectural boundary

```text
myos-userland programs
        |
        v
     myos-libc
        |
        v
public x86_64-myos ABI
        |
        v
    MyOS kernel
```

This repository is intentionally separate from the kernel.

Userland programs must not:

- include kernel-private headers;
- call undocumented kernel entry points;
- depend on application-specific syscalls;
- bypass VFS, file descriptors, process, terminal or device interfaces;
- require kernel changes that exist only to make one command work.

When a program exposes a missing operating-system capability, that capability belongs in the appropriate reusable platform layer.

## Initial direction

The first userland is expected to grow from a very small base command set into a useful native environment.

Planned early programs include:

- `hello`;
- `echo`;
- `cat`;
- `clear`;
- `uname`;
- `meminfo`.

Later first-party utilities may include:

- an interactive calculator;
- a clock;
- a small text editor;
- a process monitor;
- reusable terminal/game helpers;
- native games such as Snake and Tetris.

The project is not intended to become a wholesale reimplementation of GNU coreutils. New programs should exist because they are useful to MyOS or because they exercise meaningful public OS interfaces.

## Build target

Programs are built for the native **`x86_64-myos`** target against the public MyOS sysroot and [myos-libc](https://github.com/crecabar/myos-libc).

A typical dependency direction is:

```text
upstream / first-party source
        ↓
x86_64-myos compiler + sysroot
        ↓
myos-libc
        ↓
native MyOS ELF
        ↓
installed root filesystem
```

## Relationship to the kernel repository

[crecabar/myos](https://github.com/crecabar/myos) owns the kernel and platform capabilities.

This repository owns applications.

Bootstrap programs may temporarily exist in the kernel tree while an interface is being brought up, but the long-term boundary is that normal user programs live here and are built independently against public interfaces.

## Related projects

- [crecabar/myos](https://github.com/crecabar/myos) — kernel and platform.
- [crecabar/myos-libc](https://github.com/crecabar/myos-libc) — C library and userspace runtime.
- [crecabar/myos-mfs](https://github.com/crecabar/myos-mfs) — native persistent filesystem.
- [crecabar/myos-vim](https://github.com/crecabar/myos-vim) — third-party Vim port and major userspace integration consumer.
- [MyOS Userland epic #100](https://github.com/crecabar/myos/issues/100) — roadmap tracking.

## Status

Early bootstrap/planning stage. The repository will become active as the public MyOS userspace ABI, libc and sysroot become usable independently from the kernel build.

## License

`myos-userland` is licensed under the **GNU General Public License version 2 only (GPL-2.0-only)**.

See [LICENSE](LICENSE).
