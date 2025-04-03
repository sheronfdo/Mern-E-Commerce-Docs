# API Documentation: Authentication

## Base URL
`http://localhost:3000/api/auth`

---

## Login

Authenticate a user and return an access token.

### Request
`POST /login`

#### Headers
- `Content-Type`: `application/json`

#### Body
```json
{
    "email": "admin@example.com",
    "password": "adminpassword"
}
```

### Response
#### Success (200 OK)
```json
{
    "success": true,
    "data": {
        "userId": "67e6237a1209986048db8560",
        "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6IjY3ZTYyMzdhMTIwOTk4NjA0OGRiODU2MCIsInJvbGUiOiJhZG1pbiIsImlhdCI6MTc0MzYxOTY0MCwiZXhwIjoxNzQzNjIzMjQwfQ.ZOzyWgTK1VOScpj6t_vD583l6QwS71nIv3V7lfnpMBg"
    }
}
```

#### Error Cases
- **400 Bad Request**: Invalid request body or missing fields
- **401 Unauthorized**: Invalid credentials
- **500 Internal Server Error**: Server error

---

## Register

Register a new user (seller or buyer).

### Request
`POST /register`

#### Headers
- `Content-Type`: `application/json`

#### Body
```json
{
    "email": "sampleseller@mail.com",
    "password": "sampassword",
    "role": "seller", // "seller" or "buyer"
    "name": "samseller",
    "phone": "+94446546545"
}
```

### Response
#### Success (201 Created)
```json
{
    "success": true,
    "data": {
        "userId": "67ed871c5b2e5327f9a9295f",
        "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6IjY3ZWQ4NzFjNWIyZTUzMjdmOWE5Mjk1ZiIsInJvbGUiOiJzZWxsZXIiLCJpYXQiOjE3NDM2MTk4NjgsImV4cCI6MTc0MzYyMzQ2OH0.kO6txZDJincWHDjxGuUdDvLlw7H2iB48uhgw-fgDgeM"
    }
}
```

#### Error Cases
- **400 Bad Request**: Invalid request body, missing fields, or invalid role
- **409 Conflict**: Email already exists
- **500 Internal Server Error**: Server error

---

## Notes
1. The returned `token` is a JWT that should be included in subsequent requests in the `Authorization` header as `Bearer <token>`
2. All requests and responses are in JSON format
3. The `role` field in registration must be either "seller" or "buyer" (case-sensitive)
4. Passwords should meet your application's security requirements (not shown in this documentation)


# **Complete Admin API Documentation**  
**Base URL**: `http://localhost:3000/api/admin`  

---

## **Authentication**  
All endpoints require an **admin JWT token** in the `Authorization` header:  
```
Authorization: Bearer <token>
```

---

## **1. Admin Management**  
### **1.1 Get All Admins**  
**Endpoint**: `GET /admins`  
**Description**: Retrieve all admin accounts.  

**Response**:  
```json
{
    "success": true,
    "data": [
        {
            "_id": "67e6237a1209986048db8560",
            "email": "admin@example.com",
            "name": "Admin User",
            "phone": "+1234567890",
            "status": "active",
            "createdAt": "2025-03-28T04:20:10.769Z"
        }
    ]
}
```

---

### **1.2 Create Admin**  
**Endpoint**: `POST /admins`  
**Description**: Register a new admin.  

**Request Body**:  
```json
{
    "email": "newadmin@example.com",
    "password": "securepassword",
    "name": "New Admin",
    "phone": "+94770470323"
}
```

**Response**:  
```json
{
    "success": true,
    "data": {
        "adminId": "67ed88035b2e5327f9a92963"
    }
}
```

---

### **1.3 Delete Admin**  
**Endpoint**: `DELETE /admins/:adminId`  
**Description**: Permanently delete an admin.  

**Response**:  
```json
{
    "success": true,
    "message": "Admin deleted"
}
```

---

## **2. Courier Management**  
### **2.1 Get All Couriers**  
**Endpoint**: `GET /couriers`  

**Response**:  
```json
{
    "success": true,
    "data": [
        {
            "_id": "67e754164d954ec2d005751e",
            "email": "courier@example.com",
            "name": "Courier User",
            "phone": "+1234567890",
            "region": "Colombo",
            "status": "active"
        }
    ]
}
```

---

### **2.2 Create Courier**  
**Endpoint**: `POST /couriers`  

**Request Body**:  
```json
{
    "email": "newcourier@mail.com",
    "password": "123",
    "name": "New Courier",
    "phone": "+94770470323",
    "region": "Galle"
}
```

**Response**:  
```json
{
    "success": true,
    "data": {
        "courierId": "67ed88da5b2e5327f9a9296a"
    }
}
```

---

### **2.3 Update Courier**  
**Endpoint**: `PUT /couriers/:courierId`  

**Response**:  
```json
{
    "success": true,
    "message": "Courier updated"
}
```

---

### **2.4 Delete Courier**  
**Endpoint**: `DELETE /couriers/:courierId`  

**Response**:  
```json
{
    "success": true,
    "message": "Courier deleted"
}
```

