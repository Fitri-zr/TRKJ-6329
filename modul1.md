Praktikum 1 – Integrasi Layanan Dasar (Web + DB + DNS)

Tujuan: Mahasiswa mampu mengintegrasikan layanan dasar jaringan (Web Server, Database, DNS).
Langkah Praktikum:

Install Ubuntu Server di VM (VirtualBox/VMware/Proxmox).

Install Apache/Nginx:

```bash
sudo apt update
sudo apt install apache2 -y
```

Install MySQL/MariaDB:

```bash
sudo apt install mariadb-server -y
```

Install dan konfigurasi DNS (bind9).

Uji akses web server via domain lokal (mahasiswa.lab).

Integrasikan web app dengan database (misalnya aplikasi PHPMyAdmin).
