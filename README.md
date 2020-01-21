## FIRST THING FIRST

This installation list is based on my preferences while trying out Debian 10 (Buster) on my late 2008 macbook and is not a definitive procedure. Feel free to use at your own peril.

Settings and Partitions

sda1  fat32   /boot/efi   142MiB    boot,esp
sda3  ext4    /           141.28GiB
sda2  linuxswap           7.63GiB (7812MiB)   swap

---

<br />

## WHILE IT'S STILL FRESH

<br />

- [x] **Update OS**
```
sudo apt-get update
sudo apt-get upgrade
```

<br />

- [x] **Install Broadcom Wireless**
* Add a "contrib" component to your existing repository line in `/etc/apt/sources.list` with nano or other editor. For this we'll first make a copy of the original file, then edit the new file. For example:
```
sudo mv /etc/apt/sources.list /etc/apt/sources.list.OLD
sudo nano /etc/apt/sources.list.OLD
```
```
# See https://wiki.debian.org/SourcesList for more information.
deb http://deb.debian.org/debian buster main contrib non-free
deb-src http://deb.debian.org/debian buster main contrib non-free

deb http://deb.debian.org/debian buster-updates main contrib non-free
deb-src http://deb.debian.org/debian buster-updates main contrib non-free

deb http://security.debian.org/debian-security/ buster/updates main
deb-src http://security.debian.org/debian-security/ buster/updates main
```
* *^X* to **exit**, then **save** the file as `/etc/apt/sources.list`
(Note: do not add a new line. Just add " contrib non-free" to the end of your existing line.)
* Update the list of available packages:
```
sudo apt update
```
* Install the appropriate firmware installer package:
For devices with a BCM4306 revision 3, BCM4311, BCM4318, BCM4321 or BCM4322 chip, install firmware-b43-installer:
```
sudo apt install firmware-b43-installer -y
```
* Reboot.

