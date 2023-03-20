# HTTP сервер на NodeJS

Для создания HTTP сервера на NodeJS нам нужно импортировать модуль http:

```js
import http from 'http';
```

Затем мы можем создать экземпляр сервера с помощью метода createServer, который принимает колбэк.\
Этот колбэк будет вызван каждый раз, когда кто-то обращается к серверу через браузер:

```js
http.createServer((request, response) => {
	
});
```

В этом колбэке мы можем определить два параметра: объект запроса (request) и объект ответа (response).\
Мы будем использовать объект ответа для формирования ответа, который будет отправлен обратно в браузер:

```js
http.createServer((request, response) => {
	// request - объект с данными запроса пользователя
	// response - объект, с помощью которого мы формируем ответ, отправляемый в браузер
});

```

Чтобы отправить текстовый ответ в браузер, мы можем использовать метод write объекта ответа:

```js
http.createServer((request, response) => {
	response.write('text1');
	response.write('text2');
	response.write('text3');
	response.end();
});

```

Метод end командует завершить ответ и отправить его в браузер.

Чтобы сервер начал прослушивать запросы от браузера, мы должны вызвать метод listen и указать порт, на котором сервер будет ожидать запросы:


```js
http.createServer((request, response) => {
	response.write('text1');
	response.write('text2');
	response.write('text3');
	response.end();
}).listen(3000);

```

Теперь мы можем обратиться к нашему серверу через браузер, набрав http://localhost:3000, где после двоеточия указан заданный нами порт.

Чтобы остановить сервер, нам нужно нажать клавиши Ctrl + C в терминале. \
Это завершит процесс сервера, который можно будет запустить снова.



Мы можем указать код HTTP ответа и отправлять HTTP заголовки с помощью объекта ответа. \
К примеру, мы можем указать код ответа 200 (успешный ответ) и отправить заголовок Content-Language:

```js
http.createServer((request, response) => {
	response.setHeader('Content-Language', 'ru');
	response.statusCode = 200;
	response.write('hello world');
	response.end();
});

```

Чтобы отправить HTML код страницы, мы должны использовать соответствующий тип контента и соответствующий тег HTML в тексте ответа:

```js
http.createServer((request, response) => {
	response.setHeader('Content-Type', 'text/html');
	response.statusCode = 200;
	response.write('<b>hello world</b>');
	response.end();
}).listen(3000);

```

Метод writeHead объединяет код ответа и заголовки:

```js
http.createServer((request, response) => {
        response.writeHead(200, {'Content-Type': 'text/html'});
        response.end();
    });
```


Важно понимать, что наш сервер, будучи один раз запущенным, обрабатывает запросы всех пользователей нашего сайта.\
Функция-коллбэк нашего сервера вызывается на каждый запрос, поэтому внешние переменные этой функции будут общими для всех запросов.

Например, мы можем создать счетчик запросов к серверу и отдавать его значение каждому запросу:

```js
let i = 0;

http.createServer((request, response) => {
	response.setHeader('Content-Type', 'text/html');
	response.statusCode = 200;
	response.write(String(++i));
	response.end();
}).listen(3000);
```

# Чтение данных из запроса HTTP сервера на NodeJS
Данные, отправляемые браузером, могут содержать полезную информацию для нашего приложения.\
Например, в POST-запросах могут содержаться данные, которые необходимо сохранить в базе данных.

Для чтения данных из запроса HTTP сервера на NodeJS необходимо использовать событие 'data'. \
В качестве колбека этого события мы передаем функцию, которая будет выполняться каждый раз, когда на сервер приходят данные:


```js
http.createServer((request, response) => {
    if (request.url != '/favicon.ico') {
        let body = '';
        request.on('data', (chunk) => {
            body += chunk.toString();
        });

        request.on('end', () => {
            console.log(body);
            response.writeHead(200, {'Content-Type': 'text/html'});
            response.end();
        });
    }
}).listen(3000);
```

Мы сначала создаем пустую строку, которая будет содержать данные, отправленные браузером.\
Затем мы навешиваем на объект запроса обработчик события 'data'.\
Этот обработчик вызывается каждый раз, когда на сервер приходят данные.

Внутри этого обработчика мы добавляем каждую порцию пришедших данных к нашей строке body.\
После того, как все данные будут прочитаны, будет вызван обработчик события 'end', внутри которого мы можем обработать полученную информацию.

# Отправка файлов с помощью NodeJS

