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

## A. Topologi IP Routes

![Image](<assets/img/IMG%20(1).jpg>)

## B. Konfigurasi DNS Server pada Desktop 2

### 1. Menambahkan Zona Internal

```
sudo nano /etc/bind/named.conf
```

Hasil

![Image](<assets/img/IMG%20(5).png>)

Tambahkan include "/etc/bind/named.conf.internal-zones"; pada baris bawah sendiri kemudian save perubahan tersebut. Package yang baru saja ditambahkan berguna untuk menyimpan konfigurasi zona tambahan khusus pada jaringan internal.

### 2. Konfigurasi pada named.conf.options

```
sudo nano /etc/bind/named.conf.options
```

Hasil


![Image](<assets/img/IMG%20(6).png>)

1. Membuat ACL (Access Control List) dengan jaringan yang terhubung: 192.168.10.0/24.
2. Hanya localhost dan jaringan internal-network (192.168.10.0/24) yang boleh mengirim query DNS ke server ini.
3. Hanya localhost yang diizinkan melakukan zone transfer (biasanya dipakai untuk secondary DNS).
4. Mengaktifkan rekursi, artinya server akan meneruskan permintaan DNS yang tidak diketahui ke server lain (misalnya untuk browsing internet).

### 3. Konfigurasi pada named.conf.internal-zones

```
sudo nano /etc/bind/named.conf.internal-zones
```

Hasil

![Image](<assets/img/IMG%20(7).png>)

1. Menyimpan konfigurasi zona DNS untuk domain internal (kelompok1.home).
2. Menyediakan forward dan reverse lookup untuk jaringan 192.168.10.0/24.

### 4. Konfigurasi pada default/named

```
sudo nano /etc/default/named
```

Hasil

![Image](<assets/img/IMG%20(8).png>)

1. Menjalankan BIND sebagai user non-root (bind) untuk keamanan.
2. Menonaktifkan IPv6, hanya mengaktifkan dukungan IPv4.

### 5. Konfigurasi pada Zone Files

```
sudo nano /etc/bind/kelompok1.home
```

Hasil

![Image](<assets/img/IMG%20(9).png>)

File srv.world.lan ini adalah file zona forward lookup untuk domain lokal kelompok10.home. Ia menyimpan:
1. Info DNS utama (NS)
2. Info mail server (MX)
3. Pemetaan hostname ke IP (A record)

### 6. Konfigurasi pada Zone Reverse Lookup

```
sudo nano /etc/bind/192.168.10.db
```

Hasil

![Image](<assets/img/IMG%20(10).png>)

1. Menyatakan bahwa ns.kelompok10.home adalah name server untuk zona ini (reverse zona 192.168.10.1).
2. Melakukan pemetaan IP ke nama domain:
"192.168.10.10" akan dikenali sebagai ns.kelompok10.home.
"192.168.10.11" akan dikenali sebagai www.kelompok10.home.

### 7. Verifikasi Perubahan

```
systemctl restart named
sudo nano /etc/resolv.conf
```

Hasil

![Image](<assets/img/IMG%20(11).png>)

Baris nameserver "192.168.10.10" digunakan untuk mengatur DNS resolver lokal agar mengarah ke DNS server internal (192.168.10.10).

### 7. Menggunakan perintah DIG (Domain Information Groper)

```
dig ns.kelompok10.home
```

Hasil

![Image](<assets/img/IMG%20(12).png>)

Gambar tersebut adalah perintah Dig (Domain Information Groper) yang digunakan untuk melakukan query DNS.
Dalam hal ini, saya meminta alamat IP dari domain ns.kelompok10.home.

### 8. Menggunakan perintah DIG -x (Domain Information Groper)

```
dig -x 10.0.0.1
```

Hasil

![Image](<assets/img/IMG%20(13).jpg>)

Gambar tersebut adalah digunakan untuk melakukan pencarian (query) terhadap DNS (Domain Name System) untuk melakukan reverse lookup terhadap alamat IP yang diberikan, dalam hal ini 10.0.0.1

