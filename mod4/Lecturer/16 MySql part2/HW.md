# Домашнее задание:

## Задача 1

Создайте таблицу `orders` (таблицы `users` и `orders` должны быть взаимосвязаны)

### Решение

```sql
CREATE TABLE orders (
    id INT(11) NOT NULL AUTO_INCREMENT,
    user_id INT(11) NOT NULL,
    product_name VARCHAR(50) NOT NULL,
    price FLOAT NOT NULL,
    quantity INT(11) NOT NULL,
    order_date DATE NOT NULL,
    PRIMARY KEY (id),
    FOREIGN KEY (user_id) REFERENCES users(id)
    );
```

## Задача 2

Добавьте строки в таблицу `orders`

### Решение

```sql
INSERT INTO orders (user_id, product_name, price, quantity, order_date) VALUES
    (2, 'Product B', 20.00, 1, '2023-02-20'),
    (2, 'Product C', 15.00, 3, '2023-02-23'),
    (1, 'Product D', 12.00, 2, '2023-02-25'),
    (2, 'Product E', 25.00, 1, '2023-02-26');
```

## Задача 3

Сделайте выборку используя `LEFT JOIN`

### Решение

```sql
SELECT users.name, users.email, orders.product_name, orders.order_date
    FROM users
    LEFT JOIN orders
    ON users.id = orders.user_id;
```

## Задача 4

Сделайте выборку используя `INNER JOIN`

```sql
SELECT users.name, users.email, orders.product_name, orders.order_date
    FROM users
    INNER JOIN orders
    ON users.id = orders.user_id;
```

