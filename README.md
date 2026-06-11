# Communication Protocol UTS

**Nama:** RIZKY DWI SAPUTRA  
**NIM:** 25120500017  
**Case:** REST API CRUD pada endpoint `/api/users` dan `/api/orders`
**GITHUB:** https://github.com/likicop/communication-protocol-uts

## Ringkasan
Repository ini berisi evidence praktikum UTS Communication Protocol yang menguji pola request-response pada REST API.
Pengujian dilakukan pada dua endpoint utama:
- `/api/users`
- `/api/orders`

Setiap endpoint didokumentasikan dengan urutan:
1. **GET before POST** → melihat kondisi awal resource
2. **POST** → mengirim payload JSON untuk membuat data baru
3. **GET after POST** → memverifikasi perubahan data setelah POST

## Struktur Repository
```
communication-protocol-uts/
├── README.md
├── requests/
│   ├── curl-commands.md
│   └── postman-collection.json
├── evidence/
│   ├── users/
│   └── orders/
├── analysis/
│   ├── response-analysis.md
│   ├── traffic-analysis.md
│   └── wireshark/
└── report/
```

## Isi Folder
- **requests/** → contoh command curl dan collection Postman
- **evidence/** → screenshot hasil GET before POST, POST, dan GET after POST
- **analysis/** → analisis response serta analisis encoding/traffic capture
- **report/** → file laporan dan slide presentasi

## Tools yang Digunakan
- Demo App dari dosen
- Browser / HTTP request UI
- Wireshark (sampling GET dan POST)
- Microsoft Word dan PowerPoint

## Catatan
Screenshot Wireshark pada repository ini digunakan sebagai **sampling traffic capture**, bukan seluruh packet capture.
Link GitHub dapat ditambahkan pada bagian ini sebelum submit akhir.
