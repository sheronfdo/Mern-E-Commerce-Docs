

This document provides a comprehensive set of APIs for the buyer panel of an e-commerce system specializing in used car parts. It includes all necessary endpoints to support features such as product search and filtering, cart management, order and checkout processes, payment integration, buyer profile management, communication with sellers, and reviews and returns. Each endpoint is detailed with HTTP methods, paths, headers, query parameters, request bodies, and example responses.

**Note**: All endpoints, except for authentication-related ones, require a valid buyer JWT token in the `Authorization` header for authentication.

---

## 1. Authentication APIs

### 1.1 Register
- **Endpoint**: `POST /api/auth/register`
- **Description**: Register a new buyer account.
- **Request Body**:
  ```json
  {
    "email": "user@example.com",
    "password": "******"
  }
  ```
- **Response** (201 Created):
  ```json
  {
    "success": true,
    "message": "Registration successful",
    "data": {
      "userId": "123",
      "email": "user@example.com"
    }
  }
  ```

### 1.2 Login
- **Endpoint**: `POST /api/auth/login`
- **Description**: Log in to an existing buyer account.
- **Request Body**:
  ```json
  {
    "email": "user@example.com",
    "password": "******"
  }
  ```
- **Response** (200 OK):
  ```json
  {
    "success": true,
    "data": {
      "userId": "123",
      "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
    }
  }
  ```

---

## 2. Product Search & Filtering APIs

### 2.1 Search Products
- **Endpoint**: `GET /api/products/search`
- **Description**: Search for products using keywords and filters.
- **Headers**:
  - `Authorization: Bearer <BUYER_JWT>`
- **Query Parameters**:
  - `keyword` (optional): e.g., `brake pads`
  - `vehicleMake` (optional): e.g., `Toyota`
  - `vehicleModel` (optional): e.g., `Camry`
  - `year` (optional): e.g., `2015`
  - `priceMin` (optional): e.g., `50`
  - `priceMax` (optional): e.g., `200`
  - `condition` (optional): e.g., `New` or `Used`
  - `brand` (optional): e.g., `Bosch`
  - `oem` (optional): e.g., `OEM123`
  - `availability` (optional): e.g., `in_stock`
  - `material` (optional): e.g., `Ceramic`
  - `location` (optional): e.g., `New York`
- **Response** (200 OK):
  ```json
  {
    "success": true,
    "data": [
      {
        "productId": "456",
        "title": "Brake Pads for Toyota Camry 2015",
        "price": 75,
        "seller": "AutoParts Pro",
        "condition": "New"
      }
    ]
  }
  ```

### 2.2 Get Product Details
- **Endpoint**: `GET /api/products/:productId`
- **Description**: Fetch detailed information about a specific product.
- **Headers**:
  - `Authorization: Bearer <BUYER_JWT>`
- **Path Parameters**:
  - `productId` (required): Product ID
- **Response** (200 OK):
  ```json
  {
    "success": true,
    "data": {
      "productId": "456",
      "title": "Brake Pads for Toyota Camry 2015",
      "description": "High-performance brake pads...",
      "price": 75,
      "sellerId": "789",
      "compatibility": ["Toyota Camry 2015"],
      "condition": "New",
      "brand": "Bosch",
      "stock": 10,
      "images": ["https://example.com/image1.jpg"],
      "reviews": [
        {
          "reviewId": "101",
          "rating": 5,
          "comment": "Great product!"
        }
      ]
    }
  }
  ```

### 2.3 Get Filter Options
- **Endpoint**: `GET /api/products/filters`
- **Description**: Fetch available filter options for product search.
- **Headers**:
  - `Authorization: Bearer <BUYER_JWT>`
- **Response** (200 OK):
  ```json
  {
    "success": true,
    "data": {
      "brands": ["Bosch", "NGK"],
      "conditions": ["New", "Used"],
      "priceRanges": ["0-50", "50-100"]
    }
  }
  ```

---

## 3. Cart Management APIs

