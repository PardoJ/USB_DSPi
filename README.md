# USB_DSPi
A raspberry DSP and audiophile player dedicated to usb external sound cards.

Based on:
  - Brutefir
  - mpd
  - alsa
  - raspbian
  
## Raspbian installation
DL
card write
SSH file

## RPi connection
IP
SSH connection
 
## System preparation
### Update
sudo apt-get clean all
sudo apt-get update
sudo apt-get upgrade
sudo apt-get upgrade raspberrypi-sys-mods
sudo apt-get dist-upgrade 
### Configuration
rpi-update
raspi-config
   expand FS
	 video RAM 16
	 language
	 user password
 sudo reboot
 
## Services installation
sudo apt-get install mpd brutefir

mkdir /home/pi/music
sox fir/left.wav -b 32 -e signed-integer -c 1 -r 44100 -t raw /home/shairport-sync/left.raw

sudo nano /etc/mpd.conf 
sudo nano /etc/brutefir_config
sudo nano /etc/asound.conf
sudo nano /etc/modules-load.d/snd-aloop.conf
sudo nano /etc/modprobe.d/blacklist-snd_2835.conf
sudo nano /etc/modprobe.d/snd-aloop.conf

sudo nano /lib/systemd/system/brutefir.service
systemctl daemon-reload

systemctl enable mpd
sudo service mpd restart
systemctl enable brutefir.service
service brutefir start

## System tweaks
for i in `pgrep ksoftirqd`; do chrt -p 99 $i; done
cp /etc/rc.local /etc/rc.local.old
echo "Patching /etc/rc.local - original file copied to /etc/rc.local.old"
sed -i -e "s/exit 0/for irqdps in \`pgrep ksoftirqd\`; do chrt -p 99 \$irqdps; done\n/" /etc/rc.local
echo -e "echo \"performance\" > /sys/devices/system/cpu/cpu0/cpufreq/scaling_governor\n" >> /etc/rc.local
echo -e "\n\nexit 0" >> /etc/rc.local
sysctl -w vm.swappiness=1
cp /etc/sysctl.conf /etc/sysctl.conf.old
echo "Patching /etc/sysctl.conf - original file copied to /etc/sysctl.conf.old"
echo -e "\nvm.swappiness=1" >> /etc/sysctl.conf

## Alsa tweaks

## Real time kernel patch

# After
Configuration player multimedia
speaker measurements and correction with rephase and Room EQ wizard

