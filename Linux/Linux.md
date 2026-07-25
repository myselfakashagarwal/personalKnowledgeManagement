[[Linux]] 
## Linux Architecture (HL)
Linux is a kernel which sits between software and hardware. Talking about the architecture the lowermost layer is hardware itself aka the resources like memory storage peripheral devices etc. The above lies kernel which acts as an interface between hardware and user level applications. This kernel carries out tasks such as system calls and security, memory management, process management etc. The top of the kernel like utilities like shell which executes commands which are programs used by the user/applications.
### `Shell` 
Shell is the command interpreter which executes commands aka the binaries and executables.
A user interaction with shell have three phases login (shell session starts) ->  work -> logout (shell session ends).  Shell can be interactive or non interactive. 
### `Terminal`
User cannot interact directly with the shell forit user uses terminal a terminal is the interface between shell and interface the user send input commands to terminal which sends to shell for execution.  Terminal also have a session called tty session. A terminal is also a process.  The real terminal is an old concept nowadays terminal emulator exists to provide for. 
### `Command`
A command can be defined as the program file or the compiled binary which is interpreted by the shell, These program files are distributed throughout the system. When a command with name is invoked the shell searches for the command's program file throughout  the file system location when found executes it with exec system call *other follows*. The shell is a program too when invoked becomes a process. The shell have some builtin commands too which are part of the shell process and others which are found throughout the file system, whose location is available in PATH variable. 

| Aspect            | Built-in Commands                                            | External Commands                                                |
| ----------------- | ------------------------------------------------------------ | ---------------------------------------------------------------- |
| **Definition**    | Commands executed directly by the shell.                     | Standalone executable programs in the filesystem.                |
| **Execution**     | Runs inside the shell process itself.                        | Invokes a new process using `exec()` system call.                |
| **Location**      | Not stored as separate binaries; part of shell (e.g., bash). | Stored in directories like `/bin`, `/usr/bin`, `/usr/local/bin`. |
| **Examples**      | `cd`, `echo`, `pwd`, `alias`, `export`                       | `ls`, `grep`, `cat`, `find`, `tar`                               |
| **Performance**   | Faster (no process creation overhead).                       | Slower (requires process fork/exec).                             |
| **Dependence**    | Works even if `/bin` or `/usr/bin` is inaccessible.          | Needs binary file present in PATH.                               |
| **Customization** | May vary between different shells (bash, zsh, ksh).          | Same binary behaves consistently across shells.                  |
### Types of Commands 
##### `Inbuilt` 
Inbuilt commands are part of the shell program; they also do not generate any PID and are faster in nature. For example, `pwd`. You can explore all in your environment by the command  `compgen -b` 
##### `External`
External commands are commands that are not part of the shell; rather, they have their compiled binaries stored in the system. On execution, they take more time and generate PIDs. Examples include `/bin/ls` and `/bin/pwd`. 
##### `Keyword` 
Keywords are part of the shell but are reserved. Just like inbuilt, they do not generate any PID. For example, `if`. You can explore all in your environment by the command `compgen -k`
##### `Hashed` 
Hashed commands are nothing but commands that have been stored in a hash table for faster retrieval. You can access all of them by the command `hash -l` and add one to it by the command `hash COMMAND-NAME`
##### `Alias` 
Aliases are considered as commands, though they are just set of character mapped to a command. You can get all of them by the command `compgen -a`<br> example `alias cd="rm -rf"`
## Linux boot sequence 
### `BIOS POST` 
POST(Power On Self Test) in this stage BIOS runs a post test to insure that all the hardware components attached to the device are working correctly if POST fails computer may not be operable and computer will not move to second step of boot process.
### `Boot loader`  
After the POST sequence BIOS load and executes the boot code from the hard disk of the boot device, located in the first sector of the hard disk. In linux this code is located in the /boot file path The boot loader provide user with the boot screen often with multiple os to boot into. Once the choice is provided the boot loader lodes the kernel into the memory supplying with parameters and handling the control to the kernel. Example of the boot loader is GRUB 2(Grand Unified Boot Loader version 2) it is primary boot loader for most of the linux distributions.
### `Kernel Initialisation` 
The kernel loaded into the memory is usually decompressed then the kernel loads into the memory and starts executing during this time kernel carries out tasks such as initialising hardware, cpu scheduling and memory management etc.
### `Init` 
When the kernel gets completely operational it divides the memory into two spaces user space and kernel space, The user resides in the user space along with the user’s programs this user space have restricted access to the hardware on the other hand the kernel space is unrestricted so in order to perform tasks the user use system calls. The first process in the user space is init system which spawns other processes and so on stabilising the system. The files of init can be found at `/sbin/init`
#### `Linux runlevel`
Runlevels are a legacy concept from the **SysVinit** initialization system. They define the **operational state of the machine** by specifying which services and daemons should run. Each runlevel corresponds to a particular set of scripts located in `/etc/rc.d/` or `/etc/init.d/`, executed sequentially.