---

## **3. Seller Management**  
### **3.1 Get Pending Sellers**  
**Endpoint**: `GET /sellers/pending`  

**Response**:  
```json
{
    "success": true,
    "data": [
        {
            "_id": "67ed871c5b2e5327f9a9295f",
            "email": "pendingseller@mail.com",
            "name": "Pending Seller",
            "status": "pending"
        }
    ]
}
```

---

### **3.2 Get All Sellers**  
**Endpoint**: `GET /sellers`  

**Response**:  
```json
{
    "success": true,
    "data": [
        {
            "_id": "67e754164d954ec2d0057521",
            "email": "seller@example.com",
            "name": "Approved Seller",
            "status": "active"
        }
    ]
}
```

---

### **3.3 Approve Seller**  
**Endpoint**: `PUT /sellers/:sellerId`  

**Response**:  
```json
{
    "success": true,
    "message": "Seller approved"
}
```

---

### **3.4 Delete Seller**  
**Endpoint**: `DELETE /sellers/:sellerId`  

**Response**:  
```json
{
    "success": true,
    "message": "Seller deleted"
}
```

---

## **4. Buyer Management**  
### **4.1 Get All Buyers**  
**Endpoint**: `GET /buyers`  

**Response**:  
```json
{
    "success": true,
    "data": [
        {
            "_id": "67e754164d954ec2d0057524",
            "email": "buyer@example.com",
            "name": "Buyer User",
            "status": "active"
        }
    ]
}
```

---

### **4.2 Delete Buyer**  
**Endpoint**: `DELETE /buyers/:buyerId`  

**Response**:  
```json
{
    "success": true,
    "message": "Buyer deleted"
}
```

---

## **5. Category Management**  
### **5.1 Get All Categories**  
**Endpoint**: `GET /categories`  

**Response**:  
```json
{
    "success": true,
    "data": [
        {
            "_id": "67e8ba833dc8d3b9b6c6660d",
            "name": "Engine & Transmission",
            "parentCategory": null,
            "status": "active"
        }
    ]
}
```

---

### **5.2 Get Category by ID**  
**Endpoint**: `GET /categories/:categoryId`  

**Response**:  
```json
{
    "success": true,
    "data": {
        "_id": "67e8ba833dc8d3b9b6c6660d",
        "name": "Engine & Transmission",
        "status": "active"
    }
}
```

---

### **5.3 Create Category**  
**Endpoint**: `POST /categories`  

**Request Body**:  
```json
{
    "name": "New Category",
    "parentCategory": "67e8bcea6b8eda334c2d2b2c"  // Optional
}
```

**Response**:  
```json
{
    "success": true,
    "data": {
        "categoryId": "67ed8bc95b2e5327f9a92982"
    }
}
```

---

### **5.4 Update Category**  
**Endpoint**: `PUT /categories/:categoryId`  

**Response**:  
```json
{
    "success": true,
    "message": "Category updated"
}
```

---

### **5.5 Delete Category**  
**Endpoint**: `DELETE /categories/:categoryId`  

**Response**:  
```json
{
    "success": true,
    "message": "Category marked as deleted"
}
```

---

## **Error Responses**  
| Status Code | Description |
|-------------|-------------|
| **400** | Bad Request (Invalid Input) |
| **401** | Unauthorized (Missing/Invalid Token) |
| **403** | Forbidden (Not Admin) |
| **404** | Resource Not Found |
| **409** | Conflict (Duplicate Email) |
| **500** | Server Error |

---

### **Notes**:
- All **DELETE** operations are **permanent** except for categories (soft-delete).
- **ParentCategory** is optional (used only for subcategories).
- **JWT token** must be included in all requests.

This documentation covers all **Admin API** endpoints for managing users and categories. 🚀

# **Seller Product Management API Documentation**  
**Base URL**: `http://localhost:3000/api/seller`  

---

## **Authentication**  
All endpoints require a **seller JWT token** in the `Authorization` header:  
```
Authorization: Bearer <token>
```

---

## **1. Product Management**  
### **1.1 Create Product**  
**Endpoint**: `POST /products`  
**Description**: Add a new product listing.  

**Request Body**:  
```json
{
  "title": "Toyota Camry Brake Pads",
  "description": "High-quality ceramic brake pads for Toyota vehicles",
  "price": 75,
  "category": "67e8ba893dc8d3b9b6c6665e",
  "stock": 10,
  "condition": "New",
  "brand": "Bosch",
  "oem": "OEM123",
  "aftermarket": false,
  "material": "Ceramic",
  "makeModel": [
    {"make": "Toyota", "model": "Camry"},
    {"make": "Toyota", "model": "Corolla"}
  ],
  "years": [2015, 2020],
  "images": ["https://example.com/brake-pads.jpg"]
}
```

**Response**:  
```json
{
  "success": true,
  "data": {
    "productId": "67ed8f885b2e5327f9a9298e",
    "title": "Toyota Camry Brake Pads"
  }
}
```

---

