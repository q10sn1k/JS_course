### Задача 1
Создайте компонент Vue, который будет отображать список элементов.
Каждый элемент списка должен содержать название и описание.
При клике на элемент списка должна открываться детальная информация об этом элементе.
Также должна быть возможность добавления новых элементов в список.


#### Решение
Для решения данной задачи нужно создать компонент Vue с использованием следующих элементов:
шаблон (template), скрипт (script) и стили (style).

Создадим шаблон для нашего компонента.
Он должен содержать элемент "ul" для отображения списка и элемент "div" для отображения детальной информации об элементе списка.
Для элементов списка создадим шаблон "li", в котором будут отображаться название и описание элемента:

```vue
<template>
  <div>
    <ul>
      <li v-for="(item, index) in items" :key="index" @click="showDetails(item)">
        <h2>{{ item.title }}</h2>
        <p>{{ item.description }}</p>
      </li>
    </ul>
    <div v-if="selectedItem">
      <h2>{{ selectedItem.title }}</h2>
      <p>{{ selectedItem.description }}</p>
    </div>
  </div>
</template>

```

В данном шаблоне используется директива "v-for" для отображения элементов списка.
Она перебирает массив "items" и создает элементы "li" для каждого элемента списка.
Также для каждого элемента списка устанавливается обработчик событий "click", который вызывает метод "showDetails" и передает ему выбранный элемент списка.
Если выбранный элемент существует, то отображается детальная информация об этом элементе.



Создадим скрипт для нашего компонента. Он должен определять массив "items" с элементами списка, а также методы "showDetails" и "addItem":
```vue
<script>
export default {
   name: 'ItemList',
   data() {
      return {
         items: [
            { title: 'Item 1', description: 'Description for Item 1' },
            { title: 'Item 2', description: 'Description for Item 2' },
            { title: 'Item 3', description: 'Description for Item 3' },
         ],
         selectedItem: null,
      };
   },
   methods: {
      showDetails(item) {
         this.selectedItem = item;
      },
      addItem() {
         const newItem = {
            title: 'New Item',
            description: 'Description for New Item',
         };
         this.items.push(newItem);
      },
   },
};
</script>

```

Добавим функционал для добавления новых элементов в список.
Для этого создадим кнопку "Добавить элемент" и добавим обработчик событий "click",
который будет вызывать метод "addItem":


```vue
<template>
  <div>
    <button @click="addItem">Добавить элемент</button>
    <ul>
      <li v-for="(item, index) in items" :key="index" @click="showDetails(item)">
        <h2>{{ item.title }}</h2>
        <p>{{ item.description }}</p>
      </li>
    </ul>
    <div v-if="selectedItem">
      <h2>{{ selectedItem.title }}</h2>
      <p>{{ selectedItem.description }}</p>
    </div>
  </div>
</template>

```
Добавим стили для нашего компонента.
Например, мы можем задать стили для элементов списка и для кнопки "Добавить элемент":
```vue
<style>
li {
   cursor: pointer;
   border: 1px solid #ccc;
   padding: 10px;
   margin-bottom: 10px;
}

button {
   background-color: #4caf50;
   color: white;
   border: none;
   padding: 10px;
   margin-bottom: 10px;
   cursor: pointer;
}
</style>

```