| Runlevel | Meaning                                               | Technical Behavior                                                                                         |
| -------- | ----------------------------------------------------- | ---------------------------------------------------------------------------------------------------------- |
| 0        | **Halt**                                              | Shuts down the system. All processes are terminated, filesystems unmounted, and the kernel halts.          |
| 1        | **Single User Mode**                                  | Minimal mode for recovery/maintenance. Only root shell on console, no networking, no multi-user processes. |
| 2        | **Multi-user (no networking or GUI in some distros)** | Multiple shells enabled, but often networking disabled (implementation varies by distro).                  |
| 3        | **Multi-user with Networking**                        | Full multi-user text-mode with networking. Servers like SSH, Apache, NFS start here.                       |
| 4        | **Unused/Custom**                                     | Reserved for user-defined states. Rarely used by distros.                                                  |
| 5        | **Multi-user with GUI**                               | Same as 3, but graphical display manager (GDM, LightDM, etc.) is started.                                  |
| 6        | **Reboot**                                            | All processes killed, filesystems unmounted, and system reboots.                                           |
#### `Linux systemd for target`
With the advent of **systemd**, runlevels were replaced by **targets**, which are more flexible, unit-based abstractions for defining system states. A target is essentially a collection of unit files that group services, mounts, devices, and other resources to achieve a specific operational mode. For example, `poweroff.target` is equivalent to runlevel 0, `rescue.target` to runlevel 1, `multi-user.target` to runlevel 3, and `graphical.target` to runlevel 5. Unlike runlevels, targets can have dependencies and can be extended or customised by administrators to fit complex workloads. System states are managed through `systemctl`, where commands like `systemctl isolate multi-user.target` switch the system mode, and `systemctl get-default` shows the default boot target. This model provides parallel initialization, dependency handling, and greater extensibility compared to traditional runlevels, making systemd targets a more robust replacement.

| Runlevel | Traditional State                           | Equivalent Systemd Target                      |
| -------- | ------------------------------------------- | ---------------------------------------------- |
| 0        | Halt                                        | `poweroff.target`                              |
| 1        | Single-user                                 | `rescue.target`                                |
| 2        | Multi-user (no networking, distro-specific) | `multi-user.target` (Debian treats 2,3,4 same) |
| 3        | Multi-user with Networking                  | `multi-user.target`                            |
| 4        | User-defined                                | `multi-user.target` or custom target           |
| 5        | Multi-user with GUI                         | `graphical.target`                             |
| 6        | Reboot                                      | `reboot.target`                                |
## Linux Environment 
An environment is the virtual space where a user, along with their data, works and where tasks or workloads reside. It is built upon various interconnected factors, starting with physical aspects like system architecture, connected devices (e.g., Linux-compatible peripherals), and hardware specifications such as memory, storage, and I/O devices. At the operating system level, the type of kernel used has a low to moderate impact, while the distribution plays a more significant role by determining the package manager, available/preinstalled utilities, dependency libraries, and file structure. Configurations within the OS further define how utilities and processes function, influencing interactions and performance. Process-specific configurations also play a role, including the behaviour of the shell session, where users interact based on their privileges. External factors like network connectivity, which may include VPNs, also contribute to shaping the environment by impacting accessibility and security. Together, these elements define the structure and functionality of the environment.

[[LinuxDataFlowAndStreams]] [[LinuxFile]] [[LinuxOperations]] [[LinuxProcessAndServices]] [[LinuxStorage]] [[LinuxUserAndGroups]] 

[[Git]]
[[BashScripting]]
[[Docker]]