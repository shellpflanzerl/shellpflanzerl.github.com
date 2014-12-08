Title: Prevent systems from running fsck on boot
Date: 2014-12-08 10:00
Tags: Linux
Authors: Martin Zehetmayer

In some situation the fsck of a filesystem after a system reboot could be disastrous -
for example if you are just migrating a productive server with a very large filesystem on it.

How can this be achieved?

1. Check if the files /forcefsck or /.autofsck exists in your root directory. Delete them. 
2. Set the fs_passno in your /etc/fsck entry to zero (0).
3. If you are using a system with systemd you can simply add 'fsck=skip' to your boot options.

To check if a system is running a fsck on the next reboot you can check if the mount count or the 
mount interval is exceeding the configured values.

    :::console
    # dumpe2fs -h /dev/mapper/my_vol_to_check 
    ...
    Mount count:              8
    Maximum mount count:      28
    Check interval:           15552000 (6 months)
    Next check after:         Wed Jan  7 12:18:30 2015
    ...

If you want to disable these checks you use tune2fs: 

    :::console
    tune2fs -c0 -i0 /dev/mapper/my_vol_to_check
