# Домашнее задание 

## Задача 1. 

создайте роутер postRouter.js

```js
const express = require('express');
const postController = require('../controllers/postController');

const router = express.Router();

// Получение списка всех постов
router.get('/', postController.getAllPosts);

// Получение поста по ID
router.get('/:id', postController.getPostById);

// Создание нового поста
router.post('/', postController.createPost);

// Обновление поста
router.put('/update-post', postController.updatePost);

// Удаление поста
router.delete('/delete-post', postController.deletePost);

module.exports = router;


```