## C. Konfigurasi Web Server Apache pada Desktop 2

### 1. Mengatur Konfigurasi Keamanan Apache

```
nano /etc/apache2/conf-enabled/security.conf
```

Hasil

![Image](<assets/img/IMG%20(14).jpg>)

Gambar diatas adalah berisi pengaturan terkait aspek keamanan untuk server Apache, seperti pengaturan otorisasi
pembatasan akses, header HTTP, dan lainnya.
disini saya menambahkan ServerTokens Prod: Membatasi informasi yang diberikan oleh server tentang dirinya sendiri, hanya menampilkan nama dan versi utama produk. Ini membantu mengurangi informasi yang dapat digunakan oleh pihak yang tidak bertanggung jawab untuk mengeksploitasi kerentanannya.

### 2. Mengatur Konfigurasi Apache Aktif

```
nano /etc/apache2/mods-enabled/dir.conf
```
Hasil

![Image](<assets/img/IMG%20(15).jpg>)

 DirectoryIndex: Direktif ini memberitahu Apache file apa yang harus dicari dan ditampilkan secara default ketika sebuah direktori diakses tanpa menyebutkan nama file.

index.html index.htm: Daftar file yang akan dicari oleh Apache secara berurutan. Jika ada index.html, maka itu yang ditampilkan. Jika tidak ada, maka akan mencoba index.htm.

### 3. Menambahkan domain server

```
nano /etc/apache2/apache2.conf
```

Hasil

![Image](<assets/img/IMG%20(16).jpg>)

ServerName www.kelompok10.home: Baris ini menetapkan nama host utama (FQDN) dari server Apache. Ini mencegah peringatan seperti Could not reliably determine the server's fully qualified domain name saat Apache dimulai, dan digunakan untuk merespons permintaan HTTP.

### 4. Menambahkan domain server

```
nano /etc/apache2/sites-enabled/000-default.conf
```
Hasil

![Image](<assets/img/IMG%20(17).jpg>)

ServerAdmin webmaster@kelompok10.home: Baris ini menetapkan alamat email administrator web (webmaster) yang akan ditampilkan di halaman error server (misalnya error 500) agar pengguna tahu harus menghubungi siapa jika ada masalah.

### 5. Mengakses Webserver

Hasil

![Image](<assets/img/IMG%20(19).jpg>)

Mengakses pada browser untuk validasi webserver dapat berfungsi dengan baik dengan mengetik sesuai IP "http://192.168.10.10".

## C. Konfigurasi Mail Server
### 1. Mengatur jaringan local dan eksternal

```
nano /etc/postfix/main.cf
```

Hasil

![Image](<assets/img/IMG%20(20).jpg>)

inet_interfaces = all Memungkinkan server untuk menerima koneksi dari semua jaringan yang terhubung ke server, baik itu dari localhost atau jaringan eksternal.

### 2. Mengatur Perilaku Server

```
nano /etc/postfix/main.cf
```

Hasil

![Image](<assets/img/IMG%20(21).jpg>)

mail_owner = postfix
Menentukan user sistem yang akan menjalankan proses Postfix.

### 3. Menambahkan hostname

```
nano /etc/postfix/main.cf
```

Hasil

![Image](<assets/img/IMG%20(21).jpg>)

myhostname = mail.kelompok10.home
Menentukan hostname dari server email ini.

### 4. Menambahkan Jaringan

```
nano /etc/postfix/main.cf
```

Hasil

![Image](<assets/img/IMG%20(22).jpg>)

mynetworks = 127.0.0.0/8, 192.168.10.0/24
Spesifik jaringan lokal yang diizinkan relay email melalui server.

### 5. Mengizinkan Jaringan

```
nano /etc/postfix/main.cf
```

Hasil

![Image](<assets/img/IMG%20(23).jpg>)

mynetworks_style = subnet
Mengizinkan semua alamat dalam subnet lokal untuk mengirim email.

