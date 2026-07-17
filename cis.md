daftar isi itu sudah di bagi pergrup
di service, yang harus di hafal adalah kata kedua, 
patern = apa yang diulang  
objek = apa yang berulang 
## initial setup
### filesystem
#### configure filesystem kernel modules
```
blacklist [nama modul]
install [nama modul] /bin/false
```
list module nya
- cramfs
- freevxfs
- hfs
- hfsplus
- jffs2
- overlay
- squashfs
- udf
- firewire-core
- usb-storage
- unused filesystem
#### configure filesystem partitions
memisahkan partisi dan memounting nya
```
[device] [mount_target] [mount_option] 0 0
```
list mount option:
1. nodev,nosuid,noexec
2. nodev,nosuid,noexec
3. nodev,nosuid
4. nodev,nosuid
5. nodev,nosuid,noexec
6. nodev,nosuid,noexec
7. nodev,nosuid,noexec

list mount page:
1. /tmp
2. /dev/shm
3. /home
4. /var
5. /var/tmp
6. /var/log
7. /var/log/audit

Contoh :
1. /dev/sda1/home nodev,nosuid
2. /dev/sda1/var nodev,nosuid
3. /dev/sda1/var/tmp nodev,nosuid,noexec

**Note**: 
yang gak ada noexec nya /home dan /var
### package management
#### configure package update
regularly update arch linux
```
pacman -Syu
```

#### configure bootloader
itu sudah dilakukan pada mounting boot nya

#### configure additional process hardening

| name                   | path                                  | value                      |
| ---------------------- | ------------------------------------- | -------------------------- |
| core                   | /etc/security/limits.d/60-limits.conf | * hard core 0              |
| fs.protected_hardlinks | /etc/sysctl.d/fs-prottected.conf      | fs.protected_hardlinks = 1 |
| fs.protected_symlinks  | /etc/sysctl.d/fs-symlink.conf         | s.protected_symlinks = 1   |
| fs.suid_dumpable       | /etc/sysctl.d/fs-dumpable.conf        | fs.suid_dumpable = 0       |
| kernel.dmesg_restrict  | /etc/sysctl.d/dmesg_restrict.conf     | kernel.dmesg_restrict = 1  |
| kernel.kptr_restrict   | /etc/sysctl.d/kptr_restrict.conf      | kernel.kptr_restrict = 2   |
| kernel.yama.ptrace_scope | /etc/sysctl.d/yama.ptrace_scop.conf | kernel.yama.ptrace_scope = 1 |
| kernel.randomize_va_space | /etc/sysctl.d/randomize_va_space.conf | kernel.randomize_va_space = 2|
| systemd-coredump ProcessSizeMax | /etc/systemd/coredump.conf.d/coredump ProcessSizeMax.conf | ProcessSizeMax=0 |
| systemd-coredump Storage | /etc/systemd/coredump.conf.d/coredump Storage.conf | Storage=none | 

kptr N = 1 or 2
yama N = 1 or 2 or 3
## services
##### convigure server service
pattern
1. systemctl stop [service]
2. systemctl mask [service name]

Contoh :

1. systemctl stop autofs.service
2. systemctl mask autofs.service

| no  | service               |
| --- | --------------------- |
| 1   | autofs.service        |
| 2   | avahi-daemon.socket   |
| 3   | avahi-daemon.service  |
| 4   | cockpit.socket        |
| 5   | cockpit.service       |
| 6   | kea-dhcp-ddns.service |
| 7   | kea-dhcp4.service     |
| 8   | kea-dhcp6.service     |
| 9   | named.service         |
| 10  | dnsmasq.service       | 
| 11  | vsftpd.service        | 
| 12  | dovecot.socket        | 
| 13  | dovecot.service       | 
| 14  | cyrus-imapd.service   |
| 15  | nfs-server.service    |
| 16  | cups.socket           |
| 17  | cups.service          |
| 18  | rpcbind.socket        |
| 19  | rpcbind.service       |
| 20  | rsyncd.socket         |
| 21  | rsyncd.service        |
| 22  | smb.service           |

#### configure client services 
pattern nya
```
pacman -Rs [nama package]
```
list nama packagenya:
- ftp
- ldap
- telnet
- tftp
 
## networks
### networks kernel modules
pattern
```
blacklist [nama modul]
install [nama modul] /bin/false
```
list nama modulenya:
- atm
- can
- dccp
- tipc
- rds
- sctp
### configure network kernel parameters (ipv4 dan ipv6)
##### ipv4

