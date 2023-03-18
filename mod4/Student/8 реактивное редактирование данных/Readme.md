# Реактивное редактирование данных

Реактивное редактирование данных компонента в Vue позволяет изменять данные в компоненте без необходимости обновлять страницу целиком. \
В Vue это достигается с помощью использования директивы v-model для связывания данных с элементом ввода и метода для обработки изменений.

Создадим компонент Task, который будет представлять задачу со следующими полями: название и описание.\
Для редактирования задачи мы будем использовать модальное окно.

```vue
<template>
  <div>
    <!-- Отображаем название и описание задачи -->
    <h3>{{ task.name }}</h3>
    <p>{{ task.description }}</p>
    <!-- Кнопка для открытия формы редактирования задачи -->
    <button @click="showModal = true">Edit Task</button>

    <!-- Форма редактирования задачи -->
    <div v-if="showModal">
      <div>
        <!-- Инпут для редактирования названия задачи -->
        <input v-model="editedTask.name" />

        <!-- Текстовое поле для редактирования описания задачи -->
        <textarea v-model="editedTask.description"></textarea>

        <!-- Кнопка для сохранения изменений -->
        <button @click="saveTask">Save</button>

        <!-- Кнопка для отмены редактирования -->
        <button @click="showModal = false">Cancel</button>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  props: ['task'],
  data() {
    return {
      showModal: false, // Флаг для отображения/скрытия формы редактирования
      editedTask: { // Объект для хранения изменяемых данных задачи
        name: '',
        description: '',
      },
    };
  },
  methods: {
    saveTask() {
      // Обновляем данные задачи на основе введенных пользователем значений
      this.task.name = this.editedTask.name;
      this.task.description = this.editedTask.description;

      // Скрываем форму редактирования
      this.showModal = false;
    },
  },
};
</script>
```

В компоненте есть свойство task, которое передается из родительского компонента и содержит информацию о задаче.

При нажатии на кнопку Edit Task устанавливается значение флага showModal в true, что приводит к отображению формы редактирования.

В форме редактирования используются два элемента: инпут для редактирования названия задачи и текстовое поле для редактирования описания задачи. \
Значения этих элементов связываются с объектом editedTask через директиву v-model.

При нажатии на кнопку Save обновленные данные задачи записываются в свойства name и description объекта task. После этого форма редактирования скрывается.


# Реактивное удаление компонентов в Vue

Реактивное удаление компонентов в Vue позволяет удалять компоненты из списка без необходимости обновлять страницу целиком.\
В Vue это достигается с помощью использования метода $remove или фильтрации массива.

Для примера, создадим компонент TaskList, который будет представлять список задач.\
Для удаления задачи мы будем использовать кнопку "Delete".

```vue
<template>
  <div>
    <!-- Выводим список задач -->
    <ul>
      <li v-for="(task, index) in tasks" :key="task.id">
        <h3>{{ task.name }}</h3>
        <p>{{ task.description }}</p>
        <button @click="deleteTask(index)">Delete</button>
      </li>
    </ul>
  </div>
</template>
<script>
export default {
  data() {
    return {
      // Задаем массив с задачами
      tasks: [
        { id: 1, name: 'Task 1', description: 'Description 1' },
        { id: 2, name: 'Task 2', description: 'Description 2' },
        { id: 3, name: 'Task 3', description: 'Description 3' },
      ],
    };
  },
  methods: {
    // Определяем метод для удаления задачи из списка
    deleteTask(index) {
      // Используем метод splice для удаления элемента из массива
      this.tasks.splice(index, 1);
    },
  },
};
</script>
```

## Чеклист

src/components/ChecklistComponent.vue

