_______________
# 1 Введение в Vue
_____________
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

____________
____________
____________



________________________
# 2 Введение в Vue часть 2
_______________________________

# Домашнее задание:

### Задача 1
Создайте компонент с именем "MyText" и добавьте в него объект data,
который будет содержать два свойства: "text1" со значением "Hello" и
"text2" со значением "World". Выведите значения свойств "text1" и "text2" в представлении компонента в двух отдельных абзацах. Добавьте в компонент метод "changeText", который будет менять значение свойства "text1" на "Goodbye".


#### Решение

```vue
<template>
  <div>
    <p>{{ text1 }}</p>
    <p>{{ text2 }}</p>
    <button @click="changeText">Change Text</button>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        text1: 'Hello',
        text2: 'World'
      }
    },
    methods: {
      changeText() {
        this.text1 = 'Goodbye'
      }
    }
  }
</script>

```

В этом компоненте мы создали объект data, содержащий два свойства text1 и text2 со значениями "Hello" и "World".
Затем мы вывели значения этих свойств в представлении компонента в двух отдельных абзацах.

Также мы добавили метод changeText, который меняет значение свойства text1 на "Goodbye".
Этот метод вызывается при клике на кнопку "Change Text", которую мы также добавили в компонент.

Когда пользователь кликает на кнопку, значение свойства text1 изменяется на "Goodbye",
и это изменение автоматически отражается в представлении.

### Задача 2

Создайте компонент с именем "Counter". Добавьте в него объект data,
который будет содержать свойство "counter" со значением 0.
Выведите значение свойства "counter" в представлении компонента в абзаце.
Добавьте в компонент методы "increment" и "decrement",
которые будут увеличивать и уменьшать значение свойства "counter" на 1 соответственно.
Создайте кнопки в представлении компонента, при нажатии на которые будут вызываться методы "increment" и "decrement" и изменять значение свойства "counter".

#### Решение
```vue
<template>
  <div>
    <button @click="increment">+</button>
    <span>{{ count }}</span>
    <button @click="decrement">-</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      count: 0,
    };
  },
  methods: {
    increment() {
      this.count++;
    },
    decrement() {
      this.count--;
    },
  },
};
</script>

```

### Задача 3
Создайте методы в Vue, которые будут складывать, вычитать, умножать и делить два числа. Каждая операция должна быть своим методом.\
Добавьте на страницу форму с двумя полями ввода для чисел и четыре кнопки для операций.\
По клику на каждую кнопку должна вызываться соответствующая операция и выводиться результат в какой-нибудь элемент на странице.


#### Решение
```vue
<template>
  <div>
    <h1>Калькулятор</h1>
    <form>
      <label for="num1">Первое число: </label>
      <input type="number" id="num1" v-model="num1">
      <br>
      <label for="num2">Второе число: </label>
      <input type="number" id="num2" v-model="num2">
      <br>
      <button @click.prevent="add">Сложить</button>
      <button @click.prevent="subtract">Вычесть</button>
      <button @click.prevent="multiply">Умножить</button>
      <button @click.prevent="divide">Делить</button>
    </form>
    <div v-if="result !== null">
      <h2>Результат: {{ result }}</h2>
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      num1: null,
      num2: null,
      result: null
    };
  },
  methods: {
    add() {
      this.result = this.num1 + this.num2;
    },
    subtract() {
      this.result = this.num1 - this.num2;
    },
    multiply() {
      this.result = this.num1 * this.num2;
    },
    divide() {
      if (this.num2 === 0) {
        this.result = null;
        alert("Деление на ноль невозможно!");
      } else {
        this.result = this.num1 / this.num2;
      }
    }
  }
};
</script>
```

В этом примере мы создали методы add, subtract, multiply и divide, каждый из которых выполняет соответствующую операцию.\
В форме на странице есть два поля ввода, куда пользователь вводит числа, и четыре кнопки, по нажатию на которые вызываются методы.\
Результат операции выводится на страницу в блоке, который появляется только после выполнения операции.\
Мы добавили проверку на деление на ноль, чтобы не было ошибок в случае, если пользователь попытается поделить на ноль. Если деление на ноль происходит, то выводится сообщение об ошибке, а результат становится равным null.

