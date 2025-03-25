
---

## **1. Product Management APIs**

### **1.1 Product Listing**

#### **POST `/api/seller/products`**  
**Description**: Create a new product listing.  
**Headers**:  
  - `Authorization: Bearer <SELLER_JWT>`  

**Request Body**:  
```json
{
  "title": "Brake Pads",
  "price": 50,
  "category": "Braking System",
  "stock": 10,
  "condition": "New",
  "compatibility": ["Toyota Corolla 2018"]
}
```

**Example Request**:  
```http
POST /api/seller/products
Headers: { "Authorization": "Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." }
Body: { "title": "Brake Pads", "price": 50, "category": "Braking System", "stock": 10 }
```

**Example Response (201 Created)**:  
```json
{
  "success": true,
  "data": {
    "productId": "789",
    "title": "Brake Pads",
    "price": 50,
    "stock": 10,
    "status": "active"
  }
}
```

---

#### **GET `/api/seller/products`**  
**Description**: List all products listed by the seller.  
**Headers**:  
  - `Authorization: Bearer <SELLER_JWT>`  

**Query Parameters**:  
  - `status=active` (optional)  

**Example Request**:  
```http
GET /api/seller/products?status=active
Headers: { "Authorization": "Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." }
```

**Example Response (200 OK)**:  
```json
{
  "success": true,
  "data": [
    {
      "productId": "789",
      "title": "Brake Pads",
      "price": 50,
      "stock": 10
    }
  ]
}
```

---

#### **PUT `/api/seller/products/:id`**  
**Description**: Update product details.  
**Headers**:  
  - `Authorization: Bearer <SELLER_JWT>`  

**Path Parameters**:  
  - `id`: Product ID  

**Request Body**:  
```json
{ "price": 45, "stock": 5 }
```

**Example Request**:  
```http
PUT /api/seller/products/789
Headers: { "Authorization": "Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." }
Body: { "price": 45, "stock": 5 }
```

**Example Response (200 OK)**:  
```json
{
  "success": true,
  "data": {
    "productId": "789",
    "price": 45,
    "stock": 5
  }
}
```

---

#### **DELETE `/api/seller/products/:id`**  
**Description**: Remove a product.  
**Headers**:  
  - `Authorization: Bearer <SELLER_JWT>`  

**Path Parameters**:  
  - `id`: Product ID  

**Example Request**:  
```http
DELETE /api/seller/products/789
Headers: { "Authorization": "Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." }
```

**Example Response (200 OK)**:  
```json
{
  "success": true,
  "message": "Product removed successfully"
}
```

---

### **1.2 Product Boost (Optional)**

#### **POST `/api/seller/products/:id/boost`**  
**Description**: Boost a product listing.  
**Headers**:  
  - `Authorization: Bearer <SELLER_JWT>`  

**Path Parameters**:  
  - `id`: Product ID  

**Request Body**:  
```json
{ "duration": 7, "budget": 20 }
```

**Example Request**:  
```http
POST /api/seller/products/789/boost
Headers: { "Authorization": "Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." }
Body: { "duration": 7, "budget": 20 }
```

**Example Response (200 OK)**:  
```json
{
  "success": true,
  "message": "Product boosted for 7 days",
  "data": { "productId": "789", "boostStatus": "active" }
}
```

---

### **1.3 Inventory Management**

#### **PATCH `/api/seller/products/:id/stock`**  
**Description**: Update product stock.  
**Headers**:  
  - `Authorization: Bearer <SELLER_JWT>`  

**Path Parameters**:  
  - `id`: Product ID  

**Request Body**:  
```json
{ "stock": 15 }
```

**Example Request**:  
```http
PATCH /api/seller/products/789/stock
Headers: { "Authorization": "Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." }
Body: { "stock": 15 }
```

**Example Response (200 OK)**:  
```json
{
  "success": true,
  "data": { "productId": "789", "stock": 15 }
}
```

---

## **2. Order Management APIs**

### **2.1 Order Tracking**

#### **GET `/api/seller/orders`**  
**Description**: List all seller orders.  
**Headers**:  
  - `Authorization: Bearer <SELLER_JWT>`  

**Query Parameters**:  
  - `status=processing` (optional)  

