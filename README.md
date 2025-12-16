OK guys, I'm moving to 100% satisfying solution with [libimobiledevice](https://libimobiledevice.org/), which is packaged in most Linux distributions, and [Jackson Coxson's netmuxd](https://github.com/jkcoxson/netmuxd). Netmuxd complements usbmuxd with network capabilities and both must run together.

Here are the steps to prepare my Fedora 42 and **successfully backup my iPhone over the network**. Nothing has to be done as root, **all scripts and commands executed by my regular user**.

## Prepare things
This script will discover [latest version of netmuxd](https://github.com/jkcoxson/netmuxd/releases/tag/v0.3.0) for my platform and download it
```shell
prepare-script
```

## First pair device with a USB cable
```shell
idevicepair pair
```
This creates some required signatures in `/var/lib/lockdown/{device_UDID}.plist`

## Make backup
This method assumes usbmuxd is also running in your system, which is the regular setup for most Linux distros. [Other methods are documented in netmuxd home page](https://github.com/jkcoxson/netmuxd).

This script runs netmuxd, waits a bit until it finds my device in the network and then use `idevicebackup2` to make the backup. A folder named with my device's UDID will be created under `$folder` (if it doesn't exists).

Since backup is mostly controlled by the device, this method also works for incremental backups even if the backup folder already has a backup created by Apple official software.

Your device will ask for your pin and then start backup.
```shell
backup-script
```
You might need to change these scripts a bit if you have multiple devices to backup.

iOS backup over the network takes a long time, like 20 minutes for an incremental backup, but is pretty solid and stable and I can use my device while roaming through multiple WiFi APs at home.

