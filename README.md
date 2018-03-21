# Linux on Chrome Book Pixel (Link)

My Linux setup on a **Chrome Book Pixel** (Link / 2013) with a lightweight WM (XFCE4).

NOTE: Don't mind me if you brick your machine :)

*Work in Progress*

<!-- TOC -->

- [Linux on Chrome Book Pixel (Link)](#linux-on-chrome-book-pixel-link)
  - [Preparation](#preparation)
    - [Remove ChromeOS](#remove-chromeos)
    - [BIOS](#bios)
    - [Expand Hard Drive Capacity](#expand-hard-drive-capacity)
  - [Install](#install)
    - [Antergos](#antergos)
      - [libinput-gestures](#libinput-gestures)
    - [Tweaks](#tweaks)
      - [Theme](#theme)
      - [Fonts](#fonts)
      - [Capitaine Cursors](#capitaine-cursors)
      - [Power Management](#power-management)
    - [GalliumOS](#galliumos)
      - [libinput](#libinput)
      - [Keyboard](#keyboard)
      - [Disable bluetooth](#disable-bluetooth)
      - [WIFI / WLAN](#wifi-wlan)
  - [WIP](#wip)
    - [lightdm](#lightdm)
    - [suspend](#suspend)

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

### Expand Hard Drive Capacity

* Replace "LTE Slot Dummy" with "Mini PCIe Memory Card Adapter"
  * https://www.amazon.de/gp/product/B01AOVJ456/ref=oh_aui_detailpage_o04_s00?ie=UTF8&psc=1
* Expand with 2 SD Cards
    * Card 1 (Used for /home)
    * Card 2 (Used for develpment storage)

## Install

### Antergos

https://antergos.com/

#### libinput-gestures

* Install "libinput-gestures"
  * libinput-gestures-setup 
* Configure
  * conf/libinput-gestures.conf

### Tweaks

#### Theme

Style
* Arc-GalliumOS
Icons
* Numix Circle GalliumOS
Windows Manager Style
* Arc-GalliumOS

#### Fonts

* Settings
* Appearance
* Roboto 10
* Custom DPI Settings: 162

#### Capitaine Cursors

Install https://github.com/keeferrourke/capitaine-cursors

* Settings
* Mouse & Touchpad
* Theme
* Capitaine Cursors
* Cursor Size: 38
 (a bit large, but... )

#### Power Management

Install TLP tools:

```
sudo pacman -S tlp tlp-rdw
```

Enable TLP tools:

```
sudo systemctl enable tlp
sudo systemctl enable tlp-sleep
```


### GalliumOS

My first try, but I've problems with WIFI:
https://galliumos.org/releases/nightly/galliumos-braswell-xenon-20171227T072217Z.iso

Other Issues

#### libinput

```
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

#### Keyboard

Keyboard, Layout, Chromebook (most models), No overlay

#### Disable bluetooth

```
sudo apt-get install dconf
dconf-editor
navigate to org.blueman.plugins.powermanager
set "auto-power-on" to "false"
```

#### WIFI / WLAN

GalliumOS lost connectivity :(

https://github.com/GalliumOS/galliumos-distro/issues/253


## WIP

### lightdm

* xdp // font

### suspend

needed?

* tpm_tis force=1 interrupts=0