### Задача 4
Создайте новый Vue-компонент и определите в нем вспомогательный метод, который будет принимать массив чисел и возвращать их сумму.\
Создайте в этом же компоненте метод, который будет вызывать вспомогательный метод и выводить результат на экран.\
Навесьте на кнопку обработчик события click, который будет вызывать этот метод.
#### Решение

```vue
<template>
  <div>
    <button @click="showSum">Показать сумму чисел</button>
    <p>Сумма чисел: {{ sum }}</p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      numbers: [1, 2, 3, 4, 5],
      sum: null
    };
  },
  methods: {
    showSum() {
      this.sum = this.getSum(this.numbers);
    },
    getSum(numbers) {
      return numbers.reduce((accumulator, currentValue) => accumulator + currentValue);
    }
  }
};
</script>

```

В этом коде мы создали компонент, у которого есть массив чисел и свойство sum, которое будет хранить сумму этих чисел. \
В методе showSum мы вызываем вспомогательный метод getSum, который принимает массив чисел и возвращает их сумму. \
После этого мы записываем результат в свойство sum, чтобы отобразить его в HTML-разметке.\
Мы навесили на кнопку обработчик события click, который вызывает метод showSum при клике на кнопку.


_____________
_____________
_____________

_________________
# 3 Условный рендеринг в Vue
_________________
# Домашнее задание:

### Задача 1
Создайте компонент Vue, который содержит кнопку и абзац с текстом "Hello, World!". \
По нажатию на кнопку абзац должен скрываться/показываться.\
Текст на кнопке должен меняться в зависимости от того, скрыт ли абзац или нет.

#### Решение:

```vue
<template>
  <div>
    <button @click="toggleVisibility">{{ isVisible ? 'Hide' : 'Show' }}</button>
    <p v-if="isVisible">Hello, World!</p>
  </div>
</template>
<script>
export default {
  data() {
    return {
      isVisible: true,
    };
  },
  methods: {
    toggleVisibility() {
      this.isVisible = !this.isVisible;
    },
  },
};
</script>

```

В этом компоненте мы создаем кнопку, на которую мы добавляем обработчик событий @click. Этот обработчик вызывает метод toggleVisibility, который переключает значение свойства isVisible между true и false.

В элементе \<p> мы используем директиву v-if, чтобы показывать или скрывать абзац в зависимости от значения isVisible.

На кнопке мы используем тернарный оператор, чтобы изменять текст кнопки в зависимости от значения isVisible.\
Если isVisible равно true, то на кнопке будет написано "Hide", иначе будет написано "Show".


### Задача 2
Создать форму регистрации с проверкой на корректность введенных данных, используя сложные условия и директивы во Vue.

#### Решение

```vue
<template>
  <div>
    <form @submit.prevent="submitForm">
      <label for="email">Email:</label>
      <input type="email" id="email" v-model="email" required>
      <div v-if="emailError" class="error">{{ emailError }}</div>
      <label for="password">Password:</label>
      <input type="password" id="password" v-model="password" required>
      <div v-if="passwordError" class="error">{{ passwordError }}</div>
      <label for="confirmPassword">Confirm Password:</label>
      <input type="password" id="confirmPassword" v-model="confirmPassword" required>
      <div v-if="confirmPasswordError" class="error">{{ confirmPasswordError }}</div>
      <button type="submit" :disabled="formError">Register</button>
    </form>
  </div>
</template>

<script>
export default {
  data() {
    return {
      email: "",
      password: "",
      confirmPassword: "",
      emailError: "",
      passwordError: "",
      confirmPasswordError: "",
    };
  },
  methods: {
    submitForm() {
      this.emailError = "";
      this.passwordError = "";
      this.confirmPasswordError = "";
      let emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
      if (!emailRegex.test(this.email)) {
        this.emailError = "Неверный адрес электронной почты";
      }
      if (this.password.length < 8) {
        this.passwordError = "Пароль должен быть не менее 8 символов";
      }
      if (this.confirmPassword !== this.password) {
        this.confirmPasswordError = "Пароли не совпадают";
      }
    },
  },
  computed: {
    formError() {
      return this.emailError || this.passwordError || this.confirmPasswordError;
    },
  },
};
</script>

```


_____________
_____________
_____________

_________________
# 4 Vue условия и циклы
_________________

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

______________________________
______________________________
_____________________________

_____________________
# 5 Стилизация компонентов в Vue
_____________________

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