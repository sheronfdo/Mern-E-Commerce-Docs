Below is the fully complete API documentation for the seller panel of an e-commerce system for used car parts. This documentation includes all discussed endpoints, incorporates missing parameters identified in the requirements, and ensures that all required fields are detailed. Each endpoint is organized by category for clarity, with comprehensive descriptions, methods, paths, headers, query parameters, request bodies (where applicable), and example responses.

---



All endpoints require a valid seller JWT token in the `Authorization` header for authentication.

---

## **1. Product Management APIs**

### **1.1 Product Listing**

- **POST `/api/seller/products`**  
  **Description**: Create a new product listing.  
  **Headers**:  
  - `Authorization: Bearer <SELLER_JWT>`  

  **Request Body**:  
  ```json
  {
    "title": "Brake Pads",
    "description": "High-performance brake pads for Toyota Camry 2015",
    "price": 50,
    "category": "Braking System",
    "stock": 10,
    "condition": "New",
    "brand": "Bosch",
    "oem": "OEM",
    "material": "Ceramic",
    "compatibility": ["Toyota Camry 2015"],
    "images": ["https://example.com/image1.jpg", "https://example.com/image2.jpg"]
  }
  ```

  **Example Response (201 Created)**:  
  ```json
  {
    "success": true,
    "data": {
      "productId": "789",
      "title": "Brake Pads",
      "description": "High-performance brake pads for Toyota Camry 2015",
      "price": 50,
      "category": "Braking System",
      "stock": 10,
      "condition": "New",
      "brand": "Bosch",
      "oem": "OEM",
      "material": "Ceramic",
      "compatibility": ["Toyota Camry 2015"],
      "images": ["https://example.com/image1.jpg", "https://example.com/image2.jpg"],
      "status": "active"
    }
  }
  ```

---

- **GET `/api/seller/products`**  
  **Description**: List all products listed by the seller.  
  **Headers**:  
  - `Authorization: Bearer <SELLER_JWT>`  

  **Query Parameters**:  
  - `status=active` (optional, filters by status)  

  **Example Response (200 OK)**:  
  ```json
  {
    "success": true,
    "data": [
      {
        "productId": "789",
        "title": "Brake Pads",
        "price": 50,
        "stock": 10,
        "status": "active"
      }
    ]
  }
  ```

---

- **PUT `/api/seller/products/:id`**  
  **Description**: Update product details.  
  **Headers**:  
  - `Authorization: Bearer <SELLER_JWT>`  

  **Path Parameters**:  
  - `id`: Product ID (required)  

  **Request Body**:  
  ```json
  {
    "title": "Updated Brake Pads",
    "description": "Updated description",
    "price": 45,
    "category": "Braking System",
    "stock": 5,
    "condition": "New",
    "brand": "Bosch",
    "oem": "OEM",
    "material": "Ceramic",
    "compatibility": ["Toyota Camry 2015", "Toyota Camry 2016"],
    "images": ["https://example.com/image1.jpg"]
  }
  ```

  **Example Response (200 OK)**:  
  ```json
  {
    "success": true,
    "data": {
      "productId": "789",
      "title": "Updated Brake Pads",
      "description": "Updated description",
      "price": 45,
      "category": "Braking System",
      "stock": 5,
      "condition": "New",
      "brand": "Bosch",
      "oem": "OEM",
      "material": "Ceramic",
      "compatibility": ["Toyota Camry 2015", "Toyota Camry 2016"],
      "images": ["https://example.com/image1.jpg"]
    }
  }
  ```

---

- **DELETE `/api/seller/products/:id`**  
  **Description**: Remove a product.  
  **Headers**:  
  - `Authorization: Bearer <SELLER_JWT>`  

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

### **1.2 Product Boost (Optional)**

- **POST `/api/seller/products/:id/boost`**  
  **Description**: Boost a product listing for increased visibility.  
  **Headers**:  
  - `Authorization: Bearer <SELLER_JWT>`  

  **Path Parameters**:  
  - `id`: Product ID (required)  

  **Request Body**:  
  ```json
  {
    "duration": 7,
    "budget": 20
  }
  ```

  **Example Response (200 OK)**:  
  ```json
  {
    "success": true,
    "message": "Product boosted for 7 days",
    "data": {
      "productId": "789",
      "boostStatus": "active"
    }
  }
  ```

