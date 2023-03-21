# Домашнее задание:

Будем работать с разработанным ранее разработанным нами веб-приложением. (part 13 Express JS)

### Задача 1

Добавьте на главную страницу приложения дату и время последнего обновления страницы.

#### Решение

Мы можем добавить дату и время последнего обновления страницы, создав middleware-функцию, которая будет добавлять соответствующий заголовок ответа.\
Для этого создадим файл last-updated.js со следующим содержимым:

```js
function lastUpdated(req, res, next) {
    res.set('Last-Modified', new Date().toUTCString());
    next();
}

module.exports = lastUpdated;

```

Затем мы можем подключить эту middleware-функцию к маршруту главной страницы в файле app.js:

```js
const lastUpdated = require('./middlewares/last-updated');

app.get('/', lastUpdated, (req, res) => {
  res.render('index');
});

```

Теперь при каждом обновлении страницы будет устанавливаться соответствующий заголовок ответа, который будет содержать дату и время последнего обновления страницы.

### Задача 2

Добавьте на страницу пользователя кнопку для удаления его из списка пользователей.

#### Решение

Мы можем добавить на страницу пользователя кнопку для удаления его из списка пользователей, создав новый маршрут для обработки DELETE-запросов и обновив соответствующие шаблоны.

Для этого сначала добавим на страницу пользователя кнопку для удаления пользователя:

```html
<!DOCTYPE html>
<html>
<head>
    <title><%= user.name %></title>
    <link rel="stylesheet" href="/styles.css">
</head>
<body>
<h1><%= user.name %></h1>
<p>ID: <%= user.id %></p>
<p>Name: <%= user.name %></p>
<form method="post" action="/users/<%= user.id %>?_method=DELETE">
    <button type="submit">Delete user</button>
</form>
<a href="/users">Back to users</a>
</body>
</html>

```

Далее добавим новый маршрут для обработки DELETE-запросов в файл users.js:

```js
router.delete('/:id', (req, res) => {
  const userId = req.params.id;
  const userIndex = users.findIndex(user => user.id == userId);
  if (userIndex === -1) {
    res.status(404).send('User not found');
  } else {
    users.splice(userIndex, 1);
    res.redirect('/users');
  }
});

```


Мы определили маршрут для удаления пользователя по ID.\
При получении DELETE-запроса мы находим индекс пользователя в массиве users и удаляем его с помощью метода splice. \
Далее мы перенаправляем пользователя на страницу со списком пользователей.

Теперь, когда мы добавили новый маршрут и обновили соответствующие шаблолоны, мы можем добавить обработку DELETE-запросов на страницу пользователя, добавив middleware-функцию methodOverride в файл app.js:


```js
const methodOverride = require('method-override');
app.use(methodOverride('_method'));

```

Эта middleware-функция позволяет использовать методы PUT и DELETE в HTML-формах, которые не поддерживаются стандартом HTTP.\
Мы указываем символ _method в качестве параметра, чтобы определить, какой метод должен быть использован для обработки запроса.

### Задача 3

Добавьте на страницу со списком пользователей возможность редактирования имени пользователя.

#### Решение

Мы можем добавить на страницу со списком пользователей ссылки для редактирования имени пользователя, а также создать новый маршрут для обработки PUT-запросов на обновление имени пользователя.\
Для этого сначала добавим на страницу со списком пользователей ссылки для редактирования имени пользователя:

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Users</title>
    <link rel="stylesheet" href="/styles.css">
  </head>
  <body>
    <h1>Users</h1>
    <ul>
      <% users.forEach(user => { %>
        <li>
          <a href="/users/<%= user.id %>"><%= user.name %></a>
          <form method="post" action="/users/<%= user.id %>?_method=PUT">
            <input type="text" name="name" value="<%= user.name %>">
            <button type="submit">Update</button>
          </form>
        </li>
      <% }); %>
    </ul>
    <form method="post" action="/users">
      <input type="text" name="name" placeholder="Enter user name">
      <button type="submit">Add user</button>
    </form>
  </body>
</html>

```

Далее добавим новый маршрут для обработки PUT-запросов в файл users.js:


```js
router.put('/:id', (req, res) => {
  const userId = req.params.id;
  const userIndex = users.findIndex(user => user.id == userId);
  if (userIndex === -1) {
    res.status(404).send('User not found');
  } else {
    const newName = req.body.name;
    if (!newName) {
      res.status(400).send('Name is required');
    } else {
      users[userIndex].name = newName;
      res.redirect('/users');
    }
  }
});

