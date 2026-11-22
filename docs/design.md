
1. Introduction

1.1 Purpose

This document describes the preliminary design of the MarketStock system. It explains the system architecture, major components, database structure, API design and deployment approach.

1.2 System Overview

MarketStock is a phone-friendly web application designed for Mami Ngum’s shop. It helps manage products, sales, stock, customer credit and profit reports.

The system will be used by the shop owner and assistant to manage daily shop activities efficiently.

1.3 Design Scope

The design covers the architecture, database, system components, API endpoints and deployment of the MarketStock application.
2. Architecture Style and Justification

2.1 Architecture Style

MarketStock will use a three-layer (layered) client-server architecture.

The architecture consists of three main layers:

* Presentation Layer: Provides the user interface through which the shop owner and assistant interact with the system using a phone or web browser.
* Application Layer: Processes business logic, including product management, stock updates, sales recording, credit management and report generation.
* Data Layer: Stores and manages system information, including users, products, customers, sales, restocks and repayments.

2.2 Justification

The layered architecture is suitable for MarketStock because:

1. It separates the user interface, business logic and data management.
2. It makes the system easier to maintain and update.
3. It supports the phone-friendly web interface required by the system.
4. It helps organize the different operations of the application.
5. It allows changes in one layer to be made without unnecessarily affecting the others.

The architecture supports the functional requirements of MarketStock, including FR-01 (adding products), FR-04 (restocking products), FR-05 (recording sales), FR-11 (recording repayments) and FR-13 (generating reports).
# 3. Architecture Diagram

