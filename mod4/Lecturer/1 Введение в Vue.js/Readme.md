#Vue.js - это JavaScript-фреймворк для создания пользовательских интерфейсов.

Vue.js используется для создания одностраничных приложений и может быть использован как в качестве основного фреймворка,
так и для улучшения существующих приложений.

Vue.js предлагает разработчикам ряд преимуществ, включая высокую производительность, простоту использования и гибкость.
Он также имеет обширную документацию и поддерживается большим сообществом разработчиков.

Установка Vue.js

Перед началом работы с Vue.js необходимо установить его на компьютер.

Для установки через npm, необходимо выполнить следующую команду в командной строке:
```
npm install vue
```
ИЛИ для установки через CDN можно использовать следующий тег script в HTML-документе
```
<script src="https://cdn.jsdelivr.net/npm/vue"></script>
```

Создание первого приложения на Vue.js

После установки Vue.js можно создать свое первое приложение.

Для этого необходимо:
1) Создайте директорию проекта и перейти в нее

2) Установите Vue CLI, используя команду
```
   npm install -g @vue/cli
```
в терминале или командной строке.
Эта команда установит Vue CLI глобально на вашем компьютере.

3) Создайте новый проект Vue.js с помощью команды 
```
vue create project-name
```

   где project-name - это имя вашего проекта.
   my-first-vue-app.


4) Выберете настройки по умолчанию
```
    ? Please pick a preset: Default ([Vue 3] babel, eslint)
```
5) После создания проекта вам будет доступен стартовый файл App.vue, который вы можете изменить и настроить в соответствии с требованиями.

6) App.vue
```
<template>
  <div>
    <h1>{{ message }}</h1>
  </div>
</template>
// Тег <template> используется для определения внешнего вида компонента и содержит HTML-разметку.
// В данном случае это простой див с заголовком <h1>
<script>
export default {
  data() {
    return {
      message: 'My First Vue App'
    }
  }
}
</script>
// Тег <script> содержит JavaScript-код, который относится к компоненту и экспортирует его для использования в других местах.
// В этом коде определяется состояние компонента с помощью объекта data, в котором мы определяем свойство message

// export default экспортирует данный компонент по умолчанию, что означает,
// что он может быть использован в других компонентах или в главном приложении.


<style>
h1 {
  color: red;
}
</style>
// Тег <style> используется для добавления стилей к компоненту.
// В данном случае мы применяем к тегу <h1> красный цвет текста с помощью свойства color.

// В итоге, данный компонент будет отображать на странице заголовок "Hello Vue!" красного цвета.
```
7) Запустите проект, используя команду npm run serve,
   которая запустит локальный сервер и откроет приложение в вашем браузере по адресу http://localhost:8080/

_________________________________________

# Структура компонента Vue
Каждый компонент в Vue.js состоит из трех частей: шаблон, скрипт и стили. Шаблон содержит разметку HTML, которая отображается в браузере. Скрипт содержит код JavaScript, который описывает логику компонента. Стили содержат CSS-код, который определяет внешний вид компонента.

# Шаблон компонента
Шаблон компонента определяет, как компонент будет отображаться в браузере. Шаблон состоит из разметки HTML и Vue-директив.

Vue-директивы - это специальные атрибуты, которые используются для связывания данных с элементами в разметке HTML. В шаблоне компонента Vue-директивы обозначаются символом "@" или "v-".

Например, директива "v-if" используется для условного рендеринга элементов в шаблоне. Если условие, заданное в директиве, истинно, то элемент будет отображаться, иначе - скрыт.

# Пример шаблона компонента:

```vue
<template>
  <div>
    <h1>{{ title }}</h1>
    <p v-if="showText">Some text</p>
    <button @click="toggleText">Toggle text</button>
  </div>
</template>

```

В приведенном выше примере шаблона компонента используется директива "v-if",
чтобы скрыть или показать элемент "p", в зависимости от значения переменной "showText". 
Также используется директива "@" для связывания обработчика событий "click" с кнопкой.

#Скрипт компонента
Скрипт компонента содержит код JavaScript, который определяет логику компонента.
В скрипте компонента могут определяться различные свойства, методы, вычисляемые свойства,
жизненные циклы и другие настройки компонента.

#Пример скрипта компонента:

```vue
<script>
export default {
  name: 'MyComponent',
  data() {
    return {
      title: 'My Component',
      showText: false
    }
  },
  methods: {
    toggleText() {
      this.showText = !this.showText
    }
  }
}
</script>

```

В приведенном выше примере скрипта компонента определяется экспортируемый объект,
который содержит различные свойства и методы компонента. 
В объекте указывается имя компонента ("name"), данные компонента ("data") и
методы компонента ("methods").

Свойство "data" - это функция, которая возвращает объект с данными компонента. 
В приведенном выше примере определяются два свойства: "title" и "showText".
Свойство "title" содержит значение "My Component", 
а свойство "showText" изначально устанавливается в значение "false".

Метод "toggleText" - это обработчик событий "click", который переключает значение
свойства "showText" между "true" и "false".

Структура компонента Vue включает также секцию "стили", 
которая определяет внешний вид компонента. 
#Пример использования секции стилей:

```vue
<style>
  .title {
    font-size: 24px;
    font-weight: bold;
  }
</style>

```

В приведенном выше примере определяется класс "title",
который задает размер шрифта 24 пикселя и жирность шрифта "bold".

Вместе шаблон, скрипт и стили формируют компонент Vue. 
#Пример полного компонента:

```vue
<template>
  <div>
    <h1 class="title">{{ title }}</h1>
    <p v-if="showText">Some text</p>
    <button @click="toggleText">Toggle text</button>
  </div>
</template>

<script>
export default {
  name: 'MyComponent',
  data() {
    return {
      title: 'My Component',
      showText: false
    }
  },
  methods: {
    toggleText() {
      this.showText = !this.showText
    }
  }
}
</script>

<style>
  .title {
    font-size: 24px;
    font-weight: bold;
  }
</style>

```
В данном примере компонента определяется заголовок, текст и кнопка. 
Заголовок отображается с помощью элемента "h1" и класса "title", который определяется в секции стилей. 
Текст и кнопка отображаются с помощью директивы "v-if" и метода "toggleText". 
Свойства и методы компонента определяются в секции скрипта.
_________________________________
__________________________________
#Домашнее задание
###Задача 1
Создайте компонент Vue, который будет отображать список элементов. 
Каждый элемент списка должен содержать название и описание. 
При клике на элемент списка должна открываться детальная информация об этом элементе. 
Также должна быть возможность добавления новых элементов в список.
####Решение
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

#Полный код:
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