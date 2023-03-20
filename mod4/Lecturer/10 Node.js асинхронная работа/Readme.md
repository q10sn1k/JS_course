# Node.js асинхронная работа

# Асинхронное чтение файла
Для асинхронного чтения файла в Node.js используется метод fs.readFile. \
Этот метод принимает три параметра: путь к файлу, кодировку и функцию коллбэк.\
Функция коллбэк выполняется после чтения файла и принимает два параметра: объект ошибки (если он есть) и данные файла.

```js
fs.readFile('file.txt', 'utf8', function(err, data) {
  if (err) {
    // обработка ошибок
  } else {
    // обработка данных
  }
});

```

Мы передаем функцию коллбэк в качестве параметра для fs.readFile.

# Проверка асинхронности

Можно убедиться в том, что чтение файла происходит асинхронно, выводя что-нибудь в консоль после вызова fs.readFile.

```js
fs.readFile('file.txt', 'utf8', function(err, data) {
  if (err) {
    // обработка ошибок
  } else {
    // обработка данных
  }
});

console.log('!!!');

```

Если вы запустите этот код, то увидите, что строка с восклицательными знаками выводится в консоль раньше, чем данные файла.

# Обработка исключительных ситуаций

Чтение файла может привести к ошибке, например, если файл не найден.\
Чтобы предотвратить падение приложения, мы должны обработать ошибку.\
Для этого мы проверяем наличие объекта ошибки в функции коллбэк.

```js
fs.readFile('file.txt', 'utf8', function(err, data) {
    if (!err) {
        // обработка данных
    } else {
        // обработка ошибок
    }
});

```

# Асинхронная запись файла

Для асинхронной записи текста в файл используется метод fs.writeFile. \
Этот метод принимает три параметра: путь к файлу, данные для записи и функцию коллбэк. \
Функция коллбэк вызывается после завершения записи и принимает объект ошибки (если он есть).

```js
fs.writeFile('file.txt', 'Hello world!', function(err) {
  if (err) {
    // обработка ошибок
  } else {
    // успешная запись
  }
});

```

# Асинхронная работа с fs через then в NodeJS

Кроме использования функций коллбэк для работы с fs, в Node.js также можно использовать промисы для асинхронной работы с файловой системой.\
Для этого модуль fs предоставляет свойство promises, которое содержит промисные аналоги методов для работы с файлами.

Например, для чтения файла можно использовать метод fs.promises.readFile, который возвращает промис.\
При успешном выполнении промиса данные файла будут переданы в функцию then, а при ошибке - в функцию catch.

```js
fs.promises.readFile('file.txt', 'utf8')
  .then(data => {
    // обработка данных
  })
  .catch(err => {
    // обработка ошибок
  });

```


# Чтение и запись

Можно прочитать файл, выполнить какие-то действия с его содержимым, а затем записать данные обратно в файл. \
Для этого можно использовать методы fs.promises.readFile и fs.promises.writeFile.

```js
fs.promises.readFile('file.txt', 'utf8')
  .then(data => {
    // обработка данных
    data = data + 'Hello world!';
    return fs.promises.writeFile('file.txt', data);
  })
  .then(() => {
    // успешная запись
  })
  .catch(err => {
    // обработка ошибок
  });

```

Мы сначала читаем файл с помощью fs.promises.readFile, затем обрабатываем данные и записываем их в файл с помощью fs.promises.writeFile.\
При успешной записи вызывается функция then, а при ошибке - catch.

# Массовая работа

Предположим, у нас есть несколько файлов, и мы хотим прочитать их, объединить содержимое в одну строку и записать ее в новый файл. \
Вместо чтения файлов по очереди можно использовать Promise.all, чтобы собрать промисы для чтения файлов в массив и выполнять запись только после их завершения.

```js
let names = ['file1.txt', 'file2.txt', 'file3.txt'];
let promises = [];

for (let name of names) {
  promises.push(fs.promises.readFile(name, 'utf8'));
}

Promise.all(promises)
  .then(data => {
    let result = data.join('');
    return fs.promises.writeFile('result.txt', result);
  })
  .then(() => {
    // успешная запись
  })
  .catch(err => {
    // обработка ошибок
  });

```

Создаем массив промисов, которые выполняют чтение файлов, с помощью цикла for и метода fs.promises.readFile. Затем мы передаем этот массив в Promise.all, который выполняет промисы параллельно.\
После завершения всех промисов результаты объединяются в одну строку и записываются в новый файл.


## Реализуем рассмотренные выше методы для асинхронной работы с модулем fs в Node.js:

