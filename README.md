# UTS Communication Protocol — WebSocket Real-Time Communication

**Nama:** Rizky Dwi Saputra  
**NIM:** 25120500017  
**Kelas:** ComP2  
**Case:** Case 8 — WebSocket Echo Protocol  
**Demo App:** Communication Protocols Demo App (localhost:8088)  

---

## Deskripsi Case

Praktikum ini menggunakan **WebSocket Echo** pada P3 WebSocket dari Communication Protocols Demo App. WebSocket memungkinkan komunikasi real-time dua arah (full-duplex) antara client dan server.

**Endpoint WebSocket:**
```
ws://localhost:8088/ws/echo?channel={channel}
```

**Endpoint HTTP (REST):**
```
POST /api/realtime/events
GET  /api/realtime/events?channel={channel}
```

---

## Struktur Repository

```
communication-protocol-uts/
├── README.md
├── requests/
│   └── curl-commands.md          # Semua command curl
├── evidence/
│   ├── wireshark-start.png       # Wireshark capture aktif
│   ├── ws-connect-dashboard.png  # WebSocket connect channel dashboard
│   ├── ws-send-message.png       # Send message & echo response
│   ├── post-01-dashboard.png     # POST Event dashboard (HTTP 201)
│   ├── get-01-dashboard.png      # GET Events dashboard (HTTP 200)
│   ├── wireshark-packets.png     # WebSocket packets di Wireshark
│   ├── ws-tcp-stream.png         # TCP Stream analysis
│   ├── ws-close-dashboard.png    # WebSocket close dashboard
│   ├── ws-connect-orders.png     # WebSocket connect channel orders
│   ├── post-02-orders.png        # POST Event orders (HTTP 201)
│   ├── get-02-orders.png         # GET Events orders (HTTP 200)
│   └── ws-close-orders.png       # WebSocket close orders
├── analysis/
│   └── encoding-analysis.md      # Analisis JSON encoding & traffic
└── report/
    ├── UTS_CommProtocol_RizkyDwiSaputra_25120500017.pdf
    └── UTS_CommProtocol_RizkyDwiSaputra_25120500017.pptx
```

---

## Ringkasan Interaksi (4 Request Setara)

| # | Tipe | Aksi | Status |
|---|------|------|--------|
| 1 | GET | GET /api/realtime/events?channel=dashboard | 200 OK |
| 2 | GET | GET /api/realtime/events?channel=orders | 200 OK |
| 3 | POST | POST /api/realtime/events (dashboard_refresh) | 201 Created |
| 4 | POST | POST /api/realtime/events (order_created) | 201 Created |
| + | WS | Connect → Send → Close (dashboard & orders) | 101 → Echo |

---

## Tools yang Digunakan

- **Browser** — Communication Protocols Demo App (localhost:8088)
- **Wireshark** — Traffic capture, filter `tcp.port == 8088`, TCP Stream analysis
- **MacOS** — MacBook Air (Apple Silicon)

---

## Temuan Utama

1. WebSocket handshake menggunakan HTTP GET dengan header `Upgrade: websocket`
2. Server merespons dengan `HTTP 101 Switching Protocols`
3. Heartbeat Ping/Pong setiap 15 detik mempertahankan koneksi
4. Format JSON digunakan untuk semua payload
5. Field `transport` membedakan event dari WebSocket vs HTTP POST