Для отправки файлов с помощью NodeJS необходимо использовать модуль fs. \
Давайте напишем функцию, которая будет отдавать файл из указанной директории по указанному пути:

```js
const fs = require('fs');
const path = require('path');

function sendFile(filePath, response) {
fs.readFile(filePath, (error, content) => {
if (error) {
response.writeHead(500, {'Content-Type': 'text/html'});
response.write('Error loading file');
response.end();
} else {
const ext = path.extname(filePath);
const contentType = getContentType(ext);
    response.writeHead(200, {'Content-Type': contentType});
    response.end(content, 'utf-8');
}
});
}

function getContentType(ext) {
    switch(ext) {
        case '.html':
            return 'text/html';
        case '.css':
            return 'text/css';
        case '.js':
            return 'text/javascript';
        case '.json':
            return 'application/json';
        case '.png':
            return 'image/png';
        case '.jpg':
            return 'image/jpg';
        default:
            return 'application/octet-stream';
    }
}

http.createServer((request, response) => {
    if (request.url != '/favicon.ico') {
        if (request.url == '/') {
            sendFile('./index.html', response);
        } else {
            sendFile('.' + request.url, response);
        }
    }
}).listen(3000);
```

Функция sendFile использует метод readFile модуля fs для чтения файла и отправки его содержимого в ответ на запрос браузера. \
В данном примере мы отправляем файл index.html, который должен находиться в той же папке, что и скрипт сервера:

```js
import http from 'http';
import fs from 'fs';

http.createServer((request, response) => {
    if (request.url != '/favicon.ico') {
        let filePath = '.' + request.url;
        if (filePath == './') {
            filePath = './index.html';
        }
        fs.readFile(filePath, (error, content) => {
            if (error) {
                response.writeHead(404, {'Content-Type': 'text/html'});
                response.end();
            }
            else {
                response.writeHead(200, {'Content-Type': 'text/html'});
                response.write(content);
                response.end();
            }
        });
    }
}).listen(3000);

```

Мы проверяем, если адрес запроса равен '/', то вместо этого мы запрашиваем файл index.html. \
Если при чтении файла возникла ошибка, то мы отдаем статус 404.\
В противном случае мы отправляем содержимое файла с типом содержимого 'text/html'.

Для того, чтобы данный код заработал, необходимо создать файл index.html в той же папке, где находится данный скрипт сервера.\
Файл index.html должен содержать HTML код, который будет отображаться в браузере при запросе к серверу.

Пример файла index.html:

```html
<!DOCTYPE html>
<html>
<head>
	<title>My webpage</title>
</head>
<body>
	<h1>Welcome to my webpage</h1>
	<p>This is a test page.</p>
</body>
</html>

```

После создания файла index.html и запуска сервера по адресу http://localhost:3000 будет отображаться содержимое этого файла.


#Маршрутизация в NodeJS
До сих пор мы обрабатывали запросы к конкретным страницам. Давайте теперь сделаем более универсальный подход, используя маршрутизацию.

Для этого нам нужно определить обработчики для каждого маршрута. \
Мы будем использовать объект для хранения обработчиков.\
Ключами в этом объекте будут являться маршруты, а значениями - функции-обработчики.


```js
import http from 'http';

const handlers = {
    '/': (request, response) => {
        response.writeHead(200, {'Content-Type': 'text/html'});
        response.write('Hello, world!');
        response.end();
    },
    '/page1': (request, response) => {
        response.writeHead(200, {'Content-Type': 'text/html'});
        response.write('Page 1');
        response.end();
    },
    '/page2': (request, response) => {
        response.writeHead(200, {'Content-Type': 'text/html'});
        response.write('Page 2');
        response.end();
    },
    '/page3': (request, response) => {
        response.writeHead(200, {'Content-Type': 'text/html'});
        response.write('Page 3');
        response.end();
    },
    'default': (request, response) => {
        response.writeHead(404, {'Content-Type': 'text/html'});
        response.write('Page not found');
        response.end();
    }
};

http.createServer((request, response) => {
    if (request.url != '/favicon.ico') {
        const handler = handlers[request.url] || handlers['default'];
        handler(request, response);
    }
}).listen(3000);

```

Обратите внимание на объект handlers, в котором мы храним обработчики для каждого маршрута. \
Если адрес запроса соответствует одному из ключей объекта handlers, мы вызываем соответствующий обработчик.\
Если же адрес запроса не соответствует ни одному из ключей, мы отдаем страницу 404 с кодом 404.

