setup-raspberry-pi-as-status-monitor
====================================

Minimal installation guide to setup a Raspberry Pi as status monitor (buildserver, service-status etc.)

## Preparation and Setup

### Install [Raspbian](http://www.raspberrypi.org/downloads)
- [Format](https://www.sdcard.org/downloads/formatter_4/) an SD-Card
- Install [Raspbian](http://www.raspberrypi.org/downloads) on the SD-Card
  - Before writing the image file to the SD card, the volume needs to be unmounted.

    `diskutil umount /dev/diskXs1` (Replace X with the correct disk node number)

  -  We can write the image from the downloaded ZIP file by unzipping it on the fly and writing it directly onto the SD card. The `bs` (blocksize) parameter ensures dd reads and writes in those size blocks (here: 8 MiB).

    `unzip -p debian-wheezy.zip|dd bs=8388608 of=/dev/diskX` (Replace X with the correct disk node number)

  - To write the disk image normally, unzip it and  
    `dd bs=8388608 if=debian-wheezy.img of=/dev/diskX` (Replace X with the correct disk node number)


### Boot
When you boot for the first time the config-tool will open
  - Extend filesystem to full capacity
  - Enable boot into GUI
  - Advanced > enable SSH

## Install packages
    sudo apt-get install chromium htop netatalk avahi-daemon x11vnc
    
## Enable screen-sharing via [x11vnc](http://www.karlrunge.com/x11vnc/)
- Set VNC password:  
  `x11vnc -storepasswd`
    
- Autostart VNC:  
  `mkdir -p /home/pi/.config/autostart`  
  `nano /home/pi/.config/autostart/x11vnc.desktop`
  ```
  [Desktop Entry]  
  Encoding=UTF-8  
  Type=Application  
  Name=X11VNC  
  Exec=x11vnc -forever -usepw -display :0 -ultrafilexfer  
  StartupNotify=false  
  Terminal=false  
  Hidden=false  
  ```

- Setup remote framebuffer [rfb](http://en.wikipedia.org/wiki/RFB_protocol)  
  `sudo nano /etc/avahi/services/rfb.service`  
  ```xml
  <?xml version="1.0" standalone='no'?>
  <!DOCTYPE service-group SYSTEM "avahi-service.dtd">
  <service-group>
    <name replace-wildcards="yes">%h</name>
    <service>
    <type>_rfb._tcp</type>
      <port>5901</port>
    </service>
  </service-group>
  ```

- Disable screen-saver  
  `nano /home/pi/.xsessionrc`  
  ```
  # turn off default screensaver
  xset s off
  # turn off default standby, hibernate, ... after n minutes
  xset -dpms
  # don't blank the video device
  xset s noblank
  logger "Screen disabled."
  ```

## Announce in local network via [avahi](http://en.wikipedia.org/wiki/Avahi_(software))
  `sudo update-rc.d avahi-daemon defaults`  
