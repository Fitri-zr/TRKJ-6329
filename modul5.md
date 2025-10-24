# Modul Praktikum 5 - Security Integration

---

## Tujuan Pembelajaran

1. Mahasiswa memahami pentingnya keamanan dalam integrasi sistem.
2. Mahasiswa mampu mengimplementasikan **VPN site-to-site** untuk integrasi jaringan.
3. Mahasiswa dapat melakukan konfigurasi **firewall rules** untuk komunikasi antar sistem.
4. Mahasiswa memahami implementasi **SSL/TLS** pada web server.

---

## Bahan Kajian

* Konsep keamanan jaringan (CIA Triad).
* Virtual Private Network (VPN) site-to-site.
* Firewall (iptables, UFW, atau pfSense).
* SSL/TLS pada web server (Apache/Nginx).

---

## Alat & Bahan

* 2 server/laptop (bisa VM VirtualBox/VMware).
* Linux OS (Ubuntu/Debian).
* OpenVPN atau WireGuard.
* Firewall (iptables/UFW/pfSense).
* Web server (Nginx/Apache).
* Domain gratis (opsional, untuk SSL via Let’s Encrypt).

---

## Langkah Praktikum

### **Langkah 1 – Konfigurasi VPN Site-to-Site (OpenVPN)**

1. Instal OpenVPN di kedua server:

   ```bash
   sudo apt update
   sudo apt install openvpn easy-rsa -y
   ```
2. Generate sertifikat dan kunci dengan easy-rsa.
3. Konfigurasi server OpenVPN (`/etc/openvpn/server.conf`).
4. Konfigurasi client VPN dengan file `.ovpn`.
5. Jalankan dan uji koneksi antar server dengan `ping`.

---

### **Langkah 2 – Firewall Rules**

1. Aktifkan UFW:

   ```bash
   sudo ufw enable
   ```
2. Tambahkan aturan untuk membatasi akses hanya dari subnet VPN:

   ```bash
   sudo ufw allow from 10.8.0.0/24 to any port 22
   sudo ufw allow from 10.8.0.0/24 to any port 80
   ```
3. Uji akses dari luar subnet → harus ditolak.

---

### **Langkah 3 – SSL/TLS di Web Server**

1. Instal Nginx:

   ```bash
   sudo apt install nginx -y
   ```
2. Pasang sertifikat SSL gratis dengan Let’s Encrypt:

   ```bash
   sudo apt install certbot python3-certbot-nginx -y
   sudo certbot --nginx -d domainanda.com
   ```
3. Uji akses web via `https://domainanda.com`.

---

### **Langkah 4 – Integrasi**

* Integrasikan aplikasi web di server dengan **VPN + firewall + SSL/TLS**.
* Pastikan hanya client di dalam VPN yang bisa mengakses API atau database.

---

## Hasil yang Diharapkan

* Jaringan antar server berhasil dihubungkan dengan VPN.
* Firewall berjalan efektif untuk membatasi akses.
* Web server hanya dapat diakses dengan **HTTPS (SSL/TLS)**.

---

## Pertanyaan Diskusi

1. Apa perbedaan VPN **site-to-site** dengan **remote access VPN**?
2. Mengapa firewall perlu diatur walaupun sudah menggunakan VPN?
3. Bagaimana peran SSL/TLS dalam menjaga keamanan data pada web server?

---

## Referensi

1. OpenVPN Documentation – [https://openvpn.net](https://openvpn.net)
2. WireGuard Documentation – [https://www.wireguard.com](https://www.wireguard.com)
3. Nginx SSL/TLS Guide – [https://nginx.org/en/docs/http/configuring_https_servers.html](https://nginx.org/en/docs/http/configuring_https_servers.html)
4. UFW Documentation – [https://help.ubuntu.com/community/UFW](https://help.ubuntu.com/community/UFW)
5. Stallings, W. (2018). *Network Security Essentials: Applications and Standards*. Pearson.

---
