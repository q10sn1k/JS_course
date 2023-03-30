# server --part2

Создадим контроллер postController

```js
const postModel = require('../models/post');

// Получение списка всех постов
exports.getAllPosts = async (req, res, next) => {
  try {
    const posts = await postModel.getAllPosts();
    res.status(200).json(posts);
  } catch (err) {
    next(err);
  }
};

// Получение поста по ID
exports.getPostById = async (req, res, next) => {
  const postId = req.params.id;
  try {
    const post = await postModel.getPostById(postId);
    if (!post) {
      return res.status(404).json({ message: 'Post not found' });
    }
    res.status(200).json(post);
  } catch (err) {
    next(err);
  }
};

// Создание нового поста
exports.createPost = async (req, res, next) => {
  const { title, content } = req.body;
  try {
    const postId = await postModel.createPost(title, content);
    res.status(201).json({ message: 'Post created', postId });
  } catch (err) {
    next(err);
  }
};

// Обновление поста
exports.updatePost = async (req, res, next) => {
  const postId = req.params.id;
  const { title, content } = req.body;
  try {
    const affectedRows = await postModel.updatePost(postId, title, content);
    if (affectedRows === 0) {
      return res.status(404).json({ message: 'Post not found' });
    }
    res.status(200).json({ message: 'Post updated', affectedRows });
  } catch (err) {
    next(err);
  }
};

// Удаление поста
exports.deletePost = async (req, res, next) => {
  const postId = req.params.id;
  try {
    const affectedRows = await postModel.deletePost(postId);
    if (affectedRows === 0) {
      return res.status(404).json({ message: 'Post not found' });
    }
    res.status(200).json({ message: 'Post deleted', affectedRows });
  } catch (err) {
    next(err);
  }
};

```

создадим userController

```js
const userModel = require('../models/user');
const bcrypt = require('bcrypt');

// Создание нового пользователя
exports.createUser = async (req, res, next) => {
  try {
    const { username, email, password } = req.body;
    const userId = await userModel.createUser(username, email, password);
    res.status(201).json({ message: 'Пользователь успешно создан', userId });
  } catch (err) {
    next(err);
  }
};

// Получение пользователя по ID
exports.getUserById = async (req, res, next) => {
  const userId = req.params.id;
  try {
    const user = await userModel.getUserById(userId);
    if (!user) {
      return res.status(404).json({ message: 'User not found' });
    }
    res.status(200).json(user);
  } catch (err) {
    next(err);
  }
};

// Получение пользователя по email
exports.getUserByEmail = async (req, res, next) => {
  const email = req.params.email;
  try {
    const user = await userModel.getUserByEmail(email);
    if (!user) {
      return res.status(404).json({ message: 'User not found' });
    }
    res.status(200).json(user);
  } catch (err) {
    next(err);
  }
};

// Аутентификация пользователя
exports.authenticateUser = async (req, res, next) => {
  try {
    const { email, password } = req.body;
    const user = await userModel.authenticateUser(email, password);
    if (!user) {
      return res.status(401).json({ message: 'Неверный email или пароль' });
    }
    res.status(200).json({ message: 'Аутентификация прошла успешно', userId: user.id });
  } catch (err) {
    next(err);
  }
};
```

Создадим userRouter

```js
const express = require('express');
const userController = require('../controllers/userController');

const router = express.Router();

// Регистрация нового пользователя
router.post('/register', userController.createUser);

// Получение пользователя по ID
router.get('/:id', userController.getUserById);

// Получение пользователя по email
router.get('/:email', userController.getUserByEmail);

// Аутентификация пользователя
router.post('/login', userController.authenticateUser);

module.exports = router;

```

`app.js`


```js
const express = require('express');
const bodyParser = require('body-parser');
const cors = require('cors');
const userRoutes = require('./routes/userRoutes');
const postRoutes = require('./routes/postRoutes');
// const helmet = require("helmet");

const app = express();

// Middleware
app.use(cors());
app.use(bodyParser.json());
app.use(bodyParser.urlencoded({ extended: true }));

// Routes
app.use('/api/users', userRoutes);
app.use('/api/posts', postRoutes);

// Error handling
app.use((err, req, res, next) => {
  console.error(err.stack);
  res.status(500).json({ message: 'Ошибка сервера' });
});

const PORT = process.env.PORT || 5000;
// app.use('*', express.static('build'))
// app.use(express.static('build'));
// app.use(helmet());

let server;

function startServer() {
  server = app.listen(PORT, () => {
    console.log(`Server is running on port ${PORT}`);
  });
}

function stopServer() {
  server.close(() => {
    console.log('Server is stopped');
  });
}

module.exports = {
  app,
  startServer,
  stopServer
};

startServer();
```
_______________________

# Домашнее задание 

## Задача 1. 

создайте роутер postRouter.js