### **1.2 Get Seller Products**  
**Endpoint**: `GET /products`  
**Query Parameters**:  
- `page` (default: `1`)  
- `limit` (default: `10`)  

**Response**:  
```json
{
  "success": true,
  "data": [
    {
      "_id": "67ed8f885b2e5327f9a9298e",
      "title": "Toyota Camry Brake Pads",
      "price": 75,
      "stock": 10,
      "availability": "In Stock",
      "images": ["https://example.com/brake-pads.jpg"]
    }
  ],
  "pagination": {
    "page": 1,
    "limit": 10,
    "total": 1
  }
}
```

---

### **1.3 Update Product**  
**Endpoint**: `PUT /products/:productId`  
**Description**: Modify product details.  

**Request Body**:  
```json
{
  "price": 80,
  "stock": 100,
  "description": "Updated description"
}
```

**Response**:  
```json
{
  "success": true,
  "message": "Product updated",
  "data": {
    "productId": "67ed8f885b2e5327f9a9298e",
    "title": "Toyota Camry Brake Pads"
  }
}
```

---

### **1.4 Delete Product**  
**Endpoint**: `DELETE /products/:productId`  
**Description**: Soft-delete a product (marks as deleted).  

**Response**:  
```json
{
  "success": true,
  "message": "Product marked as deleted"
}
```

---

## **Error Responses**  
| Status Code | Description |
|-------------|-------------|
| **400** | Bad Request (Invalid Input) |
| **401** | Unauthorized (Missing/Invalid Token) |
| **403** | Forbidden (Not Seller) |
| **404** | Product Not Found |
| **500** | Server Error |

---

### **Notes**:
- **Soft Delete**: Products are marked as `deleted` but remain in the database.
- **Images**: Must be valid URLs (hosted externally).
- **Pagination**: Used for `GET /products` to limit response size.
- **Required Fields**: `title`, `price`, `category`, `stock`.

---

### **Example Workflow**  
1. **Create Product** → `POST /products`  
2. **List Products** → `GET /products?page=1&limit=10`  
3. **Update Stock/Price** → `PUT /products/:id`  
4. **Remove Product** → `DELETE /products/:id`  

This API allows sellers to **manage their automotive parts inventory** efficiently. 🚗🔧

# **Buyer API Documentation**  
**Base URL**: `http://localhost:3000/api/buyer`  

---

## **Authentication**  
All endpoints require a **buyer JWT token** in the `Authorization` header:  
```
Authorization: Bearer <token>
```

---

## **1. Product Browsing**  
### **1.1 Get All Products**  
**Endpoint**: `GET /products`  
**Query Parameters**:  
- `page` (default: `1`)  
- `limit` (default: `10`)  

**Response**:  
```json
{
  "success": true,
  "data": [
    {
      "_id": "67ea96914abbf7f7e41bb436",
      "title": "Nissan Altima Radiator",
      "price": 130,
      "category": {
        "_id": "67e8ba843dc8d3b9b6c66616",
        "name": "Cooling System"
      },
      "stock": 5,
      "condition": "New",
      "brand": "Denso",
      "images": ["https://example.com/radiator-nissan.jpg"],
      "availability": "In Stock"
    }
  ],
  "pagination": {
    "page": 1,
    "limit": 10,
    "total": 45
  }
}
```

---

### **1.2 Search Products with Filters**  
**Endpoint**: `GET /products/search`  
**Query Parameters**:  
| Parameter       | Example           | Description                          |
|-----------------|-------------------|--------------------------------------|
| `keyword`       | `brake`           | Search in title/description          |
| `minPrice`      | `50`              | Minimum price filter                 |
| `maxPrice`      | `100`             | Maximum price filter                 |
| `condition`     | `New`             | `New`/`Used`/`Refurbished`           |
| `brand`         | `Brembo`          | Filter by brand                      |
| `aftermarket`   | `true`/`false`    | OEM (false) or aftermarket (true)    |
| `availability`  | `In Stock`        | Stock status                         |
| `make`          | `Toyota`          | Vehicle make                         |
| `model`         | `Camry`           | Vehicle model                        |
| `years`         | `2019,2020`       | Comma-separated year range           |
| `material`      | `Cast Iron`       | Product material                     |
| `sellerLocation`| `North`           | Seller region filter                 |

**Response**:  
```json
{
  "success": true,
  "data": [],  // Returns filtered products (same structure as GET /products)
  "pagination": {
    "page": 1,
    "limit": 10,
    "total": 0
  }
}
```

---

### **1.3 Get Product by ID**  
**Endpoint**: `GET /products/:productId`  

**Response**:  
```json
{
  "success": true,
  "data": {
    "_id": "67ea96914abbf7f7e41bb436",
    "title": "Nissan Altima Radiator",
    "description": "Aluminum radiator for cooling",
    "price": 130,
    "stock": 5,
    "brand": "Denso",
    "condition": "New",
    "makeModel": [
      {"make": "Nissan", "model": "Altima"}
    ],
    "years": [2015, 2016],
    "images": ["https://example.com/radiator-nissan.jpg"]
  }
}
```

---

