<div align="center">
  <h1><b>Nyxian-Unlocked</b></h1>
  <p><i>A full native IDE and a userspace microkernel for building and running iOS apps entirely on-device. offline, unjailbroken, iOS 18.0 all the way through iOS 27.3 Beta 1 (At least for Nyxian-Unlocked), this is a unstable forked version of 12.0 that will be MAINTAINED BY ME AND ME ONLY, besides that this is to disable the TrustStore and possibly more???</i></p>
</div>

</p>

## What is it?
Nyxian is an iOS app that empowers developers with a full toolchain they can use while even being offline for iOS development on iPhone. It supports Swift, C, Objective-C, C++ and Objective-C++. It provides a native on-device build environment, a userspace microkernel, and the tooling to compile, run, and manage app packages without needing a desktop or a jailbroken device.

## Core focus: ProcEnvironment/Surface
Everything I care about is located at `Nyxian/LindChain/ProcEnvironment/Surface`.

This is the core process-environment and runtime surface of Nyxian: the low-level layer where the microkernel, process abstraction, filesystem and tty plumbing, trust/key handling, memory mapping, and syscall-facing behavior are assembled. In practical terms, it is the foundation that connects Nyxian's userspace environment to the host iOS runtime and exposes the machinery needed for native app execution, process isolation, and system-like services.

## Installation
To start using Nyxian view the [Installation Guide](https://emexlabs.org/Nyxian/docs/installation/).

## Limitations
The only current limitation is that we cannot intercept a arm64 supervisor call, but that is not so much of a problem, because remember we are on iOS, processes have nearly no priveleges, so by by not intercepting a supervisor call we can still do most things we need to do.

## Todo
- IDE
  - Compiling code
    - [x] C support
    - [x] Objective-C support
    - [x] C++ support
    - [x] Objective-C++ support
    - [x] Swift support
  - Typechecking
    - [x] C support
    - [x] Objective-C support
    - [x] C++ support (limited without indexing)
    - [x] Objective-C++ support (limited without indexing)
    - [x] Swift support
  - [x] Linking objects to MachO
  - [ ] Indexing
- Offline Code Execution
  - [x] Code execution (via NSExtension)
  - Micro Kernel (ksurface)
    - [x] radix trees
    - [x] object API
    - [x] process object
    - [x] privelege model
    - [x] custom entitlement blob (translated from AppleCS to NXT2 as well!)
    - Kext Link Editor (kxld)
      - [x] validate code signature of kext
      - [x] validate it's nyxian trust blob
      - [x] load kext executable into nyxian address space
      - [x] find it's kmod
      - [x] apply fixups for kext
      - [x] fix ObjC
      - [ ] fix thread local storage
      - [x] handle it's exports
      - [x] reseal constant data section
      - [x] run it's initializers
      - [x] act upon `kmod_info_t`
    - Syscall handling
      - [x] Mach IPC syscall server
      - [x] Task port handoff (usually they are guarded we bypass that by moving a receive right after the send right has been set as exception port to the host and then executing a `brk` instruction to move the task to a new address space that respects our init and their rest of data)
      - [x] Memory copy in/out of guests (yep out of the iOS processes and into them without assistance, this is not a typo lol)
    - Subprocess Patches
      - [x] `posix_spawn`/`posix_spawnp` fix
      - [x] `vfork` fix
      - [x] `sysctl` fix
      - [x] tty support on iPhone and the necessary `ioctl` fix (tho not entirely yet, it only works when NXWindowSessionTerminal creates it, but it is already progress)
      - [x] libproc fix
      - [x] `task_for_pid`/`task_name_for_pid` fix (you heard right, that is not a typo)
      - [x] patches to credential syscalls like `setuid` or `setgid`
  - [x] Signing executables
  - [x] NSBundle think it is loaded **as a binary**
  - [x] Actually make iOS apps and binaries use the version as DYLD version they have been made for
  - [ ] Find NSExtension spawn limit
  - [x] File permissions in guest file system (using sandbox file extensions and clever self-written fs sandbox APIs)
