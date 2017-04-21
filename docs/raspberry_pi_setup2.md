# Setting Up Raspberry Pi using a Raspberry Pi

If you have a running Raspberry Pi it can be used to initialize a fresh SD card including Wi-Fi configuration.

Connect the Micro SD to one of the running Pi's USB ports.
```bash
$ ssh pi@raspberrypi.local
$ df -hl
Filesystem      Size  Used Avail Use% Mounted on
/dev/root       7.2G  4.0G  2.9G  59% /
devtmpfs        459M     0  459M   0% /dev
tmpfs           463M     0  463M   0% /dev/shm
tmpfs           463M  6.4M  457M   2% /run
tmpfs           5.0M  4.0K  5.0M   1% /run/lock
tmpfs           463M     0  463M   0% /sys/fs/cgroup
/dev/mmcblk0p1   63M   21M   42M  33% /boot
tmpfs            93M     0   93M   0% /run/user/1000
/dev/sda1        15G  1.1M   15G   1% /media/pi/UNTITLED
```
The Raspberry Pi will auto mount the SD card and will show up in the devices list.
In the above output the SD card partition is listed as `/dev/sda1`, minus the partition number will give
the device `/dev/sda`.

## Write Raspbian Image to SD Card
```bash
$ wget https://downloads.raspberrypi.org/raspbian_latest
$ unzip -p raspbian_latest | sudo dd of=/dev/sda bs=4096
```
