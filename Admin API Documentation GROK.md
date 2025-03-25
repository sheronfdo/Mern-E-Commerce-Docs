Below is the fully complete API documentation for the admin panel of an e-commerce system for used car parts. This documentation includes all the previously discussed endpoints, incorporates missing endpoints identified in the requirements, and ensures that all required parameters (including previously missing ones) are detailed. Each endpoint is organized by category for clarity, with comprehensive descriptions, methods, paths, headers, query parameters, request bodies (where applicable), and example responses.

---



All endpoints require a valid admin JWT token in the `Authorization` header for authentication.

---

### **1. User Management APIs**

#### **1.1 Seller Approval**

- **GET `/api/admin/sellers/pending`**  
  **Description**: Fetch pending seller applications.  
  **Headers**:  
  - `Authorization: Bearer <ADMIN_JWT>`  

  **Query Parameters**:  
  - `status=pending` (default, optional)  

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

- **PUT `/api/admin/sellers/:id`**  
  **Description**: Approve or reject a seller.  
  **Headers**:  
  - `Authorization: Bearer <ADMIN_JWT>`  

  **Path Parameters**:  
  - `id`: Seller ID (required)  

  **Request Body**:  
  ```json
  {
    "status": "approved",
    "reason": "Documents verified"
  }
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

- **DELETE `/api/admin/sellers/:id`**  
  **Description**: Remove a seller.  
  **Headers**:  
  - `Authorization: Bearer <ADMIN_JWT>`  

  **Path Parameters**:  
  - `id`: Seller ID (required)  

  **Example Response (200 OK)**:  
  ```json
  {
    "success": true,
    "message": "Seller removed successfully"
  }
  ```

---

#### **1.2 User Moderation**

- **GET `/api/admin/users`**  
  **Description**: List all users (buyers and sellers).  
  **Headers**:  
  - `Authorization: Bearer <ADMIN_JWT>`  

  **Query Parameters**:  
  - `role=seller` (optional, filters by role: "seller" or "buyer")  
  - `search=email@example.com` (optional, searches by email)  

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

- **DELETE `/api/admin/users/:id`**  
  **Description**: Suspend or delete a user.  
  **Headers**:  
  - `Authorization: Bearer <ADMIN_JWT>`  

  **Path Parameters**:  
  - `id`: User ID (required)  

  **Example Response (200 OK)**:  
  ```json
  {
    "success": true,
    "message": "User suspended successfully"
  }
  ```

---

- **GET `/api/admin/users/logs`**  
  **Description**: View user activity logs.  
  **Headers**:  
  - `Authorization: Bearer <ADMIN_JWT>`  

  **Query Parameters**:  
  - `userId=123` (required, specifies the user)  
  - `action=login` (optional, filters by action type)  

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

### **2. Product Moderation APIs**

#### **2.1 Product Management**

- **GET `/api/admin/products`**  
  **Description**: List all products.  
  **Headers**:  
  - `Authorization: Bearer <ADMIN_JWT>`  

  **Query Parameters**:  
  - `status=flagged` (optional, filters by status)  
  - `sellerId=456` (optional, filters by seller)  

  **Example Response (200 OK)**:  
  ```json
  {
    "success": true,
    "data": [
      {
        "productId": "789",
        "name": "Brake Pads",
        "status": "flagged",
        "sellerId": "456",
        "category": "Braking System",
        "price": 50,
        "stock": 10
      }
    ]
  }
  ```

---

- **DELETE `/api/admin/products/:id`**  
  **Description**: Remove a product.  
  **Headers**:  
  - `Authorization: Bearer <ADMIN_JWT>`  

  **Path Parameters**:  
  - `id`: Product ID (required)  

  **Example Response (200 OK)**:  
  ```json
  {
    "success": true,
    "message": "Product removed successfully"
  }
  ```

---

- **PATCH `/api/admin/products/:id`**  
  **Description**: Update product status.  
  **Headers**:  
  - `Authorization: Bearer <ADMIN_JWT>`  

  **Path Parameters**:  
  - `id`: Product ID (required)  

  **Request Body**:  
  ```json
  { "status": "out_of_stock" }
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

#### **2.2 Category Management**

- **POST `/api/admin/categories`**  
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

