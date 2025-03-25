

This document provides a comprehensive set of APIs for the courier panel of an e-commerce system specializing in used car parts. It includes all necessary endpoints to support features such as order management, delivery status updates, earnings and analytics, team coordination, and profile management. Each endpoint is detailed with HTTP methods, paths, headers, query parameters, request bodies, and example responses. Additional endpoints and parameters have been included to ensure completeness beyond the initially provided set.

**Note**: All endpoints require a valid courier JWT token in the `Authorization` header for authentication.

---

## Table of Contents
1. [Order Management APIs](#1-order-management-apis)
2. [Earnings & Analytics APIs](#2-earnings--analytics-apis)
3. [Team Coordination APIs](#3-team-coordination-apis)
4. [Profile & Settings APIs](#4-profile--settings-apis)
5. [General Notes](#general-notes)

---

## 1. Order Management APIs

### 1.1 Get Orders
- **Endpoint**: `GET /api/courier/orders`
- **Description**: Fetch orders assigned to the courier or team.
- **Headers**:
  - `Authorization: Bearer <COURIER_JWT>` (required)
- **Query Parameters**:
  - `status` (optional): Filter by order status (e.g., `pending`, `in_progress`, `delivered`, `cancelled`)
  - `limit` (optional, default: 10): Number of orders to return
  - `page` (optional, default: 1): Pagination page number
  - `sortBy` (optional, default: `createdAt`): Sort by field (e.g., `createdAt`, `total`)
  - `order` (optional, default: `desc`): Sort order (`asc` or `desc`)
- **Response** (200 OK):
  ```json
  {
    "success": true,
    "data": [
      {
        "orderId": "123",
        "address": "123 Main St, New York",
        "status": "pending",
        "buyerName": "John Doe",
        "items": [
          {
            "productId": "456",
            "name": "Brake Pads",
            "quantity": 2,
            "price": 75.00
          }
        ],
        "total": 150.00,
        "deliveryInstructions": "Leave at the front door",
        "createdAt": "2024-04-20T10:00:00Z"
      }
    ],
    "meta": {
      "total": 25,
      "page": 1,
      "limit": 10
    }
  }
  ```

### 1.2 Update Order Status
- **Endpoint**: `PUT /api/courier/orders/:id/status`
- **Description**: Update the delivery status of an order.
- **Headers**:
  - `Authorization: Bearer <COURIER_JWT>` (required)
- **Path Parameters**:
  - `id` (required): Order ID (string)
- **Request Body**:
  ```json
  {
    "status": "delivered",
    "notes": "Left at front door",
    "updatedAt": "2024-04-20T12:00:00Z" // Optional, defaults to current timestamp
  }
  ```
- **Response** (200 OK):
  ```json
  {
    "success": true,
    "data": {
      "orderId": "123",
      "status": "delivered",
      "updatedAt": "2024-04-20T12:00:00Z"
    }
  }
  ```

### 1.3 Upload Proof of Delivery
- **Endpoint**: `POST /api/courier/orders/:id/proof`
- **Description**: Upload proof of delivery (e.g., signature or photo).
- **Headers**:
  - `Authorization: Bearer <COURIER_JWT>` (required)
- **Path Parameters**:
  - `id` (required): Order ID (string)
- **Request Body**:
  ```json
  {
    "imageUrl": "https://example.com/signature.jpg",
    "type": "photo" // Optional: 'photo' or 'signature', defaults to 'photo'
  }
  ```
- **Response** (200 OK):
  ```json
  {
    "success": true,
    "message": "Proof of delivery uploaded",
    "data": {
      "orderId": "123",
      "proof": {
        "imageUrl": "https://example.com/signature.jpg",
        "type": "photo",
        "uploadedAt": "2024-04-20T12:05:00Z"
      }
    }
  }
  ```

### 1.4 Get Order Details
- **Endpoint**: `GET /api/courier/orders/:id`
- **Description**: Fetch detailed information about a specific order.
- **Headers**:
  - `Authorization: Bearer <COURIER_JWT>` (required)
- **Path Parameters**:
  - `id` (required): Order ID (string)
- **Response** (200 OK):
  ```json
  {
    "success": true,
    "data": {
      "orderId": "123",
      "address": "123 Main St, New York",
      "buyerName": "John Doe",
      "phone": "+1234567890",
      "items": [
        {
          "productId": "456",
          "name": "Brake Pads",
          "quantity": 2,
          "price": 75.00
        }
      ],
      "total": 150.00,
      "status": "pending",
      "deliveryInstructions": "Leave at the front door",
      "createdAt": "2024-04-20T10:00:00Z",
      "updatedAt": "2024-04-20T10:00:00Z"
    }
  }
  ```

### 1.5 Cancel Order
- **Endpoint**: `PUT /api/courier/orders/:id/cancel`
- **Description**: Cancel an order assigned to the courier (if allowed by policy).
- **Headers**:
  - `Authorization: Bearer <COURIER_JWT>` (required)
- **Path Parameters**:
  - `id` (required): Order ID (string)
- **Request Body**:
  ```json
  {
    "reason": "Customer unavailable",
    "notes": "Attempted delivery twice"
  }
  ```
- **Response** (200 OK):
  ```json
  {
    "success": true,
    "data": {
      "orderId": "123",
      "status": "cancelled",
      "reason": "Customer unavailable"
    }
  }
  ```

---

## 2. Earnings & Analytics APIs

### 2.1 Get Earnings
- **Endpoint**: `GET /api/courier/analytics/earnings`
- **Description**: Fetch earnings data for a specified period.
- **Headers**:
  - `Authorization: Bearer <COURIER_JWT>` (required)
- **Query Parameters**:
  - `period` (required): Time range (e.g., `daily`, `weekly`, `monthly`, `custom`)
  - `startDate` (optional, required if `period=custom`): Start date in ISO format (e.g., `2024-04-01`)
  - `endDate` (optional, required if `period=custom`): End date in ISO format (e.g., `2024-04-30`)
- **Response** (200 OK):
  ```json
  {
    "success": true,
    "data": {
      "period": "2024-04-15 to 2024-04-21",
      "earnings": 500.00,
      "currency": "USD",
      "completedOrders": 20
    }
  }
  ```

### 2.2 Get Performance Metrics
- **Endpoint**: `GET /api/courier/analytics/performance`
- **Description**: Fetch performance metrics for the courier.
- **Headers**:
  - `Authorization: Bearer <COURIER_JWT>` (required)
- **Query Parameters**:
  - `metric` (optional): Specific metric to filter (e.g., `on_time_rate`, `completed_orders`, `avg_delivery_time`)
  - `period` (optional, default: `monthly`): Time range (e.g., `daily`, `weekly`, `monthly`)
- **Response** (200 OK):
  ```json
  {
    "success": true,
    "data": {
      "onTimeRate": "95%",
      "completedOrders": 50,
      "avgDeliveryTime": "2h 15m",
      "period": "monthly"
    }
  }
  ```

### 2.3 Get Earnings Breakdown
- **Endpoint**: `GET /api/courier/analytics/earnings/breakdown`
- **Description**: Fetch detailed breakdown of earnings by order.
- **Headers**:
  - `Authorization: Bearer <COURIER_JWT>` (required)
- **Query Parameters**:
  - `startDate` (required): Start date in ISO format (e.g., `2024-04-01`)
  - `endDate` (required): End date in ISO format (e.g., `2024-04-30`)
  - `limit` (optional, default: 10): Number of records to return
  - `page` (optional, default: 1): Pagination page number
- **Response** (200 OK):
  ```json
  {
    "success": true,
    "data": [
      {
        "orderId": "123",
        "completedAt": "2024-04-20T12:00:00Z",
        "earnings": 25.00,
        "currency": "USD"
      }
    ],
    "meta": {
      "total": 15,
      "page": 1,
      "limit": 10
    }
  }
  ```

---

## 3. Team Coordination APIs

### 3.1 Assign Order
- **Endpoint**: `POST /api/courier/orders/assign`
- **Description**: Assign an order to a regional team or specific courier.
- **Headers**:
  - `Authorization: Bearer <COURIER_JWT>` (required)
- **Request Body**:
  ```json
  {
    "orderId": "123",
    "region": "North",
    "courierId": "456" // Optional if assigning to a region only
  }
  ```
- **Response** (200 OK):
  ```json
  {
    "success": true,
    "message": "Order 123 assigned to courier 456 in North region",
    "data": {
      "orderId": "123",
      "assignedTo": "456",
      "region": "North"
    }
  }
  ```

### 3.2 Get Team Members
- **Endpoint**: `GET /api/courier/teams`
- **Description**: List couriers in the assigned region.
- **Headers**:
  - `Authorization: Bearer <COURIER_JWT>` (required)
- **Query Parameters**:
  - `region` (required): Region name (e.g., `North`, `South`)
  - `status` (optional): Filter by availability (e.g., `active`, `inactive`)
- **Response** (200 OK):
  ```json
  {
    "success": true,
    "data": [
      {
        "courierId": "456",
        "name": "Alice Smith",
        "availability": "active",
        "phone": "+1234567890"
      }
    ]
  }
  ```

### 3.3 Send Team Message
- **Endpoint**: `POST /api/courier/teams/chat`
- **Description**: Send a message to the courier team.
- **Headers**:
  - `Authorization: Bearer <COURIER_JWT>` (required)
- **Request Body**:
  ```json
  {
    "message": "Urgent: Delivery delay due to traffic",
    "region": "North" // Optional, defaults to sender's region
  }
  ```
- **Response** (200 OK):
  ```json
  {
    "success": true,
    "data": {
      "messageId": "789",
      "message": "Urgent: Delivery delay due to traffic",
      "region": "North",
      "sentAt": "2024-04-20T12:10:00Z"
    }
  }
  ```

### 3.4 Get Team Chat History
- **Endpoint**: `GET /api/courier/teams/chat`
- **Description**: Retrieve chat history for the courier's team.
- **Headers**:
  - `Authorization: Bearer <COURIER_JWT>` (required)
- **Query Parameters**:
  - `region` (required): Region name (e.g., `North`)
  - `limit` (optional, default: 20): Number of messages to return
  - `page` (optional, default: 1): Pagination page number
- **Response** (200 OK):
  ```json
  {
    "success": true,
    "data": [
      {
        "messageId": "789",
        "message": "Urgent: Delivery delay due to traffic",
        "senderId": "123",
        "senderName": "John Doe",
        "sentAt": "2024-04-20T12:10:00Z"
      }
    ],
    "meta": {
      "total": 35,
      "page": 1,
      "limit": 20
    }
  }
  ```

---

## 4. Profile & Settings APIs

### 4.1 Get Profile
- **Endpoint**: `GET /api/courier/profile`
- **Description**: Fetch the courier's profile information.
- **Headers**:
  - `Authorization: Bearer <COURIER_JWT>` (required)
- **Response** (200 OK):
  ```json
  {
    "success": true,
    "data": {
      "courierId": "123",
      "name": "John Doe",
      "phone": "+1234567890",
      "email": "john.doe@example.com",
      "region": "North",
      "availability": "active",
      "createdAt": "2024-01-01T00:00:00Z"
    }
  }
  ```

### 4.2 Update Profile
- **Endpoint**: `PUT /api/courier/profile`
- **Description**: Update the courier's profile information.
- **Headers**:
  - `Authorization: Bearer <COURIER_JWT>` (required)
- **Request Body**:
  ```json
  {
    "name": "John Doe",
    "phone": "+1234567890",
    "email": "john.doe@example.com"
  }
  ```
- **Response** (200 OK):
  ```json
  {
    "success": true,
    "data": {
      "courierId": "123",
      "name": "John Doe",
      "phone": "+1234567890",
      "email": "john.doe@example.com"
    }
  }
  ```

### 4.3 Update Availability
- **Endpoint**: `PUT /api/courier/availability`
- **Description**: Update the courier's availability status.
- **Headers**:
  - `Authorization: Bearer <COURIER_JWT>` (required)
- **Request Body**:
  ```json
  {
    "status": "active" // Options: 'active', 'inactive'
  }
  ```
- **Response** (200 OK):
  ```json
  {
    "success": true,
    "data": {
      "courierId": "123",
      "status": "active",
      "updatedAt": "2024-04-20T12:15:00Z"
    }
  }
  ```

### 4.4 Change Password
- **Endpoint**: `PUT /api/courier/profile/password`
- **Description**: Update the courier's password.
- **Headers**:
  - `Authorization: Bearer <COURIER_JWT>` (required)
- **Request Body**:
  ```json
  {
    "currentPassword": "oldPass123",
    "newPassword": "newPass456"
  }
  ```
- **Response** (200 OK):
  ```json
  {
    "success": true,
    "message": "Password updated successfully"
  }
  ```

---

## General Notes

- **Authentication**: All endpoints require a valid JWT token in the `Authorization` header as `Bearer <COURIER_JWT>`. Tokens are assumed to be issued via a separate authentication service.
- **Error Handling**: Errors are returned in the following format:
  ```json
  {
    "success": false,
    "error": "Unauthorized",
    "message": "Invalid token",
    "statusCode": 401
  }
  ```
  Common status codes:
  - `400`: Bad Request (e.g., missing required parameters)
  - `401`: Unauthorized (e.g., invalid or missing token)
  - `403`: Forbidden (e.g., insufficient permissions)
  - `404`: Not Found (e.g., invalid order ID)
  - `500`: Internal Server Error
- **Timestamps**: All timestamps are in ISO 8601 format (e.g., `2024-04-20T12:00:00Z`).
- **Pagination**: Endpoints with lists (e.g., `GET /api/courier/orders`) include `meta` fields for pagination details.

This documentation provides a fully complete set of APIs for the courier panel, including all initially provided endpoints and additional ones to cover missing functionality, such as order cancellation, earnings breakdown, team chat history, and profile management enhancements. All required parameters are explicitly defined to ensure clarity and usability.