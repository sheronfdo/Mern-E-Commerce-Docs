Below is a comprehensive MongoDB database schema designed for an e-commerce system for used car parts. This schema accommodates all the parameters and features discussed, including support for buyers, sellers, couriers, and admins. It is structured to ensure efficient data storage, retrieval, and management within MongoDB.

---

The schema consists of multiple collections, each tailored to store specific data for the entities and their interactions within the system. The collections are designed with appropriate fields, relationships, and indexes to support the functionality of the e-commerce platform.

### Collections

1. **Users**
2. **Products**
3. **Orders**
4. **Carts**
5. **Addresses**
6. **Messages**
7. **Reviews**
8. **Returns**
9. **Complaints**
10. **Categories**
11. **Analytics**
12. **Payouts**
13. **Teams**

---

### 1. Users Collection

The `Users` collection stores information about all users, including buyers, sellers, couriers, and admins. The `role` field differentiates between these user types.

```json
{
  "_id": ObjectId,
  "role": String, // "buyer", "seller", "courier", "admin"
  "email": String,
  "password": String, // Hashed
  "name": String,
  "phone": String,
  "storeName": String, // Optional, for sellers
  "region": String, // Optional, for couriers
  "availability": String, // Optional, for couriers: "active", "inactive"
  "createdAt": Date,
  "updatedAt": Date
}
```

- **Indexes**:
  - `email` (unique)
  - `role`

---

### 2. Products Collection

The `Products` collection stores detailed information about each product listed by sellers.

```json
{
  "_id": ObjectId,
  "title": String,
  "description": String,
  "price": Number,
  "category": ObjectId, // Reference to Categories collection
  "stock": Number,
  "condition": String, // "new", "used", etc.
  "brand": String,
  "oem": String, // OEM or Aftermarket
  "material": String,
  "compatibility": [String], // Array of compatible vehicles
  "images": [String], // Array of image URLs
  "sellerId": ObjectId, // Reference to Users collection (seller)
  "status": String, // "active", "out_of_stock", "flagged", etc.
  "boostStatus": String, // "active", "inactive" (for boosted products)
  "createdAt": Date,
  "updatedAt": Date
}
```

- **Indexes**:
  - `sellerId`
  - `category`
  - `status`

---

### 3. Orders Collection

The `Orders` collection stores information about each order, including buyer, seller, items, and delivery details.

```json
{
  "_id": ObjectId,
  "buyerId": ObjectId, // Reference to Users collection (buyer)
  "sellerId": ObjectId, // Reference to Users collection (seller)
  "items": [
    {
      "productId": ObjectId, // Reference to Products collection
      "quantity": Number,
      "price": Number
    }
  ],
  "total": Number,
  "status": String, // "processing", "shipped", "delivered", "cancelled", etc.
  "shippingAddress": ObjectId, // Reference to Addresses collection
  "deliveryInstructions": String,
  "trackingNumber": String,
  "courierId": ObjectId, // Reference to Users collection (courier)
  "createdAt": Date,
  "updatedAt": Date
}
```

- **Indexes**:
  - `buyerId`
  - `sellerId`
  - `courierId`
  - `status`

---

### 4. Carts Collection

The `Carts` collection stores the current cart items for each buyer.

```json
{
  "_id": ObjectId,
  "buyerId": ObjectId, // Reference to Users collection (buyer)
  "items": [
    {
      "productId": ObjectId, // Reference to Products collection
      "quantity": Number
    }
  ],
  "createdAt": Date,
  "updatedAt": Date
}
```

- **Indexes**:
  - `buyerId` (unique)

---

### 5. Addresses Collection

The `Addresses` collection stores shipping and billing addresses for buyers.

```json
{
  "_id": ObjectId,
  "buyerId": ObjectId, // Reference to Users collection (buyer)
  "street": String,
  "city": String,
  "state": String,
  "zip": String,
  "country": String,
  "addressType": String, // "shipping", "billing"
  "createdAt": Date,
  "updatedAt": Date
}
```

- **Indexes**:
  - `buyerId`

---

### 6. Messages Collection

The `Messages` collection stores communication between buyers and sellers.

```json
{
  "_id": ObjectId,
  "senderId": ObjectId, // Reference to Users collection (buyer or seller)
  "recipientId": ObjectId, // Reference to Users collection (seller or buyer)
  "productId": ObjectId, // Reference to Products collection (optional)
  "message": String,
  "timestamp": Date
}
```

