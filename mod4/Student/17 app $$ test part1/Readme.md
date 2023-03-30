## Разработаем серверную архитектуру приложения

```
├── server
│   ├── config
│   │   └── db.js
│   │
│   ├── controllers
│   │   ├── userController.js
│   │   └── postController.js
│   │
│   ├── models
│   │   ├── user.js
│   │   └── post.js
│   │
│   ├── routes
│   │   ├── userRoutes.js
│   │   └── postRoutes.js
│   │
│   ├── app.js
│   ├── .env
│   ├── package.json
│   └── package-lock.json
│
└── README.md
```

`project`: корневая директория проекта.\
`server`: директория с серверной частью приложения на Node.js.\
`config`: конфигурационные файлы.\
`db.js`: конфигурация подключения к базе данных MySQL.\
`controllers`: контроллеры приложения.\
`userController.js`: контроллер пользователей.\
`postController.js`: контроллер постов.\
`models`: модели приложения.\
`user.js`: модель пользователя.\
`post.js`: модель поста.\
`routes`: маршруты приложения.\
`userRoutes.js`: маршруты пользователей.\
`postRoutes.js`: маршруты постов.\
`app.js`: основной файл серверной части приложения.\
`.env`: файл с переменными окружения.\
`package.json`, `package-lock.json`: файлы с описанием зависимостей и настроек проекта для серверной части.\


создадим `config/db.js`: конфигурация подключения к базе данных MySQL.\


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

`.env`: файл с переменными окружения. укажем значения для коннекта с Базой Данных

```env
DB_HOST=localhost
DB_USER=mydbuser
DB_PASSWORD=mydbpassword
DB_NAME=mydbname
```

далее пишем тест `db.test.js`

Для написания тестов в формате `describe` и `it` используется библиотека Mocha. \
Для запуска тестов необходимо установить эту библиотеку.

```sql
// Импортирование зависимостей
const assert = require('assert');
const connection = require('./db');

// Описание тестов
describe('Подключение к базе данных', () => {
  // Проверка соединения с базой данных
  it('должно подключиться к базе данных без ошибок', (done) => {
    connection.connect((err) => {
      assert.strictEqual(err, null); // Проверяем, что ошибки нет
      done(); // Завершаем тест
    });
  });

  // Проверка типа объекта соединения
  it('должно экспортироваться соединение с базой данных типа object', () => {
    assert.strictEqual(typeof connection, 'object');
  });

  // Проверка свойства state соединения
  it('должно экспортироваться соединение с базой данных', () => {
  assert.strictEqual(typeof connection, 'object'); // Проверяем, что экспортирован объект
  assert.notStrictEqual(connection, null); // Проверяем, что объект не является null
});

});

```

добавим тест `db.test.js` в `package.json`

```json
{
  
  "scripts": {
    "start": "node app.js",
    "dev": "nodemon app.js",
    "test-db": "mocha config/db.test.js"
  }
  
}
```

запустим тест

```
npm run test-db
```

В базе данных создадим таблицы `users` и `posts`

```sql
CREATE TABLE IF NOT EXISTS users (
  id INT PRIMARY KEY AUTO_INCREMENT,
  username VARCHAR(255) NOT NULL,
  email VARCHAR(255) NOT NULL,
  password VARCHAR(255) NOT NULL
);

CREATE TABLE IF NOT EXISTS posts (
  id INT PRIMARY KEY AUTO_INCREMENT,
  title VARCHAR(255) NOT NULL,
  content TEXT NOT NULL,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

```

Добавим несколько тестовы постов

```sql
INSERT INTO posts (title, content)
VALUES ('Test post 1', 'Lorem ipsum dolor sit amet, consectetur adipiscing elit.'),
       ('Test post 2', 'Sed ut perspiciatis unde omnis iste natus error sit voluptatem accusantium doloremque laudantium.'),
       ('Test post 3', 'At vero eos et accusamus et iusto odio dignissimos ducimus qui blanditiis praesentium voluptatum deleniti atque corrupti quos dolores et quas molestias excepturi sint occaecati cupiditate non provident.');

```

Проверка

```sql
SELECT * FROM posts;
```



# Создадим модель post

