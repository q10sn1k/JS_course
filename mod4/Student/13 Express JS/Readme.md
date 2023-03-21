# Express js

## Введение в Express

Express - это минималистичный, гибкий и быстрый веб-фреймворк для Node.js. \
Он позволяет создавать серверные приложения с помощью простых и понятных API.

Для начала работы с Express, необходимо установить его с помощью npm:

```
npm install express
```

Далее, создадим файл app.js:

```js
const express = require('express');
const app = express();

app.get('/', (req, res) => {
  res.send('Hello, World!');
});

app.listen(3000, () => {
  console.log('Server listening on port 3000');
});

```

app.js создает новый экземпляр Express и добавляет обработчик маршрута для пути /. \
Когда приходит GET-запрос на /, сервер отправляет ответ с текстом "Hello, World!". \
Затем сервер запускается и начинает слушать порт 3000.

Запустите приложение с помощью команды node app.js и откройте в браузере страницу http://localhost:3000. \
Вы должны увидеть текст "Hello, World!".

## Обработка запросов в Express

Объект req представляет запрос, который пришел на сервер, а объект res представляет ответ, который будет отправлен обратно клиенту.\
В Express есть несколько методов, которые можно использовать для обработки разных типов запросов.

Например обработаем GET-запрос на /users:

```js
app.get('/users', (req, res) => {
  res.send('List of users');
});

```

Аналогично можно обработать POST-запрос на /users:

```js
app.post('/users', (req, res) => {
    res.send('New user added');
});

```

Используя методы get, post, put, delete и другие, можно обрабатывать различные типы запросов.


## Ответы в Express

Объект res в Express предоставляет множество методов для отправки ответов клиенту.

### Отправка текстового ответа

Для отправки текстового ответа, можно использовать метод res.send():

```js
app.get('/', (req, res) => {
  res.send('Hello, World!');
});

```

### Отправка JSON-ответа

Для отправки JSON-ответа, можно использовать метод res.json():

```js
app.get('/users', (req, res) => {
  const users = [
    { name: 'John', age: 30 },
    { name: 'Jane', age: 25 }
  ];
  res.json(users);
});

```

### Отправка статусного кода

Для отправки статусного кода, можно использовать метод res.status():

```js
app.get('/not-found', (req, res) => {
  res.status(404).send('Not found');
});

```

### Отправка заголовка

Для отправки заголовка, можно использовать метод res.set():

```js
app.get('/headers', (req, res) => {
  res.set('Content-Type', 'text/html');
  res.send('<h1>Hello, World!</h1>');
});

```

## Работа со статическими файлами в Express

Express предоставляет удобный способ обслуживания статических файлов, таких как CSS, JavaScript и изображения.

Для этого, необходимо использовать метод express.static() и передать ему путь к папке, содержащей статические файлы. \
Например, если у вас есть папка public, содержащая файл styles.css, то можно написать:

```js
app.use(express.static('public'));

```
Теперь, когда клиент запрашивает файл /styles.css, сервер автоматически ищет его в папке public.

## Работа с HTML в Express

Express позволяет формировать HTML-страницы по запросу.

Для этого, можно использовать метод res.send() и передать ему HTML-код в виде строки:

```js
app.get('/', (req, res) => {
  const html = `
    <html>
      <head>
        <title>Hello, World!</title>
      </head>
      <body>
        <h1>Hello, World!</h1>
      </body>
    </html>
  `;
  res.send(html);
});

```

## Параметры маршрутов в Express

Express позволяет использовать параметры в маршрутах, чтобы динамически генерировать ответы.

Для этого, необходимо использовать двоеточие (:) перед именем параметра.\
Например, маршрут /users/:id означает, что в этом маршруте будет использоваться параметр id.

Чтобы получить значение параметра внутри обработчика маршрута, можно использовать свойство params объекта req.

```js
app.get('/users/:id', (req, res) => {
  const userId = req.params.id;
  res.send(`User with ID ${userId}`);
});

```

Если клиент запрашивает URL /users/123, то сервер отправит ответ "User with ID 123".

## Регулярные выражения в маршрутах Express

Express позволяет использовать регулярные выражения для определения маршрутов.

