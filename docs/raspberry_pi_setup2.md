# Setting Up Raspberry Pi using a Raspberry Pi
If you have a running Raspberry Pi it can be used to initialize a fresh SD card including Wi-Fi configuration.

Connect the Micro SD to one of the running Pi's USB ports. If there are multiple USB storage devices connnected, they will be `/dev/sda`, `/dev/sdb`, etc. and the partitions will be `/dev/sda1`, `/dev/sda2`, ..., `/dev/sdb1`, etc.

Identify the device and partitions of the SD card.
```bash
$ ssh pi@raspberrypi.local
$ df -hl | grep '/dev/sd'
/dev/sda1        15G  1.1M   15G   1% /media/pi/UNTITLED
```
The Raspberry Pi will auto mount the SD card after inserting.
The SD card partition (`/dev/sda1`) in the above example is mounted at `/media/pi/UNTITLED`. Device in this case is `/dev/sda`.

## Write Raspbian Image to SD Card
Unmount all partitions of the SD card being being initialized with Raspbian and write the latest version of Raspbian to the card.
```bash
$ sudo umount /dev/sda1
$ wget -O - https://downloads.raspberrypi.org/raspbian_latest | funzip | sudo dd of=/dev/sda bs=4096
```

## Write Raspbian Settings
Mount the newly created partitions on the SD card.
Assuming the SD card is `/dev/sda`:
```bash
$ sudo mkdir -p /media/pi/boot && sudo mount /dev/sda1 /media/pi/boot
$ sudo mkdir -p /media/pi/raspbian && sudo mount /dev/sda2 /media/pi/raspbian
```

### Configure Network
SSH will allow you to log into the Raspberry Pi remotely.
```bash
$ sudo touch /media/pi/boot/ssh
```
Useful in particular when setting up Raspbian for Raspberry Pi Zero W that doesn't have an Ethernet port to connect to. Replace `NetworkName` and `password` with the settings for your network.
```bash
$ sudo sh -c 'wpa_passphrase NetworkName password >> /media/pi/raspbian/etc/wpa_supplicant/wpa_supplicant.conf'
```
Assign a different hostname for the new Pi (optional but necessary if there is another Pi named `raspberrypi` on the network)
```bash
$ sudo sed -i -e "s/raspberrypi/mynewpi/" /media/pi/raspbian/etc/hosts /media/pi/raspbian/etc/hostname
```

### Setting Keyboard Layout
This will not be applicable if you only will be working with the Pi through a `ssh` connection but if you have the need to connect the Pi to a TV and operate it with a keyboard and mouse, you will want to change the default keybaord layout which is going to be set to `gb`.
```bash
$ sudo sed -i -e "s/XKBLAYOUT=\"..\"/XKBLAYOUT=\"us\"/" /media/pi/raspbian/etc/default/keyboard
```

### Setting the Locale
By default the locale enabled on the Raspberry Pi is `en_GB.UTF-8`. Make sure the desired locale is available to the Raspberry Pi by enabling (commenting out) the line in the `locale.gen` file. To check which locales are enabled, run `localedef --list-archive`.
```bash
$ locale=en_US.UTF-8
$ sudo sed -i -e "s/# \(${locale}\)/\1/g" /media/pi/raspbian/etc/locale.gen
$ sudo sh -c "echo 'LANG=$locale' > /media/pi/raspbian/etc/locale.conf"
```

## Finish Up
Unmount device
```bash
$ sudo umount /dev/sda1 /dev/sda2
```
Stick the Micro SD card into the new Raspberry Pi and boot it up. After a about 30 - 60 seconds the new Raspberry Pi will be online. Default password will be `raspberry`. Change the password.

```bash
$ ssh pi@mynewpi.local
pi@mynewpi:~ $ passwd 
Changing password for pi.
(current) UNIX password: 
Enter new UNIX password: 
Retype new UNIX password: 
passwd: password updated successfully
```

# References
* [caffinc.github.io/2016/12/raspberry-pi-3-headless/](https://caffinc.github.io/2016/12/raspberry-pi-3-headless/)
* [wiki.archlinux.org/index.php/locale](https://wiki.archlinux.org/index.php/locale)

<hr>

Back to [Index](./index.md)