## **2. Cart Management**  
### **2.1 Add to Cart**  
**Endpoint**: `POST /cart/add`  
**Request Body**:  
```json
{
  "productId": "67ea96914abbf7f7e41bb42f",
  "quantity": 2
}
```

**Response**:  
```json
{
  "success": true,
  "message": "Item added to cart",
  "data": {
    "items": [
      {
        "productId": "67ea96914abbf7f7e41bb42f",
        "quantity": 2
      }
    ]
  }
}
```

---

### **2.2 View Cart**  
**Endpoint**: `GET /cart`  

**Response**:  
```json
{
  "success": true,
  "data": {
    "items": [
      {
        "productId": {
          "_id": "67ea96914abbf7f7e41bb42f",
          "title": "Toyota Camry Engine Block",
          "price": 1200,
          "images": ["https://example.com/engine-block-toyota.jpg"]
        },
        "quantity": 2
      }
    ],
    "total": 2400  // Sum of (price × quantity)
  }
}
```

---

### **2.3 Remove from Cart**  
**Endpoint**: `DELETE /cart/remove/:productId`  

**Response**:  
```json
{
  "success": true,
  "message": "Item removed from cart",
  "data": {
    "items": []  // Updated cart items
  }
}
```

---

### **2.4 Clear Cart**  
**Endpoint**: `DELETE /cart/clear`  

**Response**:  
```json
{
  "success": true,
  "message": "Cart cleared"
}
```

---

## **Error Responses**  
| Status Code | Description                  |
|-------------|------------------------------|
| **400**     | Invalid product ID/quantity  |
| **401**     | Unauthorized (invalid JWT)   |
| **404**     | Product not found            |
| **500**     | Server error                 |

---

## **Notes**  
1. **Cart Persistence**: Cart data is tied to the buyer account.  
2. **Stock Validation**: Items are automatically removed if out of stock.  
3. **Pagination**: Defaults to 10 items per page.  

---

### **Example Workflow**  
1. **Browse Products** → `GET /products?page=1`  
2. **Filter Results** → `GET /products/search?make=Toyota&condition=New`  
3. **Add to Cart** → `POST /cart/add`  
4. **Checkout** → (Future payment integration)  

This API allows buyers to **discover automotive parts** and **manage their cart** seamlessly. 🛒🔧

---

# **Asset Management API Documentation**  
**Base URL**: `http://localhost:3000/api/media`  

---

## **Authentication**  
All endpoints require a **JWT token** in the `Authorization` header:  
```
Authorization: Bearer <token>
```

---

## **1. Media Upload**  
### **1.1 Upload Assets**  
**Endpoint**: `POST /upload`  
**Description**: Uploads one or multiple media files (images or videos) and returns their URLs and IDs for use in products or user profiles.

**Request Body**:  
- **Type**: `multipart/form-data`  
- **Fields**:  
  | Field    | Type  | Description                          |  
  |----------|-------|--------------------------------------|  
  | `media`  | File  | Media file(s) to upload (repeatable) |  

- **Accepted Types**: `image/jpeg`, `image/png`, `video/mp4`  
- **Max Files**: 10  

**Example Request**:  
```bash
curl --location 'http://localhost:3000/api/media/upload' \
--header 'Authorization: Bearer <token>' \
--form 'media=@"/path/to/image1.jpg"' \
--form 'media=@"/path/to/image2.png"'
```

**Response**:  
```json
{
  "success": true,
  "message": "Media uploaded successfully",
  "data": [
    {
      "id": "1634567890123-456789123-image1.jpg",
      "url": "http://localhost:3000/uploads/1634567890123-456789123-image1.jpg"
    },
    {
      "id": "1634567890124-456789124-image2.png",
      "url": "http://localhost:3000/uploads/1634567890124-456789124-image2.png"
    }
  ]
}
```

---

## **Error Responses**  
| Status Code | Description                        | Example Response                                                                       |     |
| ----------- | ---------------------------------- | -------------------------------------------------------------------------------------- | --- |
| **400**     | No files uploaded                  | `{"success": false, "message": "No files uploaded"}`                                   |     |
| **400**     | Invalid file type                  | `{"success": false, "message": "Invalid file type. Only JPEG, PNG, and MP4 allowed."}` |     |
| **401**     | Unauthorized (missing/invalid JWT) | `{"success": false, "message": "No token, authorization denied"}`                      |     |
| **500**     | Server error                       | `{"success": false, "message": "Internal server error"}`                               |     |

---

## **Notes**  
1. **File Storage**: Uploaded files are stored in the `uploads/` directory on the server.  
2. **Multiple Uploads**: Use the `media` field multiple times in the request to upload more than one file (e.g., two `media` fields for two images).  
3. **Usage**: The returned `url` can be stored in `Product.images` or `User.profileImage` fields and accessed directly (e.g., `<img src="<url>">`).  
4. **Max Limit**: Up to 10 files can be uploaded in a single request.  

---

