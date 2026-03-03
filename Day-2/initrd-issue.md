error : file /boot/initrd.img-3.10.12-generic' not found
---------------------------

if error found during booting system, follow the steps below:

1. Boot your machine with a Live Media
2. Open terminal and get partition 
```
fdisk -l
```
3. mount Filesystem
```
sudo mount /dev/sda1 /mnt
 
sudo mount --bind /proc /mnt/proc
sudo mount --bind /dev /mnt/dev
sudo mount --bind /sys /mnt/sys
```

4.Chroot /mnt and creating a Backup of the initrd image
```
sudo chroot /mnt
ls /boot/initrd*
or ls /lib/modules

mv /boot/initrd.img-3.11.0.12-generic /boot/old-initrd.img-3.11.0.12-generic-old
```
5. Building initrd.img
```
mkinitrd /boot/initrd.img-3.11.0.12-generic 3.11.0.12-generic-old
update-initramfs -c -k 3.11.0.12-generic
```

7. Finalizing Grub loader and unmounting
```
grub2-mkconfig -o /boot/grub2/grub.cfg
update-grub
sudo unmount /mnt/proc
sudo unmount /mnt/sys
sudo unmount /mnt/dev
sudo unmount /mnt
```

8. Reboot system.
```
init 6
```
