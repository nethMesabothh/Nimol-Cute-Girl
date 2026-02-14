# Feature Specifications

## 1. CRUD Product
### Description
Complete product management system for creating, reading, updating, and deleting products in the inventory.

### Features
- **Create Product**
  - Add new products with details (name, SKU, barcode, category, price, cost, stock quantity, supplier)
  - Upload product images
  - Set product variants (size, color, etc.)
  - Define product categories and tags

- **Read/View Products**
  - Browse product catalog with search and filter capabilities
  - View product details, stock levels, pricing history
  - Product listing with pagination
  - Quick view product information

- **Update Product**
  - Edit product information (name, price, description, etc.)
  - Update stock quantities
  - Modify product images and variants
  - Bulk update operations

- **Delete Product**
  - Soft delete (archive) or hard delete products
  - Handle dependencies (orders, transactions)
  - Product deletion confirmation and audit trail

---

## 2. Staff Management
### Description
Comprehensive employee and user management system for controlling access and tracking staff activities.

### Features
- **User Accounts**
  - Create staff accounts with roles and permissions
  - User authentication and authorization
  - Password management and security policies

- **Role-Based Access Control (RBAC)**
  - Two roles: **Staff** and **Manager**
  - Assign permissions per role
  - Manager has full access to all features
  - Staff has limited access (POS operations, basic product viewing)

- **Staff Information**
  - Employee profiles (name, contact, position, department)
  - Employment history and status
  - Shift scheduling and attendance tracking

- **Activity Tracking**
  - Log staff actions and transactions
  - Audit trail for sensitive operations
  - Performance metrics and reports

---

## 3. Expiry Data Management
### Description
Advanced inventory management system for products with expiration dates, including automated alerts, FIFO handling, and discount automation.

### Features
- **Alert System**
  - Configurable alert thresholds (e.g., 7 days, 14 days before expiry)
  - Real-time notifications for expiring products
  - Dashboard alerts and email/SMS notifications
  - Expired product alerts

- **FIFO (First In, First Out)**
  - Automatic stock rotation based on expiration dates
  - Prioritize older stock for sales
  - Inventory tracking by batch/expiry date
  - FIFO compliance reports

- **Auto Discount When Nearly Expire**
  - Automatic discount rules based on days until expiry
  - Configurable discount percentages (e.g., 10% off when 3 days left, 20% off when 1 day left)
  - Price override at point of sale
  - Discount history and reporting

---

## 4. Payment
### Description
Multiple payment methods to accommodate different customer preferences and payment scenarios.

### Features
- **Cash Payment**
  - Cash transaction processing
  - Cash register management
  - Change calculation
  - Cash drawer integration

- **Scan QR Code from Mobile Device**
  - QR code generation for payment
  - Mobile payment integration (e-wallets, bank transfers)
  - QR code scanning for customer payments
  - Payment verification and confirmation
  - Support for multiple QR payment providers (e.g., DANA, OVO, GoPay, bank QRIS)

### Additional Payment Features
- Payment method selection at checkout
- Split payment (multiple methods per transaction)
- Payment receipt generation
- Refund and return processing
- Payment history and reconciliation

---

## 5. POS Management
### Description
Core Point of Sale system for processing sales transactions and managing the retail operation.

### Features
- **Sales Transaction**
  - Add items to cart
  - Apply discounts and promotions
  - Calculate totals, taxes, and change
  - Process payments
  - Generate receipts

- **Cart Management**
  - Add/remove items
  - Modify quantities
  - Apply item-level discounts
  - Hold/suspend transactions
  - Retrieve suspended transactions

- **Receipt Management**
  - Print receipts (thermal printer support)
  - Email/SMS receipt delivery
  - Digital receipt storage
  - Receipt reprint functionality

- **Sales Reports**
  - Daily sales summary
  - Sales by product, category, staff member
  - Revenue reports
  - Transaction history
  - Export capabilities (CSV, PDF)

- **Inventory Integration**
  - Real-time stock updates
  - Low stock alerts
  - Product availability checking

- **System Settings**
  - Tax configuration
  - Currency settings
  - Receipt template customization
  - Printer configuration
  - Store information management