---

### **1.3 Inventory Management**

- **PATCH `/api/seller/products/:id/stock`**  
  **Description**: Update product stock quantity.  
  **Headers**:  
  - `Authorization: Bearer <SELLER_JWT>`  

  **Path Parameters**:  
  - `id`: Product ID (required)  

  **Request Body**:  
  ```json
  {
    "stock": 15
  }
  ```

  **Example Response (200 OK)**:  
  ```json
  {
    "success": true,
    "data": {
      "productId": "789",
      "stock": 15
    }
  }
  ```

---

## **2. Order Management APIs**

### **2.1 Order Tracking**

- **GET `/api/seller/orders`**  
  **Description**: List all orders placed with the seller.  
  **Headers**:  
  - `Authorization: Bearer <SELLER_JWT>`  

  **Query Parameters**:  
  - `status=processing` (optional, filters by status)  

  **Example Response (200 OK)**:  
  ```json
  {
    "success": true,
    "data": [
      {
        "orderId": "456",
        "status": "processing",
        "total": 100.00,
        "buyerName": "John Doe",
        "shippingAddress": {
          "street": "123 Main St",
          "city": "New York",
          "zip": "10001"
        },
        "items": [
          {
            "productId": "789",
            "quantity": 2,
            "price": 50.00
          }
        ]
      }
    ]
  }
  ```

---

- **PUT `/api/seller/orders/:id`**  
  **Description**: Update the status of an order.  
  **Headers**:  
  - `Authorization: Bearer <SELLER_JWT>`  

  **Path Parameters**:  
  - `id`: Order ID (required)  

  **Request Body**:  
  ```json
  {
    "status": "shipped",
    "trackingNumber": "UPS12345"
  }
  ```

  **Example Response (200 OK)**:  
  ```json
  {
    "success": true,
    "data": {
      "orderId": "456",
      "status": "shipped"
    }
  }
  ```

---

### **2.2 Courier Assignment**

- **POST `/api/seller/orders/:id/assign-courier`**  
  **Description**: Assign an order to a courier team or specific courier.  
  **Headers**:  
  - `Authorization: Bearer <SELLER_JWT>`  

  **Path Parameters**:  
  - `id`: Order ID (required)  

  **Request Body**:  
  ```json
  {
    "courierRegion": "North",
    "courierId": "789"  // Optional, for direct assignment
  }
  ```

  **Example Response (200 OK)**:  
  ```json
  {
    "success": true,
    "message": "Order assigned to courier 789 in North region",
    "data": {
      "orderId": "456",
      "courierRegion": "North",
      "courierId": "789"
    }
  }
  ```

---

## **3. Analytics & Reports APIs**

- **GET `/api/seller/analytics/sales`**  
  **Description**: Fetch sales data for a specified period.  
  **Headers**:  
  - `Authorization: Bearer <SELLER_JWT>`  

  **Query Parameters**:  
  - `period=weekly` (required, options: "daily", "weekly", "monthly")  

  **Example Response (200 OK)**:  
  ```json
  {
    "success": true,
    "data": {
      "period": "2024-04-15 to 2024-04-21",
      "totalSales": 1500.00,
      "topProducts": [
        {
          "productId": "789",
          "sales": 20
        }
      ]
    }
  }
  ```

---

- **GET `/api/seller/analytics/earnings`**  
  **Description**: Fetch earnings breakdown for a specified period.  
  **Headers**:  
  - `Authorization: Bearer <SELLER_JWT>`  

  **Query Parameters**:  
  - `period=monthly` (required, options: "daily", "weekly", "monthly")  

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