### 6. Menentukan Format Penyimpanan

```
nano /etc/postfix/main.cf
```

Hasil

![Image](<assets/img/IMG%20(24).jpg>)

home_mailbox = Maildir/
Menentukan format penyimpanan email (Maildir).

### 7. Menetapkan Jalur

```
nano /etc/postfix/main.cf
```

Hasil

![Image](<assets/img/IMG%20(25).jpg>)

sendmail_path = /usr/sbin/postfix

menetapkan jalur untuk program sendmail yang digunakan oleh sistem untuk mengirim email.

### 8. Identifikasi SMTP

```
nano /etc/postfix/main.cf
```

Hasil

![Image](<assets/img/IMG%20(26).jpg>)

smtpd_banner = $myhostname ESMTP
Banner identifikasi SMTP yang sederhana (menghilangkan informasi distro untuk keamanan).

### 9. Menggunakan ipv4

```
nano /etc/postfix/main.cf
```

Hasil

![Image](<assets/img/IMG%20(27).jpg>)

inet_protocols = ipv4
Hanya menggunakan IPv4 (nonaktifkan IPv6).

### 10. Mengatur beberapa path

```
nano /etc/postfix/main.cf
```

Hasil

![Image](<assets/img/IMG%20(28).jpg>)

- sendmail_path = /usr/sbin/postfix
Mengatur agar sistem menggunakan Postfix sebagai pengganti perintah sendmail untuk mengirim email.
- newaliases_path = /usr/bin/newaliases
Menetapkan jalur perintah newaliases yang digunakan untuk memperbarui database alias email (/etc/aliases).
- mailq_path = /usr/bin/mailq
Menetapkan jalur perintah mailq untuk melihat antrean email yang sedang menunggu pengiriman.
- setgid_group = postdrop

Menetapkan grup postdrop untuk mengatur izin akses saat pengguna biasa mengirim email ke antrean Postfix (antrian mail drop).

### 11. Security dan Auth

```
nano /etc/postfix/main.cf
```
Hasil

![Image](<assets/img/IMG%20(29).jpg>)

Baris akhir tambahan (security & auth):

disable_vrfy_command = yes → Menonaktifkan perintah VRFY untuk keamanan.

smtpd_helo_required = yes → Memaksa client mengirim perintah HELO/EHLO.

message_size_limit = 10240000 → Batasi ukuran email 10MB.

SMTP AUTH config → Mengaktifkan otentikasi SMTP dengan Dovecot (smtpd_sasl_*).


## Kesimpulan
Konfigurasi DNS Server (BIND) pada Debian 12 bertujuan untuk membangun layanan sistem penamaan domain menggunakan perangkat lunak BIND. Proses ini mencakup instalasi paket, pengaturan file zona dan konfigurasi named.conf, serta pengujian untuk memastikan fungsi DNS berjalan dengan baik. Implementasi ini memungkinkan server berperan sebagai pengelola layanan nama domain baik untuk kebutuhan lokal maupun publik.

Konfigurasi HTTP Server (Apache2) difokuskan pada penyediaan layanan web menggunakan Apache di lingkungan Debian 12. Panduan mencakup instalasi server, pembuatan virtual host, aktivasi modul-modul penting seperti SSL, serta pengaturan direktori dan hak akses. Tujuan utamanya adalah memastikan server web dapat menyajikan konten secara optimal dan aman.

Konfigurasi Mail Server menggunakan Postfix dan Dovecot dimaksudkan untuk membangun sistem pengiriman dan penerimaan email yang lengkap. Postfix digunakan sebagai agen transfer email (MTA), sementara Dovecot sebagai agen penerima (IMAP/POP3). Panduan ini juga mencakup pengaturan pengguna, penerapan keamanan komunikasi (SSL/TLS), serta fitur proteksi tambahan seperti SPF, DKIM, dan DMARC, guna meningkatkan integritas serta keandalan sistem surat elektronik.
