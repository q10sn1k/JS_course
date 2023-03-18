# Домашнее задание:

### Задача 1
Напишите компонент Vue, который выводит список элементов массива. \
Добавьте возможность добавлять и удалять элементы из этого массива, а также редактировать их значения.

#### Решение
```vue
<template>
  <div>
    <h2>Список элементов</h2>
    <!-- Выводим список элементов с кнопками "Удалить" и "Редактировать" -->
    <ul>
      <li v-for="(item, index) in items" :key="index">
        <span>{{ item }}</span>
        <button @click="removeItem(index)">Удалить</button>
        <button @click="editItem(index)">Редактировать</button>
      </li>
    </ul>
    <!-- Форма для добавления нового элемента -->
    <form @submit.prevent="addItem">
      <input type="text" v-model="newItem">
      <button type="submit">Добавить</button>
    </form>
    <!-- Форма для редактирования элемента -->
    <div v-if="showEditForm">
      <h2>Редактирование элемента</h2>
      <form @submit.prevent="saveEditItem">
        <input type="text" v-model="editItemValue">
        <button type="submit">Сохранить</button>
      </form>
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      // Изначальный список элементов
      items: ['item 1', 'item 2', 'item 3'],
      // Переменная для добавления нового элемента
      newItem: '',
      // Флаг для показа/скрытия формы редактирования
      showEditForm: false,
      // Индекс элемента, который редактируется
      editIndex: null,
      // Новое значение элемента при редактировании
      editItemValue: '',
    };
  },
  methods: {
    // Добавление нового элемента в список
    addItem() {
      if (this.newItem.trim()) {
        this.items.push(this.newItem.trim());
        this.newItem = '';
      }
    },
    // Удаление элемента из списка
    removeItem(index) {
      this.items.splice(index, 1);
    },
    // Показ формы редактирования элемента
    editItem(index) {
      this.showEditForm = true;
      this.editIndex = index;
      this.editItemValue = this.items[index];
    },
    // Сохранение отредактированного элемента
    saveEditItem() {
      if (this.editItemValue.trim()) {
        this.items.splice(this.editIndex, 1, this.editItemValue.trim());
        this.editIndex = null;
        this.editItemValue = '';
        this.showEditForm = false;
      }
    },
  },
};
</script>
```
### Задача 2

Создайте форму входа на сайт, включающую поля ввода логина и пароля.\
При отправке формы осуществлять проверку данных на стороне клиента и сервера.\
Если данные введены корректно, перенаправлять пользователя на другую страницу.

#### Решение

```vue
<template>
  <div>
    <h2>Вход на сайт</h2>
    <form @submit.prevent="login">
      <div>
        <label for="login">Логин:</label>
        <input type="text" id="login" v-model="username">
      </div>
      <div>
        <label for="password">Пароль:</label>
        <input type="password" id="password" v-model="password">
      </div>
      <div>
        <button type="submit">Войти</button>
      </div>
    </form>
  </div>
</template>

<script>
export default {
  data() {
    return {
      username: '',
      password: '',
    };
  },
  methods: {
    login() {
      // Проверяем, что логин и пароль не пустые
      if (this.username.trim() && this.password.trim()) {
        // Отправляем данные на сервер для проверки
        axios.post('/api/login', { username: this.username, password: this.password })
          .then((response) => {
            // Если данные верны, перенаправляем на другую страницу
            if (response.data.success) {
              this.$router.push('/home');
            } else {
              // Если данные неверны, выводим сообщение об ошибке
              alert('Неверный логин или пароль');
            }
          })
          .catch((error) => {
            // Если произошла ошибка, выводим ее сообщение
            alert(error.message);
          });
      } else {
        // Если логин и пароль пустые, выводим сообщение об ошибке
        alert('Введите логин и пароль');
      }
    },
  },
};
</script>

```