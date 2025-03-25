  - [ ] Admin
  
- [ ] 1.  Can approve sellers to join
	 
	- [ ] **GET `/api/admin/sellers/pending`**
	- [ ] **PUT `/api/admin/sellers/:id 
	- [ ] **DELETE `/api/admin/sellers/:id`**  
			
- [ ]  2.  Can remove sellers

	- [ ] **DELETE `/api/admin/sellers/:id`**  
	
- [ ] 3. View complaints from the buyers.

	- [ ] **GET `/api/admin/complaints`** 
	- [ ] **PUT `/api/admin/complaints/:id`**  

- [ ]  4. Can remove buyers. 

	- [ ] **DELETE `/api/admin/sellers/:id`**  

- [ ] 5.  Should have a complaint chat portal. #conflicts

	- [ ] **POST `/api/admin/complaints/:id/chat`**  

- [ ] 6.  Can see every Product listing of the website.

	- [ ] **GET `/api/admin/products`**  
	- [ ] **GET `/api/admin/products/:id`**  
	- [ ] **DELETE `/api/admin/products/:id`**  

	- [ ] **POST `/api/admin/categories`**  #conflicts
	- [ ]  **PUT `/api/admin/categories/:id`**  #conflicts
	- [ ] **DELETE `/api/admin/categories/:id`**  #conflicts
	
- [ ]  7. Dashboard to show analytics about

	- [ ] Listed items
		**GET `/api/admin/analytics/products`**
	- [ ] Items sold
		**GET `/api/admin/analytics/sales`**  
	- [ ] Best sellers
		**GET `/api/admin/analytics/top-sellers`**  
	- [ ] Commission earned
	    **GET `/api/admin/analytics/commission`**  
		
		 

---

- [ ]  Courier #conflicts

- [ ] 1.  Can view the transferred orders from the seller.
	- [ ] **`GET /api/courier/orders`** #conflicts
- [ ] 2.  The courier team has Teams for regions.
	- [ ] 
- [ ] 3.  The courier team transfers the order details to the region team.
	- [ ] 
- [ ] 4. Courier team can update delivery status from processing to delivered.
	- [ ] 
- [ ] 5. Dashboard to see orders and earnings.
	- [ ] 

---

- [ ] Buyers

- [ ] 1.Product search
	- [ ]  **GET `/api/products/search`**
	- [ ] `GET /api/products/:productId`
	- [ ] `GET /api/products/filters`
- [ ] 2.Product filtering (will give you the filtering categories further)
	- [ ] `GET /api/products/filters`
- [ ] 3.Comment section (should contact seller)
	- [ ] `POST /api/messages/send`
	- [ ] `GET /api/messages`
- [ ] 4.Payment gateway
	- [ ] `POST /api/payment/create-intent`
- [ ] 5.Purchasing history
	- [ ] `GET /api/orders/history`
	- [ ] `GET /api/orders/:orderId/tracking`
	- [ ] `POST /api/orders/create`
- [ ] 6.Buyer profile (include the purchase history as a part of the buyer profile)
	- [ ] `GET /api/buyer/profile`

---

- [ ] Sellers
- [ ] 1.  List a product
	- [ ] **GET `/api/seller/products`** #conflicts
	- [ ] **POST `/api/seller/products`**  
	- [ ] **PUT `/api/seller/products/:id`** 
	- [ ] **DELETE `/api/seller/products/:id`**  
- [ ] 2. Seller dashboard (order details and listed items and history) #conflicts
	- [ ] **GET `/api/seller/analytics/sales`**  
	- [ ] **GET `/api/seller/analytics/earnings`**
	- [ ] **GET `/api/seller/analytics/top`**
- [ ] 3. Inventory Management
	- [ ] **PATCH `/api/seller/products/:id/stock`** 
- [ ] 4. Seller analytics (Bestselling products, daily/weekly/monthly/yearly earnings etc.) #conflicts
- [ ] 5.Order Management 
	- [ ] **GET `/api/seller/orders`**
	- [ ] **PUT `/api/seller/orders/:id`**  
- [ ] 6.There should be a part where when seller accepts the order and process it the seller.
	- [ ] **PUT `/api/seller/orders/:id`**  
- [ ] 7. should transfer the order details to the courier team. (will explain further in a meeting)
	- [ ]  **POST `/api/seller/orders/:id/assign-courier`** 
- [ ] 8. Communication 
	- [ ] **GET `/api/seller/messages`**
	- [ ] **POST `/api/seller/messages/:id/reply`** 
- [ ] 9. Returns & Refunds  #conflicts




