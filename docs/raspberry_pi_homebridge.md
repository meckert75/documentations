# Hombebridge Setup

Install HombeBridge and add control it through Apple HomeKit from your iPhone, iPad or Siri.
These instructions combine the [HomeBridge Installation](https://github.com/nfarina/homebridge)
instructions and the [Running Homebridge on a Raspberry Pi](https://github.com/nfarina/homebridge/wiki/Running-HomeBridge-on-a-Raspberry-Pi)
instructions to create a simplified single document that will get HomeBridge set up on Raspbian 8 (Jessie). Need to verify
all works with the latest version of Raspbian.

Read the full instructions mentioned above when working with a 'non-standard' OS and when running into issues as there
might be additional steps to get everything running.

**Prerequisit**

Install Raspbian on a Micro SD card for the Raspberry Pi:
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

## Install Node.js and other Dependencies
```
# Install Node.js
curl -sL https://deb.nodesource.com/setup_8.x | sudo -E bash -
sudo apt-get install -y nodejs
# Other dependencies
sudo apt-get install libavahi-compat-libdnssd-dev
```
