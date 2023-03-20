# Домашнее задание:

Во всех задачах будем использовать следующий базовый сервер:

```js
const http = require('http'); // Подключаем модуль для создания сервера
const fs = require('fs'); // Подключаем модуль для работы с файлами

http.createServer(async (request, response) => { // Создаем HTTP-сервер
    if (request.url !== '/favicon.ico') { // Игнорируем запросы на /favicon.ico
        let path = 'root' + request.url; // Получаем путь к запрошенному файлу
        let status;
        let text;
        try {
            if ((await fs.promises.stat(path)).isDirectory()) { // Если это директория, то ищем index.html
                path += '/index.html';
            }
            text = await fs.promises.readFile(path, 'utf8'); // Читаем файл
            status = 200;
        } catch (err) {
            text = 'Page not found';
            status = 404;
        }
        let mimeType = getMimeType(path); // Определяем MIME-тип файла
        response.writeHead(status, {'Content-Type': mimeType}); // Отправляем заголовок ответа
        response.write(text); // Отправляем содержимое файла
        response.end(); // Закрываем соединение
    }
}).listen(3000); // Слушаем порт 3000
```

### Задача 1

Добавьте к нашему серверу обработку запросов на статические ресурсы, такие как изображения, стили и скрипты.\
Возможные расширения файлов: .jpg, .jpeg, .png, .svg, .json, .js, .css, .ico.

#### Решение

```js
const http = require('http'); // Подключаем модуль для создания сервера
const fs = require('fs'); // Подключаем модуль для работы с файлами

/**
 * Функция для определения MIME типа по расширению файла
 * @param {string} path - путь к файлу
 * @returns {string} MIME тип
 */
function getMimeType(path) {
    let mimes = { // Объект, содержащий соответствие расширений и MIME типов
        html: 'text/html',
        jpeg: 'image/jpeg',
        jpg:  'image/jpeg',
        png:  'image/png',
        svg:  'image/svg+xml',
        json: 'application/json',
        js:   'text/javascript',
        css:  'text/css',
        ico:  'image/x-icon',
    };

    let exts = Object.keys(mimes); // Массив расширений файлов
    let extReg = new RegExp('\\.(' + exts.join('|') + ')$'); // Регулярное выражение для поиска расширения в пути к файлу

    let ext = path.match(extReg)[1]; // Получаем расширение файла из пути

    if (ext) {
        return mimes[ext]; // Возвращаем соответствующий MIME тип
    } else {
        return 'text/plain'; // Возвращаем MIME тип по умолчанию
    }
}

http.createServer(async (request, response) => { // Создаем HTTP-сервер
    if (request.url !== '/favicon.ico') { // Игнорируем запросы на /favicon.ico
        let path = 'root' + request.url; // Преобразуем URL в путь к файлу
        let status;
        let text;

        try {
            if ((await fs.promises.stat(path)).isDirectory()) { // Если запрошен директорий, то добавляем /index.html
                path += '/index.html';
            }

            text = await fs.promises.readFile(path); // Читаем содержимое файла
            status = 200; // Устанавливаем статус 200 ОК
        } catch (err) {
            text = 'Page not found'; // Выводим сообщение об ошибке
            status = 404; // Устанавливаем статус 404 Not Found
        }

        let mimeType = getMimeType(path); // Получаем MIME тип по расширению файла
        response.writeHead(status, {'Content-Type': mimeType}); // Устанавливаем заголовок с соответствующим MIME типом

        response.write(text); // Отправляем содержимое файла
        response.end(); // Закрываем соединение
    }
}).listen(3000); // Слушаем порт 3000



```

### Задача 2

На сервере добавьте возможность использования переменных в файлах шаблона, которые будут заменяться на соответствующие значения при обработке шаблона.

#### Решение

```html
<!DOCTYPE html>
<html>
    <head>
        <title>{% get title '' %}</title>
    </head>
    <body>
        <h1>Hello, {{ name }}!</h1>
    </body>
</html>
```

