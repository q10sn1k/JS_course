# Шаблонизатор Handlebars в Express

Handlebars - это шаблонизатор для HTML, который позволяет создавать шаблоны,\
которые можно заполнить данными и затем рендерить на сервере или на стороне клиента.\
В Express можно использовать Handlebars для генерации динамических HTML-страниц на сервере.

Для начала необходимо установить пакет express-handlebars, который позволяет использовать Handlebars в Express:

```
npm install express-handlebars
```

Затем нужно настроить Express для использования Handlebars в качестве шаблонизатора.\
Для этого в файле `app.js` нужно добавить следующий код:

```js
const express = require('express');
const exphbs  = require('express-handlebars');

const app = express();

app.engine('handlebars', exphbs());
app.set('view engine', 'handlebars');

```

Здесь мы настраиваем Express для использования Handlebars в качестве шаблонизатора, а также указывает, что расширение файлов шаблонов должно быть .handlebars.

Теперь можно создавать шаблоны Handlebars и рендерить их на сервере с помощью Express.\
Для этого нужно создать файл шаблона с расширением `.handlebars` в папке views, например, `index.handlebars`.\
Далее нужно указать Express, какой шаблон использовать для отображения страницы. 


```js
app.get('/', function (req, res) {
    res.render('index', { title: 'Hello World' });
});
```


Этот код указывает Express использовать шаблон `index.handlebars` для отображения страницы при обращении к корневому URL-адресу. Второй параметр метода render передает данные, которые нужно использовать для заполнения шаблона.

