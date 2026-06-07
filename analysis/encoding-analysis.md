# Analisis Encoding & Traffic — WebSocket

## 1. Format Data: JSON Encoding

Semua komunikasi menggunakan **JSON (JavaScript Object Notation)** sebagai format encoding.

### Mengapa JSON?
- Ringan dan human-readable
- Native support di browser (JavaScript)
- Mudah di-parse di kedua sisi (client & server)
- Mendukung tipe data: string, number, boolean, object, array

---

## 2. Struktur JSON per Use Case

### 2.1 Event Dashboard Refresh
```json
{
  "event": "dashboard_refresh",
  "value": 1
}
```
- `event` (string): nama tipe event
- `value` (integer): nilai yang dikirim (1 = refresh trigger)

### 2.2 Event Order Created
```json
{
  "event": "order_created",
  "orderId": "ORD-2026-001",
  "amount": 450000,
  "priority": "normal"
}
```
- `orderId` (string): ID order format ORD-YYYY-NNN
- `amount` (integer): nominal dalam Rupiah
- `priority` (string): enum "normal" | "high" | "urgent"

### 2.3 Heartbeat (client_keepalive)
```json
{
  "event": "client_keepalive",
  "sentAt": "2026-06-07T12:56:36.574Z"
}
```
- `sentAt` (ISO 8601): timestamp UTC saat heartbeat dikirim

### 2.4 Echo Response dari Server
```json
{
  "type": "echo",
  "receivedAt": "2026-06-07T19:56:36+0700",
  "channel": "dashboard",
  "message": "{\"event\":\"client_keepalive\",\"sentAt\":\"2026-06-07T12:56:36.574Z\"}"
}
```
- `type` (string): selalu "echo" untuk WebSocket echo endpoint
- `receivedAt` (ISO 8601): timestamp server menerima pesan (timezone +0700/WIB)
- `message` (string): payload asli dalam bentuk escaped JSON string

---

## 3. Analisis Traffic Wireshark

### 3.1 WebSocket Handshake (TCP Stream)

**HTTP Request (Client → Server):**
```
GET /ws/echo?channel=dashboard HTTP/1.1
Host: localhost:8088
Connection: Upgrade
Pragma: no-cache
Cache-Control: no-cache
Upgrade: websocket
Sec-WebSocket-Version: 13
Sec-WebSocket-Key: H5yB7x/LGtRG3PC00ZMaHA==
Sec-WebSocket-Extensions: permessage-deflate; client_max_window_bits
```

**HTTP Response (Server → Client):**
```
HTTP/1.0 101 Switching Protocols
Server: CommProtocolsDemo/1.0 Python/3.9.6
Date: Sun, 07 Jun 2026 12:56:21 GMT
Upgrade: websocket
Connection: Upgrade
Sec-WebSocket-Accept: EeGEKeakecAcXr4c958baD7oU9s=
```

### 3.2 Packet Types yang Tertangkap

| Packet Type | Size | Keterangan |
|-------------|------|------------|
| WebSocket Text [FIN] | 229 bytes | Echo message dari server (JSON payload) |
| WebSocket Text [FIN] [MASKED] | 126 bytes | Pesan dari client (selalu masked sesuai RFC 6455) |
| WebSocket Pong [FIN] [MASKED] | 78 bytes | Keepalive response dari client |
| WebSocket Ping [FIN] | 74 bytes | Keepalive request dari server |

### 3.3 Observasi Penting

1. **Client frame selalu MASKED**: Sesuai RFC 6455, semua frame dari client ke server harus di-mask untuk keamanan
2. **Server frame tidak di-mask**: Frame dari server ke client tidak perlu di-mask
3. **Ping/Pong cycle**: Server mengirim Ping, client merespons dengan Pong — ini mekanisme keepalive layer WebSocket
4. **Total traffic**: 26 client packets + 26 server packets dalam satu sesi (33 TCP turns)
5. **Port**: Semua traffic melalui TCP port 8088 pada loopback interface (127.0.0.1)

### 3.4 Perbandingan WebSocket vs HTTP Polling

| Aspek | WebSocket | HTTP Polling |
|-------|-----------|--------------|
| Koneksi | Persistent (1 handshake) | Baru setiap request |
| Latency | Sangat rendah | Bergantung interval polling |
| Overhead | ~2-14 bytes per frame | ~200-800 bytes header HTTP |
| Server Push | Ya (native) | Tidak (client harus polling) |
| Use Case | Real-time, event streaming | Data yang tidak time-critical |
