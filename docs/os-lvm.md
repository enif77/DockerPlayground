# Resizing disk on Ubuntu with LVM

## Links

- https://linuxhint.com/list_disks_ubuntu/
- https://www.linuxtechi.com/extend-lvm-partitions/


## HOWTO

To get the list of disks:

```
df -h
```

With output:

```
Filesystem                         Size  Used Avail Use% Mounted on
udev                               1.9G     0  1.9G   0% /dev
tmpfs                              393M  3.0M  390M   1% /run
/dev/mapper/ubuntu--vg-ubuntu--lv  234G   34G  189G  16% /
tmpfs                              2.0G     0  2.0G   0% /dev/shm
tmpfs                              5.0M     0  5.0M   0% /run/lock
tmpfs                              2.0G     0  2.0G   0% /sys/fs/cgroup
/dev/sda2                          976M  301M  609M  34% /boot
...
/dev/loop7                          33M   33M     0 100% /snap/snapd/13170
tmpfs                              393M     0  393M   0% /run/user/1000
```

The root volume name is `/dev/mapper/ubuntu--vg-ubuntu--lv`.

To see volumes:

```
sudo vgdisplay
```

With output:

```
  --- Volume group ---
  VG Name               ubuntu-vg
  System ID
  Format                lvm2
  Metadata Areas        1
  Metadata Sequence No  3
  VG Access             read/write
  VG Status             resizable
  MAX LV                0
  Cur LV                1
  Open LV               1
  Max PV                0
  Cur PV                1
  Act PV                1
  VG Size               <237.47 GiB
  PE Size               4.00 MiB
  Total PE              60792
  Alloc PE / Size       60792 / <237.47 GiB
  Free  PE / Size       0 / 0
  VG UUID               saMzah-WG9a-yYMt-RkrK-eKiG-kfpE-M8xR8n
```

Extending the root volume to fit the size of the partition and resizing the file system:

```
sudo lvextend -l +100%FREE /dev/mapper/ubuntu--vg-ubuntu--lv
sudo resize2fs /dev/mapper/ubuntu--vg-ubuntu--lv
```
