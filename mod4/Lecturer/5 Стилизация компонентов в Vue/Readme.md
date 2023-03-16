# 5 стилизация компонентов в Vue

Vue.js позволяет стилизовать компоненты, используя тег style внутри файла компонента. \
Этот тег позволяет писать CSS-код, который будет применяться только к HTML-коду компонента, а не к другим элементам на странице.

```vue
<script>
  export default {
    name: "MyComponent",
  };
</script>

<template>
  <div>
    <p>Some text here</p>
  </div>
</template>

<style>
  p {
    color: red;
  }
</style>

```

Здесь мы создали компонент MyComponent, который содержит абзац с текстом.\
Затем мы добавили тег style, в котором указали, что цвет текста внутри абзаца должен быть красным.

Чтобы изменить стиль конкретного элемента, нужно использовать селектор. Например, чтобы изменить цвет текста внутри абзаца, нужно использовать селектор p.\
Если вы хотите добавить классы или id для элементов, вы можете сделать это в шаблоне компонента и использовать их в качестве селекторов:

```vue
<template>
  <div>
    <p class="my-paragraph">Some text here</p>
  </div>
</template>

<style>
  .my-paragraph {
    color: red;
  }
</style>
```

Здесь мы добавили класс "my-paragraph" для абзаца в шаблоне компонента и использовали его в качестве селектора в теге style, чтобы изменить цвет текста внутри абзаца.

Также в Vue.js есть возможность работать с атрибутом class через директиву v-bind или ее сокращенную форму :class. \
Это позволяет динамически изменять классы элементов в зависимости от состояния компонента или его свойств.

```vue
<template>
  <div :class="{ active: isActive, 'text-danger': hasError }">
    Some text here
  </div>
</template>

<script>
  export default {
    name: "MyComponent",
    data() {
      return {
        isActive: true,
        hasError: false,
      };
    },
  };
</script>

<style>
  .active {
    font-weight: bold;
  }

  .text-danger {
    color: red;
  }
</style>

```

Здесь мы используем директиву :class, чтобы добавить классы в зависимости от значения свойств isActive и hasError.\
Если isActive равно true, то будет добавлен класс "active", который задает жирный шрифт. \
Если hasError равно true, то будет добавлен класс "text-danger", который задает красный цвет текста.

Также в Vue.js можно использовать объект для хранения классов.\
В этом случае имена классов будут ключами объекта, а значения - булевыми, которые определяют, нужно ли добавить этот класс к элементу или нет. Для этого используется директива v-bind:class.

```vue
<template>
  <div :class="classes">
    <h1>Hello, World!</h1>
  </div>
</template>

<script>
export default {
  data() {
    return {
      classes: {
        'red-text': true,
        'bold-text': false,
        'italic-text': true,
      },
    };
  },
};
</script>

<style>
.red-text {
  color: red;
}

.bold-text {
  font-weight: bold;
}

.italic-text {
  font-style: italic;
}
</style>

```

# Реактивность объекта с CSS классами

Реактивность объекта с CSS классами предназначена для того, чтобы удобно изменять классы элементов в зависимости от изменения значений свойств объекта.

Для этого необходимо использовать специальный объект computed, который будет возвращать объект со свойствами-классами, зависящими от значений свойств объекта data.

```vue
<template>
  <div :class="classes">
    Reactive classes
  </div>
</template>

<script>
export default {
  data() {
    return {
      isActive: true,
      hasError: false
    }
  },
  computed: {
    classes() {
      return {
        active: this.isActive,
        error: this.hasError
      }
    }
  }
}
</script>

<style>
.active {
  color: green;
}

.error {
  color: red;
}
</style>

```

Мы определяем объект computed с именем classes, который возвращает объект со свойствами-классами, зависящими от значений свойств isActive и hasError объекта data.\
Этот объект передается в атрибут class элемента div, который будет иметь класс active если isActive имеет значение true, и класс error если hasError имеет значение true.\
При изменении значений свойств isActive и hasError, классы элемента div будут автоматически обновляться в соответствии с этими значениями.

________________________________
_______________________________
______________________________
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