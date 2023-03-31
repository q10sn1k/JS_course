# Домашнее задание

# Задача 1

Создайте модель user

## Решение

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



// Аутентификация пользователя
exports.authenticateUser = (email, password) => {
  return new Promise((resolve, reject) => {
    db.query('SELECT * FROM users WHERE email = ?', [email], (err, results) => {
      if (err) {
        return reject(err);
      }

      if (results.length === 0) {
        return resolve(null);
      }

      const user = results[0];
      bcrypt.compare(password, user.password, (err, passwordMatches) => {
        if (err) {
          return reject(err);
        }

        if (!passwordMatches) {
          return resolve(null);
        }

        resolve(user);
      });
    });
  });
};
```

# Задача 2

Создайте тест модели user

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


добавим тест `user.test.js` в `package.json`


```json
{
  
 "scripts": {
    "start": "node app.js",
    "dev": "nodemon app.js",
    "test-db": "mocha config/db.test.js",
    "test-model-post": "mocha models/post.test.js",
    "test-model-user": "mocha models/user.test.js"
  }
  
}
```

запустим тест

```
npm run test-model-user
```


