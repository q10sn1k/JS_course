#Домашнее задание
### Задача 1
Создайте компонент Vue, который будет отображать список элементов.
Каждый элемент списка должен содержать название и описание.
При клике на элемент списка должна открываться детальная информация об этом элементе.
Также должна быть возможность добавления новых элементов в список.
#### Решение
Для решения данной задачи нужно создать компонент Vue с использованием следующих элементов:
шаблон (template), скрипт (script) и стили (style).

Создадим шаблон для нашего компонента.
Он должен содержать элемент "ul" для отображения списка и элемент "div" для отображения детальной информации об элементе списка.
Для элементов списка создадим шаблон "li", в котором будут отображаться название и описание элемента:

```vue
<template>
  <div>
    <ul>
      <li v-for="(item, index) in items" :key="index" @click="showDetails(item)">
        <h2>{{ item.title }}</h2>
        <p>{{ item.description }}</p>
      </li>
    </ul>
    <div v-if="selectedItem">
      <h2>{{ selectedItem.title }}</h2>
      <p>{{ selectedItem.description }}</p>
    </div>
  </div>
</template>

```

В данном шаблоне используется директива "v-for" для отображения элементов списка.
Она перебирает массив "items" и создает элементы "li" для каждого элемента списка.
Также для каждого элемента списка устанавливается обработчик событий "click", который вызывает метод "showDetails" и передает ему выбранный элемент списка.
Если выбранный элемент существует, то отображается детальная информация об этом элементе.



Создадим скрипт для нашего компонента. Он должен определять массив "items" с элементами списка, а также методы "showDetails" и "addItem":
```vue
<script>
export default {
   name: 'ItemList',
   data() {
      return {
         items: [
            { title: 'Item 1', description: 'Description for Item 1' },
            { title: 'Item 2', description: 'Description for Item 2' },
            { title: 'Item 3', description: 'Description for Item 3' },
         ],
         selectedItem: null,
      };
   },
   methods: {
      showDetails(item) {
         this.selectedItem = item;
      },
      addItem() {
         const newItem = {
            title: 'New Item',
            description: 'Description for New Item',
         };
         this.items.push(newItem);
      },
   },
};
</script>

```

Добавим функционал для добавления новых элементов в список.
Для этого создадим кнопку "Добавить элемент" и добавим обработчик событий "click",
который будет вызывать метод "addItem":


```vue
<template>
  <div>
    <button @click="addItem">Добавить элемент</button>
    <ul>
      <li v-for="(item, index) in items" :key="index" @click="showDetails(item)">
        <h2>{{ item.title }}</h2>
        <p>{{ item.description }}</p>
      </li>
    </ul>
    <div v-if="selectedItem">
      <h2>{{ selectedItem.title }}</h2>
      <p>{{ selectedItem.description }}</p>
    </div>
  </div>
</template>

```
Добавим стили для нашего компонента.
Например, мы можем задать стили для элементов списка и для кнопки "Добавить элемент":
```vue
<style>
li {
   cursor: pointer;
   border: 1px solid #ccc;
   padding: 10px;
   margin-bottom: 10px;
}

button {
   background-color: #4caf50;
   color: white;
   border: none;
   padding: 10px;
   margin-bottom: 10px;
   cursor: pointer;
}
</style>

```

# Полный код:
```vue
<template>
  <div>
    <button @click="addItem">Добавить элемент</button>
    <ul>
      <li v-for="(item, index) in items" :key="index" @click="showDetails(item)">
        <h2>{{ item.title }}</h2>
        <p>{{ item.description }}</p>
      </li>
    </ul>
    <div v-if="selectedItem">
      <h2>{{ selectedItem.title }}</h2>
      <p>{{ selectedItem.description }}</p>
    </div>
  </div>
</template>

<script>
export default {
  name: 'ItemList',
  data() {
    return {
      items: [
        { title: 'Item 1', description: 'Description for Item 1' },
        { title: 'Item 2', description: 'Description for Item 2' },
        { title: 'Item 3', description: 'Description for Item 3' },
      ],
      selectedItem: null,
    };
  },
  methods: {
    showDetails(item) {
      this.selectedItem = item;
    },
    addItem() {
      const newItem = {
        title: 'New Item',
        description: 'Description for New Item',
      };
      this.items.push(newItem);
    },
  },
};
</script>

<style>
li {
  cursor: pointer;
  border: 1px solid #ccc;
  padding: 10px;
  margin-bottom: 10px;
}

button {
  background-color: #4caf50;
  color: white;
  border: none;
  padding: 10px;
  margin-bottom: 10px;
  cursor: pointer;
}
</style>

```