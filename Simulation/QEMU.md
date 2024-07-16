## Choose an `STM32 emulator` or **virtualization software**

1. Several options available for *emulating / virtualizing* STM32 hardware
   - QEMU[^qemu]
   - STM32CubeIDE
   - Keil MDK
2. Set up the emulator or virtual machine: Install and configure the chosen emulator or virtualization software on your Windows PC. Follow the documentation and instructions provided with the selected tool to set up the virtual environment
---
### Set up QEMU
- For **full system emulation**.[^fse] See [[#References]]
- Makes use of [[Hypervisor]]
- 
---
### Virtualisation Accelerators
`Virtual model` of target machine[^model-contents] is provided by QEMU System Emulation
This is used to run guest OS
> [!tip] Capability of emulation of various CPUs
> Is achieved by supporting 👇
> - [[Hypervisor]]
> -  `Tiny Code Generator` which is a `JIT`

---
### Features
- QEMU System Emulation also provides various `device models` to emulate different *hw components* that are to be **added to the VM**. These are various `VirtIO` devices **tuned for virtualization**
- [Featured Block Layer](https://www.qemu.org/docs/master/interop/live-block-operations.html#live-block-operations) used for constructing storage topology which supports *redirection, networking, snapshots and migration* of data
- `chardev` system allows to handle IO using `stdio`, `files`, `unix sockets` and `TCP` networking
- Interfaces are provided👇
	- `Human Monitor Protocol` (HMP) to manage and inspect devices and system state
	- `QEMU Monitor Protocol` (QMP) is `API` for other tools[^VM-management] to create, control, manage VM's
- `gdbstub` allows for debugging common accelerators with `gdb`
---
### Running
#### Boot
Depends on architecture - some can boot a VM with *just the disc image*
Other architectures(Non x86_64) may require *more explicit command lines* to define **machine and boot behaviour**.
See [Example commands for targets](https://www.qemu.org/docs/master/system/targets.html#system-targets-ref) ➡️ **Arm System Emulator**

User friendly *alternatives to command line* are some projects built around `libvirt` framework
See [^VM-management]
These can be used to build VM images tailored to run on hardware that we have
> Uses *feature probing* under the hood

---
## References
[System Emulation - Qemu](https://qemu-project.gitlab.io/qemu/system/index.html)
[Setting up environment](https://youtu.be/Zvbarf1CSGs)
![[Pasted image 20230628154821.png]]

---
[^qemu]: QEMU is a versatile emulator that **supports various architectures**, including *ARM-based `STM32` microcontrollers*.
[^fse]: Used when emulating an entire system - *one or more processors and peripherals*
[^model-contents]: consists of CPU, memory and emulated devices
[^VM-management]: example - Virt Manager uses `libvirt` framework
