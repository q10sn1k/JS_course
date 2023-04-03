### Задача 5

модель user.js


```js
const db = require('../config/db');
const bcrypt = require('bcryptjs');

// Создание пользователя
exports.createUser = async (username, email, password) => {
  const hashedPassword = await bcrypt.hash(password, 10);

  return new Promise((resolve, reject) => {
    db.query(
      'INSERT INTO users (username, email, password) VALUES (?, ?, ?)',
      [username, email, hashedPassword],
      (err, results) => {
        if (err) {
          return reject(err);
        }
        resolve(results.insertId);
      }
    );
  });
};


// Получение пользователя по ID
exports.getUserById = (userId) => {
  return new Promise((resolve, reject) => {
    db.query('SELECT * FROM users WHERE id = ?', [userId], (err, results) => {
      if (err) {
        return reject(err);
      }
      const user = results[0];
      if (user) {
        delete user.password;
      }
      resolve(user);
    });
  });
};

// Получение пользователя по email
exports.getUserByEmail = (email) => {
  return new Promise((resolve, reject) => {
    db.query('SELECT * FROM users WHERE email = ?', [email], (err, results) => {
      if (err) {
        return reject(err);
      }
      const user = results[0];
      if (user) {
        delete user.password;
      }
      resolve(user);
    });
  });
};
```

### вспомогательный файлы

создание таблицы user
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

добавление данных в таблицу `users`

```sql
INSERT INTO users (name, email, age , country, balance) VALUES
    ('Gleb', 'gleb@exmple.com', 26, 'RUS', 90000.00),
    ('Sergey', 'sergey@example.com', 28, 'RUS', 90000.00),
    ('Andrew', 'andrew@example.com', 24, 'RUS', 95000.00),
    ('Bob', 'bob@example.com', 25, 'USA', 100000.00),
    ('Tom', 'tom@example.com', 29, 'USA', 110000.00);
```

config/db.js

```js
const mysql = require('mysql2');
const dotenv = require('dotenv');

dotenv.config();

const connection = mysql.createConnection({
  host: process.env.DB_HOST,
  user: process.env.DB_USER,
  password: process.env.DB_PASSWORD,
  database: process.env.DB_NAME,
});

connection.connect((err) => {
  if (err) {
    console.error('Ошибка подключения к базе данных:', err);
    process.exit(1);
  } else {
    console.log('Подключение к базе данных успешно установлено');
  }
});

module.exports = connection;
```

.env
```.dotenv
DB_HOST=localhost
DB_USER=mydbuser
DB_PASSWORD=mydbpassword
DB_NAME=mydbname
```



Необходимо написать тест модели user
