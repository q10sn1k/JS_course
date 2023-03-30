# Домашнее задание:

## Задача 1

Создайте таблицу `users`

### Решение

```sql
CREATE TABLE users (
    id INT(11) NOT NULL AUTO_INCREMENT,
    name VARCHAR(50) NOT NULL,
    email VARCHAR(50) NOT NULL,
    age INT(11) NOT NULL,
    country VARCHAR(50) NOT NULL,
    balance FLOAT NOT NULL,
    PRIMARY KEY (id)
    );
```

## Задача 2

Сделайте выборку данных из таблицы `users`

### Решение

```sql
SELECT * FROM users;
```

## Задача 3

Добавьте строки в таблицу `users`

### Решение

```sql
INSERT INTO users (name, email, age , country, balance) VALUES
    ('Gleb', 'gleb@exmple.com', 26, 'RUS', 90000.00),
    ('Sergey', 'sergey@example.com', 28, 'RUS', 90000.00),
    ('Andrew', 'andrew@example.com', 24, 'RUS', 95000.00),
    ('Bob', 'bob@example.com', 25, 'USA', 100000.00),
    ('Tom', 'tom@example.com', 29, 'USA', 110000.00);
```

## Задача 4

Обновите данные в таблице `users`

### Решение

```sql
UPDATE users SET name = 'Sergey' WHERE id = 3;
```