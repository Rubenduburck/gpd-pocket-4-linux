# GPD Pocket 3 Linux
![image](https://user-images.githubusercontent.com/56395503/149221749-fcedf793-c2fb-4516-9c75-bf6161a899d9.png)

## For Kali 2021.4a:
```
# apt-get update
# apt-get dist-upgrade
# apt-get install xinput tlp
```
### Screen Orientation
#### fb
```
└─# nano /etc/default/grub
GRUB_CMDLINE_LINUX_DEFAULT="quiet fbcon=rotate:1 video=eDP:panel_orientation=right_side_up"
GRUB_CMDLINE_LINUX="fbcon=rotate:1"
GRUB_GFXMODE=1600x2560x32
└─# update-grub
```
#### lightdm
```
└─# nano /etc/lightdm/lightdm.conf
[SeatDefaults]
display-setup-script=xrandr -o right
```

#### X11
```
└─# nano /etc/X11/xorg.conf.d/99-touchsreen.conf
Section "InputClass"
  Identifier    "calibration"
  Driver        "wacom"
  MatchProduct  "NVTK0603"
  Option        "TransformationMatrix" "0 1 0 -1 0 1 0 0 1"
EndSection

> 0°   Option "TransformationMatrix" "0 1 0 -1 0 1 0 0 1"
> 90°  Option "TransformationMatrix" "-1 0 1 0 -1 1 0 0 1"
> 180° Option "TransformationMatrix" "0 -1 1 1 0 0 0 0 1"
> 270° Option "TransformationMatrix" "1 0 0 0 1 0 0 0 1"
```

#### Automatic Screen Rotation
```
└─$ wget https://raw.githubusercontent.com/defencore/gpd-pocket-3-linux/main/screen-auto-rotate.c
└─$ gcc -O2 -o screen-auto-rotate screen-auto-rotate.c
└─$ ./screen-auto-rotate
```
#### Screen Rotation With Hotkeys
```
└─$ wget https://raw.githubusercontent.com/defencore/gpd-pocket-3-linux/main/rotate.sh
└─$ chmod a+x rotate.sh
└─$ sudo mv rotate.sh /usr/local/bin/rotate.sh
└─$ xfce4-keyboard-settings
     > Application Shortcuts
       > + Add
         > Command: /usr/local/bin/rotate.sh
           > OK
           > Press: Super+R / Win+R
       > Close
```
### Audio [not working on Kernel 5.15.0]
```

└─$ sudo nano /etc/modprobe.d/alsa.conf
options snd-intel-dspcfg dsp_driver=1

└─$ pulseaudio -k
└─$ systemctl --user restart pulseaudio
└─$ alsactl init
> error
```
### Powersave Tweaks
```
└─# nano /etc/tlp.conf
CPU_SCALING_GOVERNOR_ON_AC=powersave 
CPU_SCALING_GOVERNOR_ON_BAT=powersave
CPU_ENERGY_PERF_POLICY_ON_BAT=power
CPU_BOOST_ON_AC=1 
CPU_BOOST_ON_BAT=0
PLATFORM_PROFILE_ON_AC=performance 
PLATFORM_PROFILE_ON_BAT=low-power
DISK_IOSCHED="mq-deadline"
PCIE_ASPM_ON_AC=default 
PCIE_ASPM_ON_BAT=powersupersave
```

### Capture HDMI input
```
└─$ ffplay /dev/video3
```

## ToDo
* SoundCard
* GRUB Menu Orientation
* Stylus
  * Fix wrong work
  * Buttons Remap
* Multi-Touch Gestures
  * Scroll Up/Down
  * Zoom Increase/Decrease
* Hibernate / Suspend / Hybrid Sleep


## Official
- https://gpd.hk/gpdpocket3firmware
  ```
  2022-01-13 The Pocket 3 glass touchpad upgrade program solves the phenomenon of the mouse jumping randomly when moving the mouse pointer with the touchpad. The Pocket 3 high-end and low-end models are common.
  ```
- https://ubuntu-mate.org/download/gpd_pocket_3/impish/
  ```
  2022-01-21 Configuration changes for the GPD Pocket 3 include:
  1. Enable frame buffer and Xorg display rotation.
  2. Accelerometer support for automatic screen rotation. Also automatically rotates touch screen and stylus (draw and erase)
  3. Enable fractional scaling by default.
  4. Results in an effective resolution of ~1280x800 to make the display panels easily readable.
  5. Simple to toggle on/off via the Display Scaler app if you want to restore full resolution.
  6. Enable audio via the HDaudio legacy driver.
  7. Suspend is implemented via s2idle
  8. A temporary workaround until S3 sleep state is supported via the kernel.
  9. Enable scroll wheel emulation while holding down the centre trackpad button.
  10. Enable Tear-Free rendering by default.
  11. Enable double size console (tty) font resolution.
  12. Sadly, no support for the fingerprint reader.
  ```
