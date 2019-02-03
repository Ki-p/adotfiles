
I don't intend for anyone else to use this, this is just for me to easily replicate my setup.

## Dependencies
**Install everything needed**

---
su
apt install sudo xorg i3 git wget pulseaudio rxvt-unicode-256color tmux cmus feh suckless-tools firefox-esr thunar catfish python3-pip mpv vim-gtk3 idle3 mupdf abiword gimp lxappearance system-config-printer network-manager 

_Add USER in the sudo group_
adduser USER sudo
reboot

_Add contrib non-free and backports in sources.list_
sudo sed -i s/main/main contrib non-free\g /etc/apt/sources.list
sudo echo "deb http://ftp.debian.org/debian stretch-backports main" >> /etc/apt/sources.list

_Install firmware_
sudo apt install firmware-linux iwlwifi firmware-realtek

_Add i3 in xinitrc then start i3_
sudo cp /etc/X11/xinit/xinitrc ~/.xinitrc
echo "exec i3" >> .xinitrc
startx

_Install packages needed for making backports_
sudo apt install packaging-dev debian-keyring devscripts equivs 
sudo apt -t stretch-backports install "debhelper"

_Install brightnessctl_
mkdir Backports
cd Backports
mkdir brightnessctl
sudo dget -x http://deb.debian.org/debian/pool/main/b/brightnessctl/brightnessctl_0.3.2-1.dsc
cd brightnessctl_0.3.2
sudo mk-build-deps --install --remove
dch --local ~bpo9+ --distribution stretch-backports "Rebuild for stretch-backports."
fakeroot debian/rules binary
dpkg-buildpackage -us -uc
cd ..
sudo dpkg -i brightnessctl_0.3.2-1~bpo9+1_amd64.deb

_Install pulsemixer_
cd ..
mkdir pulsemixer
sudo dget -x http://deb.debian.org/debian/pool/main/p/pulsemixer/pulsemixer_1.4.0-1.dsc
cd pulsemixer_1.4.0
sudo mk-build-deps --install --remove
dch --local ~bpo9+ --distribution stretch-backports "Rebuild for stretch-backports."
fakeroot debian/rules binary
dpkg-buildpackage -us -uc
cd ..
sudo dpkg -i pulsemixer_1.4.0-1~bpo9+1_amd64,deb

_Install newsboat_
cd ..
mkdir newsboat 
cd newsboat
sudo dget -x http://deb.debian.org/debian/pool/main/n/newsboat/newsboat_2.13-1.dsc
cd newsboat_2.13
sudo mk-build-deps --install --remove
dch --local ~bpo9+ --distribution stretch-backports "Rebuild for stretch-backports."
fakeroot debian/rules binary
dpkg-buildpackage -us -uc
cd .. 
sudo dpkg -i newsboat_2.13-1~bpo9+1_amd64.deb

_Install backported youtube-dl_
sudo apt -t stretch-backports install "youtube-dl"

_No password needed for brightnessctl shutdown and reboot_
sudo touch /etc/sudoers.d/sudoers
sudo echo "%eng ALL= NOPASSWD : /sbin/shutdown,/sbin/reboot,/usr/bin/brightnessctl" /etc/sudoers.d/sudoers

_Install checkinstall_
sudo apt install checkinstall

_Install cava(a music visualizer)_
mkdir Git
git clone https://github.com/karlstav/cava.git
cd cava
sudo apt-get install libfftw3-dev libasound2-dev libncursesw5-dev libpulse-dev libtool
./autogen.sh
./configure
make
sudo checkinstall

_Get platinum9 icons_
cd
mkdir .icons
cd Git 
git clone https://github.com/grassmunk/Platinum9.git
cd Platinum9
cp -r NineIcons ~/.fonts
set in lxappearance

_Get tamsyn font_
wget http://www.fial.com/~scott/tamsyn-font/download/tamsyn-font-1.11.tar.gz
mkdir .fonts
tar -xvzf tamsyn-font-1.11.tar.gz
cp -r tamsyn-font-1.11/ ~/.fonts
remove the conf file that prevents use of pcf fonts

_Install i3-gaps_
cd Git 
git clone https://www.github.com/Airblader/i3 i3-gaps
cd i3-gaps
sudo apt install gcc make dh-autoreconf libxcb-keysyms1-dev libpango1.0-dev libxcb-util0-dev xcb libxcb1-dev libxcb-icccm4-dev libyajl-dev libev-dev libxcb-xkb-dev libxcb-cursor-dev libxkbcommon-dev libxcb-xinerama0-dev libxkbcommon-x11-dev libstartup-notification0-dev libxcb-randr0-dev libxcb-xrm0 libxcb-xrm-dev libxcb-shape0-dev

autoreconf --force --install
rm -rf build/
mkdir -p build && cd build/

sudo make
sudo checkinstall

sudo reboot

cd Git

git clone https://github.com/Ki-p/adotfiles.git
cd adotfiles
cp -r config/ .Xresources .bash_aliases .bashrc ~/      

sudo reboot
startx 

## **Done**              
