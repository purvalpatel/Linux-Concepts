An operating system (OS) is the low-level software that manages resources, controls peripherals, and provides basic services to other software. <br>
In Linux, there are 6 distinct stages in the typical booting process.
```
┌─────────────────────────────────────────────────┐
│             1. BIOS / UEFI   -- > MBR            │
│     (Power-on, hardware initialization)          │
└────────────────────┬────────────────────────────┘
                     ▼
┌─────────────────────────────────────────────────┐
│           2. Boot Loader (GRUB)                   │
│     (Loads kernel into memory)                    │
└────────────────────┬────────────────────────────┘
                     ▼
┌─────────────────────────────────────────────────┐
│             3. Kernel Initialization              │
│     (Mounts root filesystem)                      │
└────────────────────┬────────────────────────────┘
                     ▼
┌─────────────────────────────────────────────────┐
│             4. Init Process (PID 1)               │
│     systemd / SysVinit / Upstart                  │
└────────────────────┬────────────────────────────┘
                     ▼
┌─────────────────────────────────────────────────┐
│           5. Runlevel / Target                    │
│     (multi-user.target / graphical.target)        │
└────────────────────┬────────────────────────────┘
                     ▼
┌─────────────────────────────────────────────────┐
│         6. User Space & Login Prompt              │
│     (Shell, GUI, Applications)                    │
└─────────────────────────────────────────────────┘
```

### 1. BIOS

- `BIOS` stands for **Basic Input/Output System**. the BIOS loads and executes the Master Boot Record (MBR) boot loader. <br>
- When you turn on your computer, the `BIOS` first performs some integrity checks of the harddisk. Then, `BIOS` searches for, loads, and executes the boot loader program, which can be found in the Master Boot Record (MBR). The MBR is sometimes on a USB stick or CD-ROM such as with a live installation of Linux. <br>
- Once the boot loader program is detected, it's then loaded into memory and the BIOS gives control of the system to it. <brr>

### 2. MBR
- `MBR` stands for **Master Boot Record**, and is responsible for loading and executing the **GRUB boot loader**. <br>
- The `MBR` is located in the 1st sector of the bootable disk, which is typically `/dev/hda`, or `/dev/sda`, depending on your hardware. <br>
- The `MBR` also contains information about **GRUB**, or **LILO** in very old systems.

### 3. GRUB
- Sometimes called `GNU GRUB`, which is short for **GNU GRand Unified Bootloader**, is the typical boot loader for most modern Linux systems. <br>
- The GRUB splash screen is often the first thing you see when you boot your computer. <br>
- It has a simple menu where you can select some options. If you have multiple kernel images installed, you can use your keyboard to select the one you want your system to boot with. By default, the latest kernel image is selected. <br>

- The splash screen will wait a few seconds for you to select and option. If you don't, it will load the default kernel image. <br>
<img width="695" height="301" alt="image" src="https://github.com/user-attachments/assets/ceba9433-50f6-497c-81e6-93862ce24280" /> <br>

- In many systems you can find the GRUB configuration file at `/boot/grub/grub.conf` or `/etc/grub.conf`. Here's an example of a simple grub.conf file: 
```
#boot=/dev/sda
default=0
timeout=5
splashimage=(hd0,0)/boot/grub/splash.xpm.gz
hiddenmenu
title CentOS (2.6.18-194.el5PAE)
      root (hd0,0)
      kernel /boot/vmlinuz-2.6.18-194.el5PAE ro root=LABEL=/
      initrd /boot/initrd-2.6.18-194.el5PAE.img
```

### 4. Kernel

- The **kernel** is often referred to as the core of any operating system, Linux included. It has complete control over everything in your system. <br>
- In this stage of the boot process, the kernel that was selected by **GRUB** first mounts the root file system that's specified in the grub.conf file. Then it executes the `/sbin/init` program, which is always the first program to be executed. You can confirm this with its process id (PID), which should always be 1. <br>
- The kernel then establishes a temporary root file system using Initial RAM Disk (initrd) until the real file system is mounted. <br>

### 5. Init
- At this point, your system executes **runlevel** programs. At one point it would look for an init file, usually found at `/etc/inittab` to decide the Linux run level.
- Modern Linux systems use systemd to choose a run level instead. According to TecMint, these are the available run levels:

- **Run level 0** is matched by poweroff.target (and runlevel0.target is a symbolic link to poweroff.target).
- **Run level 1** is matched by rescue.target (and runlevel1.target is a symbolic link to rescue.target).
- **Run level 3** is emulated by multi-user.target (and runlevel3.target is a symbolic link to multi-user.target).
- **Run level 5** is emulated by graphical.target (and runlevel5.target is a symbolic link to graphical.target).
- **Run level 6** is emulated by reboot.target (and runlevel6.target is a symbolic link to reboot.target).

systemd will then begin executing runlevel programs. <br>

### 6. Runlevel programs

- Depending on which Linux distribution you have installed, you may be able to see different services getting started. For example, you might catch starting sendmail …. OK. <br>
- These are known as runlevel programs, and are executed from different directories depending on your run level.  <br>
- Each of the 6 runlevels described above has its own directory: <br>
```
Run level 0 – /etc/rc0.d/
Run level 1 – /etc/rc1.d/
Run level 2  – /etc/rc2.d/
Run level 3  – /etc/rc3.d/
Run level 4 – /etc/rc4.d/
Run level 5 – /etc/rc5.d/
Run level 6 – /etc/rc6.d/
```
Note that the exact location of these directories varies from distribution to distribution.