- **GET `/api/seller/analytics/top`**  
  **Description**: List top-selling products.  
  **Headers**:  
  - `Authorization: Bearer <SELLER_JWT>`  

  **Query Parameters**:  
  - `limit=5` (optional, limits the number of results)  

  **Example Response (200 OK)**:  
  ```json
  {
    "success": true,
    "data": [
      {
        "productId": "789",
        "title": "Brake Pads",
        "sales": 50
      },
      {
        "productId": "101",
        "title": "Engine Oil",
        "sales": 30
      }
    ]
  }
  ```

---

## **4. Communication APIs**

- **GET `/api/seller/messages`**  
  **Description**: List inquiries from buyers.  
  **Headers**:  
  - `Authorization: Bearer <SELLER_JWT>`  

  **Query Parameters**:  
  - `productId=789` (optional, filters by product)  
  - `buyerId=123` (optional, filters by buyer)  

  **Example Response (200 OK)**:  
  ```json
  {
    "success": true,
    "data": [
      {
        "messageId": "202",
        "productId": "789",
        "buyerId": "123",
        "question": "Is this compatible with a 2019 model?",
        "timestamp": "2024-04-20T10:00:00Z"
      }
    ]
  }
  ```

---

- **POST `/api/seller/messages/:id/reply`**  
  **Description**: Reply to a buyer inquiry.  
  **Headers**:  
  - `Authorization: Bearer <SELLER_JWT>`  

  **Path Parameters**:  
  - `id`: Message ID (required)  

  **Request Body**:  
  ```json
  {
    "message": "This part is in stock. Delivery takes 3 days."
  }
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

- **GET `/api/seller/returns`**  
  **Description**: List return requests from buyers.  
  **Headers**:  
  - `Authorization: Bearer <SELLER_JWT>`  

  **Query Parameters**:  
  - `status=pending` (optional, filters by status)  

  **Example Response (200 OK)**:  
  ```json
  {
    "success": true,
    "data": [
      {
        "returnId": "303",
        "orderId": "456",
        "status": "pending",
        "reason": "Defective item",
        "buyerName": "John Doe",
        "productId": "789"
      }
    ]
  }
  ```

---

- **PUT `/api/seller/returns/:id`**  
  **Description**: Approve or reject a return request.  
  **Headers**:  
  - `Authorization: Bearer <SELLER_JWT>`  

  **Path Parameters**:  
  - `id`: Return ID (required)  

  **Request Body**:  
  ```json
  {
    "status": "approved"
  }
  ```

  **Example Response (200 OK)**:  
  ```json
  {
    "success": true,
    "message": "Return request approved"
  }
  ```

---

- **POST `/api/seller/refunds`**  
  **Description**: Initiate a refund for an order.  
  **Headers**:  
  - `Authorization: Bearer <SELLER_JWT>`  

  **Request Body**:  
  ```json
  {
    "orderId": "456",
    "amount": 50
  }
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

- **GET `/api/seller/profile`**  
  **Description**: Fetch the seller's profile information.  
  **Headers**:  
  - `Authorization: Bearer <SELLER_JWT>`  

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

- **PUT `/api/seller/profile`**  
  **Description**: Update the seller's profile information.  
  **Headers**:  
  - `Authorization: Bearer <SELLER_JWT>`  

  **Request Body**:  
  ```json
  {
    "storeName": "AutoParts Pro",
    "phone": "+1234567890"
  }
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

- **GET `/api/seller/payouts`**  
  **Description**: View the seller's payout history.  
  **Headers**:  
  - `Authorization: Bearer <SELLER_JWT>`  

  **Query Parameters**:  
  - `status=completed` (optional, filters by status)  

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

## **Notes**

- **Authentication**: All endpoints require a valid seller JWT token passed in the `Authorization` header as `Bearer <SELLER_JWT>`.  
- **Error Handling**: Error responses follow this standard format:  
  ```json
  {
    "success": false,
    "error": "Unauthorized",
    "message": "Invalid token"
  }
  ```

---

This documentation provides a comprehensive set of APIs for the seller panel, covering product management, order management, analytics, communication, returns and refunds, and profile settings. All required endpoints and parameters are included to ensure the seller panel meets the needs of an e-commerce system for used car parts.