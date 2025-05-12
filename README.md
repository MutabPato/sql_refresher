# 🧠 1-Hour SQL Refresher Course

Welcome to your practical 1-hour refresher course in SQL. You’ll get hands-on practice with table creation, data manipulation, joins, aggregation, and more — directly from your terminal or SQL tool.

---

## ✅ Setup (5 minutes)

Make sure one of these environments is ready:

- **SQLite**: Run with `sqlite3 refresher.db`
- **MySQL**: Login with `mysql -u root -p`
- **PostgreSQL**: Login with `psql postgres`

---

## 📘 1. Create Tables & Insert Data (10 mins)

```sql
CREATE TABLE users (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    name TEXT NOT NULL,
    email TEXT NOT NULL UNIQUE,
    age INTEGER,
    created_at DATETIME DEFAULT CURRENT_TIMESTAMP
);

INSERT INTO users (name, email, age) VALUES
('Alice', 'alice@example.com', 28),
('Bob', 'bob@example.com', 35),
('Charlie', 'charlie@example.com', 30);
```

**💾 Insert Data**

```
INSERT INTO users (name, email, age) VALUES
('Alice', 'alice@example.com', 28),
('Bob', 'bob@example.com', 35),
('Charlie', 'charlie@example.com', 30);
```
**🔍 View Data**

```
SELECT * FROM users;
```

## 2. 🔍 SELECT Queries & Filtering (10 min)

**👓 Column selection**

`SELECT name, email FROM users;`

**🔍 Filtering with WHERE**

```
SELECT * FROM users WHERE age > 30;
SELECT * FROM users WHERE email LIKE '%example.com';
```

**🧠 Combining filters**

`SELECT * FROM users WHERE age > 25 AND name LIKE 'A%';`

## 3. ✏️ UPDATE & DELETE (5 min) 

```
UPDATE users SET age = 36 WHERE name = 'Bob';
DELETE FROM users WHERE name = 'Charlie';
```

⚠️ Always use WHERE with UPDATE and DELETE to avoid altering all rows.

## 4. 🔗 JOINs with a Second Table (10 min) 

**🧾 Orders Table**

```
CREATE TABLE orders (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    user_id INTEGER,
    product TEXT,
    total REAL,
    created_at DATETIME DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (user_id) REFERENCES users(id)
);

INSERT INTO orders (user_id, product, total) VALUES
(1, 'Laptop', 999.99),
(1, 'Mouse', 25.50),
(2, 'Desk', 120.00);
```

**🤝 INNER JOIN**

```
SELECT users.name, orders.product, orders.total
FROM users
JOIN orders ON users.id = orders.user_id;
```

## 5. 📊 Aggregation & GROUP BY (10 min) 

```
SELECT COUNT(*) FROM users;

SELECT user_id, SUM(total) AS total_spent
FROM orders
GROUP BY user_id;
```

**📈 Filtering aggregated results**

```
SELECT user_id, COUNT(*) AS num_orders
FROM orders
GROUP BY user_id
HAVING num_orders > 1;
```

## 6. 🧠 Views, Indexes, and Constraints (10 min)

**👁 Create a View**

```
CREATE VIEW user_orders AS
SELECT users.name, orders.product, orders.total
FROM users
JOIN orders ON users.id = orders.user_id;
```

**⚡ Indexes**

`CREATE INDEX idx_users_email ON users(email);`

**✅ Add a CHECK Constraint**

`ALTER TABLE users ADD CHECK (age >= 18);`

Note: SQLite accepts CHECK constraints but doesn’t enforce them strictly.

## 7. 🔁 Practice Challenge (Optional 10 min)

**💳 Payments Table**

```
CREATE TABLE payments (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    order_id INTEGER,
    amount REAL,
    method TEXT,
    paid_at DATETIME DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (order_id) REFERENCES orders(id)
);

INSERT INTO payments (order_id, amount, method) VALUES
(1, 999.99, 'credit_card'),
(2, 25.50, 'paypal'),
(3, 120.00, 'stripe');
```

**🧪 Challenge Queries**

Total payment per user:

```
SELECT users.name, SUM(payments.amount) AS total_paid
FROM users
JOIN orders ON users.id = orders.user_id
JOIN payments ON orders.id = payments.order_id
GROUP BY users.name;
```

Payments by method:

`SELECT method, COUNT(*) FROM payments GROUP BY method;`

Payments today (SQLite):

`SELECT * FROM payments WHERE paid_at >= DATE('now');`