```mermaid
flowchart TD
    A[Presentation Layer<br/>Phone / Web Browser]
    B[Application Layer<br/>Products, Sales, Stock, Credit, Reports]
    C[Data Layer<br/>MarketStock Database]

    A <--> B
    B <--> C

# 4. Deployment Diagram
flowchart TD
    A[User's Phone or Computer]
    B[Web Application Server]
    C[MarketStock Database Server]

    A <-->|Internet| B
    B <--> C
# 5. Database Design
The MarketStock database will store and organize information about products, sales, stock changes, customers, repayments and users. It will help the system maintain accurate records and retrieve information when needed.
Table Purpose
Users Stores login details and user information for the owner and assistant.
Products Stores product names, prices, quantities and low-stock levels.
Customers Stores customer names and phone numbers for credit sales.
Sales Records each sale and its payment information.
SaleItems Records the products and quantities included in each sale.
StockMovements Records stock changes, including restocking and sales.
Repayments Records payments made by customers towards outstanding credit.
5. Database Design

The MarketStock database will use SQLite to store and organize information about users, products, restocks, customers, sales, sale items and repayments.

5.1 Database Tables and Columns

Table	Columns	Description
users	id, name, phone, password_hash, role	Stores user details and login information. Role can be owner or assistant.
products	id, name, unit, buy_price, sell_price, quantity, min_quantity	Stores product details, prices and stock quantities.
restocks	id, product_id, quantity, unit_cost, supplier, user_id, created_at	Records each product delivery.
customers	id, name, phone, balance	Stores customers who buy on credit.
sales	id, user_id, customer_id, payment_method, total, created_at	Records each sale and its payment information.
sale_items	id, sale_id, product_id, quantity, unit_price, unit_cost	Records each product included in a sale.
repayments	id, customer_id, amount, user_id, created_at	Records payments made towards customer debts.

Database design notes:

1. Each table has a primary key called id.
2. Foreign keys connect related tables, such as product_id and user_id.
3. Passwords are stored as hashes, not plain text.
4. Prices and money values are stored as whole XAF integers.
5. The sale_items table stores the price and cost at the time of sale so that previous sales and profits remain accurate.
5.2 Database Relationships

The MarketStock database contains the following relationships:

1. Users and Restocks: One user can record many restocks.
2. Products and Restocks: One product can have many restock records.
3. Users and Sales: One user can record many sales.
4. Customers and Sales: One customer can have many sales. The customer is optional for a sale.
5. Sales and SaleItems: One sale can contain many sale items.
6. Products and SaleItems: One product can appear in many sale items.
7. Customers and Repayments: One customer can make many repayments.
8. Users and Repayments: One user can record many repayments.
5.3 Primary Keys and Foreign Keys

A primary key uniquely identifies each record in a table. A foreign key connects one table to another.

Table	Primary Key	Foreign Keys
users	id	None
products	id	None
restocks	id	product_id → products(id), user_id → users(id)
customers	id	None
sales	id	user_id → users(id), customer_id → customers(id)
sale_items	id	sale_id → sales(id), product_id → products(id)
repayments	id	customer_id → customers(id), user_id → users(id)

Note: The customer_id in the sales table can be empty because not every sale is a credit sale.
6.1 Proposed API Endpoints

HTTP Method	Endpoint	Purpose
POST	/api/auth/login	Logs in a user and returns a session.
GET	/api/products	Retrieves all products. Can filter low-stock products.
POST	/api/products	Creates a new product (owner only).
PATCH	/api/products/:id	Updates a product’s price or low-stock level (owner only).
POST	/api/restocks	Records a product delivery and increases stock.
POST	/api/sales	Records a sale and reduces stock.
GET	/api/customers?owing=true	Retrieves customers with outstanding credit balances.
POST	/api/customers/:id/repayments	Records a customer’s repayment.
GET	/api/reports/daily?date=2026-11-14	Retrieves the daily sales and profit report.
6.2 Example API Request and Response

Endpoint: POST /api/sales

Example Request:

{
  "items": [
    { "productId": 3, "quantity": 2 },
    { "productId": 7, "quantity": 1 }
  ],
  "paymentMethod": "credit",
  "customer": {
    "name": "Pa Ebai",
    "phone": "677000000"
  }
}

Example Error Response:

{
  "error": "INSUFFICIENT_STOCK",
  "message": "Only 3 Sugar (1 kg) in stock"
}

Status Codes:

* 201 Created: Sale recorded successfully.
* 400 Bad Request: Invalid request data.
* 401 Unauthorized: User is not logged in.
* 409 Conflict: Not enough stock to complete the sale.
* 500 Internal Server Error: An unexpected server error occurred.

The API checks product availability before saving a sale. If there is insufficient stock, it returns an error message instead of completing the sale.
7. Requirements Traceability

Requirements traceability connects the functional requirements in the Software Requirements Specification (SRS) to the API endpoints and database tables that implement them.

7.1 Functional Requirements Traceability Table

Requirement ID	Functional Requirement	API Endpoint(s)	Database Table(s)
FR-01	Add a new product	POST /api/products	Products
FR-02	Edit product details, prices and low-stock level	PATCH /api/products/:id	Products
FR-03	Delete a product	DELETE /api/products/:id	Products
FR-04	Restock a product	POST /api/restocks	Products, StockMovements
FR-05	Record a sale	POST /api/sales	Sales, SaleItems
FR-06	Automatically reduce stock after a sale	POST /api/sales	Products, Sales, SaleItems, StockMovements
FR-07	Reject sales when stock is insufficient	POST /api/sales	Products, Sales, SaleItems
FR-08	Record every stock change	POST /api/restocks, POST /api/sales	StockMovements, Users, Products
FR-09	Alert the owner when stock reaches the low-stock level	GET /api/products	Products
FR-10	Record credit sales with customer details	POST /api/sales	Sales, SaleItems, Customers
FR-11	Record customer repayments	POST /api/customers/:id/repayments	Customers, Repayments
FR-12	List customers with unpaid credit balances	GET /api/customers?owing=true	Customers, Sales
FR-13	Generate daily sales and profit reports	GET /api/reports/daily	Sales, SaleItems, Products
FR-14	Allow users to log in	POST /api/auth/login	Users

7.2 Traceability Review

The traceability table links the functional requirements to their proposed API endpoints and database tables. It also helps identify missing endpoints and ensures that every requirement is considered during system design.

Identified gap: The API endpoint list needs to include DELETE /api/products/:id to support FR-03, which allows the owner to delete a product.