В файле `index.handlebars` можно использовать различные конструкции Handlebars, \
такие как `{{}}` для вставки переменных, {{#each}} для итерации по массивам и т.д. \
Например выведем заголовок страницы:

```html
<!DOCTYPE html>
<html>
  <head>
    <title>{{title}}</title>
  </head>
  <body>
    <h1>{{title}}</h1>
  </body>
</html>

```

При обращении к корневому URL-адресу сервер будет возвращать HTML-страницу с заголовком "Hello World".


# Хелперы

В шаблонах Handlebars можно использовать различные функции и хелперы, которые позволяют создавать более сложные и динамичные шаблоны. \
Например, можно использовать условные конструкции для отображения различного контента в зависимости от данных.

Для использования условных конструкций в `Handlebars` можно использовать конструкцию `{{#if}}...{{else}}...{{/if}}`. \

```html
{{#if loggedIn}}
  <p>Welcome back, {{username}}!</p>
{{else}}
  <p>Please log in to continue.</p>
{{/if}}

```

Можно использовать циклы {{#each}} для вывода списка элементов. \
Например, можно создать шаблон для отображения списка пользователей:

```html
<ul>
  {{#each users}}
    <li>{{this.name}} ({{this.email}})</li>
  {{/each}}
</ul>

```

Для передачи данных в шаблон можно использовать объекты JavaScript.\
Создадим объект data:

```js
const data = {
  title: 'Мой сайт',
  loggedIn: true,
  username: 'Иван Иванов',
  users: [
    { name: 'Алиса', email: 'alice@example.com' },
    { name: 'Максим', email: 'maksim@example.com' },
    { name: 'Глеб', email: 'gleb@example.com' }
  ]
};

```

можно использовать этот объект для отображения данных в шаблоне. \
К примеру отобразим заголовок страницы, используя свойство title:

```html
<!DOCTYPE html>
<html>
  <head>
    <title>{{title}}</title>
  </head>
  <body>
    <h1>{{title}}</h1>
    {{#if loggedIn}}
      <p>Welcome back, {{username}}!</p>
    {{else}}
      <p>Log in to continue.</p>
    {{/if}}
    <ul>
      {{#each users}}
        <li>{{this.name}} ({{this.email}})</li>
      {{/each}}
    </ul>
  </body>
</html>

```

Для рендеринга шаблона с данными можно использовать метод render объекта res, который отправляет HTML-страницу с заполненным шаблоном на клиент:


```js
const express = require('express');
const exphbs = require('express-handlebars');

const app = express();

// Настраиваем Handlebars в качестве шаблонизатора
app.engine('handlebars', exphbs());
app.set('view engine', 'handlebars');

// Разрешаем использовать статические файлы из папки public
app.use(express.static('public'));

// Главная страница
app.get('/', (req, res) => {
  const data = {
    title: 'Мой сайт',
    loggedIn: true,
    username: 'Иван Иванов',
    users: [
      { name: 'Алиса', email: 'alice@example.com' },
      { name: 'Максим', email: 'maksim@example.com' },
      { name: 'Глеб', email: 'gleb@example.com' }
    ]
  };
  res.render('index', data);
});

app.listen(3000, () => {
  console.log('Сервер запущен на порту 3000');
});

```
# Создадим простейшее приложение, которое будет иметь две страницы: главную страницу и страницу "О нас".

Установим зависимости

```
npm install express express-handlebars
```


создадим файл app.js и настроим Express:

```js
// Подключаем модуль Express
const express = require('express');

// Подключаем модуль Handlebars для шаблонизации
const exphbs = require('express-handlebars');

// Создаем объект приложения
const app = express();

// Настраиваем Handlebars в качестве шаблонизатора
app.engine('handlebars', exphbs());
app.set('view engine', 'handlebars');

// Создаем маршрут для главной страницы
app.get('/', (req, res) => {
  // Рендерим шаблон index.handlebars
  res.render('index', { title: 'Главная страница' });
});

// Создаем маршрут для страницы "О нас"
app.get('/about', (req, res) => {
  // Рендерим шаблон about.handlebars
  res.render('about', { title: 'О нас' });
});

// Запускаем сервер на порту 3000
app.listen(3000, () => {
  console.log('Сервер запущен на порту 3000');
});

```


Cоздадим папку views и добавим туда два файла шаблонов: index.handlebars и about.handlebars. 


`index.handlebars`

```html
<!DOCTYPE html>
<html>
  <head>
    <title>{{title}}</title>
  </head>
  <body>
    <h1>{{title}}</h1>
    <p>Добро пожаловать на главную страницу!</p>
    <a href="/about">О нас</a>
  </body>
</html>

```
`about.handlebars`

```html
<!DOCTYPE html>
<html>
  <head>
    <title>{{title}}</title>
  </head>
  <body>
    <h1>{{title}}</h1>
    <p>Мы - это команда профессионалов!</p>
    <a href="/">Главная</a>
  </body>
</html>

```

Запустим приложение командой node `app.js` и перейдем по адресу `http://localhost:3000` или `http://localhost:3000/about`

Убежимся что все работает корректно

# Создадим простое веб-приложения с использованием Node.js и Express.js, у которого будет несколько страниц с навигационным меню. 

Будем использовать шаблонизатор Handlebars для генерации HTML-кода страниц и CSS для стилизации страниц и элементов интерфейса.

В приложении создадим следующие страницы:

Главная страница (путь "/"), на которой отображается информация о сайте и данные из объекта "data" \
На странице реализуем элементы интерфейса:
* заголовок, 
* текст, 
* список, 
* меню навигации. 

Текст и заголовок зададим в шаблоне Handlebars, список и меню навигации должны определим в коде приложения.



Страница "О нас" (путь "/about"), на которой отображается некоторая  о компании и ее сотрудниках. \
На странице реализуем следующие элементы интерфейса: 
* заголовок, 
* текст, 
* список, 
* меню навигации. 

Текст и заголовок должны зададим в шаблоне Handlebars, список и меню навигации должны определим в коде приложения.



Страница "Контакты" (путь "/contacts"), на которой отобразим контактные данные компании. \
На странице также реализуем следующие элементы интерфейса: 
* заголовок, 
* текст, 
* список, 
* меню навигации. \

Заголовок и текст зададим в шаблоне Handlebars, таблицау и меню навигации определим в коде приложения.


Будем использовать Express.js для обработки запросов,\
Handlebars для генерации HTML-кода страниц и CSS для стилизации страниц и элементов интерфейса.\

В качестве источника данных для страниц используем объект "data", который определим в коде приложения.

# архитектура приложения

```
├──public/
│      ├──css/
│           ├──style.css
├──views/
│      ├──layouts/
│           ├──main.handlebars
│      ├──index.handlebars
├──app.js

```

* `public` содержит директорию `css` с файлом стилей `style.css`. 
* `views` содержит шаблоны страниц `index.handlebars` и `layouts/main.handlebars` (общий макет для всех страниц). 
* Файл `app.js` - главный файл приложения.


app.js
```js
const express = require('express');
const exphbs = require('express-handlebars');

const app = express();

// Настраиваем Handlebars в качестве шаблонизатора
app.engine('handlebars', exphbs());
app.set('view engine', 'handlebars');

// Разрешаем использовать статические файлы из папки public
app.use(express.static('public'));

// Главная страница
app.get('/', (req, res) => {
  const data = {
    title: 'Мой сайт',
    loggedIn: true,
    username: 'Иван Иванов',
    users: [
      { name: 'Алиса', email: 'alice@example.com' },
      { name: 'Максим', email: 'maksim@example.com' },
      { name: 'Глеб', email: 'gleb@example.com' }
    ]
  };
  res.render('index', data);
});

app.listen(3000, () => {
  console.log('Сервер запущен на порту 3000');
});

```

views/layouts/main.handlebars

```html
<!DOCTYPE html>
<html>
  <head>
    <title>{{title}}</title>
    <link rel="stylesheet" href="/css/style.css">
  </head>
  <body>
    <nav>
      <ul>
        <li{{#if (eq active 'home')}} class="active"{{/if}}><a href="/">Главная</a></li>
        <li{{#if (eq active 'about')}} class="active"{{/if}}><a href="/about">О нас</a></li>
        <li{{#if (eq active 'contacts')}} class="active"{{/if}}><a href="/contacts">Контакты</a></li>
      </ul>
    </nav>
    {{{body}}}
  </body>
</html>

```
index.handlebars

```html
{{!-- Шаблон главной страницы --}}
{{!-- Подключаем общий макет --}}
{{#extend 'layouts/main'}}
  {{!-- Устанавливаем заголовок страницы --}}
  {{#content 'title'}}{{title}}{{/content}}

  {{!-- Контент страницы --}}
  <h1>{{title}}</h1>
  {{#if loggedIn}}
    <p>Добро пожаловать, {{username}}!</p>
  {{else}}
    <p>Войдите, чтобы продолжить.</p>
  {{/if}}
  <ul>
    {{#each users}}
      <li>{{this.name}} ({{this.email}})</li>
    {{/each}}
  </ul>
{{/extend}}

```

public/css/style.css

```css
/* Стили для всех страниц приложения */
body {
  font-family: Arial, sans-serif;
  font-size: 16px;
  color: #333;
}

h1 {
  font-size: 24px;
  margin-bottom: 20px;
}

a {
  color: #0077c0;
  text-decoration: none;
}

a:hover {
  text-decoration: underline;
}

p {
  margin-bottom: 10px;
}

/* Стили для меню навигации */
nav {
  margin-bottom: 20px;
}

nav ul {
  list-style: none;
  margin: 0;
  padding: 0;
}

nav li {
  display: inline-block;
  margin-right: 20px;
}

nav li.active a {
  color: #333;
  text-decoration: none;
}
```
______________________________________
______________________________________
______________________________________

# Домашнее задание:

### Задача 1

Написать интернет-магазин на Node.js с использованием фреймворка Express и шаблонизатора Handlebars. \
Сайт должен иметь несколько страниц: главную, страницу каталога товаров, страницу товара, корзину и страницы авторизации и регистрации пользователей.\
В каталоге товаров должен быть реализован функционал фильтрации товаров по цене и категории с помощью хелперов. \
Все данные о товарах должны храниться в файле `data/products.json.` \
Стили должны быть реализованы с помощью Bootstrap.

## Архитектура приложения:

```
├── app.js
├── data
│   └── products.json
├── helpers.js
├── node_modules
├── package-lock.json
├── package.json
└── views
    ├── catalog.handlebars
    ├── layouts
    │   └── main.handlebars
    └── partials
        ├── footer.handlebars
        └── header.handlebars
```