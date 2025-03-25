
---

## **1. Authentication APIs**

### **POST `/api/auth/register`**  
**Purpose**: Buyer registration (email/password or social login).  
**Request Body**:  
```json
{
  "email": "user@example.com",
  "password": "******"
}
```

**Example Response (201 Created)**:  
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

---

### **POST `/api/auth/login`**  
**Purpose**: Buyer login.  
**Request Body**:  
```json
{
  "email": "user@example.com",
  "password": "******"
}
```

**Example Response (200 OK)**:  
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

## **2. Product Search & Filtering APIs**

### **GET `/api/products/search`**  
**Purpose**: Keyword-based search (e.g., "Toyota Camry 2015 brake pads").  
**Query Parameters**:  
- `keyword=brake+pads`  
- `vehicleMake=Toyota`  
- `vehicleModel=Camry`  
- `year=2015`  
- `priceMin=50`  
- `priceMax=200`  

**Example Response (200 OK)**:  
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

---

### **GET `/api/products/:productId`**  
**Purpose**: Fetch product details by ID.  
**Path Parameter**: `productId=456`  

**Example Response (200 OK)**:  
```json
{
  "success": true,
  "data": {
    "productId": "456",
    "title": "Brake Pads for Toyota Camry 2015",
    "description": "High-performance brake pads...",
    "price": 75,
    "sellerId": "789",
    "compatibility": ["Toyota Camry 2015"]
  }
}
```

---

### **GET `/api/products/filters`**  
**Purpose**: Fetch available filter options.  

**Example Response (200 OK)**:  
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

## **3. Cart Management APIs**

### **POST `/api/cart/add`**  
**Purpose**: Add item to cart.  
**Request Body**:  
```json
{
  "productId": "456",
  "quantity": 2
}
```

**Example Response (200 OK)**:  
```json
{
  "success": true,
  "data": {
    "cartId": "789",
    "items": [
      { "productId": "456", "quantity": 2, "price": 75 }
    ]
  }
}
```

---

### **DELETE `/api/cart/remove/:productId`**  
**Purpose**: Remove item from cart.  
**Path Parameter**: `productId=456`  

**Example Response (200 OK)**:  
```json
{
  "success": true,
  "message": "Item removed from cart"
}
```

---

### **GET `/api/cart`**  
**Purpose**: Fetch cart items.  

**Example Response (200 OK)**:  
```json
{
  "success": true,
  "data": {
    "items": [
      { "productId": "456", "title": "Brake Pads", "quantity": 2, "price": 75 }
    ],
    "total": 150
  }
}
```

---

## **4. Order & Checkout APIs**

### **POST `/api/orders/create`**  
**Purpose**: Create an order from cart.  
**Request Body**:  
```json
{
  "shippingAddressId": "101",
  "paymentMethod": "stripe"
}
```

**Example Response (201 Created)**:  
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

---

### **GET `/api/orders/history`**  
**Purpose**: Fetch buyer’s order history.  

**Example Response (200 OK)**:  
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

---

### **GET `/api/orders/:orderId/tracking`**  
**Purpose**: Track order status.  
**Path Parameter**: `orderId=202`  

**Example Response (200 OK)**:  
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

## **5. Payment APIs**

### **POST `/api/payment/create-intent`**  
**Purpose**: Create Stripe/PayPal payment intent.  
**Request Body**:  
```json
{
  "amount": 200
}
```

**Example Response (200 OK)**:  
```json
{
  "success": true,
  "data": {
    "clientSecret": "pi_3P1b9x2eZvKYlo2C0..."
  }
}
```

---

## **6. Profile Management APIs**

### **PUT `/api/buyer/profile`**  
**Purpose**: Update buyer profile.  
**Request Body**:  
```json
{
  "name": "John Doe",
  "phone": "+1234567890"
}
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

### **POST `/api/buyer/address`**  
**Purpose**: Add a new shipping address.  
**Request Body**:  
```json
{
  "street": "123 Main St",
  "city": "New York",
  "zip": "10001"
}
```

**Example Response (201 Created)**:  
```json
{
  "success": true,
  "data": {
    "addressId": "303",
    "street": "123 Main St",
    "city": "New York"
  }
}
```

---

## **7. Communication APIs**

### **POST `/api/messages/send`**  
**Purpose**: Send message to seller.  
**Request Body**:  
```json
{
  "sellerId": "789",
  "message": "Is this part in stock?"
}
```

**Example Response (200 OK)**:  
```json
{
  "success": true,
  "data": {
    "messageId": "404",
    "message": "Is this part in stock?"
  }
}
```

---

### **GET `/api/messages`**  
**Purpose**: Fetch buyer-seller chat history.  

**Example Response (200 OK)**:  
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

## **8. Reviews & Returns APIs**

### **POST `/api/reviews`**  
**Purpose**: Submit product review.  
**Request Body**:  
```json
{
  "productId": "456",
  "rating": 5,
  "comment": "Great product!"
}
```

**Example Response (201 Created)**:  
```json
{
  "success": true,
  "data": {
    "reviewId": "505",
    "rating": 5
  }
}
```

---

### **POST `/api/returns/request`**  
**Purpose**: Initiate return request.  
**Request Body**:  
```json
{
  "orderId": "202",
  "reason": "Defective item"
}
```

**Example Response (201 Created)**:  
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

### **Notes**:
1. **Authentication**: All endpoints (except `/auth/register` and `/auth/login`) require a valid JWT token in the `Authorization` header.  
2. **Error Handling**:  
   ```json
   { "success": false, "error": "Invalid credentials", "message": "Email or password is incorrect" }
   ```