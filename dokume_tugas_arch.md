# cfdisk terlebih dahulu
| Nama Partisi | Kapasitas | Peruntukan (Mount Point) |
| :--- | :--- | :--- |
| `/dev/sda5` | 1.5G | EFI System Partition (`/boot/efi`) |
| `/dev/sda6` | 15G | Root (`/`) |
| `/dev/sda7` | 55.9G | Home (`/home`) |
| `/dev/sda8` | 4G | Swap |

# connect wifi
dengan iwctl
```
iwctl
````
lalu
```
station wlan0 connect-hidden namawifi
```

# format partisi
```
mkfs.ext4 /dev/sda6
```
# membuat swap
```
mkswap /dev/sda8
```
# Format efi partition
```
mkfs.fat -F 32 /dev/efi_system_partition
```
# mount file system
mount root
```
mount /dev/sda6 /mnt
```
mount boot
```
mount --mkdir /dev/sda5 /mnt/boot
```
# Instalasi Sistem Dasar
sesuaikan dengan prosesor laptop
```
pacstrap /mnt intel-ucode base base-devel linux linux-firmware iwd git neovim
```
membuat fstab
```
genfstab -U /mnt >> /mnt/etc/fstab
```
# masuk ke sistem
```
acrh-chroot /mnt
```
# mnegatur timezone
```
ln -sf /usr/share/zoneinfo/Asia/Jakarta /etc/localtime
```
Sinkronkan hardware clock:
```
hwclock --systohc
```

# locale
```
nvim /etc/locale.gen
```
```
lalu uncommenting kedua en_US
```
## generate bahasa yg di uncommenting
```
locale-gen
```
# config locale
```
nvim /etc/locale.conf
```
config file locale
```
isi lang=C menjadi lang=en_US.UTF-8
dan isi ALL=en_US.UTF-8
```

# hostname
```
echo [nama komputer] > /etc/hostname
```
# useradd
```
useradd -m [user]
passwd [user]
```

# generate initrams
```
mkinitcpio -P
```
# passwd root
```
passwd
```

# membuat grup/bootloader
```
pacman -S grub efibootmgr

grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=GRUB

grub-mkconfig -o /boot/grub/grub.cfg
```

# reboot
keluar dari chroot:
```
exit
```
umount
```
umount -R /mnt
```
reboot
```
reboot
```
setelah reboot lepas usb 


# SETELAH REBOOT TEKAN F12
### PILIH TULISAN GRUP
### LALU MASUK KE ARCHLINUX
