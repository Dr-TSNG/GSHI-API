# Genaral Syscall Hook Interface

## What is GSHI

GSHI is General Syscall Hook Interface for Android. It aims at providing a compat interface for low-level hooks on Android apps.

## Backend

The backend for GSHI can be a Zygisk module, a linux kernel module, or even an Xposed module for unrooted devices.

The Zygisk backend implementation is now under development.

## Usage
```cpp
#include "gshi.hpp"

void OpenatHandler(gshi::Handler& handler) {
    register_t& fd = handler.regs.arg0;
    register_t& filename = handler.regs.arg1;
    register_t& flags = handler.regs.arg2;
    register_t& mode = handler.regs.arg3;
    printf("openat: %s\n", (char*) filename);
    handler.regs.ret = handler.DoSyscall(SYS_openat, fd, filename, flags, mode, 0, 0);
}

gshi::GetInstance()
    ->HookSyscall(SYS_openat, OpenatHandler)
    ->Commit();
```