```js
const db = require('../config/db');

// Получение списка всех постов
exports.getAllPosts = () => {
  return new Promise((resolve, reject) => {
    db.query('SELECT * FROM posts', (err, results) => {
      if (err) {
        return reject(err);
      }
      resolve(results);
    });
  });
};

// Получение поста по ID
exports.getPostById = (postId) => {
  return new Promise((resolve, reject) => {
    db.query('SELECT * FROM posts WHERE id = ?', [postId], (err, results) => {
      if (err) {
        return reject(err);
      }
      resolve(results[0]);
    });
  });
};

// Создание нового поста
exports.createPost = (title, content) => {
  return new Promise((resolve, reject) => {
    db.query('INSERT INTO posts (title, content) VALUES (?, ?)', [title, content], (err, results) => {
      if (err) {
        return reject(err);
      }
      resolve(results.insertId);
    });
  });
};

// Обновление поста
exports.updatePost = (postId, title, content) => {
  return new Promise((resolve, reject) => {
    db.query('UPDATE posts SET title = ?, content = ? WHERE id = ?', [title, content, postId], (err, results) => {
      if (err) {
        return reject(err);
      }
      resolve(results.affectedRows);
    });
  });
};

// Удаление поста
exports.deletePost = (postId) => {
  return new Promise((resolve, reject) => {
    db.query('DELETE FROM posts WHERE id = ?', [postId], (err, results) => {
      if (err) {
        return reject(err);
      }
      resolve(results.affectedRows);
    });
  });
};

```

# Создадим тест модели пост

```js
const { expect } = require('chai');
const sinon = require('sinon');
const db = require('../config/db');
const postModel = require('../models/post');

describe('postModel', function () {
  this.timeout(5000);

  afterEach(() => {
    sinon.restore();
  });

  it('should return all posts', (done) => {
    const postList = [
      { id: 1, title: 'Post 1', content: 'Content 1' },
      { id: 2, title: 'Post 2', content: 'Content 2' },
    ];
    sinon.stub(db, 'query').callsFake((query, callback) => {
      callback(null, postList);
    });

    postModel.getAllPosts().then((posts) => {
      expect(posts).to.deep.equal(postList);
      done();
    }).catch(done);
  });

  it('should return a post by id', (done) => {
    const postId = 1;
    const post = { id: postId, title: 'Post 1', content: 'Content 1' };
    sinon.stub(db, 'query').callsFake((query, postId, callback) => {
      callback(null, [post]);
    });

    postModel.getPostById(postId).then((retrievedPost) => {
      expect(retrievedPost).to.deep.equal(post);
      done();
    }).catch(done);
  });

  it('should create a new post', (done) => {
    const newPost = { title: 'New Post', content: 'New Content' };
    const insertId = 3;
    sinon.stub(db, 'query').callsFake((query, params, callback) => {
      callback(null, { insertId });
    });

    postModel.createPost(newPost.title, newPost.content).then((createdPostId) => {
      expect(createdPostId).to.equal(insertId);
      done();
    }).catch(done);
  });

  it('should update a post', (done) => {
    const postId = 1;
    const updatedPost = { title: 'Updated Post', content: 'Updated Content' };
    const affectedRows = 1;
    sinon.stub(db, 'query').callsFake((query, params, callback) => {
      callback(null, { affectedRows });
    });

    postModel.updatePost(postId, updatedPost.title, updatedPost.content).then((result) => {
      expect(result).to.equal(affectedRows);
      done();
    }).catch(done);
  });

  it('should delete a post', (done) => {
    const postId = 1;
    const affectedRows = 1;
    sinon.stub(db, 'query').callsFake((query, postId, callback) => {
      callback(null, { affectedRows });
    });

    postModel.deletePost(postId).then((result) => {
      expect(result).to.equal(affectedRows);
      done();
    }).catch(done);
  });

});

```
# добавим тест `post.test.js` в `package.json`


```json
{
  
  "scripts": {
    "start": "node app.js",
    "dev": "nodemon app.js",
    "test-db": "mocha config/db.test.js",
    "test-model-post": "mocha models/post.test.js"
  }
  
}
```

#запустим тест

```
npm run test-model-post
```
___________________
___________________
___________________

# Домашнее задание

# Задача 1

Создайте модель user

# Задача 2

Создайте тест модели user