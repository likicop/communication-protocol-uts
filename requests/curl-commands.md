# Curl Commands — WebSocket UTS

**Base URL:** http://localhost:8088  
**WebSocket URL:** ws://localhost:8088/ws/echo?channel={channel}

---

## GET Request #1 — Events Channel Dashboard

```bash
curl -i -X GET "http://localhost:8088/api/realtime/events?channel=dashboard"
```

**Response:**
```json
HTTP/1.0 200 OK
Content-Type: application/json

{
  "count": 86,
  "channel": "dashboard",
  "events": [
    {
      "id": 1780836338760,
      "channel": "dashboard",
      "event": "client_keepalive",
      "payload": { "event": "client_keepalive", "sentAt": "..." },
      "publishedAt": "2026-06-07T19:45:38+0700",
      "transport": "websocket"
    }
  ]
}
```

---

## GET Request #2 — Events Channel Orders

```bash
curl -i -X GET "http://localhost:8088/api/realtime/events?channel=orders"
```

**Response:**
```json
HTTP/1.0 200 OK

{
  "count": 4,
  "channel": "orders",
  "events": [
    {
      "id": 1780837656501,
      "event": "order_created",
      "payload": {
        "orderId": "ORD-2026-001",
        "amount": 450000,
        "priority": "normal"
      },
      "transport": "http-post-event"
    }
  ]
}
```

---

## POST Request #1 — Event Dashboard Refresh

```bash
curl -i -X POST "http://localhost:8088/api/realtime/events" \
  -H "Content-Type: application/json" \
  -d '{"channel":"dashboard","event":"dashboard_refresh","payload":{"event":"dashboard_refresh","value":1}}'
```

**Response:**
```json
HTTP/1.0 201 Created

{
  "id": 1780837053370,
  "channel": "dashboard",
  "event": "dashboard_refresh",
  "payload": { "event": "dashboard_refresh", "value": 1 },
  "publishedAt": "2026-06-07T19:57:33+0700",
  "transport": "http-post-event"
}
```

---

## POST Request #2 — Event Order Created

```bash
curl -i -X POST "http://localhost:8088/api/realtime/events" \
  -H "Content-Type: application/json" \
  -d '{"channel":"orders","event":"order_created","payload":{"event":"order_created","orderId":"ORD-2026-001","amount":450000,"priority":"normal"}}'
```

**Response:**
```json
HTTP/1.0 201 Created

{
  "id": 1780837656501,
  "channel": "orders",
  "event": "order_created",
  "payload": {
    "event": "order_created",
    "orderId": "ORD-2026-001",
    "amount": 450000,
    "priority": "normal"
  },
  "publishedAt": "2026-06-07T20:07:36+0700",
  "transport": "http-post-event"
}
```

---

## WebSocket — Via Demo App Browser

WebSocket tidak bisa diakses langsung via curl karena membutuhkan handshake upgrade.
Interaksi dilakukan via browser di http://localhost:8088 → P3 WebSocket.

**Endpoint:** `ws://localhost:8088/ws/echo?channel=dashboard`

**Log Connect:**
```
Connected to ws://localhost:8088/ws/echo?channel=dashboard
Heartbeat active every 15 seconds.
HEARTBEAT {"event":"client_keepalive","sentAt":"2026-06-07T12:56:36.574Z"}
RECV {"type":"echo","receivedAt":"...","channel":"dashboard","message":"..."}
```

**Log Send Message:**
```
SEND { "event": "dashboard_refresh", "value": 1 }
RECV {"type":"echo","receivedAt":"2026-06-07T19:57:04+0700","channel":"dashboard","message":"{...}"}
```

**Log Close:**
```
Closed.
Not connected.
```
