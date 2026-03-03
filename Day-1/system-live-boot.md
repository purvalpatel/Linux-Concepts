How to resolve grub error on boot ?
------------------------------
<img width="635" height="478" alt="image" src="https://github.com/user-attachments/assets/5b59d13d-48c6-4302-82d9-f9db44b38df8" />

This error typically means that the GRUB bootloader (which is responsible for loading the operating system) has encountered a problem. <br>
### Cause of >Grub error: 
1. Corrupted GRUB Configuration
2. Missing or Corrupted Boot Files (Kernel, initramfs)
3. Incorrect Partition Table or Boot Partition Issues
4. GRUB Misconfiguration (Incorrect Root or UUID)
5. Corrupt Filesystem (ext4, xfs, etc.)
6. Incorrect Boot Mode (UEFI vs Legacy)

### How to resolve ?

#### Step 1 — Boot from Live CD/USB

- Boot from a live CD or USB (Ubuntu, Fedora, etc.). <br>
- Open a terminal.

#### Step 2 — Mount the System Partition

**Mount the root filesystem:** <br>
```
sudo mount /dev/sda1 /mnt  # Adjust the device name based on your system
```
Mount /boot (if separate):
```
sudo mount /dev/sda2 /mnt/boot  # Adjust as necessary
```
Mount the system directories:
```
sudo mount --bind /dev /mnt/dev
sudo mount --bind /proc /mnt/proc
sudo mount --bind /sys /mnt/sys
```

#### Step 3 — Reinstall GRUB

Reinstall GRUB to the disk:
```
sudo grub-install --root-directory=/mnt /dev/sda  # Adjust based on your disk
```

Regenerate the GRUB configuration:
```
sudo chroot /mnt
sudo update-grub
```

#### Step 4 — Reboot

Exit the chroot environment:
```
exit
```
Unmount everything:
```
sudo umount /mnt/dev /mnt/proc /mnt/sys /mnt/boot /mnt
```
Reboot the system:
```
sudo reboot
```
