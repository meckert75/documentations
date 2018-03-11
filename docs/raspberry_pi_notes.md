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

## Capture Photos

After enabling the Raspberry Camera and rebooting the Raspberry Pi, test the camera.

```
raspistill -o test.jpg -q 80 -w 640 -h 480
```
## Install Docker

```
curl -sSL https://get.docker.com | sh
```
