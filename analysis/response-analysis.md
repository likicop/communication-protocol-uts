# Response Analysis

## 1. Endpoint `/api/users`

### GET before POST
Request `GET /api/users` menghasilkan **200 OK** dan menampilkan daftar user yang sudah tersedia pada resource users.
Response body menggunakan format **JSON** dan berisi metadata resource serta array data user.

### POST
Request `POST /api/users` digunakan untuk menambahkan user baru dengan field:
- `username`
- `email`
- `role`
- `status`
- `department`

Response menampilkan **201 Created**, yang berarti server berhasil membuat resource baru.
Server juga menambahkan field seperti `id` dan `createdAt` pada object yang dikembalikan.

### GET after POST
Request GET setelah POST digunakan untuk memverifikasi bahwa data user baru sudah tersimpan.
Hasil response menunjukkan daftar user bertambah sehingga proses create tervalidasi.

---

## 2. Endpoint `/api/orders`

### GET before POST
Request `GET /api/orders` menghasilkan **200 OK** dan menampilkan daftar order yang tersedia.
Response menggunakan **JSON** dan memuat metadata resource serta data order.

### POST
Request `POST /api/orders` digunakan untuk menambahkan order baru dengan field:
- `orderId`
- `customer`
- `amount`
- `status`
- `items`
- `channel`

Response menampilkan **201 Created** dan object order baru.
Pada endpoint ini juga terlihat adanya header **Location** yang menunjuk ke resource baru.

### GET after POST
Request GET setelah POST dipakai untuk membuktikan bahwa order baru benar-benar masuk ke collection.
Hasil akhir menunjukkan perubahan data sebelum dan sesudah POST.

---

## 3. Makna Status Code
- **200 OK** → request berhasil diproses untuk pembacaan data.
- **201 Created** → resource baru berhasil dibuat oleh server.

## 4. Makna Method
- **GET** → membaca resource tanpa mengubah data.
- **POST** → mengirim data untuk membuat resource baru.