```js
const fs = require('fs');

// Асинхронное чтение файла через коллбэк
fs.readFile('file.txt', 'utf8', function(err, data) {
  if (!err) {
    console.log('Данные файла:', data);
  } else {
    console.log('Ошибка чтения файла:', err);
  }
});

// Проверка асинхронности чтения файла
fs.readFile('file.txt', 'utf8', function(err, data) {
  if (!err) {
    console.log('Данные файла:', data);
  } else {
    console.log('Ошибка чтения файла:', err);
  }
});
console.log('!!!');

// Обработка исключительных ситуаций при чтении файла
fs.readFile('file.txt', 'utf8', function(err, data) {
  if (!err) {
    console.log('Данные файла:', data);
  } else {
    console.log('Ошибка чтения файла:', err);
  }
});

// Асинхронная запись файла
fs.writeFile('file.txt', 'Hello world!', function(err) {
  if (!err) {
    console.log('Данные успешно записаны в файл');
  } else {
    console.log('Ошибка записи в файл:', err);
  }
});

// Асинхронное чтение файла через промисы
fs.promises.readFile('file.txt', 'utf8')
  .then(data => {
    console.log('Данные файла:', data);
  })
  .catch(err => {
    console.log('Ошибка чтения файла:', err);
  });

// Обработка исключительных ситуаций при чтении файла через промисы
fs.promises.readFile('file.txt', 'utf8')
  .then(data => {
    console.log('Данные файла:', data);
  })
  .catch(err => {
    console.log('Ошибка чтения файла:', err);
  });

// Чтение и запись файла через промисы
fs.promises.readFile('file.txt', 'utf8')
  .then(data => {
    data = data + 'Hello world!';
    return fs.promises.writeFile('file.txt', data);
  })
  .then(() => {
    console.log('Данные успешно записаны в файл');
  })
  .catch(err => {
    console.log('Ошибка записи в файл:', err);
  });

// Массовая работа с файлами через промисы
let names = ['file1.txt', 'file2.txt', 'file3.txt'];
let promises = [];

for (let name of names) {
  promises.push(fs.promises.readFile(name, 'utf8'));
}

Promise.all(promises)
  .then(data => {
    let result = data.join('');
    return fs.promises.writeFile('result.txt', result);
  })
  .then(() => {
    console.log('Данные успешно записаны в файл');
  })
  .catch(err => {
    console.log('Ошибка записи в файл:', err);
  });

```

# Асинхронная работа с fs через async-await в NodeJS

В Node.js асинхронное чтение и запись файлов можно осуществлять не только через коллбэки и промисы, но и через async-await. \
Это делает код более читабельным и понятным, а также сокращает количество оберток и проверок ошибок.

Прочитаем текст файла и выведем его в консоль:



```js
// объявляем асинхронную функцию
async function func() {
  try {
    // используем асинхронный метод readFile из модуля fs.promises для чтения файла
    // передаем первым параметром имя файла или путь к файлу, вторым параметром кодировку
    let data = await fs.promises.readFile('readme.txt', 'utf8');
    console.log(data); // выводим данные файла в консоль
  } catch (err) {
    console.log('что-то пошло не так'); // выводим сообщение об ошибке в консоль
  }
}

func();
```

Добавим обработку ошибок:

```js
async function func() {
    try {
        let data = await fs.promises.readFile('readme.txt', 'utf8');
        console.log(data);
    } catch (err) {
        console.log('что-то пошло не так');
    }
}

func();
```

Прочитаем три файла, сольем их текст и выведем в консоль:

```js
async function func() {
try {
let data1 = await fs.promises.readFile('1.txt', 'utf8');
let data2 = await fs.promises.readFile('2.txt', 'utf8');
let data3 = await fs.promises.readFile('3.txt', 'utf8');
console.log(data1 + data2 + data3);
} catch (err) {
console.log('что-то пошло не так');
}
}

func();
```

Запишем текст трех файлов в новый файл:

```js
async function func() {
try {
let data1 = await fs.promises.readFile('1.txt', 'utf8');
let data2 = await fs.promises.readFile('2.txt', 'utf8');
let data3 = await fs.promises.readFile('3.txt', 'utf8');
await fs.promises.writeFile('res.txt', data1 + data2 + data3);
} catch (err) {
console.log('что-то пошло не так');
}
}

func();

```

Пусть имена наших файлов записаны в массиве.\
Давайте прочитаем данные наших файлов в цикле, а затем запишем их в новый файл:

```js
async function func() {
try {
let names = ['1.txt', '2.txt', '3.txt'];
let data = [];
for (let name of names) {
  data.push(await fs.promises.readFile(name, 'utf8'));
}

await fs.promises.writeFile('res.txt', data.join(''));
} catch (err) {
console.log('что-то пошло не так');
}
}

func();
```

