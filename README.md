# Bekie E-Commerce Platform

A full-featured e-commerce web application inspired by the Nike web interface, developed as part of a WorldSkills Thailand regional competition. The system supports two distinct user roles — seller and customer — each with a dedicated interface and workflow.

---

## Screenshots

### Home Page
![image](https://github.com/prakan-suma/bekie_web/assets/55022692/b9135be4-4e38-45ed-8ab6-6eca1fa0e1a4)

### Registration & UI
![image](https://github.com/prakan-suma/bekie_web/assets/55022692/c9eb318f-166a-4ae5-9e2f-51ac1570ade9)
![image](https://github.com/prakan-suma/bekie_web/assets/55022692/6caf7260-663a-42aa-a21e-366412725659)

---

## Overview

Bekie is a marketplace-style platform where sellers can register a shop, list products, and track incoming orders, while customers can browse, search, and purchase products through a session-based shopping cart. The project was built from scratch without any framework, using native PHP with a custom database abstraction layer and Bootstrap 4 for the UI.

---

## Features

### Customer

- Register and manage a personal account
- Browse all products with pagination (20 items per page)
- Search products by keyword
- Add items to a session-based shopping cart
- Adjust item quantities and recalculate totals
- Checkout with automatic VAT (7%) and shipping cost calculation
- View order history and purchase details

### Seller

- Register a shop with contact information and address
- List, add, edit, and delete products (with image upload)
- Track product dimensions and weight per listing
- View incoming purchase orders and order details per buyer

### General

- Role-based navigation that adapts to the logged-in user type
- Flash alert system using PHP sessions for success, warning, and error feedback
- Bestseller section on the homepage ranked by purchase count using SQL aggregation

---

## Tech Stack

| Layer      | Technology                        |
|------------|-----------------------------------|
| Language   | PHP 7+                            |
| Database   | MySQL (via MySQLi)                |
| Frontend   | HTML5, Bootstrap 4.3, Ionicons    |
| Scripting  | jQuery 3.3.1                      |
| Styling    | Custom CSS + Bootstrap            |
| Storage    | Local file system (product images)|

---

## Project Structure

```
bekie-ecommerce-worldskill-th-region/
|
|- db.php                        # Database connection and shared utility functions
|- header.php                    # Global HTML head, Bootstrap links, menu include
|- footer.php                    # Closing HTML tags
|- menu.php                      # Role-aware navigation bar
|
|- index.php                     # Homepage: bestsellers + paginated product listing
|- search.php                    # Keyword search results
|- login.php / login_check.php   # Authentication
|- logout.php                    # Session destroy
|
|- register_customer.php         # Customer registration form
|- register_customer_save.php    # Customer registration handler
|- register_seller.php           # Seller registration form
|- register_seller_save.php      # Seller registration handler
|
|- customer.php                  # Customer profile overview
|- customer_profile.php          # Edit customer profile
|- customer_update.php           # Customer update handler
|- customer_purchase.php         # Customer order history
|- customer_purchase_detail.php  # Customer order detail view
|
|- seller.php                    # Seller overview
|- seller_profile.php            # Seller profile page
|- seller_edit.php / seller_register.php  # Edit seller info
|- seller_purchase.php           # Seller order list
|- seller_purchase_detail.php    # Seller order detail view
|
|- product.php                   # Seller product management list
|- product_add.php               # Add new product form
|- product_save.php              # Add product handler (with image upload)
|- product_edit.php              # Edit product form
|- product_update.php            # Edit product handler
|- product_delete.php            # Delete product handler
|
|- cart.php                      # Shopping cart view
|- cart_add.php                  # Add item to cart
|- cart_update.php               # Update item quantities
|- cart_confirm.php              # Order confirmation and DB write
|- cart_detail.php               # Post-checkout order summary
|
|- css/                          # Bootstrap 4 and custom styles
|- js/                           # jQuery, Popper, Bootstrap JS
|- upload_picture/               # Uploaded product images
```

---

## Database Schema

The application uses a MySQL database named `web_ecommerce`. The core tables are:

| Table           | Description                                      |
|-----------------|--------------------------------------------------|
| `customer`      | Customer accounts and profile data               |
| `seller`        | Seller accounts, shop name, contact, and address |
| `product`       | Product listings linked to a seller              |
| `purchase`      | Order records linked to a customer               |
| `purchase_list` | Line items for each order linked to products     |

---

## Setup and Installation

### Requirements

- PHP 7.0 or higher
- MySQL 5.7 or higher
- A local server environment such as XAMPP or Laragon

### Steps

1. Clone the repository into your web server's document root:

   ```bash
   git clone https://github.com/your-username/bekie-ecommerce-worldskill-th-region.git
   ```

2. Create a MySQL database named `web_ecommerce`.

3. Import the database schema. (A `.sql` dump file should be provided separately or generated from the running instance.)

4. Open `db.php` and verify the connection settings match your local environment:

   ```php
   $conn = new mysqli('localhost', 'root', '', 'web_ecommerce');
   ```

5. Start your local server and navigate to `http://localhost/bekie-ecommerce-worldskill-th-region/`.

---

## Key Implementation Details

**Custom database helper functions** defined in `db.php` abstract all SQL operations:

- `get($sql)` — executes a SELECT query and returns all rows as an associative array
- `set($sql)` / `save($sql)` — executes INSERT, UPDATE, or DELETE queries
- `auth()` — redirects unauthenticated users to the login page
- `sellerAuth()` — restricts access to seller-only pages
- `alertSess($class, $message, $redirect)` — stores a flash message in session and redirects
- `alertShow()` — renders the stored flash message as a Bootstrap alert

**Image upload** in `product_save.php` uses `md5()` combined with a timestamp and client IP to generate a unique, collision-resistant filename before copying the file to `upload_picture/`.

**Bestseller ranking** on the homepage uses a SQL `INNER JOIN` between `purchase_list` and `product`, grouped by `product_id` and ordered by count descending, limited to the top 10.

**Shopping cart** is stored entirely in the PHP session (`$_SESSION['cart']`) and supports quantity updates, with totals, 7% VAT, and shipping cost recalculated on update.

---

## Context

This project was developed as a regional-level submission for WorldSkills Thailand in the Web Technologies category. It demonstrates core backend web development skills including relational database design, session management, file handling, role-based access control, and form processing — all implemented without a framework.

---

## License

This project is intended for portfolio and educational purposes.
