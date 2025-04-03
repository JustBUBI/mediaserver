lsblk

elete and recreate sda3 (since parted can’t resize extended partitions directly):
sudo parted /dev/sda
    In the parted shell:
    print → Note the start sector of sda3 (e.g., 1234MB).
    rm 3 → Delete sda3.
    mkpart primary 1234MB 100% → Recreate sda3 to fill the disk.
    set 3 lvm on → Flag it as an LVM partition.
    quit → Exit.

Reload the partition table:
sudo partprobe /dev/sda

Tell LVM to use the new space in sda3:
sudo pvresize /dev/sda3

Verify free space:
sudo vgs
Output should show free space under VFree (e.g., 132.00g).

Expand the root LV (ubuntu--lv) to use all free space:
sudo lvextend -l +100%FREE /dev/ubuntu-vg/ubuntu-lv
(Replace ubuntu-vg and ubuntu-lv with your actual names from sudo lvdisplay.)

Resize the Filesystem:
sudo resize2fs /dev/mapper/ubuntu--vg-ubuntu--lv

Check disk space:
df -h /

Should now show all used.
eg: /dev/mapper/ubuntu--vg-ubuntu--lv  256G   25G  220G  10% /