# Домашнее задание:

### Задача 1

Напишите приложение на Node.js, которое будет принимать GET-запросы на URL-адрес /hello и возвращать текст "Hello, World!" в качестве ответа.

#### Решение

```js
const express = require('express');
const app = express();

app.get('/hello', (req, res) => {
  res.send('Hello, World!');
});

app.listen(3000, () => {
  console.log('Server listening on port 3000');
});

```

### Задача 2

Напишите приложение на Node.js, которое будет принимать POST-запросы на URL-адрес /login и проверять, правильно ли пользователь ввел логин и пароль. \
Если логин и пароль верны, то приложение должно возвращать ответ с кодом 200 и текстом "Success", в противном случае – ответ с кодом 401 и текстом "Unauthorized".

#### Решение

```js
const express = require('express');
const bodyParser = require('body-parser');

const app = express();

// Используем middleware для парсинга тела запроса
app.use(bodyParser.json());

// Симулируем базу данных с пользователями
const users = [
  { username: 'user1', password: 'password1' },
  { username: 'user2', password: 'password2' },
];

app.post('/login', (req, res) => {
  const { username, password } = req.body;

  // Проверяем, есть ли такой пользователь в базе данных
  const user = users.find(u => u.username === username && u.password === password);

  if (user) {
    res.status(200).send('Success');
  } else {
    res.status(401).send('Unauthorized');
  }
});

app.listen(3000, () => {
  console.log('Server listening on port 3000');
});

```

### Задача 3

Напишите приложение на Node.js, которое будет принимать GET-запросы на URL-адрес /random и возвращать случайное число от 1 до 100 в качестве ответа.

#### Решение

```js
const express = require('express');
const app = express();

app.get('/random', (req, res) => {
  const randomNumber = Math.floor(Math.random() * 100) + 1;
  res.send(`Random number: ${randomNumber}`);
});

app.listen(3000, () => {
  console.log('Server listening on port 3000');
});

```

### Задача 4

Напишите приложение на Node.js, которое будет копировать содержимое одного файла в другой файл.

#### Решение

```js
import fs from 'fs';

// Функция для копирования содержимого одного файла в другой
function copyFile(source, target) {
  try {
    // Читаем содержимое файла
    const content = fs.readFileSync(source, 'utf-8');
    // Записываем содержимое в новый файл
    fs.writeFileSync(target, content);
    console.log('Файл скопирован успешно!');
  } catch (err) {
    console.error('При копировании файла произошла ошибка:', err);
  }
}

// Вызываем функцию для копирования содержимого из файла source.txt в файл target.txt
copyFile('source.txt', 'target.txt');

```

### Задача 5

Напишите приложение на Node.js, которое будет переименовывать файлы в указанной директории.\
Новое имя каждого файла должно быть задано в формате "номер_файла.расширение", где номер файла - порядковый номер файла в директории.

#### Решение

```js
import fs from 'fs';

// Функция для переименования файлов в директории
function renameFilesInDir(dirPath) {
  try {
    // Получаем список файлов в директории
    const files = fs.readdirSync(dirPath);
    // Проходим по всем файлам и переименовываем их
    for (let i = 0; i < files.length; i++) {
      const oldPath = dirPath + '/' + files[i];
      // Формируем новое имя файла
      const newName = `${i+1}_${files[i]}`;
      const newPath = dirPath + '/' + newName;
      // Переименовываем файл
      fs.renameSync(oldPath, newPath);
      console.log(`Файл ${oldPath} переименован в ${newPath}`);
    }
  } catch (err) {
    console.error('При переименовании файлов произошла ошибка:', err);
  }
}

// Вызываем функцию для переименования файлов в директории ./files
renameFilesInDir('./files');

```