### 3.1 Add to Cart
- **Endpoint**: `POST /api/cart/add`
- **Description**: Add a product to the buyer's cart.
- **Headers**:
  - `Authorization: Bearer <BUYER_JWT>`
- **Request Body**:
  ```json
  {
    "productId": "456",
    "quantity": 2
  }
  ```
- **Response** (200 OK):
  ```json
  {
    "success": true,
    "data": {
      "cartId": "789",
      "items": [
        {
          "productId": "456",
          "quantity": 2,
          "price": 75
        }
      ]
    }
  }
  ```

### 3.2 Remove from Cart
- **Endpoint**: `DELETE /api/cart/remove/:productId`
- **Description**: Remove a product from the buyer's cart.
- **Headers**:
  - `Authorization: Bearer <BUYER_JWT>`
- **Path Parameters**:
  - `productId` (required): Product ID
- **Response** (200 OK):
  ```json
  {
    "success": true,
    "message": "Item removed from cart"
  }
  ```

### 3.3 Get Cart
- **Endpoint**: `GET /api/cart`
- **Description**: Fetch the buyer's current cart items.
- **Headers**:
  - `Authorization: Bearer <BUYER_JWT>`
- **Response** (200 OK):
  ```json
  {
    "success": true,
    "data": {
      "items": [
        {
          "productId": "456",
          "title": "Brake Pads",
          "quantity": 2,
          "price": 75
        }
      ],
      "total": 150
    }
  }
  ```

---

## 4. Order & Checkout APIs

### 4.1 Create Order
- **Endpoint**: `POST /api/orders/create`
- **Description**: Create a new order from the cart.
- **Headers**:
  - `Authorization: Bearer <BUYER_JWT>`
- **Request Body**:
  ```json
  {
    "shippingAddressId": "101",
    "paymentMethod": "stripe"
  }
  ```
- **Response** (201 Created):
  ```json
  {
    "success": true,
    "data": {
      "orderId": "202",
      "status": "processing",
      "total": 150
    }
  }
  ```

### 4.2 Get Order History
- **Endpoint**: `GET /api/orders/history`
- **Description**: Fetch the buyer's order history.
- **Headers**:
  - `Authorization: Bearer <BUYER_JWT>`
- **Response** (200 OK):
  ```json
  {
    "success": true,
    "data": [
      {
        "orderId": "202",
        "date": "2024-04-20",
        "total": 150,
        "status": "delivered"
      }
    ]
  }
  ```

### 4.3 Track Order
- **Endpoint**: `GET /api/orders/:orderId/tracking`
- **Description**: Track the status of a specific order.
- **Headers**:
  - `Authorization: Bearer <BUYER_JWT>`
- **Path Parameters**:
  - `orderId` (required): Order ID
- **Response** (200 OK):
  ```json
  {
    "success": true,
    "data": {
      "status": "shipped",
      "trackingNumber": "UPS12345",
      "estimatedDelivery": "2024-04-25"
    }
  }
  ```

---

## 5. Payment APIs

### 5.1 Create Payment Intent
- **Endpoint**: `POST /api/payment/create-intent`
- **Description**: Create a payment intent for Stripe or PayPal.
- **Headers**:
  - `Authorization: Bearer <BUYER_JWT>`
- **Request Body**:
  ```json
  {
    "amount": 200,
    "orderId": "202"
  }
  ```
- **Response** (200 OK):
  ```json
  {
    "success": true,
    "data": {
      "clientSecret": "pi_3P1b9x2eZvKYlo2C0..."
    }
  }
  ```

---

## 6. Profile Management APIs

### 6.1 Get Profile
- **Endpoint**: `GET /api/buyer/profile`
- **Description**: Fetch the buyer's profile, including purchase history.
- **Headers**:
  - `Authorization: Bearer <BUYER_JWT>`
- **Response** (200 OK):
  ```json
  {
    "success": true,
    "data": {
      "userId": "123",
      "email": "user@example.com",
      "name": "John Doe",
      "phone": "+1234567890",
      "purchaseHistory": [
        {
          "orderId": "202",
          "date": "2024-04-20",
          "total": 150,
          "status": "delivered"
        }
      ]
    }
  }
  ```