| name                                       | path                               | value                                          |
| ------------------------------------------ | ---------------------------------- | ---------------------------------------------- |
| net.ipv4.ip_forward                        | /etc/sysctl.d/ipv4.ip_forward.conf | net.ipv4.ip_forward = 0                        |
| net.ipv4.conf.all.forwarding               | /etc/sysctl.d/[nama file].conf     | net.ipv4.conf.all.forwarding = 0               |
| net.ipv4.conf.default.forwarding           | /etc/sysctl.d/[nama file].conf     | net.ipv4.conf.default.forwarding = 0           |
| net.ipv4.conf.all.send_redirects           | /etc/sysctl.d/[nama file].conf     | net.ipv4.conf.all.send_redirects = 0           |
| net.ipv4.conf.default.send_redirects       | /etc/sysctl.d/[nama file].conf     | net.ipv4.conf.default.send_redirects = 0       |
| net.ipv4.icmp_ignore_bogus_error_responses | /etc/sysctl.d/[nama file].conf     | net.ipv4.icmp_ignore_bogus_error_responses = 1 |
| net.ipv4.icmp_echo_ignore_broadcasts       | /etc/sysctl.d/[nama file].conf     | net.ipv4.icmp_echo_ignore_broadcasts = 1       |
| net.ipv4.conf.all.accept_redirects         | /etc/sysctl.d/[nama file].conf     | net.ipv4.conf.all.accept_redirect = 0          |
| net.ipv4.conf.default.accept_redirects     | /etc/sysctl.d/[nama file].conf     | net.ipv4.conf.default.accept_redirects = 0     |
| net.ipv4.conf.all.secure_redirects         | /etc/sysctl.d/[nama file].conf     | net.ipv4.conf.all.secure_redirects = 0         |
| net.ipv4.conf.default.secure_redirects     | /etc/sysctl.d/[nama file].conf     | net.ipv4.conf.default.secure_redirects = 0     |
| net.ipv4.conf.all.rp_filter                | /etc/sysctl.d/[nama file].conf     | net.ipv4.conf.all.rp_filter = 1                |
| net.ipv4.conf.default.rp_filter            | /etc/sysctl.d/[nama file].conf     | net.ipv4.conf.default.rp_filter = 1            |
| net.ipv4.conf.all.accept_source_route      | /etc/sysctl.d/[nama file].conf     | net.ipv4.conf.all.accept_source_route = 0      |
| net.ipv4.conf.default.accept_source_route  | /etc/sysctl.d/[nama file].conf     | net.ipv4.conf.default.accept_source_route = 0  |
| net.ipv4.conf.all.log_martians             | /etc/sysctl.d/[nama file].conf     | net.ipv4.conf.all.log_martians = 1             |
| net.ipv4.conf.default.log_martians         | /etc/sysctl.d/[nama file].conf     | net.ipv4.conf.default.log_martians = 1         |
| net.ipv4.conf.tcp_syncookies               | /etc/sysctl.d/[nama file].conf     | net.ipv4.conf.tcp_syncookies = 1               |

kalau yang ada "default"  itu hanya berlaku ke defaultnya saja kalau ada "all" itu berlakunya ke semua
##### ipv6

| name                                      | path                           | value                                         |
| ----------------------------------------- | ------------------------------ | --------------------------------------------- |
| net.ipv6.conf.all.forwarding              | /etc/sysctl.d/[nama file].conf | net.ipv6.conf.all.forwarding = 0              |
| net.ipv6.conf.default.forwarding          | /etc/sysctl.d/[nama file].conf | net.ipv6.conf.default.forwarding = 0          |
| net.ipv6.conf.all.accept_redirects        | /etc/sysctl.d/[nama file].conf | net.ipv6.conf.default.accept_redirects = 0    |
| net.ipv6.conf.default.accept_redirects    | /etc/sysctl.d/[nama file].conf | net.ipv6.conf.default.accept_redirects = 0    |
| net.ipv6.conf.all.accept_source_route     | /etc/sysctl.d/[nama file].conf | net.ipv6.conf.all.accept_source_route = 0     |
| net.ipv6.conf.default.accept_source_route | /etc/sysctl.d/[nama file].conf | net.ipv6.conf.default.accept_source_route = 0 |
| net.ipv6.conf.all.accept_ra               | /etc/sysctl.d/[nama file].conf | net.ipv6.conf.all.accept_ra = 0               |
| net.ipv6.conf.default.accept_ra           | /etc/sysctl.d/[nama file].conf | net.ipv6.conf.default.accept_ra =             |

