# kickstart_rockylinux
How to install Rocklinux with new ISO custom image

```
mkdir /mnt/rockyiso
sudo mount -o loop Rocky-9.3-x86_64-dvd.iso /mnt/rockyiso
```

```
mkdir ~/rockyiso_custom
rsync -av /mnt/rockyiso/ ~/rockyiso_custom/
```

```
vi ~/rockyiso_custom/ks.cfg
```

```
# Sample Kickstart configuration
clearpart --drives=sda --initlabel --disklabel=msdos
autopart --type=lvm --nohome
network --bootproto=static --ip=192.153.102.13 --netmask=255.255.255.0 --gateway=192.153.102.1 --nameserver=192.0.0.1 --device=ens192 --onboot=on --hostname=watsonxdata
rootpw --plaintext IBM4ever
user --name=watsonx --password IBM4ever --plaintext --gecos="watsonx" --groups=wheel
timezone Europe/Berlin
keyboard --xlayouts='de'
lang en_US.UTF-8
firewall --enabled

%packages
@^minimal-environment
@gnome-desktop
kexec-tools
%end

%post
dnf update -y
%end
```

```
vi ~/rockyiso_custom/isolinux/isolinux.cfg
```
-> add: ks=cdrom:/ks.cfg quiet
```
vi ~/rockyiso_custom/EFI/BOOT/grub.cfg
```
-> add: inst.ks=cdrom:/ks.cfg
```
cd ~/rockyiso_custom
sudo genisoimage -U -r -v -T -J -joliet-long -V "Rocky-9-3-x86_64-dvd" -volset "Rocky-9-3-x86_64-dvd" -A "Rocky-9-3-x86_64-dvd" -b isolinux/isolinux.bin -c isolinux/boot.cat -no-emul-boot -boot-load-size 4 -boot-info-table -eltorito-alt-boot -e images/efiboot.img -no-emul-boot -o ~/Rocky_Custom.iso .
```

### Important Label: Rocky-9-3-x86_64-dvd must match in group
