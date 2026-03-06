### Example Scenario:

#### Trying to create file:
```
touch test.txt
```
Error:
```
touch: cannot touch 'test.txt': Read-only file system
```
Check mount:
```
mount | grep /dev/sda1
```
Fix:
```
sudo mount -o remount,rw /dev/sda1
```

#### Common Real-World Cause (Very Frequent)

**On servers:** <br>

```
EXT4-fs error → kernel remounts filesystem read-only
```
This protects data.
<br>
**You must run:**
```
fsck -y /dev/sda1
```
after **reboot**.
<br>
<br>
Quick Troubleshooting Checklist
```
df -h
mount
dmesg | tail
lsblk
```