### **Example Workflow**  
1. **Upload Single Image** → `POST /upload` with one `media` field (e.g., `image1.jpg`)  
2. **Upload Multiple Images** → `POST /upload` with multiple `media` fields (e.g., `image1.jpg`, `image2.png`)  
3. **Store URLs** → Save the returned `url` values in your database (e.g., product or user records).  
4. **Display Assets** → Use the URLs in your frontend to render images or videos.  

This API enables seamless **media asset management** for your application. 📸🎥

--- 

This matches the format you provided, focusing solely on the asset upload endpoint. Let me know if you need adjustments!

Below is the API documentation for the buyer order-related endpoints based on the provided `curl` requests and responses, formatted in the style you requested (similar to the "Buyer API Documentation" example).

---

# **Buyer Order API Documentation**  
**Base URL**: `http://localhost:3000/api/buyer`  

---

## **Authentication**  
All endpoints require a **buyer JWT token** in the `Authorization` header:  
```
Authorization: Bearer <token>
```

---

## **1. Order Management**  
### **1.1 Create Order**  
**Endpoint**: `POST /order`  
**Description**: Creates a new order with specified items and shipping address, initiating payment via PayHere.

**Request Body**:  
```json
{
  "items": [
    {
      "productId": "string",
      "quantity": number
    }
  ],
  "shippingAddress": {
    "street": "string",
    "city": "string",
    "country": "string",
    "postalCode": "string"
  }
}
```

**Example Request**:  
```bash
curl --location 'http://localhost:3000/api/buyer/order' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer <token>' \
--data '{
  "items": [
    { "productId": "67ea96914abbf7f7e41bb436", "quantity": 2 }
  ],
  "shippingAddress": {
    "street": "123 Main St",
    "city": "Colombo",
    "country": "Sri Lanka",
    "postalCode": "10000"
  }
}'
```

**Response**:  
```json
{
  "success": true,
  "message": "Order created, proceed to payment",
  "data": {
    "orderId": "67eebd32e224323526dcc692",
    "payhereData": {
      "sandbox": true,
      "merchant_id": "1229991",
      "return_url": "http://localhost:3000/success",
      "cancel_url": "http://localhost:3000/cancel",
      "notify_url": "http://localhost:3000/api/buyer/order/notify",
      "order_id": "67eebd32e224323526dcc692",
      "items": "Product 67ea96914abbf7f7e41bb436",
      "currency": "LKR",
      "amount": "260.00",
      "first_name": "Buyer",
      "last_name": "",
      "email": "sample@mail.com",
      "phone": "1234567890",
      "address": "123 Main St",
      "city": "Colombo",
      "country": "Sri Lanka",
      "delivery_address": "123 Main St",
      "delivery_city": "Colombo",
      "delivery_country": "Sri Lanka",
      "hash": "512898178BA5426F3A8D3A9A678EC8AF"
    }
  }
}
```

---

### **1.2 Notify Order (PayHere Callback)**  
**Endpoint**: `POST /order/notify`  
**Description**: Updates the order status based on PayHere payment notification (no authentication required for callback).

**Request Body**:  
```json
{
  "merchant_id": "string",
  "order_id": "string",
  "status_code": "string",
  "md5sig": "string",
  "amount": "string",
  "currency": "string"
}
```

**Example Request**:  
```bash
curl --location 'http://localhost:3000/api/buyer/order/notify' \
--header 'Content-Type: application/json' \
--data '{
  "merchant_id": "1229991",
  "order_id": "67eebbd0e224323526dcc67b",
  "status_code": "2",
  "md5sig": "4E1CA725211C746D7D9E56B27A7CF15C",
  "amount": "260.00",
  "currency": "LKR"
}'
```

**Response**:  
```
OK
```

---

### **1.3 Cancel Order**  
**Endpoint**: `POST /order/cancel/:orderId`  
**Description**: Cancels an existing order by its ID.

**Example Request**:  
```bash
curl --location --request POST 'http://localhost:3000/api/buyer/order/cancel/67eebd32e224323526dcc692' \
--header 'Authorization: Bearer <token>'
```

**Response**:  
```json
{
  "success": true,
  "message": "Order cancelled successfully"
}
```

---

### **1.4 Track Order**  
**Endpoint**: `GET /order/track/:orderId`  
**Description**: Retrieves the status and history of a specific order.

**Example Request**:  
```bash
curl --location 'http://localhost:3000/api/buyer/order/track/67eebbd0e224323526dcc67b' \
--header 'Authorization: Bearer <token>'
```

**Response**:  
```json
{
  "success": true,
  "data": {
    "_id": "67eebbd0e224323526dcc67b",
    "status": "Confirmed",
    "statusHistory": []
  }
}
```

---

### **1.5 Get Order History**  
**Endpoint**: `GET /orders`  
**Description**: Retrieves all orders for the authenticated buyer.

**Example Request**:  
```bash
curl --location 'http://localhost:3000/api/buyer/orders' \
--header 'Authorization: Bearer <token>'
```

