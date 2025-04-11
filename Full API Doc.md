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
            "status": "active",
            "createdAt": "2025-03-30T03:29:07.653Z",
            "updatedAt": "2025-03-30T03:29:07.653Z",
            "__v": 0,
            "categoryOption": [
                {
                    "_id": "67e8ba833dc8d3b9b6c66610",
                    "name": "Engine Components",
                    "parentCategory": {
                        "_id": "67e8ba833dc8d3b9b6c6660d",
                        "name": "Engine & Transmission"
                    },
                    "status": "active",
                    "createdAt": "2025-03-30T03:29:07.798Z",
                    "updatedAt": "2025-03-30T03:29:07.798Z",
                    "__v": 0
                },
                {
                    "_id": "67e8ba833dc8d3b9b6c66612",
                    "name": "Transmission & Drivetrain",
                    "parentCategory": {
                        "_id": "67e8ba833dc8d3b9b6c6660d",
                        "name": "Engine & Transmission"
                    },
                    "status": "active",
                    "createdAt": "2025-03-30T03:29:07.947Z",
                    "updatedAt": "2025-03-30T03:29:07.947Z",
                    "__v": 0
                },
                {
                    "_id": "67e8ba843dc8d3b9b6c66614",
                    "name": "Fuel System",
                    "parentCategory": {
                        "_id": "67e8ba833dc8d3b9b6c6660d",
                        "name": "Engine & Transmission"
                    },
                    "status": "active",
                    "createdAt": "2025-03-30T03:29:08.111Z",
                    "updatedAt": "2025-03-30T03:29:08.111Z",
                    "__v": 0
                },
                {
                    "_id": "67e8ba843dc8d3b9b6c66616",
                    "name": "Cooling System",
                    "parentCategory": {
                        "_id": "67e8ba833dc8d3b9b6c6660d",
                        "name": "Engine & Transmission"
                    },
                    "status": "active",
                    "createdAt": "2025-03-30T03:29:08.271Z",
                    "updatedAt": "2025-03-30T03:29:08.271Z",
                    "__v": 0
                }
            ]
        },
        {
            "_id": "67e8ba843dc8d3b9b6c6660f",
            "name": "Suspension & Steering",
            "parentCategory": null,
            "status": "active",
            "createdAt": "2025-03-30T03:29:08.433Z",
            "updatedAt": "2025-03-30T03:29:08.433Z",
            "__v": 0,
            "categoryOption": [
                {
                    "_id": "67e8ba843dc8d3b9b6c66611",
                    "name": "Suspension Components",
                    "parentCategory": {
                        "_id": "67e8ba843dc8d3b9b6c6660f",
                        "name": "Suspension & Steering"
                    },
                    "status": "active",
                    "createdAt": "2025-03-30T03:29:08.598Z",
                    "updatedAt": "2025-03-30T03:29:08.598Z",
                    "__v": 0
                },
                {
                    "_id": "67e8ba843dc8d3b9b6c66613",
                    "name": "Steering Components",
                    "parentCategory": {
                        "_id": "67e8ba843dc8d3b9b6c6660f",
                        "name": "Suspension & Steering"
                    },
                    "status": "active",
                    "createdAt": "2025-03-30T03:29:08.762Z",
                    "updatedAt": "2025-03-30T03:29:08.762Z",
                    "__v": 0
                }
            ]
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
| Status Code | Description                                      | Example Response                                                   |
| ----------- | ------------------------------------------------ | ------------------------------------------------------------------ |
| **400**     | Invalid input (e.g., multiple default addresses) | `{"success": false, "message": "Only one address can be default"}` |
| **401**     | Unauthorized (invalid JWT)                       | `{"success": false, "message": "No token, authorization denied"}`  |
| **404**     | User not found                                   | `{"success": false, "message": "User not found"}`                  |
| **500**     | Server error                                     | `{"success": false, "message": "Internal server error"}`           |

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

Below is the API documentation for the Courier-related APIs in the requested format, covering the individual endpoints from `courierController.js`. This includes `GET /orders`, `GET /order/:id`, `PUT /order/:id/status`, `POST /order/:id/report-issue`, and `GET /analytics`, with the automatic tracking number generation feature included.

---

# **Courier API Documentation**
**Base URL**: `http://localhost:3000/api/courier`

---

## **Authentication**
All endpoints require a **JWT token** in the `Authorization` header:  
```
Authorization: Bearer <courier-jwt-token>
```

---

## **1. Courier Management**

### **1.1 Get Courier Orders**
**Endpoint**: `GET /orders`  
**Description**: Retrieves all orders assigned to the courier or available in their region (district) with status "Shipped" and no assigned courier.

**Example Request**:  
```bash
curl --location 'http://localhost:3000/api/courier/orders' \
--header 'Authorization: Bearer <courier-jwt-token>'
```

**Response**:  
```json
{
  "success": true,
  "data": [
    {
      "_id": "67eedcf8c2c643224aa2588b",
      "buyerId": {
        "_id": "67e754164d954ec2d0057524",
        "name": "Buyer User",
        "email": "buyer@example.com",
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
          "sellerStatus": "Shipped"
        }
      ],
      "total": 150,
      "status": "Shipped",
      "shippingAddress": {
        "street": "123 Main St",
        "city": "Colombo",
        "district": "Colombo",
        "country": "Sri Lanka",
        "postalCode": "10000"
      },
      "courierDetails": {
        "courierId": null,
        "trackingNumber": "TRK-12345"
      },
      "courierStatus": "Pending",
      "statusHistory": [
        {
          "status": "Handed over to courier service",
          "updatedBy": { "role": "seller", "userId": "67e754164d954ec2d0057521" },
          "updatedAt": "2025-04-05T10:00:00.000Z"
        }
      ],
      "createdAt": "2025-04-05T09:00:00.000Z",
      "updatedAt": "2025-04-05T10:00:00.000Z"
    }
  ]
}
```

---

### **1.2 Get Courier Order by ID**
**Endpoint**: `GET /order/:id`  
**Description**: Retrieves details of a specific order assigned to the courier or available in their region.

**Example Request**:  
```bash
curl --location 'http://localhost:3000/api/courier/order/67eedcf8c2c643224aa2588b' \
--header 'Authorization: Bearer <courier-jwt-token>'
```

**Response**:  
```json
{
  "success": true,
  "data": {
    "_id": "67eedcf8c2c643224aa2588b",
    "buyerId": {
      "_id": "67e754164d954ec2d0057524",
      "name": "Buyer User",
      "email": "buyer@example.com",
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
        "sellerStatus": "Shipped"
      }
    ],
    "total": 150,
    "status": "Shipped",
    "shippingAddress": {
      "street": "123 Main St",
      "city": "Colombo",
      "district": "Colombo",
      "country": "Sri Lanka",
      "postalCode": "10000"
    },
    "courierDetails": {
      "courierId": "67e754164d954ec2d0057525",
      "trackingNumber": "TRK-123456-7890"
    },
    "courierStatus": "Picked Up",
    "statusHistory": [
      {
        "status": "Courier assigned",
        "updatedBy": { "role": "courier", "userId": "67e754164d954ec2d0057525" },
        "updatedAt": "2025-04-05T11:00:00.000Z"
      },
      {
        "status": "Courier updated to Picked Up",
        "updatedBy": { "role": "courier", "userId": "67e754164d954ec2d0057525" },
        "updatedAt": "2025-04-05T11:00:00.000Z"
      }
    ],
    "createdAt": "2025-04-05T09:00:00.000Z",
    "updatedAt": "2025-04-05T11:00:00.000Z"
  }
}
```

---

### **1.3 Update Courier Status**
**Endpoint**: `PUT /order/:id/status`  
**Description**: Updates the courier status of an order. A tracking number is automatically generated when status is set to "Picked Up" if not already present.

**Request Body**:  
```json
{
  "status": "string", // "Picked Up", "In Transit", "Out for Delivery", "Delivered", "Failed Delivery"
  "reason": "string"  // Required if status is "Failed Delivery"
}
```

**Example Request**:  
```bash
curl --location --request PUT 'http://localhost:3000/api/courier/order/67eedcf8c2c643224aa2588b/status' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer <courier-jwt-token>' \
--data '{
  "status": "Picked Up"
}'
```

**Response**:  
```json
{
  "success": true,
  "message": "Courier status updated to Picked Up",
  "trackingNumber": "TRK-123456-7890"
}
```

**Example Request (Failed Delivery)**:  
```bash
curl --location --request PUT 'http://localhost:3000/api/courier/order/67eedcf8c2c643224aa2588b/status' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer <courier-jwt-token>' \
--data '{
  "status": "Failed Delivery",
  "reason": "Customer not available"
}'
```

**Response**:  
```json
{
  "success": true,
  "message": "Courier status updated to Failed Delivery",
  "trackingNumber": "TRK-123456-7890"
}
```

---

### **1.4 Report Delivery Issue**
**Endpoint**: `POST /order/:id/report-issue`  
**Description**: Reports a delivery issue for an order, setting the courier status to "Failed Delivery".

**Request Body**:  
```json
{
  "reason": "string" // Required
}
```

**Example Request**:  
```bash
curl --location --request POST 'http://localhost:3000/api/courier/order/67eedcf8c2c643224aa2588b/report-issue' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer <courier-jwt-token>' \
--data '{
  "reason": "Address not found"
}'
```

**Response**:  
```json
{
  "success": true,
  "message": "Delivery issue reported"
}
```

---

### **1.5 Get Courier Analytics**
**Endpoint**: `GET /analytics`  
**Description**: Retrieves analytics data for the courier, including order status breakdown, success rate, and average delivery time.

**Example Request**:  
```bash
curl --location 'http://localhost:3000/api/courier/analytics' \
--header 'Authorization: Bearer <courier-jwt-token>'
```

**Response**:  
```json
{
  "success": true,
  "data": {
    "statusBreakdown": {
      "totalOrders": 20,
      "pending": 5,
      "pickedUp": 3,
      "inTransit": 2,
      "outForDelivery": 1,
      "delivered": 8,
      "failed": 1
    },
    "successRate": "88.89",
    "averageDeliveryTime": "24.50 hours"
  }
}
```

---

## **Error Responses**
| Status Code | Description                  | Example Response                                      |
|-------------|------------------------------|-----------------------------------------------------|
| **400**     | Invalid input (e.g., invalid status) | `{"success": false, "message": "Invalid status"}` |
| **400**     | Missing reason for "Failed Delivery" | `{"success": false, "message": "Reason required for Failed Delivery"}` |
| **401**     | Unauthorized (invalid JWT)   | `{"success": false, "message": "No token, authorization denied"}` |
| **403**     | Order assigned to another courier | `{"success": false, "message": "Order assigned to another courier"}` |
| **404**     | Order not found or inaccessible | `{"success": false, "message": "Order not found or not accessible"}` |
| **500**     | Server error                 | `{"success": false, "message": "Internal server error"}` |

---

### Notes
- **Tracking Number**: Automatically generated on "Picked Up" status if not already set (e.g., `TRK-123456-7890`).
- **Order Access**: Couriers can only access orders assigned to them or unassigned orders in their region (district).
- **Status History**: Updates are logged in `statusHistory` for all status changes and issue reports.

---

# **Analytics API Documentation**

---

## **Authentication**
All endpoints require a **JWT token** in the `Authorization` header:  
```
Authorization: Bearer <token>
```

---

## **1. Admin Analytics**
**Base URL**: `http://localhost:3000/api/admin`

### **1.1 Get Admin Analytics**
**Endpoint**: `GET /analytics`  
**Description**: Retrieves comprehensive analytics for the admin panel, including total orders, revenue, user stats, order status breakdown, top products, top sellers, and courier performance.

**Example Request**:  
```bash
curl --location 'http://localhost:3000/api/admin/analytics' \
--header 'Authorization: Bearer <admin-jwt-token>'
```

**Response**:  
```json
{
  "success": true,
  "data": {
    "totalOrders": 100,
    "totalRevenue": 75000,
    "usersByRole": {
      "admin": 1,
      "seller": 10,
      "courier": 5,
      "buyer": 50
    },
    "orderStatusBreakdown": {
      "Pending": 20,
      "Confirmed": 30,
      "Shipped": 25,
      "Delivered": 20,
      "Cancelled": 5
    },
    "topProducts": [
      {
        "_id": "67eed74dc1daf109f57052ca",
        "title": "Toyota Camry Brake Pads",
        "totalSold": 50,
        "revenue": 3750
      }
    ],
    "topSellers": [
      {
        "_id": "67e754164d954ec2d0057521",
        "name": "Seller 1",
        "storeName": "Auto Parts",
        "revenue": 20000
      }
    ],
    "courierPerformance": [
      {
        "_id": "67e754164d954ec2d0057525",
        "name": "Courier 1",
        "delivered": 15,
        "failed": 2
      }
    ]
  }
}
```

---

## **2. Seller Analytics**
**Base URL**: `http://localhost:3000/api/seller`

### **2.1 Get Seller Analytics**
**Endpoint**: `GET /analytics`  
**Description**: Retrieves analytics for the authenticated seller, including total orders, revenue, status breakdown for their items, top products, and low stock products.

**Example Request**:  
```bash
curl --location 'http://localhost:3000/api/seller/analytics' \
--header 'Authorization: Bearer <seller-jwt-token>'
```

**Response**:  
```json
{
  "success": true,
  "data": {
    "totalOrders": 25,
    "totalRevenue": 20000,
    "statusBreakdown": {
      "Pending": 5,
      "Processing": 5,
      "Shipped": 10,
      "Delivered": 5
    },
    "topProducts": [
      {
        "_id": "67eed74dc1daf109f57052ca",
        "title": "Toyota Camry Brake Pads",
        "totalSold": 50,
        "revenue": 3750
      }
    ],
    "lowStockProducts": [
      {
        "_id": "67eed74dc1daf109f57052cb",
        "title": "Honda Civic Oil Filter",
        "stock": 8
      }
    ]
  }
}
```

---

## **3. Courier Analytics**
**Base URL**: `http://localhost:3000/api/courier`

### **3.1 Get Courier Analytics**
**Endpoint**: `GET /analytics`  
**Description**: Retrieves analytics for the authenticated courier, including order status breakdown, delivery success rate, and average delivery time.

**Example Request**:  
```bash
curl --location 'http://localhost:3000/api/courier/analytics' \
--header 'Authorization: Bearer <courier-jwt-token>'
```

**Response**:  
```json
{
  "success": true,
  "data": {
    "statusBreakdown": {
      "totalOrders": 20,
      "pending": 5,
      "pickedUp": 3,
      "inTransit": 2,
      "outForDelivery": 1,
      "delivered": 8,
      "failed": 1
    },
    "successRate": "88.89",
    "averageDeliveryTime": "24.50 hours"
  }
}
```

---

## **4. Buyer Analytics**
**Base URL**: `http://localhost:3000/api/buyer`

### **4.1 Get Buyer Analytics**
**Endpoint**: `GET /analytics`  
**Description**: Retrieves analytics for the authenticated buyer, including total orders, total spent, average order value, and order status breakdown.

**Example Request**:  
```bash
curl --location 'http://localhost:3000/api/buyer/analytics' \
--header 'Authorization: Bearer <buyer-jwt-token>'
```

**Response**:  
```json
{
  "success": true,
  "data": {
    "totalOrders": 10,
    "totalSpent": 1500,
    "averageOrderValue": "150.00",
    "statusBreakdown": {
      "Pending": 2,
      "Confirmed": 3,
      "Shipped": 2,
      "Delivered": 3
    }
  }
}
```

---

## **Error Responses**
| Status Code | Description                  | Example Response                                      |
|-------------|------------------------------|-----------------------------------------------------|
| **400**     | Invalid role or missing data | `{"success": false, "message": "Invalid courier or region not assigned"}` (Courier) |
| **401**     | Unauthorized (invalid JWT)   | `{"success": false, "message": "No token, authorization denied"}` |
| **403**     | Access denied (wrong role)   | `{"success": false, "message": "Access denied, admins only"}` (Admin) |
| **500**     | Server error                 | `{"success": false, "message": "Internal server error"}` |

---

### Notes
- **Admin**: Requires `role: "admin"` in the JWT payload.
- **Seller**: Filters data to the authenticated seller’s products only.
- **Courier**: Filters data to the authenticated courier’s assigned orders; `averageDeliveryTime` is "N/A" if no completed deliveries exist.
- **Buyer**: Reflects the authenticated buyer’s order history; revenue/spending is based on "Delivered" orders.

This documentation covers all "Get Analytics" endpoints as requested. Let me know if you need additional details or adjustments!

# Admin Oversight

---
#### 1. Get All Orders
**Endpoint**: `GET /api/admin/orders`  
**Description**: Retrieves all orders in the system with optional filters for status, date range, and district.

**Query Parameters**:  
- `status` (optional): Filter by order status (e.g., "Pending", "Shipped", "Delivered").
- `startDate` (optional): Filter orders created on or after this date (ISO format, e.g., "2025-01-01").
- `endDate` (optional): Filter orders created on or before this date (ISO format, e.g., "2025-04-07").
- `district` (optional): Filter by shipping address district (e.g., "Colombo").

**Example Request**:  
```bash
curl --location 'http://localhost:3000/api/admin/orders?status=Shipped&district=Colombo' \
--header 'Authorization: Bearer <admin-jwt-token>'
```

**Response**:  
```json
{
  "success": true,
  "data": [
    {
      "_id": "67eedcf8c2c643224aa2588b",
      "buyerId": {
        "_id": "67e754164d954ec2d0057524",
        "name": "Buyer User",
        "email": "buyer@example.com",
        "phone": "+1234567890"
      },
      "items": [
        {
          "productId": {
            "_id": "67eed74dc1daf109f57052ca",
            "title": "Toyota Camry Brake Pads",
            "price": 75,
            "brand": "Bosch",
            "condition": "New",
            "images": ["https://example.com/brake-pads.jpg"]
          },
          "quantity": 2,
          "price": 75,
          "sellerStatus": "Shipped"
        }
      ],
      "total": 150,
      "status": "Shipped",
      "shippingAddress": {
        "street": "123 Main St",
        "city": "Colombo",
        "district": "Colombo",
        "country": "Sri Lanka",
        "postalCode": "10000"
      },
      "courierDetails": {
        "courierId": {
          "_id": "67e754164d954ec2d0057525",
          "name": "Courier 1",
          "phone": "+9876543210",
          "region": "Colombo"
        },
        "trackingNumber": "TRK-123456-7890"
      },
      "courierStatus": "Picked Up",
      "statusHistory": [
        {
          "status": "Handed over to courier service",
          "updatedBy": { "role": "seller", "userId": "67e754164d954ec2d0057521" },
          "updatedAt": "2025-04-05T10:00:00.000Z"
        }
      ],
      "createdAt": "2025-04-05T09:00:00.000Z",
      "updatedAt": "2025-04-05T11:00:00.000Z"
    }
  ]
}
```

---

#### 2. Get All Products
**Endpoint**: `GET /api/admin/products`  
**Description**: Retrieves all products listed in the system with optional filters for status, category, and seller.

**Query Parameters**:  
- `status` (optional): Filter by product status (e.g., "active", "deleted").
- `category` (optional): Filter by category ID (e.g., "67eed74dc1daf109f57052ca").
- `sellerId` (optional): Filter by seller ID (e.g., "67e754164d954ec2d0057521").

**Example Request**:  
```bash
curl --location 'http://localhost:3000/api/admin/products?status=active&sellerId=67e754164d954ec2d0057521' \
--header 'Authorization: Bearer <admin-jwt-token>'
```

**Response**:  
```json
{
  "success": true,
  "data": [
    {
      "_id": "67eed74dc1daf109f57052ca",
      "title": "Toyota Camry Brake Pads",
      "description": "High-quality brake pads for Toyota Camry",
      "price": 75,
      "category": {
        "_id": "67eed74dc1daf109f57052cb",
        "name": "Brake Parts"
      },
      "stock": 20,
      "condition": "New",
      "brand": "Bosch",
      "oem": "OEM123",
      "aftermarket": false,
      "material": "Ceramic",
      "makeModel": [
        { "make": "Toyota", "model": "Camry" }
      ],
      "years": [2019, 2020],
      "availability": "In Stock",
      "images": ["https://example.com/brake-pads.jpg"],
      "sellerId": {
        "_id": "67e754164d954ec2d0057521",
        "name": "Seller 1",
        "email": "seller1@example.com",
        "storeName": "Auto Parts Hub"
      },
      "status": "active",
      "createdAt": "2025-01-01T08:00:00.000Z",
      "updatedAt": "2025-04-05T10:00:00.000Z"
    }
  ]
}
```



---

# **Buyer Product Filter Options API Documentation**
**Base URL**: `http://localhost:3000/api/buyer`

---

## **Authentication**
All endpoints require a **JWT token** in the `Authorization` header:  
```
Authorization: Bearer <buyer-jwt-token>
```

---

## **1. Get Product Filter Options**
**Endpoint**: `GET /product-filter-options`  
**Description**: Retrieves all available filter options for product search dropdowns based on the `Product` model. This includes dynamic values from the database for fields like condition, brand, OEM, material, availability, aftermarket, make, model, years, and category, as well as a calculated price range for numeric inputs. The response is designed to populate frontend dropdown menus and price range inputs.

**Example Request**:  
```bash
curl --location 'http://localhost:3000/api/buyer/product-filter-options' \
--header 'Authorization: Bearer <buyer-jwt-token>'
```

**Response**:  
```json
{
  "success": true,
  "data": {
    "condition": ["All", "New", "Refurbished", "Used"],
    "brand": ["All", "Bosch", "Brembo", "Denso"],
    "oem": ["All", "OEM123", "OEM456", "OEM789"],
    "material": ["All", "Aluminum", "Cast Iron", "Ceramic", "Steel"],
    "availability": ["All", "In Stock", "Pre-order"],
    "aftermarket": ["All", "true", "false"],
    "make": ["All", "Chrysler", "Nissan", "Toyota"],
    "model": ["All", "Altima", "Camry", "Corolla", "Pacifica"],
    "years": ["All", "2015", "2016", "2018", "2019", "2020"],
    "category": ["All", "67eed74dc1daf109f57052ca", "67eed74dc1daf109f57052cb"],
    "priceRange": {
      "min": 10,
      "max": 500
    }
  }
}
```

---

## **Response Fields**
| **Field**         | **Type**         | **Description**                                                                 |
|-------------------|------------------|--------------------------------------------------------------------------------|
| `condition`       | Array of Strings | All possible product conditions, with "All" as the default option.             |
| `brand`           | Array of Strings | Unique brands from products, with "All" added.                                 |
| `oem`             | Array of Strings | Unique OEM numbers from products, with "All" added.                            |
| `material`        | Array of Strings | Unique materials from products, with "All" added.                              |
| `availability`    | Array of Strings | All possible availability statuses, with "All" added.                          |
| `aftermarket`     | Array of Strings | Boolean options as strings ("true", "false"), with "All" added.                |
| `make`            | Array of Strings | Unique makes from the `makeModel` array, with "All" added.                     |
| `model`           | Array of Strings | Unique models from the `makeModel` array, with "All" added.                    |
| `years`           | Array of Strings | Unique years from products, converted to strings, with "All" added.            |
| `category`        | Array of Strings | Unique category IDs from products, with "All" added.                           |
| `priceRange`      | Object           | Minimum and maximum price range for numeric inputs.                            |
| `priceRange.min`  | Number           | Lowest price among active products (defaults to 0 if no data).                 |
| `priceRange.max`  | Number           | Highest price among active products (defaults to 1000 if no data).             |

---

## **Error Responses**
| Status Code | Description                  | Example Response                                      |
|-------------|------------------------------|-----------------------------------------------------|
| **401**     | Unauthorized (invalid JWT)   | `{"success": false, "message": "No token, authorization denied"}` |
| **500**     | Server error                 | `{"success": false, "message": "Internal server error"}` |


---

# **Analytics API Documentation**

## **Base URL**
- **Admin**: `http://localhost:3000/api/admin`
- **Seller**: `http://localhost:3000/api/seller`
- **Courier**: `http://localhost:3000/api/courier`

---

## **Authentication**
All endpoints require a **JWT token** in the `Authorization` header:  
```
Authorization: Bearer <jwt-token>
```
- The token must correspond to the appropriate role:
  - Admin endpoints: `role: "admin"`
  - Seller endpoints: `role: "seller"`
  - Courier endpoints: `role: "courier"`

---

# **1. Admin Analytics API**

## **Get Admin Analytics**
**Endpoint**: `GET /analytics`  
**Base URL**: `http://localhost:3000/api/admin`  
**Description**: Retrieves comprehensive analytics for the admin, including total orders, revenue, user counts by role, order status breakdown, top products, top sellers, and courier performance.

**Example Request**:  
```bash
curl --location 'http://localhost:3000/api/admin/analytics' \
--header 'Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6IjY3ZTYyMzdhMTIwOTk4NjA0OGRiODU2MCIsInJvbGUiOiJhZG1pbiIsImlhdCI6MTc0NDM1MjIyMiwiZXhwIjoxNzQ0MzU1ODIyfQ.-PMIYd0k51WtATVPk7lqpT-jRTELRVxvYXMHvoBiCoU'
```

**Response**:  
```json
{
    "success": true,
    "data": {
        "totalOrders": 3,
        "totalRevenue": 300,
        "usersByRole": {
            "admin": 2,
            "courier": 4,
            "buyer": 4,
            "seller": 3
        },
        "orderStatusBreakdown": {
            "Shipped": 1,
            "Pending": 1,
            "Delivered": 1
        },
        "topProducts": [],
        "topSellers": [],
        "courierPerformance": [
            {
                "_id": "67f2823d2fe8a424edc522bd",
                "delivered": 1,
                "failed": 1,
                "name": "sam courier"
            }
        ]
    }
}
```

### **Response Fields**
| **Field**                | **Type**         | **Description**                                                                 |
|--------------------------|------------------|--------------------------------------------------------------------------------|
| `totalOrders`            | Number           | Total number of orders in the system.                                          |
| `totalRevenue`           | Number           | Total revenue from all orders (in dollars).                                    |
| `usersByRole`            | Object           | Count of users by role (keys: `admin`, `courier`, `buyer`, `seller`).          |
| `orderStatusBreakdown`   | Object           | Breakdown of orders by status (e.g., `Shipped`, `Pending`, `Delivered`).       |
| `topProducts`            | Array            | List of top-selling products (empty if no data).                               |
| `topSellers`             | Array            | List of top-performing sellers (empty if no data).                             |
| `courierPerformance`     | Array of Objects | Performance metrics for couriers.                                              |
| `courierPerformance._id` | String           | Courier’s unique ID.                                                           |
| `courierPerformance.delivered` | Number     | Number of orders successfully delivered by the courier.                        |
| `courierPerformance.failed` | Number      | Number of orders that failed delivery by the courier.                          |
| `courierPerformance.name` | String        | Name of the courier.                                                           |

---

# **2. Admin Orders API**

## **Get Orders**
**Endpoint**: `GET /orders`  
**Base URL**: `http://localhost:3000/api/admin`  
**Description**: Retrieves a list of orders with optional filters for status, district, start date, and end date. Includes detailed order information such as buyer, items, shipping address, and courier details.

**Query Parameters**:  
- `status` (optional): Filter by order status (e.g., `Shipped`, `Pending`).
- `district` (optional): Filter by shipping district (e.g., `Puttalam`).
- `startDate` (optional): Filter by orders created after this ISO date.
- `endDate` (optional): Filter by orders created before this ISO date.

**Example Request**:  
```bash
curl --location 'http://localhost:3000/api/admin/orders?status=Shipped&district=Puttalam' \
--header 'Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6IjY3ZTYyMzdhMTIwOTk4NjA0OGRiODU2MCIsInJvbGUiOiJhZG1pbiIsImlhdCI6MTc0NDM1MjIyMiwiZXhwIjoxNzQ0MzU1ODIyfQ.-PMIYd0k51WtATVPk7lqpT-jRTELRVxvYXMHvoBiCoU'
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
                "postalCode": "10000",
                "district": "Puttalam"
            },
            "courierDetails": {
                "courierId": {
                    "_id": "67f2823d2fe8a424edc522bd",
                    "name": "sam courier",
                    "phone": "+94770470323",
                    "region": "Puttalam"
                },
                "trackingNumber": "TRK-12345"
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
                        "images": ["https://example.com/brake-pads.jpg"]
                    },
                    "quantity": 2,
                    "price": 75,
                    "sellerStatus": "Shipped",
                    "_id": "67eed9faa46e254dd61d172a"
                }
            ],
            "total": 150,
            "status": "Shipped",
            "statusHistory": [
                {
                    "status": "Handed over to courier service",
                    "updatedAt": "2025-04-05T20:06:28.527Z",
                    "_id": "67f18d4497a13a1892047b8d"
                },
                {
                    "status": "Courier updated to Picked Up",
                    "updatedAt": "2025-04-06T13:51:02.991Z",
                    "_id": "67f286c62fe8a424edc522d5"
                },
                {
                    "status": "Courier updated to Picked Up",
                    "updatedAt": "2025-04-06T18:54:50.430Z",
                    "_id": "67f2cdfa7d3451ffb102e68b"
                },
                {
                    "status": "Delivery failed: Address not found",
                    "updatedAt": "2025-04-06T18:55:54.423Z",
                    "_id": "67f2ce3a7d3451ffb102e692"
                }
            ],
            "createdAt": "2025-04-03T18:56:58.631Z",
            "updatedAt": "2025-04-06T18:55:54.426Z",
            "__v": 4,
            "courierStatus": "Failed Delivery"
        }
    ]
}
```

### **Response Fields**
| **Field**                  | **Type**         | **Description**                                                                 |
|----------------------------|------------------|--------------------------------------------------------------------------------|
| `shippingAddress`          | Object           | Shipping address details.                                                      |
| `shippingAddress.street`   | String           | Street address.                                                                |
| `shippingAddress.city`     | String           | City.                                                                          |
| `shippingAddress.country`  | String           | Country.                                                                       |
| `shippingAddress.postalCode` | String        | Postal code.                                                                   |
| `shippingAddress.district` | String           | District.                                                                      |
| `courierDetails`           | Object           | Courier information.                                                           |
| `courierDetails.courierId` | Object           | Courier details (ID, name, phone, region).                                     |
| `courierDetails.trackingNumber` | String     | Tracking number for the shipment.                                              |
| `_id`                      | String           | Order ID.                                                                      |
| `buyerId`                  | Object           | Buyer details (ID, email, name, phone).                                        |
| `items`                    | Array of Objects | List of items in the order.                                                    |
| `items.productId`          | Object           | Product details (ID, title, price, condition, brand, images).                  |
| `items.quantity`           | Number           | Quantity ordered.                                                              |
| `items.price`              | Number           | Price per unit.                                                                |
| `items.sellerStatus`       | String           | Seller’s status for this item (e.g., `Shipped`).                               |
| `total`                    | Number           | Total order amount (in dollars).                                               |
| `status`                   | String           | Current order status (e.g., `Shipped`).                                        |
| `statusHistory`            | Array of Objects | History of status updates with timestamps.                                     |
| `createdAt`                | String           | Order creation timestamp (ISO).                                                |
| `updatedAt`                | String           | Last update timestamp (ISO).                                                   |
| `courierStatus`            | String           | Current courier status (e.g., `Failed Delivery`).                              |

---

# **3. Admin Products API**

## **Get Products**
**Endpoint**: `GET /products`  
**Base URL**: `http://localhost:3000/api/admin`  
**Description**: Retrieves a list of products with optional filters for status, category, and seller ID. Includes detailed product information such as title, price, stock, and seller details.

**Query Parameters**:  
- `status` (optional): Filter by product status (e.g., `active`).
- `category` (optional): Filter by category ID.
- `sellerId` (optional): Filter by seller ID.

**Example Request**:  
```bash
curl --location 'http://localhost:3000/api/admin/products?status=active&sellerId=67e754164d954ec2d0057521' \
--header 'Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6IjY3ZTYyMzdhMTIwOTk4NjA0OGRiODU2MCIsInJvbGUiOiJhZG1pbiIsImlhdCI6MTc0NDM1MjIyMiwiZXhwIjoxNzQ0MzU1ODIyfQ.-PMIYd0k51WtATVPk7lqpT-jRTELRVxvYXMHvoBiCoU'
```

**Response**:  
```json
{
    "success": true,
    "data": [
        {
            "_id": "67eed74dc1daf109f57052ca",
            "title": "Toyota Camry Brake Pads",
            "description": "High-quality ceramic brake pads for Toyota vehicles",
            "price": 75,
            "category": {
                "_id": "67e8ba893dc8d3b9b6c6665e",
                "name": "Transmission Fluids"
            },
            "stock": 2,
            "condition": "New",
            "brand": "Bosch",
            "oem": "OEM123",
            "aftermarket": false,
            "material": "Ceramic",
            "makeModel": [
                {
                    "make": "Toyota",
                    "model": "Camry",
                    "_id": "67eed74dc1daf109f57052cb"
                },
                {
                    "make": "Toyota",
                    "model": "Corolla",
                    "_id": "67eed74dc1daf109f57052cc"
                }
            ],
            "years": [2015, 2020],
            "availability": "In Stock",
            "images": ["https://example.com/brake-pads.jpg"],
            "sellerId": {
                "_id": "67e754164d954ec2d0057521",
                "email": "seller@example.com",
                "name": "Updated seller"
            },
            "status": "active",
            "createdAt": "2025-04-03T18:45:33.826Z",
            "updatedAt": "2025-04-03T18:45:33.826Z",
            "__v": 0
        }
    ]
}
```

### **Response Fields**
| **Field**         | **Type**         | **Description**                            |
| ----------------- | ---------------- | ------------------------------------------ |
| `_id`             | String           | Product ID.                                |
| `title`           | String           | Product title.                             |
| `description`     | String           | Product description.                       |
| `price`           | Number           | Price per unit (in dollars).               |
| `category`        | Object           | Category details (ID, name).               |
| `stock`           | Number           | Available stock quantity.                  |
| `condition`       | String           | Product condition (e.g., `New`).           |
| `brand`           | String           | Product brand (e.g., `Bosch`).             |
| `oem`             | String           | OEM number.                                |
| `aftermarket`     | Boolean          | Indicates if the product is aftermarket.   |
| `material`        | String           | Material of the product (e.g., `Ceramic`). |
| `makeModel`       | Array of Objects | Compatible makes and models.               |
| `makeModel.make`  | String           | Vehicle make (e.g., `Toyota`).             |
| `makeModel.model` | String           | Vehicle model (e.g., `Camry`).             |
| `years`           | Array of Numbers | Compatible years.                          |
| `availability`    | String           | Stock availability (e.g., `In Stock`).     |
| `images`          | Array of Strings | URLs of product images.                    |
| `sellerId`        | Object           | Seller details (ID, email, name).          |
| `status`          | String           | Product status (e.g., `active`).           |
| `createdAt`       | String           | Creation timestamp (ISO).                  |
| `updatedAt`       | String           | Last update timestamp (ISO).               |

---

# **4. Seller Analytics API**

## **Get Seller Analytics**
**Endpoint**: `GET /analytics`  
**Base URL**: `http://localhost:3000/api/seller`  
**Description**: Retrieves analytics specific to the authenticated seller, including total orders, revenue, order status breakdown, top products, and low-stock products.

**Example Request**:  
```bash
curl --location 'http://localhost:3000/api/seller/analytics' \
--header 'Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6IjY3ZTc1NDE2NGQ5NTRlYzJkMDA1NzUyMSIsInJvbGUiOiJzZWxsZXIiLCJpYXQiOjE3NDQzNTI0NzgsImV4cCI6MTc0NDM1NjA3OH0.XxkmlecTHMUuAj-Oh3h_PymO_s8cYcCmlNqy3JxXDfU'
```

**Response**:  
```json
{
    "success": true,
    "data": {
        "totalOrders": 3,
        "totalRevenue": 0,
        "statusBreakdown": {
            "Shipped": 3
        },
        "topProducts": [],
        "lowStockProducts": [
            {
                "_id": "67eed74dc1daf109f57052ca",
                "title": "Toyota Camry Brake Pads",
                "stock": 2
            }
        ]
    }
}
```

### **Response Fields**
| **Field**                | **Type**         | **Description**                                            |
| ------------------------ | ---------------- | ---------------------------------------------------------- |
| `totalOrders`            | Number           | Total number of orders for the seller.                     |
| `totalRevenue`           | Number           | Total revenue from the seller’s orders (in dollars).       |
| `statusBreakdown`        | Object           | Breakdown of orders by status (e.g., `Shipped`).           |
| `topProducts`            | Array            | List of top-selling products (empty if no data).           |
| `lowStockProducts`       | Array of Objects | List of products with low stock.                           |
| `lowStockProducts._id`   | String           | Product ID.                                                |
| `lowStockProducts.title` | String           | Product title.                                             |
| `lowStockProducts.stock` | Number           | Current stock quantity (low threshold defined by backend). |

---

# **5. Courier Analytics API**

## **Get Courier Analytics**
**Endpoint**: `GET /analytics`  
**Base URL**: `http://localhost:3000/api/courier`  
**Description**: Retrieves analytics specific to the authenticated courier, including order status breakdown, success rate, and average delivery time.

**Example Request**:  
```bash
curl --location 'http://localhost:3000/api/courier/analytics' \
--header 'Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6IjY3ZjI4MjNkMmZlOGE0MjRlZGM1MjJiZCIsInJvbGUiOiJjb3VyaWVyIiwiaWF0IjoxNzQ0MzUyNjEwLCJleHAiOjE3NDQzNTYyMTB9.buoNIcz1ZwywbS3yrfZDRo7Gq7yUwfR6RWFRQKDmA5o'
```

**Response**:  
```json
{
    "success": true,
    "data": {
        "statusBreakdown": {
            "totalOrders": 2,
            "pending": 0,
            "pickedUp": 0,
            "inTransit": 0,
            "outForDelivery": 0,
            "delivered": 1,
            "failed": 1
        },
        "successRate": "50.00",
        "averageDeliveryTime": "N/A"
    }
}
```

### **Response Fields**
| **Field**                        | **Type** | **Description**                                          |
| -------------------------------- | -------- | -------------------------------------------------------- |
| `statusBreakdown`                | Object   | Breakdown of orders by courier status.                   |
| `statusBreakdown.totalOrders`    | Number   | Total number of orders assigned to the courier.          |
| `statusBreakdown.pending`        | Number   | Orders in `pending` status.                              |
| `statusBreakdown.pickedUp`       | Number   | Orders in `pickedUp` status.                             |
| `statusBreakdown.inTransit`      | Number   | Orders in `inTransit` status.                            |
| `statusBreakdown.outForDelivery` | Number   | Orders in `outForDelivery` status.                       |
| `statusBreakdown.delivered`      | Number   | Orders successfully delivered.                           |
| `statusBreakdown.failed`         | Number   | Orders that failed delivery.                             |
| `successRate`                    | String   | Percentage of successful deliveries (e.g., `50.00`).     |
| `averageDeliveryTime`            | String   | Average time to deliver (e.g., `N/A` if not calculated). |

---

## **Error Responses (All Endpoints)**
| Status Code | Description                | Example Response                                                  |
| ----------- | -------------------------- | ----------------------------------------------------------------- |
| **401**     | Unauthorized (invalid JWT) | `{"success": false, "message": "No token, authorization denied"}` |
| **403**     | Forbidden (wrong role)     | `{"success": false, "message": "Access denied: Admins only"}`     |
| **500**     | Server error               | `{"success": false, "message": "Internal server error"}`          |

---