```

Мы определили маршрут для обновления имени пользователя по ID. \
При получении PUT-запроса мы находим индекс пользователя в массиве users и обновляем его имя. \
Далее мы перенаправляем пользователя на страницу со списком пользователей. \
Если новое имя не было передано или пустое, мы возвращаем ошибку со статусом 400.

Теперь, когда мы добавили новый маршрут и обновили соответствующие шаблоны, мы можем добавить обработку PUT-запросов на страницу со списком пользователей, добавив middleware-функцию methodOverride в файл app.js:

```js
const methodOverride = require('method-override');
app.use(methodOverride('_method'));

```

Эта middleware-функция позволяет использовать методы PUT и DELETE в HTML-формах, которые не поддерживаются стандартом HTTP. \
Мы указываем символ _method в качестве параметра, чтобы определить, какой метод должен быть использован для обработки запроса.

# Ненадо добавлять эту функцию если вы решили задачу 2

### Задача 4

Добавьте на страницу сообщения возможность добавления и удаления комментариев к сообщению.

#### Решение

Мы можем добавить на страницу сообщения формы для добавления комментариев и ссылки для удаления комментариев, а также создать новый маршрут для обработки POST-запросов на добавление комментариев и DELETE-запросов на удаление комментариев. \
Для этого сначала добавим на страницу сообщения форму для добавления комментариев и ссылки для удаления комментариев:

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Message <%= message.id %></title>
    <link rel="stylesheet" href="/styles.css">
  </head>
  <body>
    <h1>Message <%= message.id %></h1>
    <p>ID: <%= message.id %></p>
    <p>Text: <%= message.text %></p>
    <h2>Comments</h2>
    <ul>
      <% message.comments.forEach(comment => { %>
        <li>
          <%= comment.text %>
          <form method="post" action="/messages/<%= message.id %>/comments/<%= comment.id %>?_method=DELETE">
            <button type="submit">Delete</button>
          </form>
        </li>
      <% }); %>
    </ul>
    <form method="post" action="/messages/<%= message.id %>/comments">
      <input type="text" name="text" placeholder="Enter comment">
      <button type="submit">Add comment</button>
    </form>
    <a href="/messages">Back to messages</a>
  </body>
</html>

```

Затем добавим новый маршрут для обработки POST-запросов на добавление комментариев в файл comments.js:

```js
router.post('/:messageId/comments', (req, res) => {
  const messageId = req.params.messageId;
  const messageIndex = messages.findIndex(message => message.id == messageId);
  if (messageIndex === -1) {
    res.status(404).send('Message not found');
  } else {
    const newComment = { id: uuidv4(), text: req.body.text };
    messages[messageIndex].comments.push(newComment);
    res.redirect(`/messages/${messageId}`);
  }
});

```

Мы определили маршрут для добавления комментария к сообщению по ID. \
При получении POST-запроса мы находим индекс сообщения в массиве messages и добавляем новый комментарий в массив comments данного сообщения. \
Затем мы перенаправляем пользователя на страницу с сообщением.

Далее добавим новый маршрут для обработки DELETE-запросов на удаление комментариев в файл comments.js:

```js
router.delete('/:messageId/comments/:commentId', (req, res) => {
  const messageId = req.params.messageId;
  const commentId = req.params.commentId;
  const messageIndex = messages.findIndex(message => message.id == messageId);
  if (messageIndex === -1) {
    res.status(404).send('Message not found');
  } else {
    const commentIndex = messages[messageIndex].comments.findIndex(comment => comment.id == commentId);
    if (commentIndex === -1) {
      res.status(404).send('Comment not found');
    } else {
      messages[messageIndex].comments.splice(commentIndex, 1);
      res.redirect(`/messages/${messageId}`);
    }
  }
});

```

Мы определили маршрут для удаления комментария к сообщению по ID сообщения и ID комментария. \
При получении DELETE-запроса мы находим индексы сообщения и комментария в массивах messages и comments соответственно и удаляем комментарий с помощью метода splice. \
Далее мы перенаправляем пользователя на страницу с сообщением.

Когда мы добавили новые маршруты и обновили соответствующие шаблоны, мы можем добавить обработку POST- и DELETE-запросов на страницу с сообщением, добавив middleware-функцию methodOverride в файл app.js:

```js
const methodOverride = require('method-override');
app.use(methodOverride('_method'));

```

Эта middleware-функция позволяет использовать методы PUT и DELETE в HTML-формах, которые не поддерживаются стандартом HTTP. Мы указываем символ _method в качестве параметра, чтобы определить, какой метод должен быть использован для обработки запроса.

# Не надо добавлять эту функцию если вы решили задачу 2 или 3 (2 и 3).