```js
const http = require('http');
const fs = require('fs');

// Функция для определения MIME типа по расширению файла
function getMimeType(path) {
    let mimes = {
        html: 'text/html',
        jpeg: 'image/jpeg',
        jpg:  'image/jpeg',
        png:  'image/png',
        svg:  'image/svg+xml',
        json: 'application/json',
        js:   'text/javascript',
        css:  'text/css',
        ico:  'image/x-icon',
    };

    // Регулярное выражение для поиска расширения файла
    let exts = Object.keys(mimes);
    let extReg = new RegExp('\\.(' + exts.join('|') + ')$');

    // Получение расширения файла из пути
    let ext = path.match(extReg)[1];

    // Возвращаем соответствующий MIME тип
    if (ext) {
        return mimes[ext];
    } else {
        return 'text/plain';
    }
}

http.createServer(async (request, response) => { // Создаем HTTP-сервер
    if (request.url !== '/favicon.ico') { // Игнорировать запросы на /favicon.ico
        let path = 'root' + request.url; // Преобразуем URL в путь к файлу
        let status;
        let text;
        let variables = {name: 'World'}; // Создаем объект переменных для шаблонизации

        try {
            if ((await fs.promises.stat(path)).isDirectory()) {
                path += '/index.html'; // Добавляем /index.html, если это директория
            }

            text = await fs.promises.readFile(path, 'utf8'); // Читаем содержимое файла
            status = 200; // Статус успешного ответа

            const reg = /\{\{\s*([\w.-]+)\s*\}\}/g; // Регулярное выражение для поиска переменных в шаблоне
            const match = text.match(reg); // Ищем все переменные в шаблоне

            if (match) { // Если нашли переменные
                for (let i = 0; i < match.length; i++) { // Проходимся по ним
                    const key = match[i].replace(/\{\{\s*|\s*\}\}/g, ''); // Получаем имя переменной
                    const value = variables[key] || ''; // Получаем значение переменной из объекта переменных или пустую строку, если значение не задано
                    text = text.replace(match[i], value); // Заменяем переменную в шаблоне на соответствующее значение
                }
            }
        } catch (err) {
            text = 'Page not found';
            status = 404;
        }

        let mimeType = getMimeType(path); // Определяем MIME тип
        response.writeHead(status, {'Content-Type': mimeType}); // Отправляем заголовок с типом контента

        response.write(text); // Отправляем содержимое файла
        response.end(); // Закрываем соединение
    }
}).listen(3000); // Слушаем порт 3000

```

### Задача 3

Реализуйте возможность добавления пользовательских элементов в шаблон сайта.\
При этом эти элементы будут загружаться из отдельных файлов с расширением .html, расположенных в папке elems.\
Команда для добавления элемента: {% get element 'имя элемента' %}.

#### Решение

```html
<!DOCTYPE html>
<html>
    <head>
        <title>{% get title '' %}</title>
    </head>
    <body>
        <div id="header">{% get element 'header' %}</div>
        <div id="content">{% get content '' %}</div>
        <div id="footer">{% get element 'footer' %}</div>
    </body>
</html>

```

```js
const http = require('http');
const fs = require('fs');
const path = require('path');

function getMimeType(path) {
    let mimes = {
        html: 'text/html',
        jpeg: 'image/jpeg',
        jpg:  'image/jpeg',
        png:  'image/png',
        svg:  'image/svg+xml',
        json: 'application/json',
        js:   'text/javascript',
        css:  'text/css',
        ico:  'image/x-icon',
    };

    let exts = Object.keys(mimes);
    let extReg = new RegExp('\\.(' + exts.join('|') + ')$');

    let ext = path.match(extReg)[1];

    if (ext) {
        return mimes[ext];
    } else {
        return 'text/plain';
    }
}

function loadTemplate(name, variables) {
    let lpath = path.join(__dirname, 'templates', name);
    let layout = fs.readFileSync(lpath, 'utf8');

    let reg = /\{\{\s*([\w.-]+)\s*\}\}/g;
    layout = layout.replace(reg, (match0, match1) => {
        let value = variables[match1] || '';
        return value;
    });

    return layout;
}

http.createServer(async (request, response) => {
    if (request.url !== '/favicon.ico') {
        let path = 'root' + request.url;
        let status;
        let text;
        let variables = {name: 'World'};

        try {
            if ((await fs.promises.stat(path)).isDirectory()) {
                path += '/index.html';
            }

            text = await fs.promises.readFile(path, 'utf8');
            status = 200;

            const reg = /\{\{\s*([\w.-]+)\s*\}\}/g;
            const match = text.match(reg);

            if (match) {
                for (let i = 0; i < match.length; i++) {
                    const key = match[i].replace(/\{\{\s*|\s*\}\}/g, '');
                    const value = variables[key] || '';
                    text = text.replace(match[i], value);
                }
            }

            let mimeType = getMimeType(path);
            response.writeHead(status, {'Content-Type': mimeType});
            response.write(text);
            response.end();
        } catch (err) {
            status = 404;
            let layout = loadTemplate('404.html', {name: 'World'});
            let mimeType = getMimeType('404.html');
            response.writeHead(status, {'Content-Type': mimeType});
            response.write(layout);
            response.end();
        }
    }
}).listen(3000);

```

