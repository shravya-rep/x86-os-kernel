# Weenix — A Unix-like Kernel

A bootable x86 Unix-like kernel written in C, implementing process scheduling, virtual memory with copy-on-write, a Unix file system, and a complete syscall interface. Runs real user programs in QEMU.

---

## Components Built

### Process & Thread Management
- Kernel thread creation, scheduling, cancellation, and exit
- Round-robin scheduler with context switching
- `fork()` — duplicates a process with a cloned address space
- `exec()` — loads and executes ELF binaries in userspace
- `waitpid()` / `exit()` — zombie reaping, orphan reparenting, process cleanup
- Kernel mutexes and condition variables for thread synchronization

### Virtual Memory
- Per-process virtual address space (`vmmap`) with memory-mapped regions
- **Copy-on-write** via shadow objects — forked processes share physical pages until either writes
- Anonymous page mapping with demand paging
- Page fault handler
- Buddy allocator for physical page allocation
- Slab allocator for efficient kernel object allocation

### File System
- **S5FS** — a simplified Unix file system with inodes, directories, and data blocks
- **VFS (Virtual File System)** layer — clean abstraction over the concrete file system, supporting `open`, `read`, `write`, `close`, `stat`, `mkdir`, `rmdir`, `link`, `unlink`
- Per-process file descriptor tables, inherited across `fork`
- Hard links

### System Call Interface
- Full syscall dispatcher bridging userspace and kernel
- Implements: `fork`, `exec`, `exit`, `waitpid`, `read`, `write`, `open`, `close`, `stat`, `mkdir`, `rmdir`, `link`, `unlink`, `getpid`, `mmap`, and more

### Drivers & Hardware Abstraction
- ATA disk driver (block device)
- TTY driver with 3 virtual terminals
- Keyboard input and screen output
- PCI device enumeration
- x86 boot: GDT, IDT, APIC initialization

### Userspace Programs
- `init` — first userspace process
- `kshell` — interactive kernel debug shell
- `sh` — basic Unix shell
- `ls`, `stat`, `cat`, `wc`, `hd`, `ed` — standard utilities
- `fork-and-wait`, `spin`, `hello` — process and scheduler test programs
- `halt` — clean system shutdown

---

## Running

Requires a Linux environment (Ubuntu 22.04 recommended).

```bash
# Install dependencies
sudo apt-get install gcc gdb make qemu-system-x86 genisoimage xorriso grub-pc-bin python3

# Build
cd kernel
make

# Run in QEMU
./weenix -n

# Run with GDB
./weenix -n -d gdb
```