**Response**:  
```json
{
  "success": true,
  "data": [
    {
      "_id": "67ed04242d98327984f97e2f",
      "buyerId": "67e754164d954ec2d0057524",
      "items": [
        {
          "productId": {
            "_id": "67ea96914abbf7f7e41bb42f",
            "title": "Toyota Camry Engine Block",
            "price": 1200,
            "condition": "New",
            "brand": "Toyota",
            "images": ["https://example.com/engine-block-toyota.jpg"]
          },
          "quantity": 1,
          "price": 1200,
          "_id": "67ed04242d98327984f97e30"
        }
      ],
      "total": 1200,
      "status": "Pending",
      "createdAt": "2025-04-02T09:32:20.257Z",
      "updatedAt": "2025-04-02T09:32:20.261Z",
      "__v": 0,
      "statusHistory": []
    },
    {
      "_id": "67eeaf7e72d66a30d2476f80",
      "buyerId": "67e754164d954ec2d0057524",
      "items": [
        {
          "productId": {
            "_id": "67ea96914abbf7f7e41bb436",
            "title": "Nissan Altima Radiator",
            "price": 130,
            "condition": "New",
            "brand": "Denso",
            "images": ["https://example.com/radiator-nissan.jpg"]
          },
          "quantity": 2,
          "price": 130,
          "_id": "67eeaf7e72d66a30d2476f81"
        }
      ],
      "total": 260,
      "status": "Cancelled",
      "shippingAddress": {
        "street": "123 Main St",
        "city": "Colombo",
        "country": "Sri Lanka",
        "postalCode": "10000"
      },
      "statusHistory": [
        {
          "status": "Cancelled",
          "updatedAt": "2025-04-03T16:27:16.148Z",
          "_id": "67eeb6e472d66a30d2476f87"
        }
      ],
      "createdAt": "2025-04-03T15:55:42.267Z",
      "updatedAt": "2025-04-03T16:27:16.304Z",
      "__v": 1
    }
  ]
}
```

---

## **Error Responses**  
| Status Code | Description                  | Example Response                                      |
|-------------|------------------------------|-----------------------------------------------------|
| **400**     | Invalid input (e.g., insufficient stock) | `{"success": false, "message": "Insufficient stock for Nissan Altima Radiator"}` |
| **401**     | Unauthorized (invalid JWT)   | `{"success": false, "message": "No token, authorization denied"}` |
| **404**     | Product/order not found      | `{"success": false, "message": "Product 67ea96914abbf7f7e41bb436 not found"}` |
| **500**     | Server error                 | `{"success": false, "message": "Internal server error"}` |

---

## **Notes**  
1. **Order Creation**: If `items` are not provided, the order uses the buyer’s cart contents.  
2. **Payment Integration**: Orders are created with a PayHere payment link in `payhereData`.  
3. **Status Updates**: The `/notify` endpoint updates order status based on PayHere callbacks (`2` = Confirmed, `0` = Pending, `-1`/`-2` = Cancelled).  
4. **Shipping Address**: Optional in the request; defaults to placeholder values if not provided.  
5. **Tracking**: `statusHistory` tracks status changes (e.g., Cancelled updates).  

---

### **Example Workflow**  
1. **Create Order** → `POST /order` with items and shipping address.  
2. **Process Payment** → Use `payhereData` to complete payment on PayHere.  
3. **Notify Order** → PayHere calls `POST /order/notify` to confirm payment.  
4. **Track Order** → `GET /order/track/:orderId` to check status.  
5. **Cancel Order** → `POST /order/cancel/:orderId` if needed.  
6. **View History** → `GET /orders` to see all past orders.  

This API enables buyers to **create, track, and manage orders** efficiently. 🛍️🚚

--- 


---

# **Seller Order Management API Documentation**  
**Base URL**: `http://localhost:3000/api/seller`  

---

## **Authentication**  
All endpoints require a **seller JWT token** in the `Authorization` header:  
```
Authorization: Bearer <token>
```

---

## **1. Order Management**  
### **1.1 Get Seller Orders**  
**Endpoint**: `GET /orders`  
**Description**: Retrieves all orders containing products belonging to the authenticated seller.

**Example Request**:  
```bash
curl --location 'http://localhost:3000/api/seller/orders' \
--header 'Authorization: Bearer <token>'
```