# Полный код:
```vue
<template>
  <div>
    <button @click="addItem">Добавить элемент</button>
    <ul>
      <li v-for="(item, index) in items" :key="index" @click="showDetails(item)">
        <h2>{{ item.title }}</h2>
        <p>{{ item.description }}</p>
      </li>
    </ul>
    <div v-if="selectedItem">
      <h2>{{ selectedItem.title }}</h2>
      <p>{{ selectedItem.description }}</p>
    </div>
  </div>
</template>

<script>
export default {
  name: 'ItemList',
  data() {
    return {
      items: [
        { title: 'Item 1', description: 'Description for Item 1' },
        { title: 'Item 2', description: 'Description for Item 2' },
        { title: 'Item 3', description: 'Description for Item 3' },
      ],
      selectedItem: null,
    };
  },
  methods: {
    showDetails(item) {
      this.selectedItem = item;
    },
    addItem() {
      const newItem = {
        title: 'New Item',
        description: 'Description for New Item',
      };
      this.items.push(newItem);
    },
  },
};
</script>

<style>
li {
  cursor: pointer;
  border: 1px solid #ccc;
  padding: 10px;
  margin-bottom: 10px;
}

button {
  background-color: #4caf50;
  color: white;
  border: none;
  padding: 10px;
  margin-bottom: 10px;
  cursor: pointer;
}
</style>

```

____________
____________
____________


### Задача 2

Напишите приложение на Node.js, которое будет принимать GET-запросы на URL-адрес /hello и возвращать текст "Hello, World!" в качестве ответа.

#### Решение

```js
const express = require('express');
const app = express();

app.get('/hello', (req, res) => {
  res.send('Hello, World!');
});

app.listen(3000, () => {
  console.log('Server listening on port 3000');
});

```


____________
____________
____________

### Задача 3

Напишите приложение на Node.js, которое будет принимать GET-запросы на URL-адрес /random и возвращать случайное число от 1 до 100 в качестве ответа.

#### Решение

```js
const express = require('express');
const app = express();

app.get('/random', (req, res) => {
  const randomNumber = Math.floor(Math.random() * 100) + 1;
  res.send(`Random number: ${randomNumber}`);
});

app.listen(3000, () => {
  console.log('Server listening on port 3000');
});

```

____________
____________
____________

### Задача 4

Напишите скрипт, который читает содержимое указанного пользователем файла и выводит его в консоль.

#### Решение

```js
const fs = require('fs');

const filePath = process.argv[2];

fs.promises.readFile(filePath, 'utf8')
  .then(data => console.log(data))
  .catch(err => console.error(err));

```

_________________
_________________
_________________

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

## Решение

```js
const request = require('supertest');
const expect = require('chai').expect;
const { app, startServer, stopServer } = require('../app');
const userRoutes = require('../routes/userRoutes');

const TIMEOUT = 10000; // 10 seconds

describe('User API tests', function () {
  this.timeout(TIMEOUT);

  before(function (done) {
    startServer();
    done();
  });

  after(function (done) {
    stopServer();
    done();
  });

  describe('POST /api/users/register', function () {
    this.timeout(TIMEOUT);

    it('should register a new user', async () => {
      const newUser = {
        username: 'TestUser',
        email: 'testuser@example.com',
        password: 'testpassword',
      };

      const response = await request(app)
        .post('/api/users/register')
        .send(newUser);

      expect(response.status).to.equal(201);
      expect(response.body.message).to.equal('Пользователь успешно создан'); // Исправлено на русский язык
      expect(response.body.userId).to.be.a('number');
    });
  });

  describe('GET /api/users/:id', function () {
    this.timeout(TIMEOUT);

    let userId;
    before(async () => {
      const newUser = {
        username: 'TestUser',
        email: 'testuser@example.com',
        password: 'testpassword',
      };

      const response = await request(app)
        .post('/api/users/register')
        .send(newUser);

      userId = response.body.userId;
    });

    it('should get a user by id', async () => {
      const response = await request(app).get(`/api/users/${userId}`);

      expect(response.status).to.equal(200);
      expect(response.body.id).to.equal(userId);
      expect(response.body.username).to.equal('TestUser');
      expect(response.body.email).to.equal('testuser@example.com');
    });

    it('should return 404 when user not found', async () => {
      const nonExistentUserId = 999;
      const response = await request(app).get(`/api/users/${nonExistentUserId}`);

      expect(response.status).to.equal(404);
      expect(response.body.message).to.equal('User not found');
    });
  });
});

```