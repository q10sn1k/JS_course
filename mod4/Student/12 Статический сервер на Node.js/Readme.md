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
        let text = await fs.promises.readFile(path, 'utf8');

        response.writeHead(200, {'Content-Type': 'text/html'});
        response.write(text);
        response.end();
    }
}).listen(3000);
```


Здесь мы используем метод stat из модуля fs.promises, чтобы проверить, является ли запрошенный путь директорией.\
Если это так, мы добавляем /index.html к пути, и затем считываем содержимое файла и отправляем его клиенту.

# Выдача ресурсов

Теперь наш сервер умеет отдавать HTML файлы, но что если мы хотим отдавать другие типы файлов, такие как изображения, CSS или JavaScript? \
Для этого нам нужно добавить функцию, которая будет определять MIME тип запрошенного файла по его расширению.\
Мы можем использовать расширение файла, чтобы определить MIME тип, например, расширение .jpg обычно соответствует типу image/jpeg. Вот пример функции getMimeType:

```js
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
```

Эта функция принимает путь к файлу и возвращает соответствующий ему MIME тип. \
Она использует объект mimes для сопоставления расширения файла с MIME типом. \
Если расширение файла не соответствует ни одному из значений в объекте mimes, функция возвращает тип text/plain.

Теперь нам нужно изменить наш сервер так, чтобы он определял MIME тип запрошенного файла и отправлял его в заголовке Content-Type.\
Мы можем использовать функцию getMimeType для этого:


```js
const http = require('http');
const fs = require('fs');

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

http.createServer(async (request, response) => {
    if (request.url !== '/favicon.ico') {
        let path = 'root' + request.url;
        let status;
        let text;
        try {
            if ((await fs.promises.stat(path)).isDirectory()) {
                path += '/index.html';
            }

            text = await fs.promises.readFile(path, 'utf8');
            status = 200;
        } catch (err) {
            text = 'Page not found';
            status = 404;
        }

        let mimeType = getMimeType(path); // Определяем MIME тип
        response.writeHead(status, {'Content-Type': mimeType}); // Отправляем заголовок с типом контента
        response.write(text);
        response.end();
    }
}).listen(3000);
```


Здесь мы изменяем наш сервер так, чтобы он определял MIME тип запрошенного файла, используя функцию getMimeType.\
Затем мы отправляем заголовок Content-Type с соответствующим MIME типом.
# Элементы в шаблоне сайта на NodeJS

Поработаем с шаблоном сайта на NodeJS. \
Шаблон - это файл, который содержит общие части страницы, такие как хедер, футер и сайдбары. \
Каждая страница на сайте может использовать этот файл, чтобы получить общий стиль и структуру.\
Шаблон обычно состоит из HTML, CSS и JavaScript кода.

Мы хотим добавить возможность использовать отдельные файлы для каждой из общих частей страницы.\
Для этого мы будем использовать специальную команду {% get element '' %}, где в кавычках будет имя элемента.\
Мы будем искать этот элемент в отдельных файлах и заменять команду на содержимое файла.

Если мы используем следующий файл шаблона:

```html
<!DOCTYPE html>
<html>
    <head>
        <title>{% get title '' %}</title>
    </head>
    <body>
        <div id="wrapper">
            <header>
                {% get element 'header' ''}
            </header>
            <main>
                {% get content '' %}
            </main>
            <aside>
                {% get element 'sidebar' ''}
            </aside>
            <footer>
                {% get element 'footer' ''}
            </footer>
        </div>
    </body>
</html>
```

То мы можем использовать следующие файлы для каждой из частей:

 * header.html
 * content.html
 * sidebar.html
 * footer.html\

Когда мы обрабатываем этот шаблон, мы заменяем команды {% get element '' %} на содержимое соответствующего файла.

# Реализация

Реализуем эту функциональность в нашем сервере на NodeJS.\
Мы будем использовать модуль fs для чтения содержимого файлов и регулярные выражения для поиска команд {% get element '' %} в файле шаблона.


```js
const http = require('http');
const fs = require('fs');

/**
 * Функция для определения MIME типа по расширению файла
 * @param {string} path - путь к файлу
 * @returns {string} MIME тип
 */
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

http.createServer(async (request, response) => {
    if (request.url !== '/favicon.ico') {
        let path = 'root' + request.url;
        let status;
        let text;
        
        try {
            if ((await fs.promises.stat(path)).isDirectory()) {
                path += '/index.html';
            }
            
            text = await fs.promises.readFile(path, 'utf8');
            status = 200;
        } catch (err) {
            text = 'Page not found';
            status = 404;
        }
        
        let mimeType = getMimeType(path);
        response.writeHead(status, {'Content-Type': mimeType});
        
        // Обработка команд {% get element '' %}
        let reg = /\{% get element '(.+?)' %\}/g;
        text = text.replace(reg, async (match0, match1) => {
            let elemPath = 'elems/' + match1 + '.html';
            let elemText = await fs.promises.readFile(elemPath, 'utf8');
            return elemText;
        });
        
        response.write(text);
        response.end();
    }
}).listen(3000);