**Response**:  
```json
{
  "success": true,
  "data": [
    {
      "shippingAddress": {
        "street": "123 Main St",
        "city": "Colombo",
        "country": "Sri Lanka",
        "postalCode": "10000"
      },
      "_id": "67eed9faa46e254dd61d1729",
      "buyerId": {
        "_id": "67e754164d954ec2d0057524",
        "email": "buyer@example.com",
        "name": "buyer User",
        "phone": "+1234567890"
      },
      "items": [
        {
          "productId": {
            "_id": "67eed74dc1daf109f57052ca",
            "title": "Toyota Camry Brake Pads",
            "price": 75,
            "condition": "New",
            "brand": "Bosch",
            "images": ["https://example.com/brake-pads.jpg"],
            "sellerId": "67e754164d954ec2d0057521"
          },
          "quantity": 2,
          "price": 75,
          "sellerStatus": "Pending",
          "_id": "67eed9faa46e254dd61d172a"
        }
      ],
      "total": 150,
      "status": "Confirmed",
      "statusHistory": [],
      "createdAt": "2025-04-03T18:56:58.631Z",
      "updatedAt": "2025-04-03T18:57:20.266Z",
      "__v": 0
    },
    {
      "shippingAddress": {
        "street": "123 Main St",
        "city": "Colombo",
        "country": "Sri Lanka",
        "postalCode": "10000"
      },
      "_id": "67eedcf8c2c643224aa2588b",
      "buyerId": {
        "_id": "67e754164d954ec2d0057524",
        "email": "buyer@example.com",
        "name": "buyer User",
        "phone": "+1234567890"
      },
      "items": [
        {
          "productId": {
            "_id": "67eed74dc1daf109f57052ca",
            "title": "Toyota Camry Brake Pads",
            "price": 75,
            "condition": "New",
            "brand": "Bosch",
            "images": ["https://example.com/brake-pads.jpg"],
            "sellerId": "67e754164d954ec2d0057521"
          },
          "quantity": 2,
          "price": 75,
          "sellerStatus": "Pending",
          "_id": "67eedcf8c2c643224aa2588c"
        }
      ],
      "total": 300,
      "status": "Pending",
      "statusHistory": [],
      "createdAt": "2025-04-03T19:09:44.839Z",
      "updatedAt": "2025-04-03T19:09:44.840Z",
      "__v": 0
    }
  ]
}
```

---

### **1.2 Get Order by ID**  
**Endpoint**: `GET /order/:id`  
**Description**: Retrieves details of a specific order, showing only items belonging to the authenticated seller.

**Example Request**:  
```bash
curl --location 'http://localhost:3000/api/seller/order/67eedcf8c2c643224aa2588b' \
--header 'Authorization: Bearer <token>'
```

**Response**:  
```json
{
  "success": true,
  "data": {
    "shippingAddress": {
      "street": "123 Main St",
      "city": "Colombo",
      "country": "Sri Lanka",
      "postalCode": "10000"
    },
    "_id": "67eedcf8c2c643224aa2588b",
    "buyerId": {
      "_id": "67e754164d954ec2d0057524",
      "email": "buyer@example.com",
      "name": "buyer User",
      "phone": "+1234567890"
    },
    "items": [
      {
        "productId": {
          "_id": "67eed74dc1daf109f57052ca",
          "title": "Toyota Camry Brake Pads",
          "price": 75,
          "condition": "New",
          "brand": "Bosch",
          "images": ["https://example.com/brake-pads.jpg"],
          "sellerId": "67e754164d954ec2d0057521"
        },
        "quantity": 2,
        "price": 75,
        "sellerStatus": "Shipped",
        "_id": "67eedcf8c2c643224aa2588c"
      }
    ],
    "total": 300,
    "status": "Pending",
    "statusHistory": [
      {
        "status": "Seller updated to Shipped",
        "updatedAt": "2025-04-03T19:18:30.179Z",
        "_id": "67eedf06658f7010dc50bd6b"
      }
    ],
    "createdAt": "2025-04-03T19:09:44.839Z",
    "updatedAt": "2025-04-03T19:18:30.184Z",
    "__v": 1
  }
}
```

---

### **1.3 Update Order Status**  
**Endpoint**: `PUT /order/:id/status`  
**Description**: Updates the `sellerStatus` of items in an order that belong to the authenticated seller.

**Request Body**:  
```json
{
  "status": "string" // Allowed values: "Processing", "Shipped", "Delivered"
}
```

**Example Request**:  
```bash
curl --location --request PUT 'http://localhost:3000/api/seller/order/67eedcf8c2c643224aa2588b/status' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer <token>' \
--data '{"status": "Shipped"}'
```

**Response**:  
```json
{
  "success": true,
  "message": "Order status updated to Shipped"
}
```

---

## **Error Responses**  
| Status Code | Description                  | Example Response                                      |
|-------------|------------------------------|-----------------------------------------------------|
| **400**     | Invalid status provided      | `{"success": false, "message": "Invalid status"}`   |
| **401**     | Unauthorized (invalid JWT)   | `{"success": false, "message": "No token, authorization denied"}` |
| **403**     | Not a seller                 | `{"success": false, "message": "Access denied, sellers only"}` |
| **404**     | Order not found or no items  | `{"success": false, "message": "No items in this order belong to you"}` |
| **500**     | Server error                 | `{"success": false, "message": "Internal server error"}` |

---

## **Notes**  
1. **Seller Scope**: Only items where `productId.sellerId` matches the authenticated seller’s ID are included in responses.  
2. **Status Management**: 
   - `sellerStatus` tracks individual item progress (`Pending`, `Processing`, `Shipped`, `Delivered`).
   - Overall order `status` (`Pending`, `Confirmed`, `Shipped`, `Delivered`, `Cancelled`) is managed at the buyer level.
3. **Mixed Orders**: If an order contains products from multiple sellers, only the authenticated seller’s items are shown.  
4. **History**: Updates to `sellerStatus` are logged in `statusHistory` with a descriptive message (e.g., "Seller updated to Shipped").  

---

