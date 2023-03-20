# Домашнее задание:

### Задача 1

Создайте сервер, который будет отдавать файл style.css при обращении к адресу /style.css и index.html при обращении к адресу /.

#### Решение

```js
const http = require('http');
const fs = require('fs');

http.createServer(async (request, response) => {
  let data;
  let type;
  if (request.url === '/style.css') {
    data = await fs.promises.readFile('style.css', 'utf8');
    type = 'text/css';
  } else {
    data = await fs.promises.readFile('index.html', 'utf8');
    type = 'text/html';
  }
  response.writeHead(200, {'Content-Type': type});
  response.write(data);
  response.end();
}).listen(3000);

```

### Задача 2

Создайте сервер, который будет возвращать статус 404 и сообщение "Page not found" при обращении к несуществующему адресу.

#### Решение

```js
const http = require('http');

http.createServer((request, response) => {
    response.writeHead(404, {'Content-Type': 'text/plain'});
    response.write('Page not found');
    response.end();
}).listen(3000);

```

### Задача 3

Создайте сервер, который будет отдавать содержимое файла index.html при обращении к адресу /, и при обращении к адресу /style.css - содержимое файла style.css.\
При обращении к любому другому адресу сервер должен возвращать статус 404 и сообщение "Page not found".

#### Решение

```js
const http = require('http');
const fs = require('fs');

http.createServer(async (request, response) => {
  let data;
  let type;
  if (request.url === '/') {
    data = await fs.promises.readFile('index.html', 'utf8');
    type = 'text/html';
  } else if (request.url === '/style.css') {
    data = await fs.promises.readFile('style.css', 'utf8');
    type = 'text/css';
  } else {
    response.writeHead(404, {'Content-Type': 'text/plain'});
    response.write('Page not found');
    response.end();
    return;
  }
  response.writeHead(200, {'Content-Type': type});
  response.write(data);
  response.end();
}).listen(3000);

```

### Задача 4

Создайте сервер, который будет отдавать страницу с ошибкой 500 при любом запросе.

#### Решение


```js
const http = require('http');

http.createServer((request, response) => {
  response.writeHead(500, {'Content-Type': 'text/plain'});
  response.write('Internal Server Error');
  response.end();
}).listen(3000);

```
