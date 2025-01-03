# Obscreen README
### Installing the "Studio" Server
Reference: https://docs.obscreen.io/install/studio-server/docker.html

#### Managing the Obscreen "Studio" server
URL: http://grover:5000/playlist/list/0

#### Playing an Obscreen playlist
For a playlist called "reference" 
URL: http://grover:5000/use/reference

#Autostarting Chromium full screen
Raspian Bookworm will use Wayland as your backend. You will want to switch to X11 or Wayfire to more easily setup autostart features.

I used Wayfire so I'll describe that here.

Use raspi-config to set Wayfire as your default.

From the terminal run

```$ sudo raspi-config```

Go to 6 Advanced Options

Go to A6 Wayland and switch to the Wayfire window manager with Wayland backend.

Finish raspi-config and reboot.

Alt + F4 will get you out of full screen mode. Note: You may need to hold the Function key as well.

### Create a wayfire.ini to setup autostart.

I created my wayfire.ini file from the terminal using the vim text editor. Raspian doesn't come with vim built-in so I had to go get it.

```sudo apt-get install neovim```

OK. Now that you have vim, run 

```vim ~/.config/wayfire.ini```

```
[autostart]
chromium = sleep 10;chromium-browser --kiosk --noerrdialogs --disable-infobars --ozone-platform=wayland --enable-features=OverlayScrollbar --start-maximized --disk-cache-size=2147483648 --ignore-certificate-errors --disable-web-security --disable-restore-session-state --autoplay-policy=no-user-gesture-required --allow-running-insecure-content --remember-cert-error-decisions --noerrdialogs --single-process
screensaver = false
dpms = false
```