- **Indexes**:
  - `senderId`
  - `recipientId`
  - `productId`

---

### 7. Reviews Collection

The `Reviews` collection stores product reviews submitted by buyers.

```json
{
  "_id": ObjectId,
  "productId": ObjectId, // Reference to Products collection
  "buyerId": ObjectId, // Reference to Users collection (buyer)
  "rating": Number, // 1 to 5
  "comment": String,
  "createdAt": Date
}
```

- **Indexes**:
  - `productId`
  - `buyerId`

---

### 8. Returns Collection

The `Returns` collection stores return requests for orders.

```json
{
  "_id": ObjectId,
  "orderId": ObjectId, // Reference to Orders collection
  "buyerId": ObjectId, // Reference to Users collection (buyer)
  "sellerId": ObjectId, // Reference to Users collection (seller)
  "reason": String,
  "status": String, // "pending", "approved", "rejected"
  "createdAt": Date,
  "updatedAt": Date
}
```

- **Indexes**:
  - `orderId`
  - `buyerId`
  - `sellerId`
  - `status`

---

### 9. Complaints Collection

The `Complaints` collection stores complaints raised by buyers.

```json
{
  "_id": ObjectId,
  "buyerId": ObjectId, // Reference to Users collection (buyer)
  "sellerId": ObjectId, // Reference to Users collection (seller)
  "orderId": ObjectId, // Reference to Orders collection (optional)
  "description": String,
  "status": String, // "open", "resolved"
  "createdAt": Date,
  "updatedAt": Date
}
```

- **Indexes**:
  - `buyerId`
  - `sellerId`
  - `orderId`
  - `status`

---

### 10. Categories Collection

The `Categories` collection stores product categories.

```json
{
  "_id": ObjectId,
  "name": String,
  "parentCategory": ObjectId, // Reference to Categories collection (for subcategories)
  "createdAt": Date,
  "updatedAt": Date
}
```

- **Indexes**:
  - `name` (unique)
  - `parentCategory`

---

### 11. Analytics Collection

The `Analytics` collection stores analytics data for sellers and admins.

```json
{
  "_id": ObjectId,
  "type": String, // "sales", "earnings", "top_products", etc.
  "period": String, // "daily", "weekly", "monthly", etc.
  "data": {
    "totalSales": Number,
    "grossEarnings": Number,
    "netEarnings": Number,
    "topProducts": [
      {
        "productId": ObjectId,
        "sales": Number
      }
    ],
    "commissionEarned": Number // For admin analytics
  },
  "sellerId": ObjectId, // Reference to Users collection (for seller analytics)
  "createdAt": Date
}
```

- **Indexes**:
  - `type`
  - `period`
  - `sellerId`

---

### 12. Payouts Collection

The `Payouts` collection stores payout information for sellers.

```json
{
  "_id": ObjectId,
  "sellerId": ObjectId, // Reference to Users collection (seller)
  "amount": Number,
  "status": String, // "pending", "completed", "failed"
  "createdAt": Date,
  "updatedAt": Date
}
```

- **Indexes**:
  - `sellerId`
  - `status`

---

### 13. Teams Collection

The `Teams` collection stores information about courier teams by region.

```json
{
  "_id": ObjectId,
  "region": String,
  "members": [ObjectId], // Array of courier user IDs
  "createdAt": Date,
  "updatedAt": Date
}
```

- **Indexes**:
  - `region` (unique)

---

### Design Notes

- **Embedded Documents vs. References**:
  - **Embedded Documents**: Used for data frequently accessed together, such as `items` within the `Orders` collection.
  - **References**: Used for data referenced across multiple documents, such as `productId` in `Orders`, `Carts`, and `Reviews`.

- **Data Validation**:
  - MongoDB's schema validation can enforce required fields and data types (e.g., `price` as a number, `email` as unique).

- **Indexing**:
  - Indexes are applied to frequently queried fields (e.g., `buyerId`, `sellerId`, `status`) to optimize performance.

- **Security**:
  - Passwords should be hashed before storage.
  - Sensitive data (e.g., passwords, payment details) should be excluded from API responses.

- **Scalability**:
  - The schema supports a large number of products, orders, and users. Sharding can be implemented for significant growth.

- **Localization**:
  - For global use, consider adding fields like currency or language preferences in relevant collections (e.g., `Users`, `Products`).

This schema provides a robust foundation for an e-commerce system for used car parts, ensuring all discussed parameters and features are efficiently supported within MongoDB.