# Curl Commands

Berikut contoh command yang mewakili request yang diuji pada demo app lokal.

## Users

### 1. GET before POST
```bash
curl -i -X GET "http://127.0.0.1:8088/api/users"
```

### 2. POST create user
```bash
curl -i -X POST "http://127.0.0.1:8088/api/users" \
  -H "Content-Type: application/json" \
  -d '{
    "username": "data.student",
    "email": "data.student@example.edu",
    "role": "student",
    "status": "active",
    "department": "Sains Data"
  }'
```

### 3. GET after POST
```bash
curl -i -X GET "http://127.0.0.1:8088/api/users"
```

## Orders

### 1. GET before POST
```bash
curl -i -X GET "http://127.0.0.1:8088/api/orders"
```

### 2. POST create order
```bash
curl -i -X POST "http://127.0.0.1:8088/api/orders" \
  -H "Content-Type: application/json" \
  -d '{
    "orderId": "ORD-UTS-003",
    "customer": "Student Mart",
    "amount": 325000,
    "status": "created",
    "items": ["api-lab-kit"],
    "channel": "postman"
  }'
```

### 3. GET after POST
```bash
curl -i -X GET "http://127.0.0.1:8088/api/orders"
```
