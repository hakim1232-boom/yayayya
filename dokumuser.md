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
| :--- | :--- |
