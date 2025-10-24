#  Modul Praktikum 1 – Integrasi Layanan Dasar (Web + DB + DNS)

## Tujuan Pembelajaran

Setelah menyelesaikan modul ini, mahasiswa mampu:

1. Menginstal dan mengonfigurasi web server, database server, dan DNS server secara terintegrasi.
2. Menghubungkan aplikasi web dengan database dan domain internal.
3. Memahami prinsip integrasi layanan dasar dalam sistem jaringan.

---

## Dasar Teori

Integrasi layanan dasar adalah fondasi utama sistem terdistribusi modern, di mana **web server**, **database server**, dan **DNS server** bekerja bersama untuk menyediakan layanan aplikasi yang stabil dan mudah diakses.

* **Web Server (Apache/Nginx)** melayani permintaan pengguna dan mengelola halaman web dinamis.
* **Database Server (MySQL/MariaDB/PostgreSQL)** menyimpan data aplikasi yang diakses melalui query SQL.
* **DNS Server (Bind9 / Dnsmasq)** mengubah nama domain menjadi alamat IP agar layanan dapat diakses dengan nama domain lokal.

Integrasi ketiganya menghasilkan sistem yang efisien, mudah di-maintain, dan scalable, yang menjadi dasar pengembangan sistem berbasis cloud maupun enterprise.

---

## Alat dan Bahan

* Komputer / VM Linux Ubuntu 22.04
* Koneksi jaringan lokal (misalnya melalui VirtualBox Host-Only Adapter)
* Software:

  * Apache2 atau Nginx
  * MySQL / MariaDB
  * PHP
  * Bind9 (DNS Server)
* Browser web (Firefox / Chrome)

---

## Langkah Praktikum

#### **Langkah 1: Instalasi Layanan Dasar**

```bash
sudo apt update
sudo apt install apache2 mysql-server php libapache2-mod-php php-mysql bind9 -y
```

#### **Langkah 2: Verifikasi Instalasi**

Pastikan semua layanan berjalan:

```bash
sudo systemctl status apache2
sudo systemctl status mysql
sudo systemctl status bind9
```

#### **Langkah 3: Konfigurasi DNS Server**

Edit file zona:

```bash
sudo nano /etc/bind/named.conf.local
```

Tambahkan konfigurasi zona:

```
zone "praktikum.local" {
    type master;
    file "/etc/bind/db.praktikum.local";
};
```

Salin dan ubah file zona:

```bash
sudo cp /etc/bind/db.local /etc/bind/db.praktikum.local
sudo nano /etc/bind/db.praktikum.local
```

Isi contoh:

```
$TTL 604800
@   IN  SOA ns1.praktikum.local. admin.praktikum.local. (
        2 ; Serial
        604800 ; Refresh
        86400 ; Retry
        2419200 ; Expire
        604800 ) ; Negative Cache TTL
;
@   IN  NS  ns1.praktikum.local.
ns1 IN  A   192.168.10.1
web IN  A   192.168.10.2
db  IN  A   192.168.10.3
```

Restart DNS:

```bash
sudo systemctl restart bind9
```

#### **Langkah 4: Konfigurasi Database**

Masuk ke MySQL:

```bash
sudo mysql -u root -p
```

Buat database dan user:

```sql
CREATE DATABASE praktikum;
CREATE USER 'praktikan'@'%' IDENTIFIED BY '12345';
GRANT ALL PRIVILEGES ON praktikum.* TO 'praktikan'@'%';
FLUSH PRIVILEGES;
EXIT;
```

#### **Langkah 5: Integrasi Web dan Database**

Buat file PHP untuk menguji koneksi:

```bash
sudo nano /var/www/html/dbtest.php
```

Isi:

```php
<?php
$conn = new mysqli("db.praktikum.local", "praktikan", "12345", "praktikum");
if ($conn->connect_error) {
    die("Koneksi gagal: " . $conn->connect_error);
}
echo "Koneksi ke database berhasil!";
?>
```

Akses di browser:
👉 `http://web.praktikum.local/dbtest.php`

Jika muncul pesan “Koneksi ke database berhasil!”, berarti integrasi layanan berjalan dengan baik.

---

### **Tugas/Laporan Praktikum**

1. Dokumentasikan konfigurasi (tangkapan layar + deskripsi langkah).
2. Jelaskan fungsi setiap layanan yang diintegrasikan.
3. Analisis potensi error atau konflik layanan yang muncul.
4. Buat diagram arsitektur sistem integrasi Web–DB–DNS.
5. Upload laporan ke LMS atau Google Classroom sesuai instruksi dosen.

---

### **Referensi**

1. Silberschatz, A., Galvin, P. B., & Gagne, G. (2020). *Operating System Concepts* (10th ed.). Wiley.
2. Kurose, J. F., & Ross, K. W. (2021). *Computer Networking: A Top-Down Approach* (8th ed.). Pearson.
3. Ubuntu Documentation. (2024). *Apache2, MySQL, PHP and BIND9 Configuration Guides.*
   [https://help.ubuntu.com](https://help.ubuntu.com)
4. DigitalOcean Tutorials. (2023). *How To Configure BIND as a DNS Server on Ubuntu 22.04.*
   [https://www.digitalocean.com/community/tutorials](https://www.digitalocean.com/community/tutorials)

---
