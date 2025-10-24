# Modul Praktikum 6 - Monitoring & Visualization (Zabbix/Nagios + Grafana)

---

## Tujuan Pembelajaran

1. Mahasiswa memahami konsep **monitoring sistem dan jaringan**.
2. Mahasiswa mampu menginstal dan mengonfigurasi **Zabbix/Nagios** untuk monitoring.
3. Mahasiswa mampu mengintegrasikan data monitoring dengan **Grafana** untuk visualisasi.
4. Mahasiswa dapat menganalisis hasil monitoring untuk evaluasi kinerja sistem.

---

## Bahan Kajian

* Konsep monitoring infrastruktur TI.
* Zabbix/Nagios sebagai monitoring server.
* Grafana untuk dashboard visualisasi.
* Alerting system (email/telegram integration).

---

## Alat & Bahan

* 2 VM/Server:

  * **Server Monitoring** (Ubuntu/Debian).
  * **Client/Agent** (Ubuntu/Debian).
* Zabbix atau Nagios.
* Grafana.
* Web browser untuk akses dashboard.

---

## Langkah Praktikum

### **Langkah 1 – Instalasi Zabbix Server**

1. Update sistem:

   ```bash
   sudo apt update && sudo apt upgrade -y
   ```
2. Tambahkan repo Zabbix:

   ```bash
   wget https://repo.zabbix.com/zabbix/6.0/ubuntu/pool/main/z/zabbix-release/zabbix-release_6.0-4+ubuntu20.04_all.deb
   sudo dpkg -i zabbix-release_6.0-4+ubuntu20.04_all.deb
   sudo apt update
   ```
3. Instal Zabbix server, frontend, dan agent:

   ```bash
   sudo apt install zabbix-server-mysql zabbix-frontend-php zabbix-apache-conf zabbix-sql-scripts zabbix-agent -y
   ```

---

### **Langkah 2 – Konfigurasi Database Zabbix**

1. Buat database MySQL:

   ```sql
   CREATE DATABASE zabbix CHARACTER SET UTF8 COLLATE UTF8_BIN;
   CREATE USER 'zabbix'@'localhost' IDENTIFIED BY 'password';
   GRANT ALL PRIVILEGES ON zabbix.* TO 'zabbix'@'localhost';
   ```
2. Import schema bawaan Zabbix.

---

### **Langkah 3 – Konfigurasi Zabbix Agent di Client**

1. Instal agent di client server:

   ```bash
   sudo apt install zabbix-agent -y
   ```
2. Edit file `/etc/zabbix/zabbix_agentd.conf` → set `Server=<IP_Zabbix_Server>`.
3. Restart agent:

   ```bash
   sudo systemctl restart zabbix-agent
   ```

---

### **Langkah 4 – Instalasi Grafana**

1. Tambahkan repo Grafana:

   ```bash
   sudo apt install -y software-properties-common
   sudo add-apt-repository "deb https://packages.grafana.com/oss/deb stable main"
   wget -q -O - https://packages.grafana.com/gpg.key | sudo apt-key add -
   sudo apt update
   sudo apt install grafana -y
   ```
2. Start service:

   ```bash
   sudo systemctl enable grafana-server
   sudo systemctl start grafana-server
   ```
3. Akses Grafana via browser `http://<IP>:3000` (default login: admin/admin).

---

### **Langkah 5 – Integrasi Zabbix dengan Grafana**

1. Tambahkan Zabbix data source di Grafana.
2. Buat dashboard untuk menampilkan metric CPU, RAM, Disk, Network.
3. Tambahkan panel alert (misal: CPU > 80%).

---

## Hasil yang Diharapkan

* Server client berhasil terhubung dengan Zabbix server.
* Data monitoring (CPU, RAM, Disk, Network) muncul di dashboard.
* Grafana menampilkan data dengan visualisasi grafik interaktif.
* Alert berjalan saat threshold tercapai.

---

## Pertanyaan Diskusi

1. Apa perbedaan monitoring menggunakan **Zabbix** dan **Nagios**?
2. Bagaimana kelebihan **Grafana** dibandingkan dengan dashboard bawaan Zabbix?
3. Mengapa monitoring sangat penting dalam sistem terintegrasi berskala besar?

---

## Referensi

1. Zabbix Official Documentation – [https://www.zabbix.com/documentation](https://www.zabbix.com/documentation)
2. Nagios Core Documentation – [https://www.nagios.org/documentation](https://www.nagios.org/documentation)
3. Grafana Docs – [https://grafana.com/docs/](https://grafana.com/docs/)
4. Barczyński, M. (2020). *Mastering Zabbix – Fourth Edition*. Packt Publishing.
5. Turnbull, J. (2018). *The Art of Monitoring*. Turnbull Press.

---