```vue
<template>
  <div>
    <!-- Заголовок компонента -->
    <h1>{{ title }}</h1>
    <!-- Список задач -->
    <ul>
      <!-- Компонент ChecklistItem, отображающий каждую задачу в списке -->
      <checklist-item
          v-for="(item, index) in items"
          :key="index"
          :item="item"
          @toggle="toggleItem(index)"
          @delete="deleteItem(index)"
      ></checklist-item>
    </ul>
    <!-- Компонент NewItemForm, отображающий форму добавления новой задачи -->
    <new-item-form @add="addItem"></new-item-form>
  </div>
</template>

<script>
import ChecklistItem from './ChecklistItem.vue'
import NewItemForm from './NewItemForm.vue'

export default {
  name: 'ChecklistComponent',
  components: {
    ChecklistItem,
    NewItemForm
  },
  data() {
    return {
      // Заголовок компонента
      title: 'My Checklist',
      // Список задач
      items: [
        { text: 'Task 1', done: false },
        { text: 'Task 2', done: false },
        { text: 'Task 3', done: false },
      ]
    }
  },
  methods: {
    // Метод для изменения состояния задачи (отметки выполненной/невыполненной)
    toggleItem(index) {
      const item = this.items[index]
      // Проверяем, что объект существует и имеет свойство 'done'
      if (item && item.done !== undefined) {
        item.done = !item.done
      } else {
        this.items[index].done = false
      }
    },
    // Метод для удаления задачи из списка
    deleteItem(index) {
      this.items.splice(index, 1)
    },
    // Метод для добавления новой задачи в список
    addItem(text) {
      this.items.push({ text, done: false })
    }
  }
}
</script>


```

src/components/ChecklistItem.vue
```vue
<template>
  <!-- Компонент для отображения одной задачи в списке -->
  <li :class="{ 'done': localItem.done }">
    <!-- Чекбокс для отметки выполненной задачи -->
    <input type="checkbox" v-model="localItem.done" @change="toggle" />
    <!-- Текст задачи -->
    <span>{{ localItem.text }}</span>
    <!-- Кнопка для удаления задачи -->
    <button @click="deleteItem">Delete</button>
  </li>
</template>

<script>
export default {
  props: {
    // Объект задачи, передаваемый из компонента ChecklistComponent
    item: {
      type: Object,
      required: true
    }
  },
  data() {
    return {
      // Локальная копия объекта задачи, чтобы изменения не влияли на родительский компонент
      localItem: Object.assign({}, this.item)
    }
  },
  methods: {
    // Метод для изменения состояния задачи (отметки выполненной/невыполненной)
    toggle() {
      // Генерируем событие toggle и передаем его в родительский компонент
      this.$emit('toggle')
    },
    // Метод для удаления задачи из списка
    deleteItem() {
      // Генерируем событие delete и передаем его в родительский компонент
      this.$emit('delete')
    }
  },
  // Наблюдатель за изменением пропса item
  watch: {
    item(newValue) {
      // Обновляем локальную копию объекта задачи, чтобы изменения в родительском компоненте не влияли на текущий компонент
      this.localItem = Object.assign({}, newValue)
    }
  }
}
</script>

```

src/components/NewItemForm.vue

```vue
<template>
  <!-- Форма для добавления новой задачи -->
  <form @submit.prevent="addItem">
    <!-- Поле для ввода текста новой задачи -->
    <input type="text" v-model="text" placeholder="Add a new item..." />
    <!-- Кнопка для добавления новой задачи -->
    <button type="submit">Add</button>
  </form>
</template>

<script>
export default {
  data() {
    return {
      // Текст новой задачи
      text: ''
    }
  },
  methods: {
    // Метод для добавления новой задачи в список
    addItem() {
      // Проверяем, что поле ввода не пустое
      if (this.text.trim()) {
        // Генерируем событие add и передаем введенный текст в родительский компонент
        this.$emit('add', this.text.trim())
        // Очищаем поле ввода
        this.text = ''
      }
    }
  }
}
</script>

```

src/App.vue

```vue
<template>
  <div id="app">
    <checklist></checklist>
  </div>
</template>

<script>
import Checklist from './components/ChecklistComponent.vue'

export default {
  components: {
    Checklist
  }
}
</script>

```

src/main.js
```js
import { createApp } from 'vue'
import App from './App.vue'

createApp(App).mount('#app')

```

public/index.html

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title>Vue Checklist</title>
</head>
<body>
<div id="app"></div>
</body>
</html>

```
________________________
________________________
________________________
# Домашнее задание:

### Задача 1
Напишите компонент Vue, который выводит список элементов массива. \
Добавьте возможность добавлять и удалять элементы из этого массива, а также редактировать их значения.

### Задача 2

Создайте форму входа на сайт, включающую поля ввода логина и пароля.\
При отправке формы осуществлять проверку данных на стороне клиента и сервера.\
Если данные введены корректно, перенаправлять пользователя на другую страницу.
