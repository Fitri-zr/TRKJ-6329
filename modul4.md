# Modul Praktikum 4 - Cloud Integration

---

## Tujuan Pembelajaran

1. Mahasiswa memahami konsep **integrasi layanan di cloud**.
2. Mahasiswa mampu melakukan **deploy layanan terintegrasi** ke cloud (VPS/Heroku).
3. Mahasiswa dapat menghubungkan aplikasi **on-premise** dengan **layanan cloud API** (hybrid cloud).
4. Mahasiswa memahami tantangan **keamanan & skalabilitas** dalam integrasi cloud.

---

## Bahan Kajian

* Model cloud: IaaS, PaaS, SaaS.
* Cloud deployment (VPS, Heroku, Docker Hub).
* Hybrid cloud integration.
* API berbasis cloud (Google Cloud, AWS, Firebase).

---

## Alat & Bahan

* Akun cloud (contoh: **Heroku**, **AWS Free Tier**, **Google Cloud**, atau VPS seperti DigitalOcean).
* Docker & Docker Compose.
* Node.js/Python (sebagai aplikasi layanan).
* Database (MySQL/PostgreSQL).
* Internet + terminal Linux.

---

## Langkah Praktikum

### **Langkah 1 – Deploy Aplikasi ke VPS**

1. Sewa VPS (mis. DigitalOcean, AWS EC2, atau GCP VM).
2. Install stack layanan (contoh: Nginx + Node.js + MySQL).

   ```bash
   sudo apt update
   sudo apt install nginx mysql-server nodejs npm -y
   ```
3. Upload aplikasi sederhana (misalnya API REST).
4. Akses API dari client lokal.

---

### **Langkah 2 – Deploy ke Heroku (PaaS)**

1. Install Heroku CLI.

   ```bash
   curl https://cli-assets.heroku.com/install.sh | sh
   ```
2. Login & buat app baru.

   ```bash
   heroku login
   heroku create my-integrated-app
   ```
3. Deploy aplikasi Node.js:

   ```bash
   git init
   git add .
   git commit -m "first deploy"
   git push heroku main
   ```
4. Akses API di `https://my-integrated-app.herokuapp.com`.

---

### **Langkah 3 – Hybrid Cloud Integration**

1. Siapkan database **on-premise** (contoh: MySQL di lokal).
2. Deploy web API di Heroku.
3. Hubungkan API di Heroku → database on-premise via koneksi VPN atau tunnel.
4. Uji aplikasi client dapat membaca data dari on-prem DB lewat API cloud.

---

### **Langkah 4 – Integrasi dengan Cloud API**

1. Gunakan **Google Maps API** atau **Firebase** sebagai layanan tambahan.
2. Tambahkan endpoint API di aplikasi cloud yang mengambil data dari layanan Google/Firebase.
3. Uji integrasi: data lokal + data cloud = satu aplikasi.

---

## Hasil yang Diharapkan

* Aplikasi berhasil dideploy ke VPS/Heroku.
* Mahasiswa mampu menghubungkan layanan on-premise dengan cloud (hybrid).
* API aplikasi dapat memanfaatkan layanan cloud eksternal.

---

## Pertanyaan Diskusi

1. Apa kelebihan dan kekurangan menggunakan **VPS vs PaaS** (contoh Heroku)?
2. Apa tantangan terbesar dalam menghubungkan **on-premise system** dengan cloud API?
3. Bagaimana cara menjaga keamanan komunikasi dalam hybrid cloud integration?

---

## Referensi

1. Erl, T. (2013). *Cloud Computing: Concepts, Technology & Architecture*. Prentice Hall.
2. Amazon Web Services Documentation – [https://aws.amazon.com/documentation/](https://aws.amazon.com/documentation/)
3. Heroku Dev Center – [https://devcenter.heroku.com](https://devcenter.heroku.com)
4. Google Cloud Docs – [https://cloud.google.com/docs](https://cloud.google.com/docs)
5. DigitalOcean Tutorials – [https://www.digitalocean.com/community/tutorials](https://www.digitalocean.com/community/tutorials)

---