Например, чтобы определить маршрут, соответствующий всем URL, начинающимся с /users/ и заканчивающимся числом, можно использовать регулярное выражение:

```js
app.get(/^\/users\/(\d+)$/, (req, res) => {
  const userId = req.params[0];
  res.send(`User with ID ${userId}`);
});

```

Мы использовали регулярное выражение /^\/users\/(\d+)$/, которое соответствует строкам, начинающимся с /users/, затем содержащим одно или более чисел, и заканчивающимся.

## Приоритет маршрутов и обработка ошибок в Express

Когда приходит запрос, Express проверяет маршруты по порядку и выбирает первый, который соответствует запросу. \
Поэтому важно определять маршруты в правильном порядке.

Кроме того, в Express можно определить обработчики ошибок, которые вызываются, если ни один из маршрутов не соответствует запросу или происходит ошибка в обработке запроса.

Например, чтобы определить обработчик ошибки для 404-ошибки, можно:

```js
app.use((req, res, next) => {
  res.status(404).send('Not found');
});

```

Этот код определяет обработчик ошибки, который отправляет ответ с текстом "Not found" и статусом 404 для всех запросов, которые не соответствуют ни одному маршруту.

## Роутинг и массивы в Express

Express позволяет определять множество маршрутов в одном месте, используя массивы.

К примеру определим несколько маршрутов для пользователей и сообщений:

```js
const users = [
  { id: 1, name: 'John' },
  { id: 2, name: 'Jane' }
];

const messages = [
  { id: 1, text: 'Hello, World!' },
  { id: 2, text: 'How are you?' }
];

app.get(['/users', '/api/users'], (req, res) => {
  res.json(users);
});

app.get(['/messages', '/api/messages'], (req, res) => {
  res.json(messages);
});

```

Мы определили массивы users и messages, которые содержат список пользователей и сообщений соответственно. \
Затем мы определили два маршрута для каждого ресурса: один для основного URL, и один для URL, начинающегося с /api/.

Таким образом, когда клиент запрашивает /users или /api/users, сервер отправляет список пользователей, а когда клиент запрашивает /messages или /api/messages, сервер отправляет список сообщений.

## Группировка маршрутов с помощью Router в Express

Express позволяет группировать маршруты в отдельные модули с помощью объекта Router.

Для того, чтобы создать отдельный модуль для маршрутов пользователей, можно:

```js
const express = require('express');
const router = express.Router();

const users = [
  { id: 1, name: 'John' },
  { id: 2, name: 'Jane' }
];

router.get('/', (req, res) => {
  res.json(users);
});

router.get('/:id', (req, res) => {
  const userId = req.params.id;
  const user = users.find(user => user.id == userId);
  if (!user) {
    res.status(404).send('User not found');
  } else {
    res.json(user);
  }
});

module.exports = router;

```

В этом примере мы создали отдельный модуль для маршрутов пользователей, определив его в отдельном файле. \
Внутри модуля мы определили массив users и два маршрута: один для получения списка пользователей, и один для получения конкретного пользователя по ID.

Затем мы экспортируем объект router и импортируем его в основной файл приложения:

```js
const usersRouter = require('./users');
app.use('/users', usersRouter);

```

Теперь все маршруты, определенные в модуле users, будут доступны по URL, начинающемуся с /users/.

# Создадим веб-приложение на Express

Наше приложение будет представлять собой простой веб-сайт, который позволяет просматривать список пользователей и сообщений, а также добавлять новых пользователей.

При запуске приложения пользователь видит главную страницу со ссылками на страницы пользователей и сообщений.\
При переходе на страницу пользователей пользователь видит список всех пользователей, а также имеет возможность добавить нового пользователя.\
При выборе конкретного пользователя пользователь переходит на страницу с информацией о выбранном пользователе.

Аналогично, при переходе на страницу сообщений пользователь видит список всех сообщений, а при выборе конкретного сообщения - страницу с информацией о выбранном сообщении.

Весь HTML-код нашего приложения генерируется с помощью шаблонизатора EJS, а CSS-стили определяются в файле styles.css. \
Для обработки запросов и маршрутизации используется фреймворк Express. \
Вся информация о пользователе и сообщениях хранится в виде массивов в соответствующих файлах, а POST-запросы на добавление новых пользователей обрабатываются с помощью механизма форм.