More information can be found in [Debian Wiki](https://wiki.debian.org/bcm43xx) apple wifi webpage.

<br />

- [x] **Enable Tapping on Touchpad**
* Create and edit the config file `/etc/X11/xorg.conf.d/40-libinput.conf` to enable tapping.
```
sudo nano /etc/X11/xorg.conf.d/40-libinput.conf
```
```
'Section "InputClass"
     Identifier "libinput touchpad catchall"
     MatchIsTouchpad "on"
     MatchDevicePath "/dev/input/event*"
     Driver "libinput"
     Option "Tapping" "on"
EndSection'
```
* Restart the DM:
```
systemctl restart lightdm
```

More information can be found in [Debian Wiki](https://wiki.debian.org/SynapticsTouchpad#Enable_tapping_on_touchpad) synaptics touchpad webpage.

<br />

- [x] **Sound (For MacBook Aluminum - late 2008)**
```
sudo alsamixer
```
* Set all up Master, PCM, Line-Out and switch from 2ch to 6ch.
* Search for the **Volume** plug in (PulseAudio Plugin) and add it to the *Panel*, and make sure the **Enable keyboard shortcuts for volume control** is `ON`.

---

<br />

## COSMETICS

<br />

- [x] **Install Themes and Icons**
* Open *Synaptic Package Manager* and search for **-theme**, **-icon** and **greeter**
* Select and install:
```
arc-theme
papirus-icon-theme
lightdm-gtk-greeter-settings
```
* Open the *Settings Manager*
* In *Appearance -> Style* select the to **Arc-Dark**
* In *Appearance -> Icons* select the to **Papirus-Dark**
* In *Window Manager -> Style* select the to **Arc-Dark**
* Also change the **Style, Icons, etc.** in the *LightDM GTL+ Greeter settings*

<br />

- [x] **Advanced Themes and Icons**
* Most themes and icons can be downloaded from [Xfce-look.org](https://Xfce-look.org); Flat-Remix is a good choice.
* The basic details of the procedure are:
1. Download the archive.
2. Extract it with the right click of your mouse.
3. Create the **.icons** and **.themes** folders in your home directory. The fastest way to do that is by running in the terminal: `mkdir ~/.icons ~/.themes`.
4. Move the extracted theme folders to the ~/.theme folder and the extracted icons to the ~/.icons folder. You can make the .theme and .icons folders visible by pressing Ctrl+H or in the menu of your file manager *View -> Show Hidden Files*.
* More detailed information about Themes and Icons can be found [here](https://averagelinuxuser.com/xfce-look-modern-and-beautiful/).

<br />

- [x] **Configure Greeter**
* Create user configuration file for **lightDM**, the greeter that asks for user and credentials, and edit it:
```
sudo nano /usr/share/lightdm/lightdm.conf.d/01_my.conf
```
* Add the following lines to the file and save:
```
[SeatDefaults]
greeter-hide-users=false
```
* After rebooting, the user name will be prompted.

<br />

### KEYBOARD SHORTCUTS

<br />

Go to *Settings Manager -> Keyboard -> Application Shortcuts*.

- [x] **Terminal**
* `xfce4-terminal` runs with *Ctrl+Alt+T*

- [x] **Dropdown Terminal**
* `xfce4-terminal --drop-down` runs with *Super+T*

- [x] **Kill Frozen Applications**
* `xkill` runs with *Ctrl+Alt+Esc*

---

<br />

## TERMINAL STUFF --- edit from here!

<br />

- [x] **Enable PPA (Properties Commons) & Ubuntu extras**
```
sudo apt install software-properties-common
sudo apt install ubuntu-restricted-extras
```

<br />

- [x] **elementaryOS Tweaks**
```
sudo add-apt-repository ppa:philip.scott/elementary-tweaks
sudo apt-get update
sudo apt-get install elementary-tweaks
```

- [x] **Go to** `System Settings > Tweaks`
* In `Appearance/Window Controls/Layout` change to **Minimize Left**
* Turn **OFF** the setting `Files/Single click`
* Turn **OFF** the setting `Terminal/Natural copy paste`
* Turn **OFF** the setting `Terminal/Remember tabs`

<br />

- [x] **Installed TLP package to Improve Battery Life**
```
sudo apt install tlp tlp-rdw
```

<br />

### Installing Apps

- [x] **GParted** 
```
sudo apt-get install gparted
```

- [x] **GIT**
```
sudo apt-get install git
```

- [x] **VLC**
```
sudo apt install vlc
```

- [x] ~~**Google Chromium**~~
```
sudo apt install chromium-browser
```

- [x] **Firefox**
```
sudo apt install firefox
```

---

<br />

## OTHER APPS

<br />

- [x] **Atom** [atom.io](https://atom.io)
* To install Atom on Debian, Ubuntu, or related distributions, add our official package repository to your system by running the following commands:
```
wget -qO - https://packagecloud.io/AtomEditor/atom/gpgkey | sudo apt-key add -
sudo sh -c 'echo "deb [arch=amd64] https://packagecloud.io/AtomEditor/atom/any/ any main" > /etc/apt/sources.list.d/atom.list'
sudo apt-get update
```
* You can now install Atom using apt-get (or apt on Ubuntu):
```
# Install Atom
sudo apt-get install atom
# Install Atom Beta
sudo apt-get install atom-beta
```
* Alternatively, you can download the Atom .deb package and install it directly:
```
# Install Atom
sudo dpkg -i atom-amd64.deb
# Install Atom's dependencies if they are missing
sudo apt-get -f install
```

---

<br />

## FINAL APP CLEANING

<br />

- [x] ~~**Uninstall Apps**~~
```
sudo apt purge epiphany-browser epiphany-browser-data
sudo apt purge pantheon-mail
sudo apt purge noise
sudo apt purge audience
```

<br />

- [x] ~~**Clean Up OS**~~
```
sudo apt autoremove -y
sudo apt autoclean -y
```

<br />
