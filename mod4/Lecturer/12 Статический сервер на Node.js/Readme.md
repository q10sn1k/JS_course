# Статический сервер на Node.js

Разрабатвем статический сервер на NodeJS. Такой сервер может использоваться для отдачи статических файлов, таких как HTML, CSS, JavaScript, изображения и другие файлы.\
Для реализации статического сервера мы будем использовать модуль http и модуль fs для работы с файловой системой.

# Реализуем статический сервер на NodeJS

Начнем с создания простого сервера, который будет отдавать запрошенные файлы.\
Мы будем отдавать HTML файлы.

```js
const http = require('http');
const fs = require('fs');

http.createServer(async (request, response) => {
    if (request.url !== '/favicon.ico') { // Игнорировать запросы на /favicon.ico
        const path = 'root' + request.url; // Преобразуем URL в путь к файлу
        const text = await fs.promises.readFile(path, 'utf8'); // Читаем содержимое файла
        response.writeHead(200, {'Content-Type': 'text/html'}); // Отправляем заголовок с типом контента
        response.write(text); // Отправляем содержимое файла
        response.end(); // Закрываем соединение
    }
}).listen(3000);

```

Мы используем модуль http для создания сервера и прослушивания порта 3000.\
Когда клиент делает запрос на наш сервер, мы проверяем, не является ли запрос запросом на /favicon.ico, и если это так, мы игнорируем этот запрос.\
Если запрос не на /favicon.ico, то мы преобразуем URL в путь к файлу, используя папку root как корневую папку нашего сайта.\
Затем мы читаем содержимое файла с помощью fs.promises.readFile и отправляем его обратно клиенту с помощью методов response.writeHead, response.write и response.end.

# Убираем расширение из URL

Когда мы запрашиваем страницу по URL типа /dir/sub/, мы хотим, чтобы был отдан файл index.html из папки /dir/sub/.\
Для этого нам нужно добавить проверку на то, является ли запрошенный путь директорией. \
Если это так, мы добавляем к пути /index.html. 


```js
const http = require('http');
const fs = require('fs');

http.createServer(async (request, response) => {
    if (request.url !== '/favicon.ico') {
        let path = 'root' + request.url;
        if ((await fs.promises.stat(path)).isDirectory())
            path += '/index.html'; // добавляем /index.html, если это директория
    }    
    let text = await fs.promises.readFile(path, 'utf8');

    response.writeHead(200, {'Content-Type': 'text/html'});
    response.write(text);
    response.end();
}
}).listen(3000);

```