**Example Request**:  
```http
GET /api/seller/orders?status=processing
Headers: { "Authorization": "Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." }
```

**Example Response (200 OK)**:  
```json
{
  "success": true,
  "data": [
    {
      "orderId": "456",
      "status": "processing",
      "total": 100.00
    }
  ]
}
```

---

#### **PUT `/api/seller/orders/:id`**  
**Description**: Update order status.  
**Headers**:  
  - `Authorization: Bearer <SELLER_JWT>`  

**Path Parameters**:  
  - `id`: Order ID  

**Request Body**:  
```json
{ "status": "shipped", "trackingNumber": "UPS12345" }
```

**Example Request**:  
```http
PUT /api/seller/orders/456
Headers: { "Authorization": "Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." }
Body: { "status": "shipped", "trackingNumber": "UPS12345" }
```

**Example Response (200 OK)**:  
```json
{
  "success": true,
  "data": { "orderId": "456", "status": "shipped" }
}
```

---

### **2.2 Courier Assignment**

#### **POST `/api/seller/orders/:id/assign-courier`**  
**Description**: Assign order to a courier team.  
**Headers**:  
  - `Authorization: Bearer <SELLER_JWT>`  

**Path Parameters**:  
  - `id`: Order ID  

**Request Body**:  
```json
{ "courierRegion": "North" }
```

**Example Request**:  
```http
POST /api/seller/orders/456/assign-courier
Headers: { "Authorization": "Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." }
Body: { "courierRegion": "North" }
```

**Example Response (200 OK)**:  
```json
{
  "success": true,
  "message": "Order assigned to North courier team",
  "data": { "orderId": "456", "courierRegion": "North" }
}
```

---

## **3. Analytics & Reports APIs**

#### **GET `/api/seller/analytics/sales`**  
**Description**: Fetch sales data.  
**Headers**:  
  - `Authorization: Bearer <SELLER_JWT>`  

**Query Parameters**:  
  - `period=weekly` (required)  

**Example Request**:  
```http
GET /api/seller/analytics/sales?period=weekly
Headers: { "Authorization": "Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." }
```

**Example Response (200 OK)**:  
```json
{
  "success": true,
  "data": {
    "period": "2024-04-15 to 2024-04-21",
    "totalSales": 1500.00,
    "topProducts": [{ "productId": "789", "sales": 20 }]
  }
}
```

---

#### **GET `/api/seller/analytics/earnings`**  
**Description**: Fetch earnings breakdown.  
**Headers**:  
  - `Authorization: Bearer <SELLER_JWT>`  

**Query Parameters**:  
  - `period=monthly` (required)  

**Example Request**:  
```http
GET /api/seller/analytics/earnings?period=monthly
Headers: { "Authorization": "Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." }
```

**Example Response (200 OK)**:  
```json
{
  "success": true,
  "data": {
    "period": "April 2024",
    "grossEarnings": 5000.00,
    "netEarnings": 4750.00
  }
}
```

---

#### **GET `/api/seller/analytics/top`**  
**Description**: List top-selling products.  
**Headers**:  
  - `Authorization: Bearer <SELLER_JWT>`  

**Query Parameters**:  
  - `limit=5` (optional)  

**Example Request**:  
```http
GET /api/seller/analytics/top?limit=5
Headers: { "Authorization": "Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." }
```

**Example Response (200 OK)**:  
```json
{
  "success": true,
  "data": [
    { "productId": "789", "title": "Brake Pads", "sales": 50 },
    { "productId": "101", "title": "Engine Oil", "sales": 30 }
  ]
}
```

---

## **4. Communication APIs**

#### **GET `/api/seller/messages`**  
**Description**: List buyer inquiries.  
**Headers**:  
  - `Authorization: Bearer <SELLER_JWT>`  

**Query Parameters**:  
  - `productId=789` (optional)  

**Example Request**:  
```http
GET /api/seller/messages?productId=789
Headers: { "Authorization": "Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." }
```

**Example Response (200 OK)**:  
```json
{
  "success": true,
  "data": [
    {
      "messageId": "202",
      "productId": "789",
      "question": "Is this compatible with a 2019 model?"
    }
  ]
}
```

---

#### **POST `/api/seller/messages/:id/reply`**  
**Description**: Reply to a buyer inquiry.  
**Headers**:  
  - `Authorization: Bearer <SELLER_JWT>`  

