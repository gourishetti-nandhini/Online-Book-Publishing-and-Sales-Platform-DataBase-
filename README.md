# 📚 Online Book Publishing and Sales Platform Database

This project is a relational **SQL database schema** designed for managing an online book publishing and sales platform. It supports book inventory, authorship, editions, customer orders, payments, shipping, wishlists, and more. The schema includes real-world features like co-authored books, special editions, pre-orders, multiple shipping addresses, and genre classification.

## 🗂️ Key Features

- Books with multiple editions, linked to publishers, genres, and authors.
- Authors can write multiple books; books can have multiple authors.
- Publishers publish multiple editions.
- Customers can:
  - Have multiple shipping addresses
  - Place multiple orders
  - Maintain a wishlist
- Orders support:
  - Multiple books per order
  - Item-level quantity and discounts
  - Payment and shipment tracking
- Genre classification for each book.
- Support for special editions and pre-order availability.


![image](https://github.com/user-attachments/assets/979789e8-0613-498d-bce4-aaa3371c91ef)
![sql image](https://github.com/user-attachments/assets/9944e34b-385c-4d49-8f99-3e88ecd8a57b)



## 📐 SQL Table Creation

### 👤 Customer and Address
```sql
CREATE TABLE customer (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(100),
    email VARCHAR(100),
    phone VARCHAR(20)
);

CREATE TABLE address (
    id INT PRIMARY KEY AUTO_INCREMENT,
    address_line VARCHAR(255),
    city VARCHAR(100),
    state VARCHAR(100),
    postal_code VARCHAR(20),
    country VARCHAR(100)
);

CREATE TABLE customer_address (
    customer_id INT,
    address_id INT,
    PRIMARY KEY (customer_id, address_id),
    FOREIGN KEY (customer_id) REFERENCES customer(id) ON DELETE CASCADE,
    FOREIGN KEY (address_id) REFERENCES address(id) ON DELETE CASCADE
);

CREATE TABLE book (
    id INT PRIMARY KEY AUTO_INCREMENT,
    title VARCHAR(255),
    isbn VARCHAR(20) UNIQUE
);

CREATE TABLE book_edition (
    id INT PRIMARY KEY,
    book_id INT,
    edition_number INT,
    publication_year YEAR,
    price DECIMAL(10, 2),
    publisher_id INT,
    is_special_edition BOOLEAN DEFAULT FALSE,
    is_preorder_available BOOLEAN DEFAULT FALSE,
    FOREIGN KEY (book_id) REFERENCES book(id) ON DELETE CASCADE,
    FOREIGN KEY (publisher_id) REFERENCES publisher(id) ON DELETE SET NULL
);

CREATE TABLE book_genre (
    book_id INT,
    genre VARCHAR(100),
    PRIMARY KEY (book_id, genre),
    FOREIGN KEY (book_id) REFERENCES book(id) ON DELETE CASCADE
);

CREATE TABLE author (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(150),
    biography TEXT
);

CREATE TABLE author_book (
    author_id INT,
    book_id INT,
    PRIMARY KEY (author_id, book_id),
    FOREIGN KEY (author_id) REFERENCES author(id) ON DELETE CASCADE,
    FOREIGN KEY (book_id) REFERENCES book(id) ON DELETE CASCADE
);

CREATE TABLE publisher (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(150),
    contact_details TEXT
);

CREATE TABLE orders (
    id INT PRIMARY KEY AUTO_INCREMENT,
    customer_id INT,
    order_date DATE,
    payment_details TEXT,
    shipment_status VARCHAR(50),
    FOREIGN KEY (customer_id) REFERENCES customer(id) ON DELETE CASCADE
);

CREATE TABLE order_item (
    order_id INT,
    edition_id INT,
    quantity INT,
    item_discount DECIMAL(5,2),
    PRIMARY KEY (order_id, edition_id),
    FOREIGN KEY (order_id) REFERENCES orders(id) ON DELETE CASCADE,
    FOREIGN KEY (edition_id) REFERENCES book_edition(id) ON DELETE CASCADE
);

CREATE TABLE wishlist (
    id INT PRIMARY KEY AUTO_INCREMENT,
    customer_id INT,
    FOREIGN KEY (customer_id) REFERENCES customer(id) ON DELETE CASCADE
);

CREATE TABLE wishlist_item (
    wishlist_id INT,
    book_id INT,
    PRIMARY KEY (wishlist_id, book_id),
    FOREIGN KEY (wishlist_id) REFERENCES wishlist(id) ON DELETE CASCADE,
    FOREIGN KEY (book_id) REFERENCES book(id) ON DELETE CASCADE
);




