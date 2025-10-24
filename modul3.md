
# Modul Praktikum 3 - Middleware & Message Queue

---

## Tujuan Pembelajaran

1. Mahasiswa memahami konsep middleware dan message broker.
2. Mahasiswa mampu melakukan instalasi dan konfigurasi **RabbitMQ** atau **Mosquitto MQTT**.
3. Mahasiswa dapat membuat aplikasi **client-server** yang berkomunikasi melalui message broker.
4. Mahasiswa memahami peran message queue dalam **sistem terintegrasi skala besar**.

---

## Bahan Kajian

* Konsep **Middleware** dalam integrasi sistem.
* Message Queue (MQ), Publish/Subscribe model.
* Arsitektur **RabbitMQ** dan **Mosquitto MQTT**.
* Client library (Python / Node.js).

---

## Alat & Bahan

* Laptop/server dengan Linux (Ubuntu/Debian).
* RabbitMQ / Mosquitto MQTT Broker.
* Python 3 dengan library `pika` (untuk RabbitMQ) atau `paho-mqtt` (untuk MQTT).
* Node.js (opsional).

---

## Langkah Praktikum

### **Langkah 1 – Instalasi RabbitMQ**

1. Tambahkan repo & instal:

   ```bash
   sudo apt update
   sudo apt install rabbitmq-server -y
   sudo systemctl enable rabbitmq-server
   sudo systemctl start rabbitmq-server
   ```
2. Cek status:

   ```bash
   sudo systemctl status rabbitmq-server
   ```
3. Aktifkan management plugin (opsional):

   ```bash
   sudo rabbitmq-plugins enable rabbitmq_management
   ```

   Akses di browser: `http://localhost:15672` (user: guest / pass: guest).

---

### **Langkah 2 – Program Publisher (Python + RabbitMQ)**

```python
import pika

connection = pika.BlockingConnection(pika.ConnectionParameters('localhost'))
channel = connection.channel()

channel.queue_declare(queue='hello')

channel.basic_publish(exchange='',
                      routing_key='hello',
                      body='Hello Message Queue!')
print(" [x] Sent 'Hello Message Queue!'")
connection.close()
```

---

### **Langkah 3 – Program Consumer (Python + RabbitMQ)**

```python
import pika

connection = pika.BlockingConnection(pika.ConnectionParameters('localhost'))
channel = connection.channel()

channel.queue_declare(queue='hello')

def callback(ch, method, properties, body):
    print(" [x] Received %r" % body)

channel.basic_consume(queue='hello',
                      on_message_callback=callback,
                      auto_ack=True)

print(' [*] Waiting for messages. To exit press CTRL+C')
channel.start_consuming()
```

---

### **Langkah 4 – Alternatif dengan Mosquitto (MQTT)**

1. Jalankan broker Mosquitto.
2. Buat publisher:

   ```python
   import paho.mqtt.client as mqtt

   client = mqtt.Client()
   client.connect("localhost", 1883, 60)
   client.publish("test/mq", "Hello via MQTT!")
   client.disconnect()
   ```
3. Buat subscriber:

   ```python
   import paho.mqtt.client as mqtt

   def on_message(client, userdata, msg):
       print(f"Topic: {msg.topic}, Message: {msg.payload.decode()}")

   client = mqtt.Client()
   client.on_message = on_message
   client.connect("localhost", 1883, 60)
   client.subscribe("test/mq")
   client.loop_forever()
   ```

---

## Hasil yang Diharapkan

* Mahasiswa berhasil menjalankan **publisher dan consumer** melalui RabbitMQ atau MQTT.
* Pesan dapat dikirimkan dan diterima melalui **message broker**.
* Mahasiswa memahami perbedaan model **queue** vs **publish/subscribe**.

---

## Pertanyaan Diskusi

1. Apa perbedaan mendasar antara **RabbitMQ** dan **Mosquitto MQTT**?
2. Mengapa message broker penting dalam sistem terdistribusi?
3. Bagaimana cara mengamankan komunikasi message queue?

---

## Referensi

1. RabbitMQ Documentation – [https://www.rabbitmq.com](https://www.rabbitmq.com)
2. MQTT Essentials – [https://mqtt.org](https://mqtt.org)
3. Al S. (2017). *RabbitMQ Essentials*. Packt Publishing.
4. Gupta, A. (2021). *Mastering MQTT: Building IoT and Messaging Applications*. BPB Publications.

---