```

Здесь мы определяем функцию getMimeType, которая используется для определения MIME типа запрошенного файла. \
Затем мы создаем сервер с помощью метода http.createServer и обрабатываем запросы с помощью функции-обработчика.

Внутри функции-обработчика мы сначала проверяем, является ли запрошенный путь директорией. \
Если это так, мы добавляем /index.html к пути. \
Далее мы считываем содержимое файла и отправляем его клиенту, определяя MIME тип с помощью функции getMimeType.

После мы используем регулярное выражение для поиска команд {% get element '' %} в файле, и заменяем их на содержимое соответствующих файлов, которые находятся в папке elems.

В итоге мы отправляем содержимое файла клиенту с правильным MIME типом и закрываем соединение.

Для работы этого сервера на Node.js, нужно создать следующую структуру файлов и папок:

```
root/
    index.html
    dir/
        index.html
        test.html
elems/
    header.html
    footer.html
index.html:
```

index.html:

```html
<!DOCTYPE html>
<html>
<head>
	<title>Главная страница</title>
	{% css '/css/main.css' %}
</head>
<body>
	{% get element 'header' %}
	<div id="content">
		<h1>Добро пожаловать!</h1>
		<p>Вы находитесь на главной странице сайта.</p>
		<p>Мы рады приветствовать вас на нашем сайте.</p>
	</div>
	{% get element 'footer' %}
	{% js '/js/main.js' %}
</body>
</html>

```

dir/index.html:

```html
<!DOCTYPE html>
<html>
<head>
	<title>Страница с директорией</title>
	{% css '/css/main.css' %}
</head>
<body>
	{% get element 'header' %}
	<div id="content">
		<h1>Добро пожаловать в директорию!</h1>
		<p>Вы находитесь в директории.</p>
		<p>Здесь можно увидеть список файлов:</p>
		<ul>
			<li><a href="test.html">test.html</a></li>
		</ul>
	</div>
	{% get element 'footer' %}
	{% js '/js/main.js' %}
</body>
</html>

```

dir/test.html:

```html
<!DOCTYPE html>
<html>
<head>
	<title>Тестовая страница</title>
	{% css '/css/main.css' %}
</head>
<body>
	{% get element 'header' %}
	<div id="content">
		<h1>Тестовая страница</h1>
		<p>Это тестовая страница.</p>
	</div>
	{% get element 'footer' %}
	{% js '/js/main.js' %}
</body>
</html>

```


elems/header.html:

```html
<header>
	<h1>Название сайта</h1>
	<nav>
		<ul>
			<li><a href="/">Главная</a></li>
			<li><a href="/dir">Директория</a></li>
			<li><a href="/non-existent-page">Страница, которой не существует</a></li>
		</ul>
	</nav>
</header>

```

elems/footer.html:

```html
<footer>
	<p>© 2023 Мой сайт</p>
</footer>

```

css/main.css:

```css
body {
	margin: 0;
	padding: 0;
	font-family: Arial, sans-serif;
	font-size: 16px;
	line-height: 1.5;
}

header {
	background-color: #333;
	color: #fff;
	padding: 10px;
}

nav ul {
	margin: 0;
	padding: 0;
	list-style: none;
	display: flex;
}

nav li {
	margin-right: 10px;
}

nav li:last-child {
	margin-right: 0;
}

nav a {
	color: #fff;
	text-decoration: none;
}

nav a:hover {
	text-decoration: underline;
}

#content {
    padding: 20px;
}

footer {
    background-color: #333;
    color: #fff;
    padding: 10px;
    text-align: center;
    position: absolute;
    bottom: 0;
    left: 0;
    right: 0;
}

```

js/main.js:

```js
console.log('Это JavaScript код, подключенный на всех страницах сайта');
```

Это простейшая структура сайта, которую можно использовать для тестирования сервера на Node.js.\
Она содержит главную страницу, страницу с директорией и тестовую страницу внутри директории, а также header (шапку) и footer (подвал) сайта, подключаемые на всех страницах.\
Также есть файлы со стилями и JavaScript кодом.


________________________________
________________________________
________________________________

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

### Задача 2

На сервере добавьте возможность использования переменных в файлах шаблона, которые будут заменяться на соответствующие значения при обработке шаблона.

### Задача 3

Реализуйте возможность добавления пользовательских элементов в шаблон сайта.\
При этом эти элементы будут загружаться из отдельных файлов с расширением .html, расположенных в папке elems.\
Команда для добавления элемента: {% get element 'имя элемента' %}.

### Задача 4

Реализуйте возможность подключения стилей и скриптов к страницам сайта. \
Для этого добавьте в шаблон сайта два новых тега: {% css 'путь к css файлу' %} и {% js 'путь к js файлу' %}. \
Эти теги должны вставлять соответствующие теги link и script в соответствующие места в head и body страницы.