- **PUT `/api/admin/categories/:id`**  
  **Description**: Edit a category.  
  **Headers**:  
  - `Authorization: Bearer <ADMIN_JWT>`  

  **Path Parameters**:  
  - `id`: Category ID (required)  

  **Request Body**:  
  ```json
  { "name": "Braking System" }
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

- **DELETE `/api/admin/categories/:id`**  
  **Description**: Delete a category.  
  **Headers**:  
  - `Authorization: Bearer <ADMIN_JWT>`  

  **Path Parameters**:  
  - `id`: Category ID (required)  

  **Example Response (200 OK)**:  
  ```json
  {
    "success": true,
    "message": "Category deleted successfully"
  }
  ```

---

### **3. Complaint & Dispute APIs**

- **GET `/api/admin/complaints`**  
  **Description**: List all complaints.  
  **Headers**:  
  - `Authorization: Bearer <ADMIN_JWT>`  

  **Query Parameters**:  
  - `status=open` (optional, filters by status)  

  **Example Response (200 OK)**:  
  ```json
  {
    "success": true,
    "data": [
      {
        "complaintId": "202",
        "description": "Wrong product delivered",
        "status": "open",
        "buyerId": "123",
        "sellerId": "456",
        "orderId": "789",
        "timestamp": "2024-04-20T10:00:00Z"
      }
    ]
  }
  ```

---

- **POST `/api/admin/complaints/:id/chat`**  
  **Description**: Send a message in the complaint chat.  
  **Headers**:  
  - `Authorization: Bearer <ADMIN_JWT>`  

  **Path Parameters**:  
  - `id`: Complaint ID (required)  

  **Request Body**:  
  ```json
  {
    "message": "Please provide order details",
    "sender": "admin"
  }
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

- **PUT `/api/admin/complaints/:id`**  
  **Description**: Resolve a complaint.  
  **Headers**:  
  - `Authorization: Bearer <ADMIN_JWT>`  

  **Path Parameters**:  
  - `id`: Complaint ID (required)  

  **Request Body**:  
  ```json
  { "status": "resolved" }
  ```

  **Example Response (200 OK)**:  
  ```json
  {
    "success": true,
    "message": "Complaint resolved successfully"
  }
  ```

---

### **4. Order & Delivery APIs**

- **GET `/api/admin/orders`**  
  **Description**: List all orders.  
  **Headers**:  
  - `Authorization: Bearer <ADMIN_JWT>`  

  **Query Parameters**:  
  - `status=delivered` (optional, filters by status)  
  - `courierRegion=North` (optional, filters by courier region)  

  **Example Response (200 OK)**:  
  ```json
  {
    "success": true,
    "data": [
      {
        "orderId": "404",
        "status": "delivered",
        "total": 200.00,
        "buyerId": "123",
        "sellerId": "456",
        "items": [
          {
            "productId": "789",
            "quantity": 2,
            "price": 100.00
          }
        ]
      }
    ]
  }
  ```

---

- **DELETE `/api/admin/orders/:id`**  
  **Description**: Cancel an order.  
  **Headers**:  
  - `Authorization: Bearer <ADMIN_JWT>`  

  **Path Parameters**:  
  - `id`: Order ID (required)  

  **Example Response (200 OK)**:  
  ```json
  {
    "success": true,
    "message": "Order canceled successfully"
  }
  ```

---

- **POST `/api/admin/orders/refund`**  
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

  **Example Response (200 OK)**:  
  ```json
  {
    "success": true,
    "message": "Refund initiated successfully"
  }
  ```

---

### **5. Analytics & Reports APIs**

- **GET `/api/admin/analytics/sales`**  
  **Description**: Fetch sales data.  
  **Headers**:  
  - `Authorization: Bearer <ADMIN_JWT>`  

  **Query Parameters**:  
  - `period=weekly` (required, options: "daily", "weekly", "monthly")  
  - `category=Engine` (optional, filters by category)  

  **Example Response (200 OK)**:  
  ```json
  {
    "success": true,
    "data": {
      "period": "2024-04-15 to 2024-04-21",
      "totalSales": 5000.00,
      "itemsSold": 100,
      "commissionEarned": 500.00,
      "categoryBreakdown": { "Engine": 3000.00, "Brakes": 2000.00 }
    }
  }
  ```

