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

# решение

`app.js`

```js
const express = require('express');
const hbs = require('hbs');
const path = require('path');
const productsData = require('./data/products.json');

const app = express();

// Устанавливаем путь к папке с шаблонами
app.set('views', path.join(__dirname, 'views'));

// Устанавливаем шаблонизатор Handlebars как движок для рендеринга страниц
app.set('view engine', 'hbs');

// Регистрируем частичные шаблоны для шапки и подвала страницы
hbs.registerPartials(path.join(__dirname, 'views/partials'));

// Регистрируем хелперы для форматирования цены и изображения товара
require('./helpers');

// Определяем маршрут для главной страницы
app.get('/', (req, res) => {
  res.render('index', {
    title: 'Интернет-магазин',
    header: 'Добро пожаловать в наш магазин!',
  });
});

// Определяем маршрут для страницы каталога товаров
app.get('/catalog', (req, res) => {
  const { category, minPrice, maxPrice } = req.query;

  // Фильтруем товары по категории и цене
  const filteredProducts = productsData.filter((product) => {
    if (category && product.category !== category) {
      return false;
    }

    if (minPrice && product.price < minPrice) {
      return false;
    }

    if (maxPrice && product.price > maxPrice) {
      return false;
    }

    return true;
  });

  res.render('catalog', {
    title: 'Каталог',
    products: filteredProducts,
  });
});

// Запускаем сервер
app.listen(3000, () => {
  console.log('Сервер запущен на порту 3000');
});
```

`data/products.json`

```json
[
  {
    "id": 1,
    "name": "Смартфон Apple iPhone 12 Pro",
    "category": "Смартфоны",
    "description": "Новый iPhone 12 Pro с OLED-экраном Super Retina XDR 6,1 дюйма. Новая камера Pro с возможностью съемки в формате Apple ProRAW и LiDAR-сканером для AR-приложений.",
    "image": "/images/iphone12pro.jpg",
    "price": 99990
  },
  {
    "id": 2,
    "name": "Ноутбук Apple MacBook Air",
    "category": "Ноутбуки",
    "description": "Новый MacBook Air с чипом M1 – наш самый популярный ноутбук в новом исполнении. Новый чип M1 обеспечивает потрясающую производительность ЦП и ГП, длительное время автономной работы и невероятную графику в одном компактном корпусе.",
    "image": "/images/macbookair.jpg",
    "price": 109990
  },
  {
    "id": 3,
    "name": "Планшет Apple iPad Pro",
    "category": "Планшеты",
    "description": "Новый iPad Pro с процессором M1, поддержкой 5G и Liquid Retina XDR-дисплеем. Улучшенная камера с возможностью съемки в формате Apple ProRAW и сканер LiDAR для AR-приложений.",
    "image": "/images/ipadpro.jpg",
    "price": 99990
  },
  {
    "id": 4,
    "name": "Умные часы Apple Watch Series 6",
    "category": "Умные часы",
    "description": "Apple Watch Series 6 с датчиком кислорода в крови и Always-On Retina-дисплеем. Можно сделать ЭКГ прямо с руки, отслеживать тренировки и получать уведомления.",
    "image": "/images/applewatch.jpg",
    "price": 49990
  },
  {
    "id": 5,
    "name": "Наушники Apple AirPods Max",
    "category": "Наушники",
    "description": "Новые наушники AirPods Max с акустической системой высшего качества и возможностью активного шумоподавления. Невероятно комфортные наушники с качественной звуковой подачей.",
    "image": "/images/airpodsmax.jpg",
    "price": 79990
  }
]

```

`helpers.js`

```js
const Handlebars = require('handlebars');

// Хелпер для форматирования цены
Handlebars.registerHelper('formatPrice', function (price) {
  return new Intl.NumberFormat('ru-RU', { style: 'currency', currency: 'RUB' }).format(price);
});

// Хелпер для создания списка категорий
Handlebars.registerHelper('categoryList', function (categories, selected) {
  let html = '<option value="">Все категории</option>';
  categories.forEach((category) => {
    html += `<option value="${category}"${category === selected ? ' selected' : ''}>${category}</option>`;
  });
  return html;
});

// Хелпер для выделения выбранного значения в селекте
Handlebars.registerHelper('isSelected', function (value, selected) {
  return value === selected ? ' selected' : '';
});

```

`package.json`

```json
{
  "name": "my-app",
  "version": "1.0.0",
  "description": "Мое приложение",
  "main": "index.js",
  "scripts": {
    "start": "node index.js",
    "dev": "nodemon index.js"
  },
  "dependencies": {
    "express": "^4.17.1",
    "handlebars": "^4.7.7"
  },
  "devDependencies": {
    "nodemon": "^2.0.13"
  }
}

```

