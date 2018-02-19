# Hombebridge Setup

Install HombeBridge and add control it through Apple HomeKit from your iPhone, iPad or Siri.
These instructions combine the [HomeBridge Installation](https://github.com/nfarina/homebridge)
instructions and the [Running Homebridge on a Raspberry Pi](https://github.com/nfarina/homebridge/wiki/Running-HomeBridge-on-a-Raspberry-Pi)
instructions to create a simplified single document that will get HomeBridge set up on Raspbian 8 (Jessie). Need to verify
all works with the latest version of Raspbian.

Read the full instructions mentioned above when working with a 'non-standard' OS and when running into issues as there
might be additional steps to get everything running.

**Prerequisit**

Install Raspbian on a Micro SD card for the Raspberry Pi 3 or Zero W:
* [Setting up a Raspberry Pi](raspberry_pi_setup.md) _or_
* [Setting Up Raspberry Pi using a Raspberry Pi](raspberry_pi_setup2.md)

# Instructions

## Access the Pi

replace `myraspi` with the hostname of the Pi and replace `pi` with the user id if it was changed from the default.
```
ssh pi@myraspi.local
```

## Keep the Pi Up-to-Date
```
sudo apt-get update
sudo apt-get upgrade
```

## Install Node.js
### For Raspberry Pi 3 (armv7l)
```
curl -sL https://deb.nodesource.com/setup_8.x | sudo -E bash -
sudo apt-get install -y nodejs
```

### For Raspberry Pi Zero W (armv6l)
Check https://nodejs.org/dist/ for the latest version for `armv6l`.
```
wget https://nodejs.org/dist/latest-v8.x/node-v8.9.4-linux-armv6l.tar.gz
tar xJvf node-v8.9.4-linux-armv6l.tar.gz
sudo mkdir -p /opt/node
sudo mv node-v8.9.4-linux-armv6l/* /opt/node/
sudo update-alternatives --install "/usr/bin/node" "node" "/opt/node/bin/node" 1
sudo update-alternatives --install "/usr/bin/npm" "npm" "/opt/node/bin/npm" 1
```

## Install Dependencies
```
sudo apt-get install libavahi-compat-libdnssd-dev
```

## Install Homebridge
```
sudo npm install -g --unsafe-perm homebridge
```

**Note**

In some instances the `homebridge` executable isn't properly symlinked to `/usr/bin/homebridge`. If that should be the case the symlink needs to be added manually. The installed executable might be in on of the following locations:
* `/usr/lib/node_modules/homebridge/bin/homebridge`
* `/opt/node/lib/node_modules/homebridge/bin/homebridge`

Create the symlink manually. Example:
```
sudo ln -s /opt/node/lib/node_modules/homebridge/bin/homebridge /usr/bin/homebridge
```

## Install Homebridge Plugins
Find Homebridge plugins by searching for `homebridge-plugin` in the [NPM package repository](https://www.npmjs.com/search?q=homebridge-plugin).

If you're going to tinker with your Raspberry, a good first plugin to install would be the [GPIO Switch plugin](https://www.npmjs.com/package/homebridge-gpioswitch).

```
npm install -g homebridge-gpio
```

## Prepare to run Homebridge

The following instructions set up homebridge as a system service that will automatically start when the Raspberry is started. (based on [Tim Leland's instructions](https://timleland.com/setup-homebridge-to-start-on-bootup/).)

```
sudo useradd --system homebridge
sudo mkdir /var/lib/homebridge
sudo chown homebridge /var/lib/homebridge
```

## Create System Service Files

Create `homebridge` environment file.
```
sudo vim /etc/default/homebridge
```

Paste these lines:
```
# Defaults / Configuration options for homebridge
# The following settings tells homebridge where to find the config.json file and store device and
# accessory data.
HOMEBRIDGE_OPTS=-U /var/lib/homebridge

# Uncomment the DEBUG line if homebridge shoule write less logs. Useful at the beginning when getting
# started to have more info if something goes wrong. View logs using: sudo journalctl -f -u homebridge
DEBUG=*
```

Create `homebridge` Service file.
```
sudo vim /etc/systemd/system/homebridge.service
```

Paste these lines:
```
[Unit]
Description=Node.js HomeKit Server 
After=syslog.target network-online.target

[Service]
Type=simple
User=homebridge
EnvironmentFile=/etc/default/homebridge
ExecStart=/usr/bin/homebridge $HOMEBRIDGE_OPTS
Restart=on-failure
RestartSec=10
KillMode=process

[Install]
WantedBy=multi-user.target
```

## Create the Config File

### New Config File
```
sudo vim /var/lib/homebridge/config.json
```

Paste these lines (change properties to any other value):
```
{
    "bridge": {
        "name": "Homebridge",
        "username": "24:40:CE:FF:C1:57",
        "port": 51826,
        "pin": "110-36-153"
    },
    
    "description": "This is an example configuration file with one fake accessory and one fake platform. You can use this as a template for creating your own configuration file containing devices you actually own.",
}
```

### Existing Config File
If there is an existing config file that was set up previously in the default location, copy that to the Homebridge directory.
```
sudo cp ~/.homebridge/config.json /var/lib/homebridge/config.json
```

### Add Plug-in Configuration

Going with the [GPIO Plug-in](https://www.npmjs.com/package/homebridge-gpioswitch) from above, the below plug-in example is to configure that module. Consult your Plug-in documentation for any other installed Homebridge Plug-in.

Paste the following inside the outermost brackets, after the `"description"` property:
```
    "accessories": [
        {
            "accessory": "GPIOSWITCH",
            "name": "Raspberry Switch",
            "pin": 7
        }
    ]
```

### Change Config File Ownership
```
sudo chown homebridge /var/lib/homebridge/config.json
```