**Path Parameters**:  
  - `id`: Message ID  

**Request Body**:  
```json
{ "message": "This part is in stock. Delivery takes 3 days." }
```

**Example Request**:  
```http
POST /api/seller/messages/202/reply
Headers: { "Authorization": "Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." }
Body: { "message": "This part is in stock. Delivery takes 3 days." }
```

**Example Response (200 OK)**:  
```json
{
  "success": true,
  "data": {
    "messageId": "202",
    "reply": "This part is in stock. Delivery takes 3 days."
  }
}
```

---

## **5. Returns & Refunds APIs**

#### **GET `/api/seller/returns`**  
**Description**: List return requests.  
**Headers**:  
  - `Authorization: Bearer <SELLER_JWT>`  

**Query Parameters**:  
  - `status=pending` (optional)  

**Example Request**:  
```http
GET /api/seller/returns?status=pending
Headers: { "Authorization": "Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." }
```

**Example Response (200 OK)**:  
```json
{
  "success": true,
  "data": [
    {
      "returnId": "303",
      "orderId": "456",
      "status": "pending"
    }
  ]
}
```

---

#### **PUT `/api/seller/returns/:id`**  
**Description**: Approve/Reject a return.  
**Headers**:  
  - `Authorization: Bearer <SELLER_JWT>`  

**Path Parameters**:  
  - `id`: Return ID  

**Request Body**:  
```json
{ "status": "approved" }
```

**Example Request**:  
```http
PUT /api/seller/returns/303
Headers: { "Authorization": "Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." }
Body: { "status": "approved" }
```

**Example Response (200 OK)**:  
```json
{
  "success": true,
  "message": "Return request approved"
}
```

---

#### **POST `/api/seller/refunds`**  
**Description**: Initiate a refund.  
**Headers**:  
  - `Authorization: Bearer <SELLER_JWT>`  

**Request Body**:  
```json
{ "orderId": "456", "amount": 50 }
```

**Example Request**:  
```http
POST /api/seller/refunds
Headers: { "Authorization": "Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." }
Body: { "orderId": "456", "amount": 50 }
```

**Example Response (200 OK)**:  
```json
{
  "success": true,
  "message": "Refund of $50 initiated for order 456"
}
```

---

## **6. Profile & Settings APIs**

#### **GET `/api/seller/profile`**  
**Description**: Fetch seller profile.  
**Headers**:  
  - `Authorization: Bearer <SELLER_JWT>`  

**Example Request**:  
```http
GET /api/seller/profile
Headers: { "Authorization": "Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." }
```

**Example Response (200 OK)**:  
```json
{
  "success": true,
  "data": {
    "storeName": "AutoParts Pro",
    "email": "seller@autoparts.com",
    "phone": "+1234567890"
  }
}
```

---

#### **PUT `/api/seller/profile`**  
**Description**: Update seller profile.  
**Headers**:  
  - `Authorization: Bearer <SELLER_JWT>`  

**Request Body**:  
```json
{ "storeName": "AutoParts Pro", "phone": "+1234567890" }
```

**Example Request**:  
```http
PUT /api/seller/profile
Headers: { "Authorization": "Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." }
Body: { "storeName": "AutoParts Pro", "phone": "+1234567890" }
```

**Example Response (200 OK)**:  
```json
{
  "success": true,
  "data": {
    "storeName": "AutoParts Pro",
    "phone": "+1234567890"
  }
}
```

---

#### **GET `/api/seller/payouts`**  
**Description**: View payout history.  
**Headers**:  
  - `Authorization: Bearer <SELLER_JWT>`  

**Query Parameters**:  
  - `status=completed` (optional)  

**Example Request**:  
```http
GET /api/seller/payouts?status=completed
Headers: { "Authorization": "Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." }
```

**Example Response (200 OK)**:  
```json
{
  "success": true,
  "data": [
    {
      "payoutId": "404",
      "amount": 1000.00,
      "status": "completed"
    }
  ]
}
```

---

**Notes**:  
- All endpoints require a valid seller JWT token in the `Authorization` header.  
- Error responses follow the format:  
  ```json
  { "success": false, "error": "Unauthorized", "message": "Invalid token" }
  ```