### Задача 4

Реализуйте возможность подключения стилей и скриптов к страницам сайта. \
Для этого добавьте в шаблон сайта два новых тега: {% css 'путь к css файлу' %} и {% js 'путь к js файлу' %}. \
Эти теги должны вставлять соответствующие теги link и script в соответствующие места в head и body страницы.

#### Решение

```html
<!DOCTYPE html>
<html>
    <head>
        <title>{% get title '' %}</title>
        {% css '/css/style.css' %}
    </head>
    <body>
        <div id="header">{% get element 'header' %}</div>
        <div id="content">{% get content '' %}</div>
        {% js '/js/main.js' %}
        <div id="footer">{% get element 'footer' %}</div>
    </body>
</html>

```

```js
const http = require('http');
const fs = require('fs');
const path = require('path');

function getMimeType(path) {
    let mimes = {
        html: 'text/html',
        jpeg: 'image/jpeg',
        jpg:  'image/jpeg',
        png:  'image/png',
        svg:  'image/svg+xml',
        json: 'application/json',
        js:   'text/javascript',
        css:  'text/css',
        ico:  'image/x-icon',
    };

    let exts = Object.keys(mimes);
    let extReg = new RegExp('\\.(' + exts.join('|') + ')$');

    let ext = path.match(extReg)[1];

    if (ext) {
        return mimes[ext];
    } else {
        return 'text/plain';
    }
}

function loadTemplate(name, variables) {
    let lpath = path.join(__dirname, 'templates', name);
    let layout = fs.readFileSync(lpath, 'utf8');

    let reg = /\{\{\s*([\w.-]+)\s*\}\}/g;
    layout = layout.replace(reg, (match0, match1) => {
        let value = variables[match1] || '';
        return value;
    });

    return layout;
}

function parseTags(text, variables) {
    let cssReg = /\{%\s*css\s*'([^']+)'\s*%\}/g;
    let jsReg = /\{%\s*js\s*'([^']+)'\s*%\}/g;

    text = text.replace(cssReg, (match0, match1) => {
        return '<link rel="stylesheet" href="' + match1 + '">';
    });

    text = text.replace(jsReg, (match0, match1) => {
        return '<script src="' + match1 + '"></script>';
    });

    let reg = /\{\{\s*([\w.-]+)\s*\}\}/g;
    let match = text.match(reg);

    if (match) {
        for (let i = 0; i < match.length; i++) {
            const key = match[i].replace(/\{\{\s*|\s*\}\}/g, '');
            const value = variables[key] || '';
            text = text.replace(match[i], value);
        }
    }

    return text;
}

http.createServer(async (request, response) => {
    if (request.url !== '/favicon.ico') {
        let path = 'root' + request.url;
        let status;
        let text;
        let variables = {name: 'World'};

        try {
            if ((await fs.promises.stat(path)).isDirectory()) {
                path += '/index.html';
            }

            text = await fs.promises.readFile(path, 'utf8');
            status = 200;

            text = parseTags(text, variables);

            let mimeType = getMimeType(path);
            response.writeHead(status, {'Content-Type': mimeType});
            response.write(text);
            response.end();
        } catch (err) {
            status = 404;
            let layout = loadTemplate('404.html', {name: 'World'});
            let mimeType = getMimeType('404.html');
            response.writeHead(status, {'Content-Type': mimeType});
            response.write(layout);
            response.end();
        }
    }
}).listen(3000);

```