### 6.2 Update Profile
- **Endpoint**: `PUT /api/buyer/profile`
- **Description**: Update the buyer's profile information.
- **Headers**:
  - `Authorization: Bearer <BUYER_JWT>`
- **Request Body**:
  ```json
  {
    "name": "John Doe",
    "phone": "+1234567890"
  }
  ```
- **Response** (200 OK):
  ```json
  {
    "success": true,
    "data": {
      "name": "John Doe",
      "phone": "+1234567890"
    }
  }
  ```

### 6.3 Add Address
- **Endpoint**: `POST /api/buyer/address`
- **Description**: Add a new shipping or billing address.
- **Headers**:
  - `Authorization: Bearer <BUYER_JWT>`
- **Request Body**:
  ```json
  {
    "street": "123 Main St",
    "city": "New York",
    "state": "NY",
    "zip": "10001",
    "country": "USA",
    "addressType": "shipping"
  }
  ```
- **Response** (201 Created):
  ```json
  {
    "success": true,
    "data": {
      "addressId": "303",
      "street": "123 Main St",
      "city": "New York",
      "state": "NY",
      "zip": "10001",
      "country": "USA",
      "addressType": "shipping"
    }
  }
  ```

---

## 7. Communication APIs

### 7.1 Send Message
- **Endpoint**: `POST /api/messages/send`
- **Description**: Send a message to a seller.
- **Headers**:
  - `Authorization: Bearer <BUYER_JWT>`
- **Request Body**:
  ```json
  {
    "sellerId": "789",
    "message": "Is this part in stock?"
  }
  ```
- **Response** (200 OK):
  ```json
  {
    "success": true,
    "data": {
      "messageId": "404",
      "message": "Is this part in stock?"
    }
  }
  ```

### 7.2 Get Messages
- **Endpoint**: `GET /api/messages`
- **Description**: Fetch the buyer's chat history with sellers.
- **Headers**:
  - `Authorization: Bearer <BUYER_JWT>`
- **Query Parameters**:
  - `sellerId` (optional): e.g., `789`
  - `productId` (optional): e.g., `456`
- **Response** (200 OK):
  ```json
  {
    "success": true,
    "data": [
      {
        "messageId": "404",
        "sellerId": "789",
        "message": "Is this part in stock?",
        "timestamp": "2024-04-20T10:00:00Z"
      }
    ]
  }
  ```

---

## 8. Reviews & Returns APIs

### 8.1 Submit Review
- **Endpoint**: `POST /api/reviews`
- **Description**: Submit a review for a product.
- **Headers**:
  - `Authorization: Bearer <BUYER_JWT>`
- **Request Body**:
  ```json
  {
    "productId": "456",
    "rating": 5,
    "comment": "Great product!"
  }
  ```
- **Response** (201 Created):
  ```json
  {
    "success": true,
    "data": {
      "reviewId": "505",
      "rating": 5
    }
  }
  ```

### 8.2 Request Return
- **Endpoint**: `POST /api/returns/request`
- **Description**: Initiate a return request for an order.
- **Headers**:
  - `Authorization: Bearer <BUYER_JWT>`
- **Request Body**:
  ```json
  {
    "orderId": "202",
    "reason": "Defective item"
  }
  ```
- **Response** (201 Created):
  ```json
  {
    "success": true,
    "data": {
      "returnId": "606",
      "status": "pending"
    }
  }
  ```

---

## General Notes

- **Authentication**: All endpoints (except `/api/auth/register` and `/api/auth/login`) require a valid JWT token in the `Authorization` header as `Bearer <BUYER_JWT>`.
- **Error Handling**: Errors are returned in the following format:
  ```json
  {
    "success": false,
    "error": "Invalid credentials",
    "message": "Email or password is incorrect"
  }
  ```

This documentation covers all required endpoints and parameters for the buyer panel, ensuring full support for an e-commerce system tailored to used car parts. Each API is designed to provide a seamless experience for buyers, from searching for products to managing their purchases and interacting with sellers.