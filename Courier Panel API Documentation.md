
---

## **1. Order Management APIs**

### **GET `/api/courier/orders`**  
**Description**: Fetch orders assigned to the courier/team.  
**Headers**:  
  - `Authorization: Bearer <COURIER_JWT>`  

**Query Parameters**:  
  - `status=pending` (optional)  

**Example Request**:  
```http
GET /api/courier/orders?status=pending
Headers: { "Authorization": "Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." }
```

**Example Response (200 OK)**:  
```json
{
  "success": true,
  "data": [
    {
      "orderId": "123",
      "address": "123 Main St, New York",
      "status": "pending",
      "buyerName": "John Doe"
    }
  ]
}
```

---

### **PUT `/api/courier/orders/:id/status`**  
**Description**: Update delivery status.  
**Headers**:  
  - `Authorization: Bearer <COURIER_JWT>`  

**Path Parameters**:  
  - `id`: Order ID  

**Request Body**:  
```json
{
  "status": "delivered",
  "notes": "Left at front door"
}
```

**Example Request**:  
```http
PUT /api/courier/orders/123/status
Headers: { "Authorization": "Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." }
Body: { "status": "delivered", "notes": "Left at front door" }
```

**Example Response (200 OK)**:  
```json
{
  "success": true,
  "data": {
    "orderId": "123",
    "status": "delivered"
  }
}
```

---

### **POST `/api/courier/orders/:id/proof`**  
**Description**: Upload proof of delivery.  
**Headers**:  
  - `Authorization: Bearer <COURIER_JWT>`  

**Path Parameters**:  
  - `id`: Order ID  

**Request Body**:  
```json
{
  "imageUrl": "https://example.com/signature.jpg"
}
```

**Example Request**:  
```http
POST /api/courier/orders/123/proof
Headers: { "Authorization": "Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." }
Body: { "imageUrl": "https://example.com/signature.jpg" }
```

**Example Response (200 OK)**:  
```json
{
  "success": true,
  "message": "Proof of delivery uploaded"
}
```

---

### **GET `/api/courier/orders/:id`**  
**Description**: Fetch order details (address, buyer info).  
**Headers**:  
  - `Authorization: Bearer <COURIER_JWT>`  

**Path Parameters**:  
  - `id`: Order ID  

**Example Request**:  
```http
GET /api/courier/orders/123
Headers: { "Authorization": "Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." }
```

**Example Response (200 OK)**:  
```json
{
  "success": true,
  "data": {
    "orderId": "123",
    "address": "123 Main St, New York",
    "buyerName": "John Doe",
    "phone": "+1234567890"
  }
}
```

---

## **2. Earnings & Analytics APIs**

### **GET `/api/courier/analytics/earnings`**  
**Description**: Fetch earnings (daily/weekly/monthly).  
**Headers**:  
  - `Authorization: Bearer <COURIER_JWT>`  

**Query Parameters**:  
  - `period=weekly` (required)  

**Example Request**:  
```http
GET /api/courier/analytics/earnings?period=weekly
Headers: { "Authorization": "Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." }
```

**Example Response (200 OK)**:  
```json
{
  "success": true,
  "data": {
    "period": "2024-04-15 to 2024-04-21",
    "earnings": 500.00,
    "currency": "USD"
  }
}
```

---

### **GET `/api/courier/analytics/performance`**  
**Description**: Fetch performance metrics.  
**Headers**:  
  - `Authorization: Bearer <COURIER_JWT>`  

**Query Parameters**:  
  - `metric=on_time_rate` (optional)  

**Example Request**:  
```http
GET /api/courier/analytics/performance?metric=on_time_rate
Headers: { "Authorization": "Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." }
```

**Example Response (200 OK)**:  
```json
{
  "success": true,
  "data": {
    "onTimeRate": "95%",
    "completedOrders": 50,
    "avgDeliveryTime": "2h 15m"
  }
}
```

---

## **3. Team Coordination APIs**

### **POST `/api/courier/orders/assign`**  
**Description**: Assign order to a regional team/courier.  
**Headers**:  
  - `Authorization: Bearer <COURIER_JWT>`  

**Request Body**:  
```json
{
  "orderId": "123",
  "region": "North",
  "courierId": "456"
}
```

**Example Request**:  
```http
POST /api/courier/orders/assign
Headers: { "Authorization": "Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." }
Body: { "orderId": "123", "region": "North", "courierId": "456" }
```

**Example Response (200 OK)**:  
```json
{
  "success": true,
  "message": "Order 123 assigned to courier 456 in North region"
}
```

---

### **GET `/api/courier/teams`**  
**Description**: List couriers in the assigned region.  
**Headers**:  
  - `Authorization: Bearer <COURIER_JWT>`  

**Query Parameters**:  
  - `region=North` (required)  

**Example Request**:  
```http
GET /api/courier/teams?region=North
Headers: { "Authorization": "Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." }
```

**Example Response (200 OK)**:  
```json
{
  "success": true,
  "data": [
    {
      "courierId": "456",
      "name": "Alice Smith",
      "availability": "active"
    }
  ]
}
```

---

### **POST `/api/courier/teams/chat`**  
**Description**: Send internal team message.  
**Headers**:  
  - `Authorization: Bearer <COURIER_JWT>`  

**Request Body**:  
```json
{
  "message": "Urgent: Delivery delay due to traffic"
}
```

**Example Request**:  
```http
POST /api/courier/teams/chat
Headers: { "Authorization": "Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." }
Body: { "message": "Urgent: Delivery delay due to traffic" }
```

**Example Response (200 OK)**:  
```json
{
  "success": true,
  "data": {
    "messageId": "789",
    "message": "Urgent: Delivery delay due to traffic"
  }
}
```

---

## **4. Profile & Settings APIs**

### **PUT `/api/courier/profile`**  
**Description**: Update courier profile (name, contact).  
**Headers**:  
  - `Authorization: Bearer <COURIER_JWT>`  

**Request Body**:  
```json
{
  "name": "John Doe",
  "phone": "+1234567890"
}
```

**Example Request**:  
```http
PUT /api/courier/profile
Headers: { "Authorization": "Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." }
Body: { "name": "John Doe", "phone": "+1234567890" }
```

**Example Response (200 OK)**:  
```json
{
  "success": true,
  "data": {
    "name": "John Doe",
    "phone": "+1234567890"
  }
}
```

---

### **PUT `/api/courier/availability`**  
**Description**: Update availability status.  
**Headers**:  
  - `Authorization: Bearer <COURIER_JWT>`  

**Request Body**:  
```json
{
  "status": "active"
}
```

**Example Request**:  
```http
PUT /api/courier/availability
Headers: { "Authorization": "Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." }
Body: { "status": "active" }
```

**Example Response (200 OK)**:  
```json
{
  "success": true,
  "data": {
    "status": "active"
  }
}
```

---

### **Notes**:  
1. **Authentication**: All endpoints require a valid JWT token in the `Authorization` header.  
2. **Error Handling**:  
   ```json
   { "success": false, "error": "Unauthorized", "message": "Invalid token" }
   ```  
3. **Status Codes**:  
   - `200 OK` for successful GET/PUT/POST requests.  
   - `401 Unauthorized` for invalid/missing tokens.  
   - `404 Not Found` if the order/courier ID does not exist.