### **Example Workflow**  
1. **View Orders** → `GET /orders` to see all orders with your products.  
2. **Check Details** → `GET /order/:id` to inspect a specific order.  
3. **Update Status** → `PUT /order/:id/status` to mark items as "Shipped" or "Delivered".  

This API enables sellers to **manage orders** for their products efficiently. 📦🚚

---



# **Profile Management API Documentation**  
**Base URL**: `http://localhost:3000/api/profile`  

---

## **Authentication**  
All endpoints require a **JWT token** in the `Authorization` header:  
```
Authorization: Bearer <token>
```

---

## **1. Profile Management**  
### **1.1 Get Profile**  
**Endpoint**: `GET /`  
**Description**: Retrieves the authenticated user’s profile details.

**Example Request**:  
```bash
curl --location 'http://localhost:3000/api/profile' \
--header 'Authorization: Bearer <token>'
```

**Response**:  
```json
{
  "success": true,
  "data": {
    "_id": "67e754164d954ec2d0057524",
    "role": "buyer",
    "email": "buyer@example.com",
    "name": "Buyer Name",
    "phone": "1234567890",
    "profileImage": "http://localhost:3000/uploads/1634567890123-profile.jpg",
    "addresses": [
      {
        "street": "123 Main St",
        "city": "Colombo",
        "country": "Sri Lanka",
        "postalCode": "10000",
        "isDefault": true
      }
    ],
    "status": "active"
  }
}
```

---

### **1.2 Update Profile**  
**Endpoint**: `PUT /`  
**Description**: Updates the authenticated user’s profile details.

**Request Body**:  
```json
{
  "name": "string",
  "phone": "string",
  "profileImage": "string",
  "password": "string",
  "addresses": [
    {
      "street": "string",
      "city": "string",
      "country": "string",
      "postalCode": "string",
      "isDefault": boolean
    }
  ]
}
```

**Example Request**:  
```bash
curl --location --request PUT 'http://localhost:3000/api/profile' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer <token>' \
--data '{
  "name": "Updated Buyer",
  "phone": "9876543210",
  "profileImage": "http://localhost:3000/uploads/1634567890124-new-profile.jpg",
  "password": "newpassword123",
  "addresses": [
    {
      "street": "456 New St",
      "city": "Kandy",
      "country": "Sri Lanka",
      "postalCode": "20000",
      "isDefault": true
    }
  ]
}'
```

**Response**:  
```json
{
  "success": true,
  "message": "Profile updated successfully",
  "data": {
    "_id": "67e754164d954ec2d0057524",
    "role": "buyer",
    "email": "buyer@example.com",
    "name": "Updated Buyer",
    "phone": "9876543210",
    "profileImage": "http://localhost:3000/uploads/1634567890124-new-profile.jpg",
    "addresses": [
      {
        "street": "456 New St",
        "city": "Kandy",
        "country": "Sri Lanka",
        "postalCode": "20000",
        "isDefault": true
      }
    ],
    "status": "active"
  }
}
```

---

### **1.3 Delete Profile**  
**Endpoint**: `DELETE /`  
**Description**: Deactivates the authenticated user’s profile (soft delete).

**Example Request**:  
```bash
curl --location --request DELETE 'http://localhost:3000/api/profile' \
--header 'Authorization: Bearer <token>'
```

**Response**:  
```json
{
  "success": true,
  "message": "Profile deactivated successfully"
}
```

---

## **Error Responses**  
| Status Code | Description                  | Example Response                                      |
|-------------|------------------------------|-----------------------------------------------------|
| **400**     | Invalid input (e.g., multiple default addresses) | `{"success": false, "message": "Only one address can be default"}` |
| **401**     | Unauthorized (invalid JWT)   | `{"success": false, "message": "No token, authorization denied"}` |
| **404**     | User not found               | `{"success": false, "message": "User not found"}` |
| **500**     | Server error                 | `{"success": false, "message": "Internal server error"}` |

---

## **Notes**  
1. **Profile Image**: Use the `/api/media/upload` endpoint to upload an image and get a URL, then pass it in the `profileImage` field.
2. **Addresses**: Optional; only one address can be marked as `isDefault`.  
3. **Soft Delete**: Deactivation sets `status` to "inactive" instead of removing the user from the database.  
4. **Password Update**: If provided, the password is hashed before saving.

---

### **Example Workflow**  
1. **Get Profile** → `GET /` to view current details.  
2. **Upload Image** → `POST /api/media/upload` to get a profile image URL.  
3. **Update Profile** → `PUT /` with new details (e.g., name, image URL).  
4. **Delete Profile** → `DELETE /` to deactivate the account.  

This API enables users to **manage their profiles** effectively. 👤✨

---

### Integration Notes
- Ensure `bcryptjs` is installed (`npm install bcryptjs`) for password hashing.
- The `authMiddleware` from `middleware/auth.js` is assumed to exist and work as in your previous code.
- Test locally with the provided `curl` requests, replacing `<jwt-token>` with a valid token from `/api/auth/login`.

Let me know if you need further adjustments or additional endpoints!