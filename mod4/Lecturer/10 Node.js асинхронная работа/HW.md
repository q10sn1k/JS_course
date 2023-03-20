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