Постоим архитектуру нашего приложения

```
app/
├── node_modules/
├── public/
│   ├── index.html
│   └── styles.css
├── routes/
│   ├── index.js
│   ├── users.js
│   └── messages.js
├── views/
│   ├── index.ejs
│   └── users.ejs
├── app.js
├── package.json
└── package-lock.json
```

* app.js - основной файл приложения, который объединяет все маршруты и настройки;

* package.json и package-lock.json - файлы, используемые для управления зависимостями приложения;

* public - папка, содержащая статические файлы, такие как изображения, CSS и JavaScript;

* routes - папка, содержащая отдельные модули маршрутов для каждого ресурса;

* views - папка, содержащая шаблоны для генерации HTML-страниц.

Для начала давайте установим Express, EJS (движок шаблонов) и body-parser (для обработки POST-запросов) с помощью следующей команды в терминале:

```
npm install express ejs body-parser --save
```

Теперь создадим файл app.js и добавим необходимые настройки. В этом примере мы будем использовать EJS в качестве движка шаблонов и body-parser для обработки POST-запросов:

```js
// Подключаем необходимые модули
const express = require('express');
const bodyParser = require('body-parser');
const path = require('path');
const app = express();

// Настраиваем EJS в качестве движка шаблонов
app.set('view engine', 'ejs');
app.set('views', path.join(__dirname, 'views'));

// Используем body-parser для обработки POST-запросов
app.use(bodyParser.urlencoded({ extended: false }));

// Подключаем маршруты
const indexRouter = require('./routes/index');
const usersRouter = require('./routes/users');
const messagesRouter = require('./routes/messages');

app.use('/', indexRouter);
app.use('/users', usersRouter);
app.use('/messages', messagesRouter);

// Запускаем сервер на порту 3000
app.listen(3000, () => {
  console.log('Server started on port 3000');
});

```

создадим файлы маршрутов в папке routes. Сначала создадим файл index.js, который будет обрабатывать главную страницу:

```js
const express = require('express');
const router = express.Router();

router.get('/', (req, res) => {
  res.render('index');
});

module.exports = router;

```

Мы определили маршрут для главной страницы, который отправляет шаблон index в качестве ответа.

Далее создадим файл users.js, который будет обрабатывать маршруты для пользователей:

```js
const express = require('express');
const router = express.Router();

const users = [
  { id: 1, name: 'John' },
  { id: 2, name: 'Jane' }
];

router.get('/', (req, res) => {
  res.render('users', { users });
});

router.get('/:id', (req, res) => {
  const userId = req.params.id;
  const user = users.find(user => user.id == userId);
  if (!user) {
    res.status(404).send('User not found');
  } else {
    res.render('user', { user });
  }
});

router.post('/', (req, res) => {
  const newUser = { id: Date.now(), name: req.body.name };
  users.push(newUser);
  res.redirect('/users');
});

module.exports = router;

```

Мы определили массив users, содержащий список пользователей.\
Затем мы определили маршрут для получения списка пользователей, который отправляет шаблон users в качестве ответа, и маршрут для получения конкретного пользователя по ID, который отправляет шаблон user в качестве ответа.

Также определили маршрут для обработки POST-запроса на добавление нового пользователя. \
Этот маршрут создает нового пользователя с уникальным ID и именем, переданным в POST-запросе, и добавляет его в массив users.\
Далее мы перенаправляем пользователя на страницу со списком пользователей.





Создадим файл messages.js, который будет обрабатывать маршруты для сообщений:

```js
const express = require('express');
const router = express.Router();

const messages = [
  { id: 1, text: 'Hello, World!' },
  { id: 2, text: 'How are you?' }
];

router.get('/', (req, res) => {
  res.render('messages', { messages });
});

router.get('/:id', (req, res) => {
  const messageId = req.params.id;
  const message = messages.find(message => message.id == messageId);
  if (!message) {
    res.status(404).send('Message not found');
  } else {
    res.render('message', { message });
  }
});

module.exports = router;

```

