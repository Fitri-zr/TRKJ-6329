# Modul Praktikum 2 – IoT Integration (ESP32/ESP8266 + MQTT + Node-RED)

## Tujuan Pembelajaran

1. Mahasiswa memahami arsitektur komunikasi IoT berbasis **MQTT**.
2. Mahasiswa dapat menghubungkan **ESP32/ESP8266** dengan **MQTT Broker**.
3. Mahasiswa mampu menampilkan data sensor ke **dashboard Node-RED**.
4. Mahasiswa memahami integrasi sensor IoT dengan layanan **Cloud IoT (ThingsBoard/Firebase)**.

---

## Bahan Kajian

* Konsep Internet of Things (IoT).
* Protokol komunikasi IoT (MQTT vs HTTP).
* MQTT Broker (Mosquitto, HiveMQ).
* Dashboard monitoring dengan Node-RED.
* Cloud IoT Platforms (Firebase / ThingsBoard).

---

## Alat & Bahan

* Hardware: ESP32 atau ESP8266, sensor (misalnya DHT11/DHT22).
* Software: Arduino IDE / PlatformIO, Node-RED, Mosquitto MQTT Broker.
* Laptop + Internet.
* Akun cloud IoT (Firebase atau ThingsBoard).

---

## Langkah Praktikum

### **Langkah 1 – Setup MQTT Broker**

1. Instal Mosquitto di server/laptop:

   ```bash
   sudo apt update
   sudo apt install mosquitto mosquitto-clients
   sudo systemctl enable mosquitto
   sudo systemctl start mosquitto
   ```
2. Uji coba publish-subscribe:

   ```bash
   # Terminal 1 (subscriber)
   mosquitto_sub -t "test/topic"
   # Terminal 2 (publisher)
   mosquitto_pub -t "test/topic" -m "Hello IoT"
   ```

---

### **Langkah 2 – Program ESP32/ESP8266**

1. Buka **Arduino IDE**, install library `PubSubClient` dan `DHT`.
2. Contoh kode ESP32 publish data sensor DHT11 ke MQTT:

   ```cpp
   #include <WiFi.h>
   #include <PubSubClient.h>
   #include "DHT.h"

   const char* ssid = "WIFI_SSID";
   const char* password = "WIFI_PASS";
   const char* mqtt_server = "broker_ip";

   #define DHTPIN 4
   #define DHTTYPE DHT11

   WiFiClient espClient;
   PubSubClient client(espClient);
   DHT dht(DHTPIN, DHTTYPE);

   void setup() {
     Serial.begin(115200);
     WiFi.begin(ssid, password);
     while (WiFi.status() != WL_CONNECTED) delay(500);
     client.setServer(mqtt_server, 1883);
     dht.begin();
   }

   void loop() {
     if (!client.connected()) reconnect();
     client.loop();

     float t = dht.readTemperature();
     float h = dht.readHumidity();

     String payload = "{\"temp\": " + String(t) + ", \"hum\": " + String(h) + "}";
     client.publish("iot/sensor", payload.c_str());
     delay(5000);
   }
   ```

---

### **Langkah 3 – Dashboard Node-RED**

1. Instal Node-RED:

   ```bash
   sudo npm install -g --unsafe-perm node-red
   node-red
   ```
2. Buka Node-RED di browser (`http://localhost:1880`).
3. Tambahkan node MQTT in (subscribe ke `iot/sensor`).
4. Tambahkan node dashboard (chart & gauge) untuk menampilkan data suhu & kelembapan.

---

### **Langkah 4 – Integrasi ke Cloud (Opsional)**

* **Firebase Realtime Database**: kirim data sensor dari ESP32 langsung ke Firebase.
* **ThingsBoard**: koneksi ESP32 → MQTT → ThingsBoard dashboard → visualisasi data.

---

## Hasil yang Diharapkan

* ESP32/ESP8266 berhasil mengirim data sensor via MQTT.
* Node-RED menampilkan data sensor dalam bentuk grafik/dashboard.
* Data sensor dapat tersimpan di platform cloud IoT.

---

## Pertanyaan Diskusi

1. Mengapa MQTT lebih efisien dibanding HTTP dalam IoT?
2. Bagaimana cara mengamankan komunikasi MQTT dengan TLS?
3. Apa perbedaan integrasi menggunakan Node-RED dan Cloud IoT platform?

---

## Referensi

1. MQTT.org – [https://mqtt.org](https://mqtt.org)
2. Node-RED Documentation – [https://nodered.org/docs](https://nodered.org/docs)
3. Firebase Realtime Database – [https://firebase.google.com](https://firebase.google.com)
4. ThingsBoard IoT Platform – [https://thingsboard.io](https://thingsboard.io)
5. Simon Monk (2017). *Programming Arduino: Getting Started with Sketches*. McGraw-Hill.

---