---

- **GET `/api/admin/analytics/top-sellers`**  
  **Description**: Get top-performing sellers.  
  **Headers**:  
  - `Authorization: Bearer <ADMIN_JWT>`  

  **Query Parameters**:  
  - `limit=10` (optional, limits the number of results)  

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

- **GET `/api/admin/analytics/products`**  
  **Description**: Fetch analytics about listed products.  
  **Headers**:  
  - `Authorization: Bearer <ADMIN_JWT>`  

  **Query Parameters**:  
  - `status=active` (optional, filters by status)  

  **Example Response (200 OK)**:  
  ```json
  {
    "success": true,
    "data": {
      "totalProducts": 1000,
      "activeProducts": 800,
      "outOfStock": 200
    }
  }
  ```

---

- **GET `/api/admin/analytics/commission`**  
  **Description**: Fetch commission earned analytics.  
  **Headers**:  
  - `Authorization: Bearer <ADMIN_JWT>`  

  **Query Parameters**:  
  - `period=monthly` (required, options: "daily", "weekly", "monthly")  

  **Example Response (200 OK)**:  
  ```json
  {
    "success": true,
    "data": {
      "period": "April 2024",
      "totalCommission": 10000.00
    }
  }
  ```

---

- **POST `/api/admin/reports/generate`**  
  **Description**: Generate a custom report.  
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

### **6. Financial Management APIs**

- **PUT `/api/admin/commission`**  
  **Description**: Update commission rate.  
  **Headers**:  
  - `Authorization: Bearer <ADMIN_JWT>`  

  **Request Body**:  
  ```json
  { "rate": 5 }
  ```

  **Example Response (200 OK)**:  
  ```json
  {
    "success": true,
    "message": "Commission rate updated to 5%"
  }
  ```

---

- **POST `/api/admin/payouts`**  
  **Description**: Initiate a seller payout.  
  **Headers**:  
  - `Authorization: Bearer <ADMIN_JWT>`  

  **Request Body**:  
  ```json
  {
    "sellerId": "123",
    "amount": 500.00
  }
  ```

  **Example Response (200 OK)**:  
  ```json
  {
    "success": true,
    "message": "Payout initiated successfully"
  }
  ```

---

- **GET `/api/admin/payouts/history`**  
  **Description**: View payout history.  
  **Headers**:  
  - `Authorization: Bearer <ADMIN_JWT>`  

  **Query Parameters**:  
  - `sellerId=123` (optional, filters by seller)  

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

### **7. System Configuration APIs**

- **PUT `/api/admin/payment/config`**  
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

  **Example Response (200 OK)**:  
  ```json
  {
    "success": true,
    "message": "Payment gateway updated successfully"
  }
  ```

---

- **PUT `/api/admin/search/filters`**  
  **Description**: Add or remove search filters.  
  **Headers**:  
  - `Authorization: Bearer <ADMIN_JWT>`  

  **Request Body**:  
  ```json
  { "action": "add", "filter": "OEM/Aftermarket" }
  ```

  **Example Response (200 OK)**:  
  ```json
  {
    "success": true,
    "message": "Filter 'OEM/Aftermarket' added successfully"
  }
  ```

---

### **Notes**

- **Authentication**: All endpoints require a valid admin JWT token in the `Authorization` header.  
- **Error Handling**: Error responses follow this format:  
  ```json
  {
    "success": false,
    "error": "Unauthorized",
    "message": "Invalid token"
  }
  ```

---

This documentation provides a comprehensive set of APIs for the admin panel, covering user management, product moderation, complaint and dispute resolution, order and delivery oversight, analytics and reporting, financial management, and system configuration. All required endpoints, including previously missing ones (e.g., `GET /api/admin/analytics/products` and `GET /api/admin/analytics/commission`), are included, along with all necessary parameters and enhanced response fields (e.g., `category`, `price`, and `stock` in `GET /api/admin/products`; `buyerId`, `sellerId`, `orderId`, and `timestamp` in `GET /api/admin/complaints`). This ensures the admin panel meets all specified requirements for managing an e-commerce system for used car parts.