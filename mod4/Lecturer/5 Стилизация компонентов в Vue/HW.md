# Домашнее задание:

### Задача 1
Создайте компонент кнопки и добавьте ему стили.\
При наведении на кнопку цвет фона должен изменяться, при нажатии на кнопку должен происходить переход на другую страницу.

### Решение:

```vue
<template>
  <router-link to="/other-page">
    <button class="my-button" @mouseover="changeColor">
      {{ buttonText }}
    </button>
  </router-link>
</template>
<script>
export default {
  name: 'MyButton',
  data() {
    return {
      buttonText: 'Click me',
    }
  },
  methods: {
    changeColor() {
      // меняем цвет фона при наведении на кнопку
      const button = document.querySelector('.my-button')
      button.style.backgroundColor = 'red'
    }
  }
}
</script>
<style scoped>
.my-button {
  background-color: blue;
  color: white;
  border: none;
  padding: 10px;
  border-radius: 5px;
  cursor: pointer;
}
</style>
```

### Задача 2
Создайте компонент поля ввода и добавьте ему стили.\
При вводе текста в поле ввода должны изменяться его размеры и цвет рамки.\
При неправильном вводе должен появляться текст с ошибкой.

### Решение:
```vue
<template>
  <div>
    <input class="my-input" :style="inputStyle" v-model="text" @input="checkInput" />
    <div v-if="showError" class="error-text">Ошибка: текст должен содержать только буквы латинского алфавита</div>
  </div>
</template>
<script>
export default {
  name: 'MyInput',
  data() {
    return {
      text: '',
      showError: false,
    }
  },
  computed: {
    inputStyle() {
      // меняем размер и цвет рамки при вводе текста
      return {
        width: this.text.length * 10 + 'px',
        borderColor: this.showError ? 'red' : 'black',
      }
    },
  },
  methods: {
    checkInput() {
      // проверяем, что введенный текст содержит только буквы латинского алфавита
      const pattern = /^[a-zA-Z]+$/
      if (!pattern.test(this.text)) {
        this.showError = true
      } else {
        this.showError = false
      }
    },
  },
}
</script>
<style scoped>
.my-input {
  font-size: 16px;
  padding: 5px;
  border: 1px solid black;
  border-radius: 5px;
  margin-bottom: 10px;
}
.error-text {
  color: red;
}
</style>
```

### Задача 3
Создайте компонент списка и добавьте ему стили. \
Каждый элемент списка должен иметь уникальный цвет фона. \
При наведении на элемент списка должен изменяться его цвет фона.

### Решение:

```vue
<template>
  <ul>
    <li v-for="(item, index) in list" :key="index" @mouseover="changeColor(index)">
      {{ item }}
    </li>
  </ul>
</template>
<script>
export default {
  name: 'MyList',
  data() {
    return {
      list: ['Item 1', 'Item 2', 'Item 3'],
    }
  },
  methods: {
    changeColor(index) {
      // меняем цвет фона элемента при наведении на него
      const li = document.querySelectorAll('li')[index]
      li.style.backgroundColor = `rgb(${Math.random() * 255}, ${Math.random() * 255}, ${Math.random() * 255})`
    }
  }
}
</script>
<style scoped>
li {
  padding: 10px;
  margin-bottom: 5px;
  cursor: pointer;
}
</style>
```