# Setting up Pi in Kiosk mode
===

## Create the script to run the webserver on startup
in the root folder (home/pi), create a file called 'startup' withough dots or extensions.

```bash 
#! /bin/sh
cd ~/dashboard/ibm_mqtt_server_status_dashboard/build && python -m SimpleHTTPServer 8000 &
```

modify permission on 'startup'

`chmod u+x startup`

Then put this line at the bottom of the .bashrc file:

`./startup`

## Configure Chromium to open in fullscreen and disable sleep/screensaver
[link](https://raspberrypi.stackexchange.com/questions/40631/setting-up-a-kiosk-with-chromium)

copy the config file to your local directory:
`cp /etc/xdg/lxsession/LXDE-pi/autostart /home/pi/.config/lxsession/LXDE-pi/autostart`

Then edit that file with the following params:
```bash
#@xscreensaver -no-splash  # comment this line out to disable screensaver
@xset s off
@xset -dpms
@xset s noblank
@chromium-browser --incognito --kiosk http://localhost/  # load chromium after boot and point to the localhost webserver in full screen mode
```

Save and reboot
