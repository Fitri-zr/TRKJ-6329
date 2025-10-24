# Modul Praktikum 7 - Containerization dengan Docker Compose

---

## Tujuan Pembelajaran

1. Mahasiswa memahami konsep **virtualisasi vs containerization**.
2. Mahasiswa dapat menginstal dan mengonfigurasi **Docker**.
3. Mahasiswa mampu membuat dan menjalankan **Docker Compose** untuk integrasi multi-service.
4. Mahasiswa dapat mengintegrasikan beberapa layanan (Web, Database, Monitoring) dalam satu cluster container.

---

## Bahan Kajian

* Konsep containerization.
* Docker engine & Docker Compose.
* Integrasi multi-service: Web server + Database + Monitoring tool.
* Deployment dengan Compose file (`docker-compose.yml`).

---

## Alat & Bahan

* Server/VM dengan OS Ubuntu/Debian.
* Docker Engine.
* Docker Compose.
* Layanan yang diintegrasikan:

  * **Web server (Nginx/Apache)**
  * **Database (MySQL/PostgreSQL)**
  * **Monitoring tool (Grafana/Portainer)**

---

## Langkah Praktikum

### **Langkah 1 – Instalasi Docker**

1. Update sistem:

   ```bash
   sudo apt update && sudo apt upgrade -y
   ```
2. Instal Docker:

   ```bash
   curl -fsSL https://get.docker.com -o get-docker.sh
   sh get-docker.sh
   ```
3. Tambahkan user ke group docker:

   ```bash
   sudo usermod -aG docker $USER
   ```

---

### **Langkah 2 – Instalasi Docker Compose**

1. Download Docker Compose:

   ```bash
   sudo curl -L "https://github.com/docker/compose/releases/download/2.20.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
   ```
2. Berikan permission:

   ```bash
   sudo chmod +x /usr/local/bin/docker-compose
   ```
3. Verifikasi:

   ```bash
   docker-compose --version
   ```

---

### **Langkah 3 – Membuat Docker Compose File**

1. Buat direktori project:

   ```bash
   mkdir integrasi-docker && cd integrasi-docker
   ```
2. Buat file `docker-compose.yml`:

   ```yaml
   version: '3'
   services:
     web:
       image: nginx
       ports:
         - "8080:80"
       volumes:
         - ./html:/usr/share/nginx/html
       depends_on:
         - db

     db:
       image: mysql:5.7
       environment:
         MYSQL_ROOT_PASSWORD: rootpass
         MYSQL_DATABASE: integrasi
         MYSQL_USER: user
         MYSQL_PASSWORD: userpass
       volumes:
         - db_data:/var/lib/mysql

     grafana:
       image: grafana/grafana
       ports:
         - "3000:3000"

   volumes:
     db_data:
   ```

---

### **Langkah 4 – Menjalankan Layanan**

1. Jalankan Compose:

   ```bash
   docker-compose up -d
   ```
2. Verifikasi container berjalan:

   ```bash
   docker ps
   ```
3. Akses layanan:

   * Web server → `http://localhost:8080`
   * Grafana → `http://localhost:3000` (login: admin/admin)
   * Database MySQL → port default 3306

---

### **Langkah 5 – Uji Integrasi**

1. Tambahkan file `index.html` di folder `html/`.
2. Integrasikan database ke aplikasi web sederhana (opsional).
3. Buat dashboard di Grafana untuk monitoring MySQL container.

---

## Hasil yang Diharapkan

* Docker Engine & Docker Compose terinstal dengan benar.
* Semua container (Web, DB, Grafana) berjalan dalam satu cluster.
* Web server dapat diakses di port 8080.
* Database dapat terhubung ke web app.
* Grafana berjalan di port 3000 dan bisa menampilkan data monitoring.

---

## Pertanyaan Diskusi

1. Apa perbedaan **VM** dan **Container**?
2. Bagaimana kelebihan menggunakan **Docker Compose** dibanding menjalankan container satu per satu?
3. Bagaimana containerization mendukung **sistem terintegrasi** dalam skala enterprise?

---

## Referensi

1. Docker Documentation – [https://docs.docker.com](https://docs.docker.com)
2. Merkel, D. (2014). *Docker: Lightweight Linux Containers for Consistent Development and Deployment*. Linux Journal.
3. Mouat, A. (2016). *Using Docker*. O’Reilly Media.
4. Grafana Docs – [https://grafana.com/docs/](https://grafana.com/docs/)

---