Теперь создадим отдельный модуль для нашего роутера.\
В первую очередь, создадим файл index.js, который будет служить точкой входа в приложение:

index.js
```js
const http = require('http');
const router = require('./router');

const server = http.createServer((request, response) => {
router.route(request, response);
});

server.listen(3000);
```

Здесь мы импортируем модуль router.js и создаем HTTP-сервер, который принимает запросы и передает их на обработку в роутер.

Теперь создадим файл router.js, в котором определим объект handlers и реализуем функцию route для обработки запросов:

router.js
```js
// Объявляем объект handlers, который содержит обработчики для каждого пути
const handlers = {
    '/': (request, response) => { // обработчик для пути "/"
        response.writeHead(200, {'Content-Type': 'text/html'}); // отправляем заголовок с типом контента
        response.write('Home page'); // отправляем содержимое страницы
        response.end(); // заканчиваем ответ сервера
    },
    '/about': (request, response) => { // обработчик для пути "/about"
        response.writeHead(200, {'Content-Type': 'text/html'});
        response.write('About page');
        response.end();
    },
    '/contacts': (request, response) => { // обработчик для пути "/contacts"
        response.writeHead(200, {'Content-Type': 'text/html'});
        response.write('Contacts page');
        response.end();
    }
};

// Функция route используется для маршрутизации запросов
function route(request, response) {
    const url = request.url; // получаем запрошенный путь
    const handler = handlers[url]; // ищем обработчик для этого пути в объекте handlers
    if (handler) { // если обработчик найден
        handler(request, response); // вызываем его, передавая запрос и ответ
    } else { // если обработчик не найден
        response.writeHead(404, {'Content-Type': 'text/html'}); // отправляем заголовок со статусом 404 Not Found
        response.write('Page not found'); // отправляем сообщение об ошибке
        response.end(); // заканчиваем ответ сервера
    }
}

// Экспортируем функцию route для использования в других модулях
module.exports = { route };
```

В этом файле мы создаем объект handlers, который содержит обработчики для каждого маршрута, и определяем функцию route, которая проверяет, соответствует ли адрес запроса одному из маршрутов в объекте handlers. \
Если соответствие найдено, вызывается соответствующий обработчик, а если нет - отдается страница 404.

Теперь запустим наше приложение с помощью команды node index.js и проверим, что все работает. \
Если все в порядке, то при переходе по адресу http://localhost:3000/ откроется домашняя страница, при переходе по адресу http://localhost:3000/about - страница "О нас", а при переходе по адресу http://localhost:3000/contacts - страница "Контакты". \
Если же адрес запроса не соответствует ни одному из маршрутов, откроется страница 404.

Мы создали простой роутер на Node.js, который позволяет нам маршрутизировать запросы и отдавать разное содержимое в зависимости от запрошенного адреса


При работе с файлами ресурсов в NodeJS следует учитывать, что браузеры автоматически запрашивают ресурсы, такие как favicon, которые могут отсутствовать на сервере. \
Это может привести к ошибкам, поэтому мы должны быть готовы обрабатывать такие запросы.

Для решения этой проблемы мы можем использовать библиотеку serve-favicon.\
Она позволяет автоматически обрабатывать запросы на favicon, и возвращать пустой ответ для таких запросов.

Для начала установим библиотеку:

```
npm install serve-favicon
```

Импортируем библиотеку и добавим middleware в наш сервер:

```js
const serveFavicon = require('serve-favicon');

const faviconPath = path.join(__dirname, 'public', 'favicon.ico');

http.createServer(async (request, response) => {
if (request.url != '/favicon.ico') {
    let data;
    let type;
    if (request.url === '/page.html') {
        data = await fs.promises.readFile('page.html', 'utf8');
        type = 'text/html';
    }

    if (request.url === '/image.png') {
        data = await fs.promises.readFile('image.png');
        type = 'image/png';
    }

    response.writeHead(200, {'Content-Type': type});
    response.write(data);
    response.end();
}
}).use(serveFavicon(faviconPath))
    .listen(3000);
```

Здесь мы добавляем middleware serve-favicon, указывая путь к файлу favicon.ico в папке public нашего проекта.

Теперь наш сервер будет корректно обрабатывать запросы на favicon и возвращать пустой ответ для таких запросов.

____________________________
____________________________
____________________________

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
