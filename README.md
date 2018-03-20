# Linux on Chrome Book Pixel (Link)

I've the luck to catch a Chrome Book Pixel (Link / 2013).
The goal is to setup a dev machine with Linux.

**Work in Progress**

## Preparation

* Remove ChromeOS
  * https://mrchromebox.tech/#devmode
  * https://chrx.org/
* BIOS
  * Remove BIOS HW Screw
  * Replace BIOS
    * https://mrchromebox.tech/#fwscript
    * Install/Update Custom coreboot Firmware (Full ROM)
* Expand Hard Drive Capacity
  * Install Mini PCIe Memory Card Adapter
    * https://www.amazon.de/gp/product/B01AOVJ456/ref=oh_aui_detailpage_o04_s00?ie=UTF8&psc=1
    * Insert 2 SD Cards
      * Card 1 /home
      * Card 2 Documents

## Install

### GalliumOS

https://galliumos.org/releases/nightly/galliumos-braswell-xenon-20171227T072217Z.iso
 
### libinput

sudo apt install xserver-xorg-input-libinput
sudo mkdir /etc/X11/xorg.conf.d/
sudo nano /etc/X11/xorg.conf.d/30-touchpad.conf

Section "InputClass"
  Identifier "MyTouchpad"
  MatchIsTouchpad "on"
  Driver "libinput"
  Option "Tapping" "on"
EndSection

tmp fix:
https://github.com/raphael/linux-samus/issues/195
xinput set-prop 11 277 1

### libinput-gestures

* install
* configure

## Tweaks

### Fonts

* Settings
* Appearance
* Custom DPI Settings: 148

### Capitaine Cursors

git clone https://github.com/keeferrourke/capitaine-cursors.git
* Settings
* Mouse & Touchpad
* Theme
* Capitaine Cursors
* Cursor Size: 38