Этот файл описывает информацию о проекте, его зависимости и скрипты, которые можно запустить из командной строки с помощью `npm run` Здесь name и version указывают на имя и версию проекта соответственно, а description содержит краткое описание проекта.

`main` указывает на главный файл приложения, который будет запускаться при старте сервера. \
`scripts` позволяет определить команды для запуска приложения или различных скриптов.

`dependencies` содержит список зависимостей, необходимых для работы приложения.\
Мы используем фреймворк `Express` и шаблонизатор `Handlebars`.

`devDependencies` содержит список зависимостей, необходимых только во время разработки
`Nodemon` для автоматической перезагрузки сервера при изменениях в коде.


`catalog.handlebars`

```html
{{#layout 'main'}}
  {{#contentFor 'title'}}Каталог{{/contentFor}}
  <h1>Каталог товаров</h1>
  <form class="form-inline mb-3" method="get" action="/catalog">
    <div class="form-group mr-3">
      <label for="category">Категория:</label>
      <select id="category" name="category" class="form-control">
        {{{categoryList categories category}}}
      </select>
    </div>
    <div class="form-group mr-3">
      <label for="minPrice">Минимальная цена:</label>
      <input type="number" id="minPrice" name="minPrice" class="form-control" value="{{minPrice}}">
    </div>
    <div class="form-group mr-3">
      <label for="maxPrice">Максимальная цена:</label>
      <input type="number" id="maxPrice" name="maxPrice" class="form-control" value="{{maxPrice}}">
    </div>
    <button type="submit" class="btn btn-primary">Применить</button>
  </form>
  <div class="row">
    {{#each products}}
      <div class="col-md-4 mb-4">
        <div class="card">
          <img src="{{image}}" class="card-img-top" alt="{{name}}">
          <div class="card-body">
            <h5 class="card-title">{{name}}</h5>
            <p class="card-text">{{description}}</p>
            <p class="card-text">{{formatPrice price}}</p>
          </div>
        </div>
      </div>
    {{/each}}
  </div>
{{/layout}}

```

`main.handlebars`

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>{{{title}}}</title>
    <link rel="stylesheet" href="/css/bootstrap.min.css">
  </head>
  <body>
    <nav class="navbar navbar-expand-lg navbar-dark bg-dark mb-3">
      <div class="container">
        <a class="navbar-brand" href="/">Мой магазин</a>
        <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarNav" aria-controls="navbarNav" aria-expanded="false" aria-label="Toggle navigation">
          <span class="navbar-toggler-icon"></span>
        </button>
        <div class="collapse navbar-collapse" id="navbarNav">
          <ul class="navbar-nav">
            <li class="nav-item">
              <a class="nav-link" href="/">Главная</a>
            </li>
            <li class="nav-item">
              <a class="nav-link" href="/catalog">Каталог</a>
            </li>
            <li class="nav-item">
              <a class="nav-link" href="/cart">Корзина</a>
            </li>
            <li class="nav-item">
              <a class="nav-link" href="/about">О нас</a>
            </li>
            <li class="nav-item">
              <a class="nav-link" href="/contacts">Контакты</a>
            </li>
          </ul>
        </div>
      </div>
    </nav>
    <div class="container">
      {{> alerts}}
      {{> yield}}
    </div>
    <script src="/js/jquery.min.js"></script>
    <script src="/js/bootstrap.min.js"></script>
  </body>
</html>

```

`footer.handlebars`

```html
<footer class="bg-light py-3">
  <div class="container">
    <p>© 2023 Мой магазин</p>
  </div>
</footer>

```

`header.handlebars`

```html
<header class="bg-white py-3">
  <div class="container">
    <div class="row align-items-center">
      <div class="col-md-3">
        <a class="navbar-brand" href="/">Мой магазин</a>
      </div>
      <div class="col-md-6">
        <form class="form-inline my-2 my-lg-0">
          <input class="form-control mr-sm-2" type="search" placeholder="Поиск товаров" aria-label="Поиск">
          <button class="btn btn-outline-primary my-2 my-sm-0" type="submit">Найти</button>
        </form>
      </div>
      <div class="col-md-3">
        <a href="/cart" class="btn btn-outline-secondary mr-3"><i class="fa fa-shopping-cart"></i> Корзина</a>
        <a href="/login" class="btn btn-primary"><i class="fa fa-user"></i> Войти</a>
      </div>
    </div>
  </div>
</header>

```