# Относительные пути в NodeJS

Для указания пути к файлу или папке в NodeJS можно использовать как абсолютные, так и относительные пути.\
Абсолютный путь начинается с корневой папки, например:


/home/user/myapp/index.js (на Linux)
C:\Users\user\myapp\index.js (на Windows)

Относительный путь указывает на путь относительно текущей рабочей папки, в которой находится запущенный скрипт.

# Имя папки со скриптом в NodeJS

-------------------------------------------
-------------------------------------------
CommonJS - это стандарт модулей для JavaScript, который определяет способ организации модулей и зависимостей в приложении.\
Этот стандарт используется во многих средах, включая Node.js. \
В стиле CommonJS, модули определяются через функции require() и module.exports, где require() используется для импорта модулей, а module.exports - для экспорта функций, классов и объектов из модуля.

```js
// Модуль math.js
function add(a, b) {
  return a + b;
}

function subtract(a, b) {
  return a - b;
}

module.exports = { add, subtract };

// Импорт модуля math.js
const math = require('./math.js');
console.log(math.add(2, 3)); // Output: 5
console.log(math.subtract(5, 2)); // Output: 3

```
-------------------------------------------
-------------------------------------------

Если ваш NodeJS работает в стиле CommonJS, то в файлах с вашими скриптами будет доступна константа __dirname:

console.log(__dirname);

В ES модулях, однако, эта константа была убрана. Впрочем, ее несложно получить самому.\
Сделаем для этого файл __dirname.js, экспортирующий нужный нам путь к папке со скриптом:

```js
import { dirname } from 'path';
import { fileURLToPath } from 'url';

const __dirname = dirname(fileURLToPath(import.meta.url));
export default __dirname;

```

Теперь в нужном исполняемом файле мы можем импортировать созданный нами модуль и получить нужную нам константу:

```js
import __dirname from './__dirname.js';
console.log(__dirname);

```


# Проверка существования файла в NodeJS

Перед началом работы с файлами может понадобиться проверить, существует ли файл по указанному пути.\
Для этого в NodeJS есть метод fs.promises.access, который проверяет доступность файла по указанному пути. \
Например:

```js
import fs from 'fs/promises';

fs.promises.access('file.txt')
.then(() => console.log('файл существует'))
.catch(() => console.log('файл не существует'));
```


Метод access выполняет проверку на чтение файла.\
Если файл не существует, метод вернет ошибку, которую мы перехватываем с помощью метода catch.

# Потоки чтения и записи в NodeJS

Представим, что у вас есть достаточно большой файл, скажем размером в 100 мегабайт. \
Пусть мы хотим что-то сделать с данными этого файла. \
Очевидно, что для этого нужно прочитать содержимое этого файла в переменную:

```js

let data = await fs.promises.readFile('readme.txt', 'utf8');
console.log(data);

```

Однако, этот подход может не сработать, если файл очень большой и не помещается в оперативной памяти. \
В этом случае нам помогут потоки чтения и записи.

Поток чтения представляет собой источник данных, который поступает порциями.\
Поток записи, в свою очередь, принимает эти порции и сохраняет их в выходной поток данных.\
Оба типа потоков являются реализациями интерфейса stream.Readable и stream.Writable соответственно.

Для чтения данных из файла с помощью потока чтения можно использовать метод fs.createReadStream.\
Например, для чтения файла в кодировке UTF-8:

```js
const fs = require('fs');

const readableStream = fs.createReadStream('file.txt', 'utf8');

readableStream.on('data', (chunk) => {
console.log(chunk);
});

readableStream.on('end', () => {
console.log('Чтение файла завершено');
});

```


Мы создаем поток чтения из файла file.txt в кодировке UTF-8 и выводим каждую порцию данных, которая поступает в поток, с помощью события 'data'. \
Когда все данные были прочитаны, событие 'end' сообщает о завершении чтения файла.

Для записи данных в файл с помощью потока записи можно использовать метод fs.createWriteStream.\
Например, для записи данных в файл output.txt:

```js

const fs = require('fs');

const writableStream = fs.createWriteStream('output.txt');

writableStream.write('Данные, которые нужно записать');

writableStream.end();

```

Мы создаем поток записи в файл output.txt и записываем в него строку 'Данные, которые нужно записать' с помощью метода write.\
Затем мы вызываем метод end, чтобы завершить запись данных в файл.




Кроме чтения и записи файлов, потоки могут использоваться для обработки данных в памяти. \
Например, можно создать поток чтения из массива данных и поток записи в другой массив данных:

```js
const { Readable, Writable } = require('stream');

const data = ['один', 'два', 'три'];

const readableStream = new Readable({
read() {
if (data.length === 0) {
this.push(null);
} else {
this.push(data.shift());
}
}
});

const writableStream = new Writable({
write(chunk, encoding, callback) {
console.log(chunk.toString());
callback();
}
});

readableStream.pipe(writableStream);
```
Мы создаем поток чтения из массива данных, в котором каждый элемент массива является порцией данных в потоке.\
Затем мы создаем поток записи, который просто выводит каждую порцию данных в консоль. \
Далее мы объединяем поток чтения и записи с помощью метода pipe, который позволяет автоматически направлять данные из одного потока в другой.

# through2

Модуль through2 позволяет создавать потоки преобразования данных, которые могут измененять данные в процессе передачи между потоками.

```js
const through2 = require('through2');

const myTransform = through2(function(chunk, encoding, callback) {
// преобразование данных
const transformedChunk = chunk.toString().toUpperCase();

// передача преобразованных данных в следующий поток
callback(null, transformedChunk);
});

// использование потока преобразования данных в цепочке потоков
fs.createReadStream('input.txt')
.pipe(myTransform)
.pipe(fs.createWriteStream('output.txt'));

```

Мы создали новый поток преобразования данных, который принимает порции данных в формате Buffer и преобразует их в верхний регистр с помощью метода toUpperCase().\
Затем мы передаем преобразованные данные в следующий поток с помощью метода callback().
Далее мы используем созданный поток в цепочке потоков, которая включает чтение данных из файла input.txt, преобразование данных с помощью нашего нового потока и запись преобразованных данных в файл output.txt.


______________________________________________
______________________________________________
______________________________________________

# Домашнее задание:

### Задача 1

Напишите скрипт, который читает содержимое указанного пользователем файла и выводит его в консоль.

#### Решение

```js
const fs = require('fs');

const filePath = process.argv[2];

fs.promises.readFile(filePath, 'utf8')
  .then(data => console.log(data))
  .catch(err => console.error(err));

```

### Задача 2

Напишите скрипт, который создает новый файл и записывает в него текст, указанный пользователем.

#### Решение

```js
const fs = require('fs');
const readline = require('readline');

const rl = readline.createInterface({
  input: process.stdin,
  output: process.stdout,
});

rl.question('Введите имя файла: ', fileName => {
  rl.question('Введите текст: ', text => {
    fs.promises.writeFile(fileName, text)
      .then(() => console.log(`Файл ${fileName} успешно создан и записан`))
      .catch(err => console.error(err))
      .finally(() => rl.close());
  });
});

```

### Задача 3

Напишите скрипт, который читает содержимое всех файлов в указанной директории и выводит их содержимое в консоль.

#### Решение

```js
const fs = require('fs');
const path = require('path');

const dirPath = process.argv[2];

fs.promises.readdir(dirPath)
  .then(files => {
    const promises = files.map(fileName => {
      const filePath = path.join(dirPath, fileName);
      return fs.promises.readFile(filePath, 'utf8');
    });
    return Promise.all(promises);
  })
  .then(data => console.log(data.join('\n')))
  .catch(err => console.error(err));

```

### Задача 4

Напишите скрипт на Node.js, который запрашивает у пользователя имя файла, подстроку, которую необходимо заменить и на что заменить, а затем:
1. Читает содержимое файла в переменную.
2. Заменяет все вхождения указанной подстроки на другую, также указанную пользователем.
3. Записывает получившуюся строку в новый файл с префиксом "_new" в имени файла.
4. Выводит сообщение о том, что операция выполнена успешно или об ошибке.

При реализации скрипта необходимо использовать промисы и обработку ошибок.

#### Решение

```js
const fs = require('fs');
const readline = require('readline');

const rl = readline.createInterface({
  input: process.stdin,
  output: process.stdout,
});

rl.question('Введите имя файла: ', fileName => {
  rl.question('Введите подстроку, которую нужно заменить: ', oldStr => {
    rl.question('Введите подстроку, на которую нужно заменить: ', newStr => {
      fs.promises.readFile(fileName, 'utf8')
        .then(data => {
          const newData = data.split(oldStr).join(newStr);
          const newFileName = `${path.basename(fileName, path.extname(fileName))}_new${path.extname(fileName)}`;
          return fs.promises.writeFile(newFileName, newData);
        })
        .then(() => console.log(`Файл успешно записан в новый файл с префиксом "_new"`))
        .catch(err => console.error(err))
        .finally(() => rl.close());
    });
  });
});

```