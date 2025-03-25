Here’s the **complete API documentation** for the admin panel, organized by category with example requests and responses:

---

## **1. User Management APIs**

### **1.1 Seller Approval**

#### **GET `/api/admin/sellers/pending`**
**Description**: Fetch pending seller applications.  
**Headers**:  
  - `Authorization: Bearer <ADMIN_JWT>`  

**Query Parameters**:  
  - `status=pending` (default)  

**Example Request**:  
```http
GET /api/admin/sellers/pending
Headers: { "Authorization": "Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." }
```

**Example Response (200 OK)**:  
```json
{
  "success": true,
  "data": [
    {
      "sellerId": "123",
      "name": "John's Auto Parts",
      "email": "john@autoparts.com",
      "documents": ["license.pdf", "tax_id.jpg"]
    }
  ]
}
```

---

#### **PUT `/api/admin/sellers/:id`**  
**Description**: Approve/Reject a seller.  
**Headers**:  
  - `Authorization: Bearer <ADMIN_JWT>`  

**Path Parameters**:  
  - `id`: Seller ID  

**Request Body**:  
```json
{
  "status": "approved",
  "reason": "Documents verified"
}
```

**Example Request**:  
```http
PUT /api/admin/sellers/123
Headers: { "Authorization": "Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." }
Body: { "status": "approved", "reason": "Documents verified" }
```

**Example Response (200 OK)**:  
```json
{
  "success": true,
  "message": "Seller approved successfully",
  "data": { "sellerId": "123", "status": "approved" }
}
```

---

#### **DELETE `/api/admin/sellers/:id`**  
**Description**: Remove a seller.  
**Headers**:  
  - `Authorization: Bearer <ADMIN_JWT>`  

**Path Parameters**:  
  - `id`: Seller ID  

**Example Request**:  
```http
DELETE /api/admin/sellers/123
Headers: { "Authorization": "Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." }
```

**Example Response (200 OK)**:  
```json
{
  "success": true,
  "message": "Seller removed successfully"
}
```

---

### **1.2 User Moderation**

#### **GET `/api/admin/users`**  
**Description**: List all users (buyers/sellers).  
**Headers**:  
  - `Authorization: Bearer <ADMIN_JWT>`  

**Query Parameters**:  
  - `role=seller` (optional)  
  - `search=email@example.com` (optional)  

**Example Request**:  
```http
GET /api/admin/users?role=seller
Headers: { "Authorization": "Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." }
```

**Example Response (200 OK)**:  
```json
{
  "success": true,
  "data": [
    {
      "userId": "456",
      "role": "seller",
      "email": "john@autoparts.com",
      "status": "active"
    }
  ]
}
```

---

#### **DELETE `/api/admin/users/:id`**  
**Description**: Suspend/Delete a user.  
**Headers**:  
  - `Authorization: Bearer <ADMIN_JWT>`  

**Path Parameters**:  
  - `id`: User ID  

**Example Request**:  
```http
DELETE /api/admin/users/456
Headers: { "Authorization": "Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." }
```

**Example Response (200 OK)**:  
```json
{
  "success": true,
  "message": "User suspended successfully"
}
```

---

#### **GET `/api/admin/users/logs`**  
**Description**: View user activity logs.  
**Headers**:  
  - `Authorization: Bearer <ADMIN_JWT>`  

**Query Parameters**:  
  - `userId=123`  
  - `action=login` (optional)  

**Example Request**:  
```http
GET /api/admin/users/logs?userId=123&action=login
Headers: { "Authorization": "Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." }
```

**Example Response (200 OK)**:  
```json
{
  "success": true,
  "data": [
    {
      "timestamp": "2024-04-20T10:00:00Z",
      "action": "login",
      "ip": "192.168.1.1"
    }
  ]
}
```

---

## **2. Product Moderation APIs**

### **2.1 Product Management**

#### **GET `/api/admin/products`**  
**Description**: List all products.  
**Headers**:  
  - `Authorization: Bearer <ADMIN_JWT>`  

