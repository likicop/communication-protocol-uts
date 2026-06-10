# Traffic Analysis

## Ringkasan
Analisis traffic dilakukan menggunakan **Wireshark** pada host lokal `127.0.0.1:8088`.
Screenshot Wireshark pada repository ini digunakan sebagai **sampling capture** untuk request GET dan POST,
sehingga tidak menampilkan seluruh packet, tetapi cukup untuk menunjukkan alur komunikasi HTTP di atas TCP.

## 1. Sampling GET `/api/users`
- Request line terlihat sebagai `GET /api/users HTTP/1.1`
- Response line terlihat sebagai `HTTP/1.0 200 OK`
- Header yang tampak antara lain `Host`, `Accept`, `Referer`, dan `User-Agent`
- Response menampilkan `Content-Type: application/json; charset=utf-8`
- JSON response dapat dibaca langsung melalui **Follow TCP Stream**

## 2. Sampling POST `/api/orders`
- Request line terlihat sebagai `POST /api/orders HTTP/1.1`
- Header request menunjukkan `Content-Type: application/json`
- Request body berisi payload order baru dalam format JSON
- Response line menampilkan `HTTP/1.0 201 Created`
- Header response menampilkan `Location: /api/orders/3`

## 3. Encoding Data
Encoding utama yang digunakan pada praktikum ini adalah **JSON**.
Format JSON dipilih karena mudah dibaca, ringan, dan cocok untuk komunikasi REST API.

## 4. Layer Protocol yang Terlihat
- **TCP** → menangani pengiriman data antar client dan server
- **HTTP** → menampilkan request dan response pada layer aplikasi
- **JSON payload** → menjadi isi utama body request maupun response

## 5. Kesimpulan Traffic
Traffic capture memperlihatkan bahwa request dari aplikasi benar-benar dikirim ke server lokal,
diproses, dan dikembalikan dalam bentuk response HTTP. Dengan demikian, hasil dari demo app dan
hasil from Wireshark saling mendukung sebagai evidence praktikum.
