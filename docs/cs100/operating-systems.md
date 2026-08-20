---
layout: default
title: Operating Systems & Command-Line Environments
parent: CS100 Computer Fundamentals
nav_order: 2
---

# Operating Systems & Command-Line Environments

When running computational workflows, software does not interact with bare hardware directly. Instead, the **Operating System (OS)** manages processor scheduling, memory allocation, and file systems. 

Understanding operating system architectures, terminal environments, and shell interpreters explains why scientific software standardizes on Linux/POSIX, why commands behave differently across platforms, and how to configure a consistent development environment.

---

## 1. What is an Operating System and How is it Deployed?

An **Operating System (OS)** is the core system software that sits between physical hardware (CPU, RAM, storage) and user applications.

```
+-----------------------------------------------------------------------+
| User Space: Applications, Python, Aligners, Compilers, Shells         |
+-----------------------------------------------------------------------+
| System Call Interface (POSIX API, Win32 API)                          |
+-----------------------------------------------------------------------+
| Kernel: Process Scheduler, Virtual Memory Manager, File System Drivers|
+-----------------------------------------------------------------------+
| Hardware: CPU, RAM, GPU, NVMe SSD, Network Controller                 |
+-----------------------------------------------------------------------+
```

* **The Kernel**: The privileged core of the OS that controls hardware access, allocates memory pages, and switches execution between CPU threads.
* **The System Call Interface**: The standardized API through which programs request kernel services (e.g., allocating memory, reading a file, opening a network socket).

### Is an OS Something You Can Download?
Yes. An operating system is software and is distributed digitally through several formats:

1. **Disk Images (`.iso` files)**: Complete bootable system files (e.g., `ubuntu-24.04-desktop.iso`).
   * **Flashing to Bare Metal**: "Flashing" means writing the raw disk image byte-by-byte onto a USB thumb drive (using tools like BalenaEtcher or Rufus). When plugged into a physical computer, the motherboard's BIOS/UEFI boots directly from the USB drive to install the OS onto the physical storage drive (**bare-metal installation**, meaning no host OS is running underneath).
   * **Virtual Machine (VM) Hypervisors**: A **hypervisor** (such as VirtualBox, VMware, or Proxmox) is software that emulates virtual hardware (virtual CPU, RAM, and disk). It mounts the `.iso` file directly and boots the guest operating system inside an isolated software window within your existing host OS.
2. **Subsystem Packages (WSL2)**: Lightweight OS distributions downloaded directly via the Windows command line (e.g., `wsl --install -d Ubuntu`).
   * **What WSL2 Actually Is**: **Windows Subsystem for Linux 2 (WSL2)** is a feature built into Windows that runs a real, lightweight Linux kernel inside an optimized, transparent virtual machine managed by Microsoft's Hyper-V. It allows users to run genuine Linux distributions (such as Ubuntu) side-by-side with Windows, executing authentic Linux command-line tools and accessing Windows files seamlessly without needing a separate computer or dual-booting.
3. **Container Base Images**: Minimal root file systems downloaded via container registries (e.g., `docker pull ubuntu:22.04` or `docker pull rockylinux:9`).
4. **Cloud Machine Images**: Pre-configured operating system disk snapshots (e.g., Amazon AMIs, Google Cloud OS Images) launched as virtual servers in seconds.

---

## 2. The Operating System Landscape: Linux, macOS/iOS, and Windows

Operating systems differ fundamentally in their kernel architectures, file system designs, and system call interfaces:

```
                  [ Operating System Architectures ]
                                  |
         +------------------------+------------------------+
         |                                                 |
  [ Unix Family ]                                    [ Windows NT ]
         |                                                 |
  +------+------+                                   - Proprietary kernel
  |             |                                   - Drive letters (C:\)
[ Linux ]   [ macOS / Darwin ]                      - Backslash paths (\)
- Ubuntu    - BSD/POSIX foundation                  - Case-insensitive
- Rocky/RHEL- Mach kernel + POSIX layer             - CRLF line endings (\r\n)
- Debian    - Native Unix Terminal                  - Win32 system calls
```

