# ubuntu-cloudimg-desktop
Ubuntu-desktop cloud images

Download xenial-server-cloudimg-amd64-desktop.tar.bz2
http://disk.crplab.ru/index.php/s/MGPfGxpbGtMi2C2/download

It has preinstalled mate desktop and xrdp
After VM creation login via ssh and set password for ubuntu user in order to connect via rdp


Created from official ubuntu 16.04 cloud image with the following commands:

```bash
wget https://cloud-images.ubuntu.com/xenial/current/xenial-server-cloudimg-amd64-disk1.img
qemu-img resize xenial-server-cloudimg-amd64-disk1.img +4G
cp xenial-server-cloudimg-amd64-disk1.img xenial-server-cloudimg-amd64-desktop.img
qemu-nbd --connect=/dev/nbd0 xenial-server-cloudimg-amd64-desktop.img
mount /dev/nbd0p1 /mnt/
mount /proc/ /mnt/proc --bind
mount /sys /mnt/sys --bind
mount /dev/ /mnt/dev --bind
chroot /mnt
source /etc/environment

apt-get update
apt-add-repository ppa:ubuntu-mate-dev/ppa -y
apt-add-repository ppa:ubuntu-mate-dev/trusty-mate -y

cp /boot/grub /tmp/grub -r
apt-get install xrdp ubuntu-mate-core ubuntu-mate-desktop -y
echo mate-session > /etc/xrdp/startwm.sh
rm /boot/grub
mv /tmp/grub /boot/grub

exit

umount /mnt -l
qemu-nbd --disconnect /dev/nbd0
```
