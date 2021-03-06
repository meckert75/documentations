# Raspberry Pi Notes

A collection of useful commands collected over time while getting to know the Raspberry Pi

## Keep the Pi Up to Date

```
sudo apt-get update
sudo apt-get upgrade
```

## Install Vim

```
sudo apt-get install vim
```

## Camera

### Documentation
* [Camera Module](https://www.raspberrypi.org/documentation/hardware/camera/README.md)
* [Application Documentation](https://www.raspberrypi.org/documentation/raspbian/applications/camera.md)

### Capture Photos

After enabling the Raspberry Camera and rebooting the Raspberry Pi, test the camera.

```
raspistill -o test.jpg -q 80 -w 640 -h 480
```

### Capture Video
*([more details](https://www.raspberrypi.org/documentation/usage/camera/raspicam/raspivid.md))*

```
raspivid -o vid.h264
```

### Wrap Videos in MP4

```
sudo apt-get install -y gpac
MP4Box -add vid.h264 vid.mp4
````

## Install Docker

```
curl -sSL https://get.docker.com | sh
```

<hr>

Back to [Index](./index.md)