### A Brief Historical Divergence: Unix vs. Windows NT
Why are operating systems so sharply divided today?

* **The Unix Lineage (1970s, Bell Labs)**: Ken Thompson, Dennis Ritchie, and colleagues created Unix with a design philosophy centered on **simplicity, modularity, and plain-text streams**: *"Write programs that do one thing and do it well, and write programs to work together over text streams."* This model was codified into the **POSIX (Portable Operating System Interface)** standard. In 1991, Linus Torvalds created the open-source **Linux** kernel following this standard, while Apple built **macOS** on top of the Unix-certified Darwin/BSD subsystem.
* **The Windows NT Lineage (1990s, Microsoft)**: Led by Dave Cutler (the architect behind DEC's VMS operating system), Microsoft designed **Windows NT** from scratch as a commercial, desktop-oriented, enterprise operating system. Instead of text streams and the POSIX model, Windows NT was built around an object-oriented kernel, structured graphical event messaging, memory handles, and the **Win32 API**.

Because Unix and Windows were built on fundamentally incompatible architectural models (plain-text files and process forking vs. object handles and graphical thread messages), software compiled for one cannot run on the other without an translation or virtualization layer.

### The Modern Landscape

* **Linux (Ubuntu, Debian, Rocky Linux, Red Hat Enterprise Linux)**:
  * Open-source, monolithic Unix-like kernel.
  * The universal standard for servers, supercomputers, cloud infrastructure, and computational biology. Over 95% of scientific command-line tools are written and tested natively for Linux.
  * Features strict POSIX compliance, case-sensitive file systems, forward-slash paths (`/`), and line feed (`LF`) line endings.
* **macOS and iOS (Darwin / Apple Ecosystem)**:
  * Built on **Darwin** (an open-source Unix core combining the Mach microkernel and FreeBSD subsystems). macOS is an officially certified **UNIX** operating system.
  * Provides a native Unix terminal (Zsh/Bash) with POSIX compatibility, making it popular for local development.
  * **iOS** shares the same Darwin kernel core as macOS, but is locked to a sandboxed touch-first mobile environment without direct shell or compiler access.
* **Windows (Windows NT)**:
  * Dominant in desktop personal computing with drive letters (`C:\`) and backslashes (`\`). Modern Windows bridges the scientific compatibility gap by embedding Linux natively via **WSL2**.

### Why OS Architecture Determines Software Compatibility
When a program is compiled from source code (e.g., C, C++, or Rust), the compiler translates code into machine code packaged in an OS-specific binary format:
* **Linux**: Executable and Linkable Format (**ELF**)
* **macOS**: Mach-O (**Mach Object**)
* **Windows**: Portable Executable (**PE / `.exe`**)

A Linux ELF binary cannot execute directly on the Windows NT kernel because it relies on Linux-specific system calls (such as `fork()`, `clone()`, `epoll()`, or POSIX file signals). Unless developers write custom Win32 port wrappers, scientific software will not run natively on Windows.

---

## 3. Demystifying Windows Terminals and Shells

A common source of confusion on Windows is the presence of multiple command-line interfaces. Understanding the difference requires separating the **Terminal** (the window host) from the **Shell** (the command interpreter).

* **Terminal Emulator** (e.g., *Windows Terminal*, *PuTTY*, *iTerm2*, *Alacritty*): The graphical application that opens a window, captures keyboard input, and renders text on screen.
* **Shell** (e.g., *Command Prompt*, *PowerShell*, *Bash*, *Zsh*): The background program running inside the terminal that interprets text commands, executes scripts, and manages processes.

```
+-----------------------------------------------------------------------+
| Windows Terminal (Graphical Window / Display Host)                   |
|                                                                       |
|  [Tab 1: cmd.exe]    [Tab 2: PowerShell]    [Tab 3: WSL2 Ubuntu Bash] |
|  - MS-DOS Legacy     - Object-Oriented      - Genuine Linux Kernel    |
|  - 'dir', 'copy'     - Cmdlets & Objects    - 'ls -la', 'grep', POSIX |
+-----------------------------------------------------------------------+
```

### Windows Command-Line Ecosystem Breakdown

| Tool / Environment | Type | What It Actually Is | When to Use |
| :--- | :--- | :--- | :--- |
| **Command Prompt (`cmd.exe`)** | Local Shell | Legacy Windows shell dating back to MS-DOS. Non-POSIX, limited syntax. | Legacy Windows system administration; generally avoided for scientific work. |
| **Windows PowerShell (`pwsh`)** | Local Shell | Modern Windows automation shell built on Microsoft .NET. Commands pass structured .NET objects through pipes rather than plain text streams. | Advanced Windows administration, Azure scripting, Windows automation. |
| **Anaconda Prompt** | Local Shell Shortcut | A standard Command Prompt or PowerShell shortcut that executes a startup script to inject Conda binary paths into `%PATH%`. | Running base Conda environments on Windows without modifying global system paths. |
| **PuTTY** | Remote Terminal Client | A standalone graphical SSH/Telnet client used on Windows to connect to remote Linux servers over the network. It does not execute local Windows commands. | Connecting to remote HPC clusters or servers from legacy Windows systems that lack a built-in OpenSSH client. |
| **WSL2 (Windows Subsystem for Linux 2)** | Virtualized Linux Environment | A lightweight utility running a **genuine Linux kernel** inside a managed virtual machine. Executes native Linux **ELF binaries** and standard Unix shells (Bash/Zsh). | **The recommended environment** for running scientific, bioinformatics, and Unix pipelines on Windows machines. |

---

## 4. Why Commands Differ Across Terminals (`pwd`, `ls`, and Shell Built-ins)

When typing a command like `ls` or `pwd`, the terminal's behavior depends on whether the command is a **Shell Built-in** or an **External Executable**, and how that specific shell resolves aliases.

```
Command Typed: 'ls'
       |
       +---> In cmd.exe --------> [Error: 'ls' is not recognized]
       |                          (cmd.exe expects the built-in 'dir')
       |
       +---> In PowerShell ------> [Alias: maps 'ls' -> 'Get-ChildItem']
       |                          (Outputs .NET System.IO.DirectoryInfo objects)
       |
       +---> In Linux/WSL2/macOS -> [Executes POSIX /bin/ls binary]
                                  (Streams raw newline-delimited text characters)
```

### 1. Shell Built-ins vs. `$PATH` Executables
* **Shell Built-in**: A command implemented directly inside the shell's own code (e.g., `cd`, `exit`, `echo`). It does not require launching a separate process.
* **External Executable**: A standalone binary program located on disk. When you run `git` or `python`, the shell searches through the directories listed in your **`$PATH`** (or Windows **`%PATH%`**) environment variable to locate and execute the binary.

### 2. Why `ls` and `pwd` Behave Differently
* **In `cmd.exe`**: Fails with `'ls' is not recognized as an internal or external command`. `cmd.exe` has no built-in `ls` or `pwd` commands; it uses `dir` and `cd` instead.
* **In PowerShell**: `ls` and `pwd` succeed, but they are **aliases**. PowerShell secretly translates `ls` to `Get-ChildItem` and `pwd` to `Get-Location`. Crucially, PowerShell returns rich structured objects with properties (e.g., `.Length`, `.LastWriteTime`) rather than raw Unix text.
* **In Linux / macOS / WSL2 (Bash/Zsh)**: `ls` and `pwd` are standard POSIX utilities that output plain ASCII/UTF-8 byte streams.

### 3. The Unix Pipe Philosophy (`|`) vs. Object Pipes
In Unix/Linux, utilities adhere to the **POSIX standard stream philosophy**:
$$\text{Program A (stdout)} \xrightarrow{\text{pipe } |} \text{Program B (stdin)} \xrightarrow{\text{pipe } |} \text{Program C (stdout)}$$

Because tools read and write standard text streams (`stdin`, `stdout`, `stderr`), developers can chain distinct modular tools together on the command line:

```bash
# Standard POSIX Pipeline: Streams text without saving intermediate disk files
cat samples.tsv | cut -f2 | sort | uniq -c | sort -nr
```

Attempting to run this pipeline in `cmd.exe` or PowerShell fails because Windows shells either lack these core utilities or treat the pipe operator as an object pipeline rather than a raw byte stream.

---

## 5. Cross-Platform Failure Modes That Break Pipelines

When moving data or scripts between Windows and Linux/macOS environments, three subtle differences frequently cause silent pipeline failures.

### A. Line Endings: `LF` vs. `CRLF`
* **Unix / Linux / macOS**: Uses **Line Feed** (`\n` or `LF`, ASCII byte `0x0A`) to terminate lines.
* **Windows**: Uses **Carriage Return + Line Feed** (`\r\n` or `CRLF`, ASCII bytes `0x0D 0x0A`).

```
Unix File:    line1\nline2\n
Windows File: line1\r\nline2\r\n
```

**The Failure Mode**:
When a script or TSV file written on Windows is transferred to a Linux server, Linux tools interpret the hidden `\r` character as part of the data string:
* A Bash script fails immediately on execution:
  ```
  /bin/bash^M: bad interpreter: No such file or directory
  ```
  *(The `^M` is the visual representation of the trailing carriage return `\r`).*
* A file path in a config file (`/data/reads.fastq\r`) fails with `File Not Found` because the operating system looks for a file containing a trailing carriage return in its name.
* **Remediation**: Use `dos2unix filename.sh` to strip carriage returns, or configure code editors (such as VS Code) to default to `LF` line endings.

### B. File Paths and Directory Separators
* **Unix / Linux / macOS**: Uses forward slashes (`/`), rooted at `/` (e.g., `/home/user/project/data.tsv`).
* **Windows**: Uses backslashes (`\`), rooted at drive letters (e.g., `C:\Users\user\project\data.tsv`).

**The Failure Mode**:
Hardcoding backslashes in Python scripts or shell wrappers breaks immediately on Linux. In Python, always use the platform-agnostic `pathlib.Path` standard library:

```python
# Recommended cross-platform path handling
from pathlib import Path

data_dir = Path("data") / "raw_reads"
file_path = data_dir / "sample1.fastq.gz"
```

### C. Case Sensitivity
* **Linux**: **Case-sensitive**. `sample.fa`, `Sample.fa`, and `SAMPLE.FA` are three separate, distinct files.
* **macOS & Windows**: Typically **case-preserving but case-insensitive**. The file system remembers capitalization for display, but treats `sample.fa` and `SAMPLE.FA` as the same file.

**The Failure Mode**:
A script developed on macOS with inconsistent file casing (e.g., creating `Sample1.txt` but reading `sample1.txt`) runs fine locally, but fails with `FileNotFoundError` as soon as it is deployed to a Linux HPC cluster.

---

## 6. Development Strategy: The Unified POSIX Approach

To avoid cross-platform friction and ensure reproducibility:

1. **Windows Users**: Install **WSL2 (Ubuntu)** and conduct all development, data processing, and scripting within the WSL2 Linux environment.
2. **macOS Users**: Utilize the built-in terminal (Zsh) and package managers like Homebrew for local work.
3. **Standards**: Default all text editors to **UTF-8 encoding** and **LF line endings**.

> **Summary**: Operating systems provide the kernel and system call foundation for executing software. Because scientific computing standardizes on POSIX/Linux, developing in a Unix-compliant environment (Linux, macOS, or WSL2) eliminates line-ending bugs, path errors, and binary incompatibilities.