**Query Parameters**:  
  - `status=flagged` (optional)  
  - `sellerId=456` (optional)  

**Example Request**:  
```http
GET /api/admin/products?status=flagged
Headers: { "Authorization": "Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." }
```

**Example Response (200 OK)**:  
```json
{
  "success": true,
  "data": [
    {
      "productId": "789",
      "name": "Brake Pads",
      "status": "flagged",
      "sellerId": "456"
    }
  ]
}
```

---

#### **DELETE `/api/admin/products/:id`**  
**Description**: Remove a product.  
**Headers**:  
  - `Authorization: Bearer <ADMIN_JWT>`  

**Path Parameters**:  
  - `id`: Product ID  

**Example Request**:  
```http
DELETE /api/admin/products/789
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

#### **PATCH `/api/admin/products/:id`**  
**Description**: Update product status.  
**Headers**:  
  - `Authorization: Bearer <ADMIN_JWT>`  

**Path Parameters**:  
  - `id`: Product ID  

**Request Body**:  
```json
{ "status": "out_of_stock" }
```

**Example Request**:  
```http
PATCH /api/admin/products/789
Headers: { "Authorization": "Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." }
Body: { "status": "out_of_stock" }
```

**Example Response (200 OK)**:  
```json
{
  "success": true,
  "data": {
    "productId": "789",
    "status": "out_of_stock"
  }
}
```

---

### **2.2 Category Management**

#### **POST `/api/admin/categories`**  
**Description**: Add a new category.  
**Headers**:  
  - `Authorization: Bearer <ADMIN_JWT>`  

**Request Body**:  
```json
{
  "name": "Engine & Transmission",
  "parentCategory": null
}
```

**Example Request**:  
```http
POST /api/admin/categories
Headers: { "Authorization": "Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." }
Body: { "name": "Engine & Transmission", "parentCategory": null }
```

**Example Response (201 Created)**:  
```json
{
  "success": true,
  "data": {
    "categoryId": "101",
    "name": "Engine & Transmission"
  }
}
```

---

#### **PUT `/api/admin/categories/:id`**  
**Description**: Edit a category.  
**Headers**:  
  - `Authorization: Bearer <ADMIN_JWT>`  

**Path Parameters**:  
  - `id`: Category ID  

**Request Body**:  
```json
{ "name": "Braking System" }
```

**Example Request**:  
```http
PUT /api/admin/categories/101
Headers: { "Authorization": "Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." }
Body: { "name": "Braking System" }
```

**Example Response (200 OK)**:  
```json
{
  "success": true,
  "data": {
    "categoryId": "101",
    "name": "Braking System"
  }
}
```

---

#### **DELETE `/api/admin/categories/:id`**  
**Description**: Delete a category.  
**Headers**:  
  - `Authorization: Bearer <ADMIN_JWT>`  

**Path Parameters**:  
  - `id`: Category ID  

**Example Request**:  
```http
DELETE /api/admin/categories/101
Headers: { "Authorization": "Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." }
```

**Example Response (200 OK)**:  
```json
{
  "success": true,
  "message": "Category deleted successfully"
}
```

---

## **3. Complaint & Dispute APIs**

#### **GET `/api/admin/complaints`**  
**Description**: List all complaints.  
**Headers**:  
  - `Authorization: Bearer <ADMIN_JWT>`  

**Query Parameters**:  
  - `status=open` (optional)  

**Example Request**:  
```http
GET /api/admin/complaints?status=open
Headers: { "Authorization": "Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." }
```

**Example Response (200 OK)**:  
```json
{
  "success": true,
  "data": [
    {
      "complaintId": "202",
      "description": "Wrong product delivered",
      "status": "open"
    }
  ]
}
```

---

#### **POST `/api/admin/complaints/:id/chat`**  
**Description**: Send message in complaint chat.  
**Headers**:  
  - `Authorization: Bearer <ADMIN_JWT>`  

**Path Parameters**:  
  - `id`: Complaint ID  

**Request Body**:  
```json
{
  "message": "Please provide order details",
  "sender": "admin"
}
```

**Example Request**:  
```http
POST /api/admin/complaints/202/chat
Headers: { "Authorization": "Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." }
Body: { "message": "Please provide order details", "sender": "admin" }
```

**Example Response (200 OK)**:  
```json
{
  "success": true,
  "data": {
    "chatId": "303",
    "message": "Please provide order details",
    "sender": "admin"
  }
}
```

---

#### **PUT `/api/admin/complaints/:id`**  
**Description**: Resolve a complaint.  
**Headers**:  
  - `Authorization: Bearer <ADMIN_JWT>`  

**Path Parameters**:  
  - `id`: Complaint ID  

**Request Body**:  
```json
{ "status": "resolved" }
```

**Example Request**:  
```http
PUT /api/admin/complaints/202
Headers: { "Authorization": "Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." }
Body: { "status": "resolved" }
```

**Example Response (200 OK)**:  
```json
{
  "success": true,
  "message": "Complaint resolved successfully"
}
```

---

## **4. Order & Delivery APIs**

#### **GET `/api/admin/orders`**  
**Description**: List all orders.  
**Headers**:  
  - `Authorization: Bearer <ADMIN_JWT>`  

**Query Parameters**:  
  - `status=delivered` (optional)  
  - `courierRegion=North` (optional)  

**Example Request**:  
```http
GET /api/admin/orders?status=delivered
Headers: { "Authorization": "Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." }
```

**Example Response (200 OK)**:  
```json
{
  "success": true,
  "data": [
    {
      "orderId": "404",
      "status": "delivered",
      "total": 200.00
    }
  ]
}
```

---

#### **DELETE `/api/admin/orders/:id`**  
**Description**: Cancel an order.  
**Headers**:  
  - `Authorization: Bearer <ADMIN_JWT>`  

**Path Parameters**:  
  - `id`: Order ID  

**Example Request**:  
```http
DELETE /api/admin/orders/404
Headers: { "Authorization": "Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." }
```

**Example Response (200 OK)**:  
```json
{
  "success": true,
  "message": "Order canceled successfully"
}
```

---

#### **POST `/api/admin/orders/refund`**  
**Description**: Initiate a refund.  
**Headers**:  
  - `Authorization: Bearer <ADMIN_JWT>`  

**Request Body**:  
```json
{
  "orderId": "404",
  "amount": 150.00
}
```

**Example Request**:  
```http
POST /api/admin/orders/refund
Headers: { "Authorization": "Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." }
Body: { "orderId": "404", "amount": 150.00 }
```

**Example Response (200 OK)**:  
```json
{
  "success": true,
  "message": "Refund initiated successfully"
}
```

---

## **5. Analytics & Reports APIs**

#### **GET `/api/admin/analytics/sales`**  
**Description**: Fetch sales data.  
**Headers**:  
  - `Authorization: Bearer <ADMIN_JWT>`  

**Query Parameters**:  
  - `period=weekly` (required)  
  - `category=Engine` (optional)  

**Example Request**:  
```http
GET /api/admin/analytics/sales?period=weekly
Headers: { "Authorization": "Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." }
```

**Example Response (200 OK)**:  
```json
{
  "success": true,
  "data": {
    "period": "2024-04-15 to 2024-04-21",
    "totalSales": 5000.00,
    "categoryBreakdown": { "Engine": 3000.00, "Brakes": 2000.00 }
  }
}
```

---

#### **GET `/api/admin/analytics/top-sellers`**  
**Description**: Get top-performing sellers.  
**Headers**:  
  - `Authorization: Bearer <ADMIN_JWT>`  

**Query Parameters**:  
  - `limit=10` (optional)  

**Example Request**:  
```http
GET /api/admin/analytics/top-sellers?limit=5
Headers: { "Authorization": "Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." }
```

**Example Response (200 OK)**:  
```json
{
  "success": true,
  "data": [
    { "sellerId": "123", "totalSales": 2500.00 },
    { "sellerId": "456", "totalSales": 1500.00 }
  ]
}
```

---

#### **POST `/api/admin/reports/generate`**  
**Description**: Generate custom report.  
**Headers**:  
  - `Authorization: Bearer <ADMIN_JWT>`  

**Request Body**:  
```json
{
  "type": "sales",
  "startDate": "2024-01-01",
  "endDate": "2024-04-30"
}
```

**Example Request**:  
```http
POST /api/admin/reports/generate
Headers: { "Authorization": "Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." }
Body: { "type": "sales", "startDate": "2024-01-01", "endDate": "2024-04-30" }
```

**Example Response (200 OK)**:  
```json
{
  "success": true,
  "data": {
    "reportId": "505",
    "downloadLink": "/reports/505.pdf"
  }
}
```

---

## **6. Financial Management APIs**

#### **PUT `/api/admin/commission`**  
**Description**: Update commission rate.  
**Headers**:  
  - `Authorization: Bearer <ADMIN_JWT>`  

**Request Body**:  
```json
{ "rate": 5 }
```

**Example Request**:  
```http
PUT /api/admin/commission
Headers: { "Authorization": "Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." }
Body: { "rate": 5 }
```

**Example Response (200 OK)**:  
```json
{
  "success": true,
  "message": "Commission rate updated to 5%"
}
```

---

#### **POST `/api/admin/payouts`**  
**Description**: Initiate seller payout.  
**Headers**:  
  - `Authorization: Bearer <ADMIN_JWT>`  

**Request Body**:  
```json
{
  "sellerId": "123",
  "amount": 500.00
}
```

**Example Request**:  
```http
POST /api/admin/payouts
Headers: { "Authorization": "Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." }
Body: { "sellerId": "123", "amount": 500.00 }
```

**Example Response (200 OK)**:  
```json
{
  "success": true,
  "message": "Payout initiated successfully"
}
```

---

#### **GET `/api/admin/payouts/history`**  
**Description**: View payout history.  
**Headers**:  
  - `Authorization: Bearer <ADMIN_JWT>`  

**Query Parameters**:  
  - `sellerId=123` (optional)  

**Example Request**:  
```http
GET /api/admin/payouts/history?sellerId=123
Headers: { "Authorization": "Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." }
```

**Example Response (200 OK)**:  
```json
{
  "success": true,
  "data": [
    {
      "payoutId": "606",
      "sellerId": "123",
      "amount": 500.00,
      "status": "completed"
    }
  ]
}
```

---

## **7. System Configuration APIs**

#### **PUT `/api/admin/payment/config`**  
**Description**: Update payment gateway keys.  
**Headers**:  
  - `Authorization: Bearer <ADMIN_JWT>`  

**Request Body**:  
```json
{
  "stripePublicKey": "pk_test_123",
  "stripeSecretKey": "sk_test_456"
}
```

**Example Request**:  
```http
PUT /api/admin/payment/config
Headers: { "Authorization": "Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." }
Body: { "stripePublicKey": "pk_test_123", "stripeSecretKey": "sk_test_456" }
```

**Example Response (200 OK)**:  
```json
{
  "success": true,
  "message": "Payment gateway updated successfully"
}
```

---

#### **PUT `/api/admin/search/filters`**  
**Description**: Add/Remove search filters.  
**Headers**:  
  - `Authorization: Bearer <ADMIN_JWT>`  

**Request Body**:  
```json
{ "action": "add", "filter": "OEM/Aftermarket" }
```

**Example Request**:  
```http
PUT /api/admin/search/filters
Headers: { "Authorization": "Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." }
Body: { "action": "add", "filter": "OEM/Aftermarket" }
```

**Example Response (200 OK)**:  
```json
{
  "success": true,
  "message": "Filter 'OEM/Aftermarket' added successfully"
}
```

---

**Notes**:  
- All endpoints require a valid admin JWT token in the `Authorization` header.  
- Error responses follow the format:  
  ```json
  { "success": false, "error": "Unauthorized", "message": "Invalid token" }
  ```