Мы определили массив messages, содержащий список сообщений.\
Затем мы определили маршрут для получения списка сообщений, который отправляет шаблон messages в качестве ответа, и маршрут для получения конкретного сообщения по ID, который отправляет шаблон message в качестве ответа.

Для генерации HTML-страниц мы будем использовать шаблоны, определенные в папке views.


Создадим файл index.ejs, который будет определять шаблон для главной страницы:

```html
<!DOCTYPE html>
<html>
<head>
    <title>Express App</title>
    <link rel="stylesheet" href="/styles.css">
</head>
<body>
<h1>Welcome to Express App</h1>
<nav>
    <ul>
        <li><a href="/users">Users</a></li>
        <li><a href="/messages">Messages</a></li>
    </ul>
</nav>
</body>
</html>

```

В этом шаблоне мы определили заголовок страницы, подключили файл стилей styles.css и создали навигационное меню, содержащее ссылки на страницы с пользовательскими и сообщениями.

Создадим теперь файлы users.ejs и user.ejs, которые будут определять шаблоны для страниц со списком пользователей и отдельного пользователя соответственно:

users.ejs:

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
        <li><a href="/users/<%= user.id %>"><%= user.name %></a></li>
      <% }); %>
    </ul>
    <form method="post" action="/users">
      <input type="text" name="name" placeholder="Enter user name">
      <button type="submit">Add user</button>
    </form>
  </body>
</html>

```

user.ejs:

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
    <a href="/users">Back to users</a>
  </body>
</html>

```

В этих шаблонах мы используем специальный синтаксис EJS для вставки переменных и циклов в HTML-разметку.



Создадим файлы messages.ejs и message.ejs, которые будут определять шаблоны для страниц со списком сообщений и отдельного сообщения соответственно:

messages.ejs:

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Messages</title>
    <link rel="stylesheet" href="/styles.css">
  </head>
  <body>
    <h1>Messages</h1>
    <ul>
      <% messages.forEach(message => { %>
        <li><a href="/messages/<%= message.id %>"><%= message.text %></a></li>
      <% }); %>
    </ul>
    <a href="/users">Back to users</a>
  </body>
</html>

```

message.ejs:

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
  <a href="/messages">Back to messages</a>

  </body>
</html>
```

Здесь мы также использовали специальный синтаксис EJS для вставки переменных в HTML-разметку.

Создадим файл styles.css в папке public, который будет определять стили для всех страниц:

```css
body {
  margin: 0;
  padding: 0;
  font-family: Arial, sans-serif;
  font-size: 16px;
}

nav {
  background-color: #333;
  color: #fff;
  padding: 10px;
}

nav ul {
  list-style: none;
  margin: 0;
  padding: 0;
}

nav ul li {
  display: inline-block;
  margin-right: 10px;
}

nav ul li a {
  color: #fff;
  text-decoration: none;
}

ul {
  list-style: none;
  margin: 0;
  padding: 0;
}

form {
  margin-top: 10px;
}

form input[type="text"] {
  padding: 5px;
  border: 1px solid #ccc;
  border-radius: 3px;
  margin-right: 5px;
}

form button[type="submit"] {
  padding: 5px 10px;
  background-color: #333;
  color: #fff;
  border: none;
  border-radius: 3px;
  cursor: pointer;
}

a {
  color: #333;
  text-decoration: none;
}

```

Мы определили стили для элементов страницы, таких как навигационное меню, списки, формы и ссылки.

Когда мы определили все необходимые файлы, мы можем запустить приложение, выполнив команду в терминале. :

```
node app.js
 
```

Откройте браузер и перейдите на адрес http://localhost:3000 

Вы на главной странице нашего приложения.


__________________________
__________________________
__________________________
# Домашнее задание:

Будем работать с разработанным ранее разработанным нами веб-приложением. (part 13 Express JS)

### Задача 1

Добавьте на главную страницу приложения дату и время последнего обновления страницы.

### Задача 2

Добавьте на страницу пользователя кнопку для удаления его из списка пользователей.


### Задача 3

Добавьте на страницу со списком пользователей возможность редактирования имени пользователя.


### Задача 4

Добавьте на страницу сообщения возможность добавления и удаления комментариев к сообщению.
