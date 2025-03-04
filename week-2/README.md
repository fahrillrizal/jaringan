<div align="center">
  <h1 style="text-align: center;font-weight: bold">Laporan Resmi Workshop Administrasi Jaringan</h1>
  <h4 style="text-align: center;">Dosen Pengampu : Dr. Ferry Astika Saputra, S.T., M.Sc.</h4>
</div>
<br />
<div align="center">
  <img src="https://upload.wikimedia.org/wikipedia/id/4/44/Logo_PENS.png" alt="Logo PENS">
  <h3 style="text-align: center;">Disusun Oleh : </h3>
  <p style="text-align: center;">
    <strong>Mochammad Fahril Rizal (3123500013)</strong><br>
  </p>
<h3 style="text-align: center;line-height: 1.5">Politeknik Elektronika Negeri Surabaya<br>Departemen Teknik Informatika Dan Komputer<br>Program Studi Teknik Informatika<br>2024/2025</h3>
  <hr><hr>
</div>

# Rangkuman UNIX dan LINUX
## Chapter 4: Process Control
Bab ini membahas bagaimana sistem operasi mengelola proses. Setiap proses memiliki ruang alamat dan struktur data dalam kernel, termasuk informasi seperti PID (Process ID), PPID (Parent Process ID), serta UID dan EUID untuk hak akses pengguna.

Proses memiliki siklus hidup yang dimulai dengan fork, di mana sebuah proses menyalin dirinya untuk membuat proses baru. Sistem juga menggunakan signals untuk komunikasi antar proses, misalnya KILL, INT, dan TERM untuk menghentikan proses.

Monitoring proses dilakukan dengan perintah seperti ps, top, dan htop untuk melihat penggunaan CPU dan memori oleh proses. Ada juga kill untuk menghentikan proses, serta nice dan renice untuk mengatur prioritas eksekusi.

Sistem berkas virtual /proc digunakan untuk menyimpan informasi tentang proses. Selain itu, alat seperti strace dapat digunakan untuk menganalisis perilaku suatu proses, dan cron digunakan untuk menjadwalkan eksekusi tugas secara otomatis.

## Chapter 5: The File System
Bab ini menjelaskan struktur sistem berkas pada UNIX/Linux, yang terdiri dari namespace (hirarki direktori), API (sistem panggilan untuk manipulasi berkas), model keamanan, dan implementasi fisik di perangkat penyimpanan.

Terdapat berbagai sistem berkas seperti ext4, XFS, NTFS, dan ISO 9660 untuk CD/DVD. Setiap file memiliki path yang dapat berupa absolute atau relative. File system dapat di-mount menggunakan perintah mount, dan di-unmount menggunakan umount.

Berbagai jenis file di UNIX termasuk regular files, directories, device files, symbolic links, dan named pipes. Hak akses file dikendalikan oleh bit read (r), write (w), dan execute (x), yang dapat diubah menggunakan chmod dan chown.

Fitur tambahan seperti Access Control Lists (ACLs) memberikan kontrol yang lebih detail atas hak akses file. Selain itu, sistem berkas memiliki mekanisme seperti sticky bit, setuid, dan setgid untuk mengatur hak eksekusi.

## Chapter 6: Software Installation and Management
Bab ini membahas pengelolaan perangkat lunak di sistem UNIX/Linux. Instalasi sistem operasi bisa dilakukan melalui media fisik seperti USB/DVD atau melalui jaringan menggunakan PXE (Preboot Execution Environment).

Linux memiliki dua sistem manajemen paket utama:

RPM (Red Hat, CentOS, Fedora, SUSE) DEB (Debian, Ubuntu) Alat manajemen paket tingkat tinggi mencakup APT (Advanced Package Tool) untuk sistem berbasis Debian dan YUM untuk distribusi berbasis RPM. Repositori paket menyimpan berbagai perangkat lunak yang dapat diunduh dan diperbarui secara otomatis.

Selain itu, konfigurasi perangkat lunak bisa disesuaikan dengan lingkungan lokal atau cloud agar lebih fleksibel dan mudah dipulihkan dalam kasus kegagalan sistem.


# Menginstall Debian Linux dengan VirtualBox melalui root terminal pada OS Linux

## 1. Check Ubuntu Version
### Perintah terminal
```
lsb_release -a
```
Perintah diatas adalah untuk cek detail versi os dari os Linux/UNIX

## 2. Check Dependensi 
### Perintah terminal
```
sudo apt --fix-broken install
```
Memperbaiki dependensi yang belum terpenuhi atau paket yang rusak pada sistem berbasis Linux

## 3. Install VirtualBox Debian menggunakan paket melalui terminal
### Perintah terminal

```
sudo dpkg -i virtualbox-7.1_7.1.6-167084~Debian~bookworm_amd64.deb
```
Fungsi untuk menginstal paket VirtualBox versi 7.1.6 dari file .deb secara manual menggunakan dpkg (Debian Package Manager).

## 4. Problem Solve Error Installing
Setelah install virtualbox jika muncul error
```
error The following NEW packages will be installed:
  libxcb-cursor0
```
### Perintah terminal
```
sudo apt install libxcb-cursor0
```
Install library libxcb-cursor0 pada sistem berbasis Linux

## 5. Menambah User VirtualBox
### Perintah terminal
```
sudo usermod -aG vboxusers$
```
menambahkan pengguna saat ini ke dalam grup vboxusers, yang diperlukan agar pengguna dapat menggunakan fitur tertentu di VirtualBox, seperti akses ke perangkat USB. 

## 6. Interface Installation VirtualBox Debian

![](assets/img/2.png)

Jika sudah download file ISO Debian 12, berikan lokasi file ISO tersebut kolom ISO Image.

Jika belum, bisa install melalui web https://www.debian.org/distrib/ atau bisa mengambil dari pc yang telah memiliki iso debian dengan cara berikut
### 1
```
sftp <IP Address>
```
untuk IP disesuaikan dengan IP PC/komputer yang telah memiliki iso debian
### 2
```
cd Downloads
```
pindah ke directory tempat iso berada
### 3
```
get debian-12.9.0-amd64-netinst.iso
```
untuk debian tergantung debian yang ada di PC/komputer tersebut

### Langkah berikutnya

![](assets/img/2.png)
Gunakan password "student"

### Penggunaan RAM dan Proccessors yang dibutuhkan

![](assets/img/3.png)

Untuk RAM minimalnya 2 GB dan untuk Proccessors menggunakan 2 CPU

### Penggunaan HDD yang dibutuhkan

![](assets/img/4.png)

Menggunakan 10 GB untuk alokasi memory pada Harddisk

### Validasi Kebutuhan install

![](assets/img/5.png)

Jika sudah sesuai dengan spesifikasi yang diinginkan lanjut klik Finish untuk memproses instalasi.

### Proses Instalasi Berjalan

![](assets/img/6.png)

Tunggu Hingga Proses instalasi selesai.

### Instalasi Virtualbox Debian Linux Berhasil

![](assets/img/7.png)
