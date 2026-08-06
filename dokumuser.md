# Dokumentasi Instalasi Sistem Operasi [Nama OS]

Dokumen ini menjelaskan langkah-langkah instalasi sistem operasi [Nama OS] secara mendalam dan terstruktur. Panduan ini dirancang khusus agar mudah dipahami dan diikuti oleh pengguna yang baru pertama kali melakukan instalasi [Nama OS].

> **Peringatan Keselamatan Data:**  
> Proses instalasi ini melibatkan pembagian dan pemformatan partisi yang dapat menghapus seluruh data pada media penyimpanan. Pastikan seluruh data penting telah dicadangkan (backup) ke media eksternal atau layanan penyimpanan awan sebelum melanjutkan.

---

## 1. Persyaratan Instalasi

Sebelum memulai proses instalasi, pastikan perangkat dan kelengkapan Anda telah memenuhi persyaratan minimum berikut:

* **Firmware:** UEFI (dibutuhkan karena instalasi menggunakan skema partisi GPT dengan partisi EFI System).
* **Media Installer:** Live environment [Nama OS] (USB/ISO boot).
* **Kapasitas Penyimpanan:** Ruang kosong pada disk minimal 20 GB.
* **Koneksi Jaringan:** Koneksi internet yang stabil, dibutuhkan untuk mengunduh package (`git`) dan meng-clone repository installer.
* **Pencadangan Data:** Seluruh data penting sudah diamankan ke tempat lain.
* **Arsitektur Perangkat:** Komputer menggunakan prosesor dengan arsitektur 64-bit (x86_64).

---

## 2. Langkah-Langkah Instalasi

### Langkah 1: Booting ke Live Environment [Nama OS]

1. Colokkan media installer ke komputer, lalu restart komputer.
2. Saat layar pertama menyala, masuk ke **Boot Menu** (umumnya `F12`, `F11`, `F8`, `F9`, atau `Delete`).
3. Pilih media installer [Nama OS] dari daftar boot.
4. Tunggu hingga sistem masuk ke lingkungan installer (terminal/command prompt), ditandai login otomatis sebagai `root`.

---

### Langkah 2: Membuat Partisi Penyimpanan

Sebelum instalasi dilakukan, disk perlu dibagi menjadi beberapa partisi menggunakan skema **GPT (GUID Partition Table)**. GPT dipilih karena:
- Wajib digunakan pada sistem yang boot melalui **UEFI** — partisi EFI System hanya dapat dibuat di atas GPT.
- Mendukung disk berukuran besar dan lebih dari 4 partisi primer, berbeda dari skema MBR/`dos` yang lebih lama.

#### Skema dan Rekomendasi Partisi (Contoh Disk 20 GB):

| Partisi | Device | Fungsi Utama | Tipe Partisi (*Type*) | Ukuran |
| :--- | :--- | :--- | :--- | :--- |
| **EFI** | `/dev/vda1` | Menyimpan file bootloader sistem | `EFI System` | 1 GB |
| **Root** | `/dev/vda2` | Menyimpan sistem operasi, konfigurasi, dan aplikasi | `Linux filesystem` | 15 GB |
| **[Nama partisi 3]** | `/dev/vda3` | [jelaskan fungsi, misal: `/home` atau `/var`] | `Linux filesystem` | 3 GB |
| **Swap** | `/dev/vda4` | Memori tambahan/cadangan ketika RAM utama penuh | `Linux swap` | ~1 GB |

#### Visualisasi Struktur Partisi Disk 20GB:
```text
Disk 20GB (/dev/vda)
├── EFI                1 GB
├── Root               15 GB
├── [Partisi Ketiga]    3 GB
└── Swap               ~1 GB
```

#### 2.1 Melihat Disk yang Tersedia
```bash
lsblk
```
> **Peringatan Penting:** Pastikan Anda memilih nama disk yang benar (contoh: `/dev/vda`). Kesalahan memilih disk dapat menyebabkan data pada drive lain terhapus.

#### 2.2 Membuat Partisi Menggunakan `cfdisk`
Buka alat pembuat partisi dengan perintah:
```bash
cfdisk /dev/vda
```

Jika disk belum memiliki partition table, akan muncul pilihan **Select label type**. Pilih **`gpt`**.

Lakukan pembuatan 4 partisi sesuai tabel di atas:

1. Pilih ruang kosong (**Free space**) → **[New]** → masukkan ukuran partisi.
2. Setelah partisi dibuat, pilih **[Type]** untuk mengatur tipe partisi sesuai kolom *Tipe Partisi* di tabel (contoh: `EFI System` untuk partisi pertama).
3. Ulangi untuk semua partisi yang direncanakan.

#### 2.3 Menyimpan dan Keluar dari `cfdisk`
1. Pilih menu **[Write]** di bagian bawah layar `cfdisk`.
2. Ketik `yes` lalu tekan `Enter` untuk mengonfirmasi perubahan partisi pada disk.
3. Pilih menu **[Quit]** untuk keluar dari utilitas `cfdisk`.

---

### Langkah 3: Menginstal Package Git

Package `git` dibutuhkan untuk mengunduh berkas installer [Nama OS] pada langkah berikutnya:
```bash
pacman -Sy git
```

---

### Langkah 4: Mengunduh Installer

Clone repository installer:
```bash
git clone https://codeberg.org/shelver/installer.git
```

> **Catatan Keamanan:** Jangan menyisipkan token akses langsung di URL clone (format `https://user:TOKEN@codeberg.org/...`). Token tersebut akan tersimpan di riwayat shell dan bisa bocor jika perangkat/dokumen dibagikan ke pihak lain. Gunakan credential helper, prompt interaktif, atau SSH key sebagai gantinya.

Masuk ke direktori script:
```bash
cd installer
cd script
```

Berikan izin eksekusi pada script:
```bash
chmod +x install.sh
```

---

### Langkah 5: Menjalankan Instalasi

Jalankan skrip otomatis instalasi:
```bash
./install.sh
```

Skrip installer akan menjalankan tahapan berikut:
1. **[Tahapan 1]** — [jelaskan, misal: instalasi sistem dasar]
2. **[Tahapan 2]** — [jelaskan, misal: konfigurasi timezone & hostname]
3. **[Tahapan 3]** — [jelaskan, misal: instalasi bootloader ke partisi EFI]
4. **[Tahapan 4]** — [jelaskan, misal: pembuatan user & password]

Setelah proses instalasi selesai, lepas media installer dan lakukan **restart** pada komputer Anda. Sistem operasi [Nama OS] siap digunakan.
