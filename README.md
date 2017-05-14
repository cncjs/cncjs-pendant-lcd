# cncjs-pendant-lcd
CNCjs Web Kiosk Display on Raspberry Pi 320 X 480 TFT LCD Touch Display

Creates a web kiosk for [CNCjs Tiny Web](https://github.com/cncjs/cncjs-pendant-tinyweb)

Hardware Options:
- HDMI Display w/ Mouse
- HDMI Display w/ Touch
- [Raspberry Pi 3.5inch HDMI LCD w/ Touch](http://www.spotpear.com/index.php/spotpear-raspberry-pi-lcd/raspberry-pi-3-5inch-hdmi-lcd)
- [3.5 Inch 320 X 480 TFT LCD Display Touch Board For Raspberry Pi 2/B+](https://www.banggood.com/3_5-Inch-320-X-480-TFT-LCD-Display-Touch-Board-For-Raspberry-Pi-BB-p-958458.html)
- [Geekcreit® 3.5 Inch 320 X 480 TFT LCD Display Touch Board For Raspberry Pi 3 Model B RPI 2B B+](https://www.banggood.com/3_5-Inch-320-X-480-TFT-LCD-Display-Touch-Board-For-Raspberry-Pi-2-Model-B-RPI-B-p-1023432.html)

# Setup Guide:

## Setup your Screen

## Setup Web Kiosk (WIP - SUBJECT TO CHANGE)
```
# Next you should configure your pi to automatically log in using, Select “3 Boot Options” and then select “B2 Console Autologin”.
sudo raspi-config


# #  This will start the x server without mouse cursor, which is a nice touch as we use it as touch display.
# cat >> /home/pi/.bashrc << EOF
# if ! pgrep "xinit" > /dev/null
# then
#     xinit -- -nocursor 2> /dev/null > /dev/null &
# fi
# EOF
# chmod 644 /home/pi/.bashrc


cat > /home/pi/.bash_profile << EOF
if [ -z "$DISPLAY" ] && [ -n "$XDG_VTNR" ] && [ "$XDG_VTNR" -eq 1 ]; then
  exec startx -- -nocursor
fi
EOF
#chmod 644 /home/pi/.bash_profile


# If you have the normal Jessie image with x you have installed the lxde desktop. There the autostart file would be “/home/pi/.config/lxsession/LXDE-pi/autostart”. If you are using our pi image, the autostart file would be “/home/pi/.config/openbox/autostart” since we use openbox to reduce memory usage. Open the file with your favorite editor and add the following:
touch /home/pi/.config/openbox/autostart
cat > /home/pi/.config/openbox/autostart << EOF
# Uncomment the following 3 commands to have display always on
#xset s off         # don't activate screensaver
#xset -dpms         # disable DPMS (Energy Star) features.
#xset s noblank     # don't blank the video device

chromium-browser --noerrdialogs --disable-suggestions-service --disable-translate --disable-save-password-bubble --disable-session-crashed-bubble --disable-infobars --touch-events=enabled --disable-gesture-typing --kiosk http://xcarve/tinyweb/#/

EOF

# Copy
#cp "/home/pi/.config/openbox/autostart" "/home/pi/.config/lxsession/LXDE-pi/autostart"


# cat > /home/pi/.xinitrc << EOF
# #xset s off         # don't activate screensaver
# #xset -dpms         # disable DPMS (Energy Star) features.
# #xset s noblank     # don't blank the video device
# exec openbox-session
# startx
# EOF
#
# chmod 755 /home/pi/.xinitrc


# https://jackbarber.co.uk/blog/2017-02-16-hide-raspberry-pi-mouse-cursor-in-raspbian-kiosk
# Install Unclutter
sudo apt-get install -y unclutter

# Edit LXDE Autostart Script, Turn the Cursor Off
nano ~/.config/lxsession/LXDE-pi/autostart
# add this line to the autostart script:
#@unclutter -idle 0

# Reboot Raspberry Pi
sudo reboot
```
