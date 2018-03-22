# Linux on Chrome Book Pixel (Link)

My Linux setup on a **Chrome Book Pixel** (Link / 2013) with a lightweight WM (XFCE4).

NOTE: Don't mind me if you brick your machine :)

*Work in Progress*

<!-- TOC -->

- [Linux on Chrome Book Pixel (Link)](#linux-on-chrome-book-pixel-link)
  - [Preparation](#preparation)
    - [Remove ChromeOS](#remove-chromeos)
    - [BIOS](#bios)
    - [Expand your Hard Drive Capacity](#expand-your-hard-drive-capacity)
  - [Install](#install)
    - [Antergos](#antergos)
      - [libinput-gestures](#libinput-gestures)
    - [Tweaks](#tweaks)
      - [Theme & Icons](#theme-icons)
      - [Fonts](#fonts)
      - [Cursor](#cursor)
      - [Power Management](#power-management)
      - [Keyboard](#keyboard)
      - [Compositor](#compositor)
      - [lightdm](#lightdm)
    - [GalliumOS](#galliumos)
      - [Issues](#issues)
        - [libinput](#libinput)
        - [Keyboard](#keyboard)
        - [Disable bluetooth](#disable-bluetooth)
        - [WIFI](#wifi)
        - [Font-size at lightdm](#font-size-at-lightdm)
        - [Suspend](#suspend)

<!-- /TOC -->

## Preparation

### Remove ChromeOS

* Set ChromeOS into Dev mode
* Remove existing ChromeOS
* Tutorial: https://mrchromebox.tech/#devmode

### BIOS

* Remove BIOS Protection HW Screw
  * https://www.ifixit.com/Guide/Remove+the+Write+Protect+Screw/86362
* Replace BIOS
  * https://mrchromebox.tech/#fwscript
  * Install/Update Custom coreboot Firmware (Full ROM)

### Expand your Hard Drive Capacity

Adding more storage to the Chromebook Pixel!

* Replace "LTE Slot Dummy" with "Mini PCIe Memory Card Adapter"
  * mSATA doesn't work :(
    * https://www.youtube.com/watch?v=x0_u8bjQFzg
  * But this adapter works :)
    * https://www.amazon.de/gp/product/B01AOVJ456/ref=oh_aui_detailpage_o04_s00?ie=UTF8&psc=1
* Expand with 2 SD Cards
    * Card 1 (Used for /home)
    * Card 2 (Used for develpment storage)

Benchmarks:

```shell
sudo hdparm -Tt /dev/sda
```

* /dev/sda:
  * Timing cached reads:   7618 MB in  2.00 seconds = 3816.76 MB/sec
  * Timing buffered disk reads: 1394 MB in  3.00 seconds = 464.49 MB/sec
* /dev/sdb:
  * Timing cached reads:   8574 MB in  2.00 seconds = 4297.24 MB/sec
  * Timing buffered disk reads:  52 MB in  3.04 seconds =  17.12 MB/sec
* /dev/sdc:
  * Timing cached reads:   8398 MB in  2.00 seconds = 4208.85 MB/sec
  * Timing buffered disk reads:  52 MB in  3.06 seconds =  16.97 MB/sec

Slow but enough storage!

## Install

### Antergos

* Install OS
  * https://antergos.com/
* Boot Manager
  * systemd-boot

#### libinput-gestures

* Install "libinput-gestures"
  * libinput-gestures-setup 
* Configure
  * conf/libinput-gestures.conf

### Tweaks

#### Theme & Icons

* Appearance
  * Style
    * Numix-Frost-Light
  * Icons
    * Numix-Circle-Light
* Windows Manager
  * Style
    * Arc-solid
* qt5ct
  * Style
    * gtk
  * Fonts
    * Roboto Regular 10
  * Icon Theme
    * Numix-Circle-Light

#### Fonts

* Settings
  * Appearance
    * Roboto 10 (Install)
    * Custom DPI Settings: 150

#### Cursor

* Install
  * capitaine-cursors
* Settings
  * Mouse & Touchpad
    * Theme
      * Capitaine Cursors
      * Cursor Size: 38
      * (a bit large, but... )

#### Power Management

Install TLP tools:

```shell
sudo pacman -S tlp tlp-rdw
```

Enable TLP tools:

```shell
sudo systemctl enable tlp
sudo systemctl enable tlp-sleep
```

#### Keyboard

"de" for a German keymap, replace with your flavor:

```shell
localectl set-x11-keymap de chromebook
```

#### Compositor

* Disable XFCE Compositor
  * Settings  
  * Window Manager Tweaks
  * Disable
* Install compton
  * cp conf/compton.conf .config/
* Autostart
  * cp conf/compton.desktop ./config/autostart/

#### lightdm

* Install lightdm-gtk-greeter
* /etc/lightdm/lightdm.conf 
  * greeter-session=lightdm-gtk-greeter
  * greeter-show-manual-login = true
  * greeter-hide-users = true
  * greeter-allow-guest=false
  * allow-guest = false
* lightdm-gtk-greeter settings
    * Appearance
      * Additional font options
        * DPI: 150
        * Font: Roboto Regular
        * Theme / Icons ...


### GalliumOS

My first try, but I've problems with WIFI.

As describted at https://mrchromebox.tech, I tried 2.2:

* https://galliumos.org/releases/nightly/galliumos-braswell-xenon-20171227T072217Z.iso

#### Issues

##### libinput

```shell
sudo apt install xserver-xorg-input-libinput
sudo mkdir /etc/X11/xorg.conf.d/
sudo nano /etc/X11/xorg.conf.d/30-touchpad.conf
```

```
Section "InputClass"
  Identifier "MyTouchpad"
  MatchIsTouchpad "on"
  Driver "libinput"
  Option "Tapping" "on"
EndSection
```

Tmp fix:
* https://github.com/raphael/linux-samus/issues/195

```
xinput set-prop 11 277 1
```

##### Keyboard

Keyboard, Layout, Chromebook (most models), No overlay

##### Disable bluetooth

```shell
sudo apt-get install dconf
dconf-editor
navigate to org.blueman.plugins.powermanager
set "auto-power-on" to "false"
```

##### WIFI

GalliumOS lost connectivity :(

* https://github.com/GalliumOS/galliumos-distro/issues/253

At this moment, no fix.

##### Font-size at lightdm

* dpi setting // font

something like:
```
# /etc/lightdm/lightdm.conf.d/dpi.conf
[SeatDefaults]
xserver-command=X -dpi 162
```

##### Suspend

needed?

* tpm_tis force=1 interrupts=0
