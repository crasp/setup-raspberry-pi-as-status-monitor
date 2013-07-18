setup-raspberry-pi-as-status-monitor
====================================

Minimal installation guide to setup a Raspberry Pi as status monitor (buildserver, service-status etc.)

## Preparation
- [Format](https://www.sdcard.org/downloads/formatter_4/) an SD-Card
- install [Raspbian](http://www.raspberrypi.org/downloads) on the SD-Card 
- on boot, the config-tool will open
  - extend filesystem to full capacity
  - enable boot into GUI
  - advanced > enable SSH

## install packages
    sudo apt-get install chromium htop netatalk avahi-daemon x11vnc
    
## enable screen-sharing via [x11vnc](http://www.karlrunge.com/x11vnc/)
- Set VNC password:  
  `x11vnc -storepasswd`
    
- Autostart VNC:  
  `mkdir -p /home/pi/.config/autostart`  
  `vim /home/pi/.config/autostart/x11vnc.desktop`
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
