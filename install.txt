ARCH LINUX INSTALLATION (Gnome GUI)

wifi-menu (if WIFI connection) / dhcpcd (if wired with dhcp router)

// Static IP
ifconfig
ip link set <eth0> up
ip addr add <192.168.1.10/24> dev <eth0>
ip route add default via <192.168.1.1>

ping -c 3 google.pl

pacman -Syy

cfdisk /dev/sda

mkfs.ext4 /dev/sda5
mkfs.ext4 /dev/sda6
mkswap /dev/sda7
swapon /dev/sda7

mount /dev/sda5 /mnt
mkdir /mnt/home
mount /dev/sda6 /mnt/home

pacstrap /mnt base base-devel

genfstab -U -p /mnt >> /mnt/etc/fstab

arch-chroot /mnt
passwd (set root password)

nano /etc/locale.gen
locale-gen
locale > /etc/locale.conf

ls -a /usr/share/zoneinfo
ln -s /usr/share/zoneinfo/Europe/Warsaw /etc/localtime
date (check if correct)

hwclock --systohc --utc 
// hwclock --systohc --localtime (if system is shared with windows on one pc)

echo myhostname > /etc/hostname
nano /etc/hosts (change localhost into myhostname @ 127.0.0.1 addr)

mkinitcpio -p linux

pacman -Syy
pacman -S dialog iw wpa_supplicant bash-completion
pacman -S grub os-prober freetype2

os-prober
grub-mkconfig -o /boot/grub/grub.cfg
grub-install --recheck /dev/sda
nano /boot/grub/grub.cfg (remove quiet from 10-linux image)

useradd -m -g users -G adm,lp,wheel,power,audio,video,disk,network,optical,storage,rfkill -s /bin/bash username
passwd username
nano /etc/sudoers

exit
umount -R /mnt
reboot

----------------------------------------------------
Post-reboot login to username
----------------------------------------------------

sudo wifi-menu
sudo dhcpcd
ping -c 3 google.pl

sudo systemctl enable dhcpcd (if wired connection is used, do not do this while using wifi)

export EDITOR=nano

sudo nano /etc/pacman.conf
// uncomment [multilib]
// add at the end:
// [archlinuxfr]
// SigLevel = Never
// Server = http://repo.archlinux.fr/$arch

sudo pacman -Syu
sudo pacman -S yaourt rsync
sudo pacman -S alsa-utils python2

sudo reboot

alsamixer
speaker-test -c2

sudo pacman -S xorg-server xorg-server-utils xorg-xinit xorg-twm xorg-xclock xterm

//only for virtualbox
sudo pacman -S virtualbox-guest-utils

sudo pacman -S xf86-video-(ati nouveau intel) xf86-video-vesa xf86-input-synaptics mesa mesa-demos
sudo pacman -S inxi dmidecode hddtemp lm_sensors 

startx
exit

sudo pacman -S gnome gnome-tweak-tool networkmanager network-manager-applet firefox archey3 imagemagick

//optional for tons of useless apps
sudo pacman -S gnome-extra

sudo systemctl enable NetworkManager
sudo systemctl enable gdm

sudo reboot