**Note:** 
- biar cepet hafal di table nama sama value itu sama hanya membedakan di  "= [angka]"
- path nya pasti sama semua 
## firewalld
configure firewalld
```
pacman -S firewalld
```

```
pacman -S nftables
```

```
systemctl enable firewalld
```

##### firewalld loopback traffic
command nya ada: 
1. 
```
firewall-cmd --get-zone-of-interface=lo
```
output nya adalah 
```
trusted
```

jika outputnya **no dvice** gunakan command
```
firewalld-cmd --permanent --zone=trusted --add-interface=lo
```

jika ingin mengganti zone nya
```
firewalld-cmd --zone=public --change-interface=lo
```

- **ACCEPT** =  menerima semua paket yang masuk, kecuali yang dinonaktifkan oleh aturan tertentu.
- **REJECT** = menonaktifkan semua paket yang masuk kecuali yang telah kamu izinkan dalam aturan tertentu, dan mesin pengirim akan diberi tahu tentang penolakan tersebut.
- **DROP** = menonaktifkan semua paket yang masuk kecuali yang telah kamu izinkan dalam aturan tertentu, dan tidak ada informasi yang dikirim kembali ke mesin pengirim.

##### service firewalld
- untuk melihat list service 
```
firewall-cmd --get-service
```

- untuk menghapus service 
```
firewall-cmd --remove-service=[nama service nya]
```
contohnya: firewall-cmd --remove-service=cockpit

- untuk menghapus atau menutup port
```
firewall-cmd --remove-port=[nomer port nya]/[type port nya]
```
contohnya: firewall-cmd --remove-port=25/tcp 

- untuk membuat setting baru presistent
```
firewalld-cmd --runtime-to-permanent
```

##### firewalld loopback source address traffic
command nya:
```
firewalld-cmd --permanent --zone=[nama zone nya] --add-rich-rule=' rule family=ipv4 source address="[ip adress]" destination not address="[ip adress]" drop"
```
## Access control 
pattern 
#### configure SSH server

hak akses

| access    | alphabet | numeric |
| --------- | -------- | ------- |
| write     | w        | 4       |
| read      | r        | 2       |
| execution | x        | 1       |

contoh:
drwx-rx-x
0731

alfabet pertama itu dihiraukan ( d )
3 pertama adalah orang memiliki file
3 kedua adalah grup
3 terakhir adalah other (tidak user dan tidak dalam grup)

untuk mengubah hak akses
```
chmod 0600 [nama filenya]
```

untuk mengubah kepemilikan
```
chown -r root:root /etc/ssh/sshd_config
```
root:root
sebelah kiri usernya sebelah kanan grub nya

/etc/ssh/sshd_config
path atau letak file yang ditargetkan 


- **AllowUsers** = hanya nama pengguna (user) yang didaftarkan di sini yang diizinkan untuk masuk ke server.
- **AllowGroups** = digunakan untuk mempermudah administrator jika ada banyak orang untuk dizinkan.
- **DenyUsers** = nama pengguna yang ada di daftar ini akan **dilarang keras** untuk masuk ke dalam server, tanpa terkecuali.
- **DenyGroups** = digunakan untuk memblokir seluruh anggota dari suatu kelompok/grup secara bersamaan.

#### sudo is installed
pattern 
```
pacman -S sudo
```

#### sudo timestamp_timeout
pattern
```
nvim /etc/sudoers
```
isi dengan 
```
Defaults    env_reset,timestamp_timeout=15
```

```
nvim /etc/sudoers
```
dan
```
nvim /etc/sudoers.d
```

isi dengan:
```
Defaults use_pty
Defaults logfile="/var/log/sudo.log"
Defaults    env_reset,timestamp_timeout=15
```

#### users must provide password 
```
nvim /etc/sudoers
```

```
nvim /etc/sudoers.d
```

kedua file tersebut hapus line yang "NOPASSWD"

#### ensure access to the su  command
```
nvim /etc/pam.d/su
```
isi dengan 
```
auth required pam_wheel.so use_uid group=sugroup
```

#### user accounts and environment

- untuk mengubah shadow password masuk ke path /etc/login.defs

## logging and auditing
## system maintenance
