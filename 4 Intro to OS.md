Operating System is a collection of specialized software that:
- Access to hardware devices
- Control and allocate system resources
- Customize and tune the system

Operating System consists of several parts:
- Bootloader: First program run by the system on start-up, which loads remainder of the OS kernel.
	- On wintel system, bootloader is in Master Boot Record(MBR) on the hard disk.
- Kernel: 
- System Program: Programs provided by OS to allow:
	- Access to programs.
	- Configuration of OS.
	- System maintenance.

Onion Model: 
- Application
- Kernel
- Device Drivers
- Machine Instructions
- Microcode
- Hardware

OS Type:
- Monolithic Kernel:
	- Major parts of OS (Device Drivers, File Systems, etc.) running in kernel space
	- Linux, Windows
- Microkernel:
	- Main part of kernel is in kernel space:
		- Scheduler, Process Management, Memory Management, etc.
	- Other parts of kernel run in user space:
		- File System, USB device drivers.
	- MacOS

User Mode & Kernel Mode:
- Protection between kernel and executing programs.
- When program needs to access sensitive resources in kernel, use **System Call**.
- During system call, running kernel code in kernel mode.
- After call, back to user mode.
