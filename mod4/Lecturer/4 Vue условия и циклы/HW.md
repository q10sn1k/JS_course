# Домашнее задание:

### Задача 1
Необходимо создать компонент Vue, который отображает список пользователей в виде списка элементов li.\
Каждый элемент списка должен содержать имя пользователя, его возраст и информацию о том, является ли он администратором.\
Для отображения списка пользователей используйте директиву v-for. \
Для уникальной идентификации элементов списка используйте атрибут key.

### Решение:

```vue
<template>
  <div>
    <h2>Список пользователей:</h2>
    <ul>
      <li v-for="user in users" :key="user.id">
        <h3>{{ user.name }}</h3>
        <p>Возраст: {{ user.age }}</p>
        <p v-if="user.isAdmin">Администратор</p>
        <p v-else>Не является администратором</p>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  data() {
    return {
      users: [
        { id: 1, name: 'Иван', age: 25, isAdmin: true },
        { id: 2, name: 'Петр', age: 30, isAdmin: false },
        { id: 3, name: 'Мария', age: 20, isAdmin: true },
        { id: 4, name: 'Анна', age: 35, isAdmin: false }
      ]
    }
  }
}
</script>

```

### Задача 2
Создайте компонент TaskList, который будет отображать список задач. \
Для этого компонента вы можете использовать массив объектов задач следующего формата:
```vue
[
  { id: 1, title: "Задача 1", description: "Описание задачи 1" },
  { id: 2, title: "Задача 2", description: "Описание задачи 2" },
  { id: 3, title: "Задача 3", description: "Описание задачи 3" }
]

```
Для каждой задачи необходимо отобразить ее заголовок и описание. Также необходимо добавить возможность удаления задачи из списка при клике на кнопку "Удалить".

#### Решение:

```vue
<template>
  <div>
    <h2>Список задач</h2>
    <div v-for="task in tasks" :key="task.id">
      <h3>{{ task.title }}</h3>
      <p>{{ task.description }}</p>
      <button @click="removeTask(task.id)">Удалить</button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      tasks: [
        { id: 1, title: "Задача 1", description: "Описание задачи 1" },
        { id: 2, title: "Задача 2", description: "Описание задачи 2" },
        { id: 3, title: "Задача 3", description: "Описание задачи 3" }
      ]
    }
  },
  methods: {
    removeTask(id) {
      this.tasks = this.tasks.filter(task => task.id !== id);
    }
  }
}
</script>

```

### Задача 3
Создайте компонент ColorList, который будет отображать список цветов. \
Для этого компонента вы можете использовать массив строк, содержащих названия цветов. \
В компоненте необходимо отобразить каждый цвет в виде круга с соответствующим цветом. Кроме того, необходимо добавить возможность удаления цвета из списка при клике на круг.

#### Решение:

```vue
<template>
  <div>
    <h2>Список цветов</h2>
    <div v-for="color in colors" :key="color" :style="{ backgroundColor: color }">
      <span>{{ color }}</span>
      <button @click="removeColor(color)">Удалить</button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      colors: ["red", "green", "blue"]
    }
  },
  methods: {
    removeColor(color) {
      this.colors = this.colors.filter(c => c !== color);
    }
  }
}
</script>

<style>
div {
  display: flex;
  align-items: center;
  margin-bottom: 10px;
  padding: 5px;
  border-radius: 5px;
}

span {
  margin-right: 10px;
}

button {
  margin-left: auto;
}
</style>

```