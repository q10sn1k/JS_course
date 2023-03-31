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

____________
____________
____________



________________________
# 6 Двусторонняя привязка данных к инпутам в Vue:
_______________________________

# Домашнее задание:

### Задача 1

На странице есть список элементов с ценами.\
Рядом с каждым элементом находится чекбокс. \
Необходимо реализовать подсчет суммы выбранных элементов из списка и вывод этой суммы на страницу.

#### Решение

```vue
<template>
  <div>
    <div v-for="(item, index) in items" :key="index">
      <input type="checkbox" v-model="selectedItems" :value="item.price">
      {{ item.name }}: ${{ item.price }}
    </div>
    <p>Total: ${{ total }}</p>
  </div>
</template>
<script>
export default {
  data() {
    return {
      items: [
        { name: 'Item 1', price: 10 },
        { name: 'Item 2', price: 20 },
        { name: 'Item 3', price: 30 },
        { name: 'Item 4', price: 40 }
      ],
      selectedItems: []
    }
  },
  computed: {
    total() {
      return this.selectedItems.reduce((sum, price) => sum + Number(price), 0)
    }
  }
}
</script>
```

### Задача 2

На странице есть форма отправки сообщения. \
Поля формы - имя, email и сообщение.\
Необходимо реализовать валидацию полей формы и отправку данных на сервер.

#### Решение

```vue
<template>
  <div>
    <form @submit.prevent="submitForm">
      <label>
        Name:
        <input type="text" v-model="name">
        <div v-if="!nameValid">Введите ваше имя</div>
      </label>
      <label>
        Email:
        <input type="email" v-model="email">
        <div v-if="!emailValid">Введите адрес электронной почты</div>
      </label>
      <label>
        Message:
        <textarea v-model="message"></textarea>
        <div v-if="!messageValid">Введите ваше сообщение</div>
      </label>
      <button :disabled="!formValid">Ок</button>
    </form>
    <p v-if="formSubmitted">Сообщение отправлено</p>
  </div>
</template>
<script>
export default {
  data() {
    return {
      name: '',
      email: '',
      message: '',
      formSubmitted: false
    }
  },
  computed: {
    nameValid() {
      // Поле "name" не должно быть пустым
      return this.name !== ''
    },
    emailValid() {
      // Проверка валидности email
      const emailRegex = /^\w+([.-]?\w+)*@\w+([.-]?\w+)*(\.\w{2,})+$/
      return emailRegex.test(this.email)
    },
    messageValid() {
      // Поле "message" не должно быть пустым
      return this.message !== ''
    },
    formValid() {
      // Все поля формы должны быть валидными
      return this.nameValid && this.emailValid && this.messageValid
    }
  },
  methods: {
    submitForm() {
      // Отправка данных на сервер
      // В данном случае мы просто выводим сообщение о том, что форма отправлена
      this.formSubmitted = true
      // Сброс значений полей формы
      this.name = ''
      this.email = ''
      this.message = ''
    }
  }
}
</script>
```

### Задача 3

На странице есть форма заказа товара. \
Каждый товар имеет свой список опций, которые можно выбирать.\
Необходимо реализовать динамический список опций для каждого товара и отправку данных на сервер при нажатии на кнопку.

#### Решение

```vue
<template>
  <div>
    <div v-for="(product, index) in products" :key="index">
      <h3>{{ product.name }}</h3>
      <p>Цена: {{ product.price }}</p>
      <label v-for="(option, optionIndex) in product.options" :key="optionIndex">
        <input type="checkbox" v-model="product.selectedOptions" :value="option">
        {{ option }}
      </label>
    </div>
    <button @click="submitForm">Оформить заказ</button>
  </div>
</template>
<script>
export default {
  data() {
    return {
      products: [
        {
          name: 'Товар 1',
          price: 100,
          options: ['Опция 1', 'Опция 2', 'Опция 3'],
          selectedOptions: []
        },
        {
          name: 'Товар 2',
          price: 200,
          options: ['Опция 4', 'Опция 5', 'Опция 6'],
          selectedOptions: []
        },
        {
          name: 'Товар 3',
          price: 300,
          options: ['Опция 7', 'Опция 8', 'Опция 9'],
          selectedOptions: []
        }
      ]
    }
  },
  methods: {
    submitForm() {
      const formData = {
        products: this.products.map((product) => {
          return {
            name: product.name,
            price: product.price,
            options: product.selectedOptions
          }
        })
      }
      console.log(formData) // отправка данных на сервер
    }
  }
}
</script>

```

### Задача 4

На странице есть форма регистрации с полями ввода имени, электронной почты и пароля. \
Необходимо реализовать валидацию полей формы и блокировку кнопки Submit, если поля формы не заполнены или заполнены некорректно.

#### Решение

```vue
<template>
  <div>
    <label for="name">Name:</label>
    <input type="text" id="name" v-model="name">
    <p v-if="!nameValid" class="error">Enter valid name</p>
    <label for="email">Email:</label>
    <input type="email" id="email" v-model="email">
    <p v-if="!emailValid" class="error">Enter valid email</p>

    <label for="password">Password:</label>
    <input type="password" id="password" v-model="password">
    <p v-if="!passwordValid" class="error">Password must be at least 6 characters long</p>

    <button :disabled="!formValid" @click="submitForm">Submit</button>
  </div>
</template>
<script>
export default {
  data() {
    return {
      name: '',
      email: '',
      password: '',
    };
  },
  computed: {
    nameValid() {
      return /^[a-zA-Z]+$/.test(this.name);
    },
    emailValid() {
      return /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(this.email);
    },
    passwordValid() {
      return this.password.length >= 6;
    },
    formValid() {
      return this.nameValid && this.emailValid && this.passwordValid;
    },
  },
  methods: {
    submitForm() {
      // submit form data to server
      console.log('Submit data form ');
    },
  },
};
</script>
<style>
.error {
  color: red;
}
</style>
```
____________
____________
____________



________________________
# 7 Расширенная работа с компонентами
_______________________________

# Домашнее задание:

### Задача 1

Необходимо разработать компонент "Чеклист" на Vue.js. Компонент должен представлять собой список задач, каждая из которых имеет возможность отмечаться как выполненная и удаляться. Также должна быть возможность добавления новых задач.

#### Решение


src/components/ChecklistComponent.vue

```vue
<template>
  <div>
    <h1>{{ title }}</h1>
    <ul>
      <checklist-item
        v-for="(item, index) in items"
        :key="index"
        :item="item"
        @toggle="toggleItem(index)"
        @delete="deleteItem(index)"
      ></checklist-item>
    </ul>
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
      title: 'My Checklist',
      items: [
        { text: 'Task 1', done: false },
        { text: 'Task 2', done: false },
        { text: 'Task 3', done: false },
      ]
    }
  },
  methods: {
    toggleItem(index) {
      const item = this.items[index]
      if (item && item.done !== undefined) {
        item.done = !item.done
      } else {
        this.items[index].done = false
      }
    },
    deleteItem(index) {
      this.items.splice(index, 1)
    },
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
  <li :class="{ 'done': localItem.done }">
    <input type="checkbox" v-model="localItem.done" @change="toggle" />
    <span>{{ localItem.text }}</span>
    <button @click="deleteItem">Delete</button>
  </li>
</template>

<script>
export default {
  props: {
    item: {
      type: Object,
      required: true
    }
  },
  data() {
    return {
      localItem: Object.assign({}, this.item)
    }
  },
  methods: {
    toggle() {
      this.$emit('toggle')
    },
    deleteItem() {
      this.$emit('delete')
    }
  },
  watch: {
    item(newValue) {
      this.localItem = Object.assign({}, newValue)
    }
  }
}
</script>

```

src/components/NewItemForm.vue

```vue
<template>
  <form @submit.prevent="addItem">
    <input type="text" v-model="text" placeholder="Add a new item..." />
    <button type="submit">Add</button>
  </form>
</template>

<script>
export default {
  data() {
    return {
      text: ''
    }
  },
  methods: {
    addItem() {
      if (this.text.trim()) {
        this.$emit('add', this.text.trim())
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
____________
____________
____________



________________________
# 8 Реактивное редактирование данных
_______________________________
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
____________
____________
____________



________________________
# 9 Node.js
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


____________
____________
____________



________________________
# 10 Node.js асинхронная работа
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

____________
____________
____________



________________________
# 11 HTTP сервер на NodeJS

____________
____________
____________
# Домашнее задание:

### Задача 1

Создайте сервер, который будет отдавать файл style.css при обращении к адресу /style.css и index.html при обращении к адресу /.

#### Решение

```js
const http = require('http');
const fs = require('fs');

http.createServer(async (request, response) => {
  let data;
  let type;
  if (request.url === '/style.css') {
    data = await fs.promises.readFile('style.css', 'utf8');
    type = 'text/css';
  } else {
    data = await fs.promises.readFile('index.html', 'utf8');
    type = 'text/html';
  }
  response.writeHead(200, {'Content-Type': type});
  response.write(data);
  response.end();
}).listen(3000);

```

### Задача 2

Создайте сервер, который будет возвращать статус 404 и сообщение "Page not found" при обращении к несуществующему адресу.

#### Решение

```js
const http = require('http');

http.createServer((request, response) => {
    response.writeHead(404, {'Content-Type': 'text/plain'});
    response.write('Page not found');
    response.end();
}).listen(3000);

```

### Задача 3

Создайте сервер, который будет отдавать содержимое файла index.html при обращении к адресу /, и при обращении к адресу /style.css - содержимое файла style.css.\
При обращении к любому другому адресу сервер должен возвращать статус 404 и сообщение "Page not found".

#### Решение

```js
const http = require('http');
const fs = require('fs');

http.createServer(async (request, response) => {
  let data;
  let type;
  if (request.url === '/') {
    data = await fs.promises.readFile('index.html', 'utf8');
    type = 'text/html';
  } else if (request.url === '/style.css') {
    data = await fs.promises.readFile('style.css', 'utf8');
    type = 'text/css';
  } else {
    response.writeHead(404, {'Content-Type': 'text/plain'});
    response.write('Page not found');
    response.end();
    return;
  }
  response.writeHead(200, {'Content-Type': type});
  response.write(data);
  response.end();
}).listen(3000);

```

### Задача 4

Создайте сервер, который будет отдавать страницу с ошибкой 500 при любом запросе.

#### Решение


```js
const http = require('http');

http.createServer((request, response) => {
  response.writeHead(500, {'Content-Type': 'text/plain'});
  response.write('Internal Server Error');
  response.end();
}).listen(3000);

```



________________________
# 12 Статический сервер на Node.js

____________
____________
____________
# Домашнее задание:

Во всех задачах будем использовать следующий базовый сервер:

```js
const http = require('http'); // Подключаем модуль для создания сервера
const fs = require('fs'); // Подключаем модуль для работы с файлами

http.createServer(async (request, response) => { // Создаем HTTP-сервер
    if (request.url !== '/favicon.ico') { // Игнорируем запросы на /favicon.ico
        let path = 'root' + request.url; // Получаем путь к запрошенному файлу
        let status,
            text;

        try {
            if ((await fs.promises.stat(path)).isDirectory()) { // Если это директория, то ищем index.html
                path += '/index.html';
            }
            text = await fs.promises.readFile(path, 'utf8'); // Читаем файл
            status = 200;
        } catch (err) {
            text = 'Page not found';
            status = 404;
        }
        let mimeType = getMimeType(path); // Определяем MIME-тип файла
        response.writeHead(status, {'Content-Type': mimeType}); // Отправляем заголовок ответа
        response.write(text); // Отправляем содержимое файла
        response.end(); // Закрываем соединение
    }
}).listen(3000); // Слушаем порт 3000
```

### Задача 1

Добавьте к нашему серверу обработку запросов на статические ресурсы, такие как изображения, стили и скрипты.\
Возможные расширения файлов: **.jpg, .jpeg, .png, .svg, .json, .js, .css, .ico**.

#### Решение

```js
const http = require('http'); // Подключаем модуль для создания сервера
const fs = require('fs'); // Подключаем модуль для работы с файлами

/**
 * Функция для определения MIME типа по расширению файла
 * @param {string} path - путь к файлу
 * @returns {string} MIME тип
 */
function getMimeType(path) {
    let mimes = { // Объект, содержащий соответствие расширений и MIME типов
        html: 'text/html',
        jpeg: 'image/jpeg',
        jpg:  'image/jpeg',
        png:  'image/png',
        svg:  'image/svg+xml',
        json: 'application/json',
        js:   'text/javascript',
        css:  'text/css',
        ico:  'image/x-icon',
    };

    let exts = Object.keys(mimes); // Массив расширений файлов
    let extReg = new RegExp('\\.(' + exts.join('|') + ')$'); // Регулярное выражение для поиска расширения в пути к файлу

    let ext = path.match(extReg)[1]; // Получаем расширение файла из пути

    if (ext) {
        return mimes[ext]; // Возвращаем соответствующий MIME тип
    } else {
        return 'text/plain'; // Возвращаем MIME тип по умолчанию
    }
}

http.createServer(async (request, response) => { // Создаем HTTP-сервер
    if (request.url !== '/favicon.ico') { // Игнорируем запросы на /favicon.ico
        let path = 'root' + request.url; // Преобразуем URL в путь к файлу
        let status;
        let text;

        try {
            if ((await fs.promises.stat(path)).isDirectory()) { // Если запрошен директорий, то добавляем /index.html
                path += '/index.html';
            }

            text = await fs.promises.readFile(path); // Читаем содержимое файла
            status = 200; // Устанавливаем статус 200 ОК
        } catch (err) {
            text = 'Page not found'; // Выводим сообщение об ошибке
            status = 404; // Устанавливаем статус 404 Not Found
        }

        let mimeType = getMimeType(path); // Получаем MIME тип по расширению файла
        response.writeHead(status, {'Content-Type': mimeType}); // Устанавливаем заголовок с соответствующим MIME типом

        response.write(text); // Отправляем содержимое файла
        response.end(); // Закрываем соединение
    }
}).listen(3000); // Слушаем порт 3000



```

### Задача 2

На сервере добавьте возможность использования переменных в файлах шаблона, которые будут заменяться на соответствующие значения при обработке шаблона.

#### Решение

```html
<!DOCTYPE html>
<html>
    <head>
        <title>{% get title '' %}</title>
    </head>
    <body>
        <h1>Hello, {{ name }}!</h1>
    </body>
</html>
```

```js
const http = require('http');
const fs = require('fs');

// Функция для определения MIME типа по расширению файла
function getMimeType(path) {
    let mimes = {
        html: 'text/html',
        jpeg: 'image/jpeg',
        jpg:  'image/jpeg',
        png:  'image/png',
        svg:  'image/svg+xml',
        json: 'application/json',
        js:   'text/javascript',
        css:  'text/css',
        ico:  'image/x-icon',
    };

    // Регулярное выражение для поиска расширения файла
    let exts = Object.keys(mimes);
    let extReg = new RegExp('\\.(' + exts.join('|') + ')$');

    // Получение расширения файла из пути
    let ext = path.match(extReg)[1];

    // Возвращаем соответствующий MIME тип
    if (ext) {
        return mimes[ext];
    } else {
        return 'text/plain';
    }
}

http.createServer(async (request, response) => { // Создаем HTTP-сервер
    if (request.url !== '/favicon.ico') { // Игнорировать запросы на /favicon.ico
        let path = 'root' + request.url; // Преобразуем URL в путь к файлу
        let variables = {name: 'World'}; // Создаем объект переменных для шаблонизации
	let status,
	    text;
	    
        try {
            if ((await fs.promises.stat(path)).isDirectory()) {
                path += '/index.html'; // Добавляем /index.html, если это директория
            }

            text = await fs.promises.readFile(path, 'utf8'); // Читаем содержимое файла
            status = 200; // Статус успешного ответа

            const reg = /\{\{\s*([\w.-]+)\s*\}\}/g; // Регулярное выражение для поиска переменных в шаблоне
            const match = text.match(reg); // Ищем все переменные в шаблоне

            if (match) { // Если нашли переменные
                for (let i = 0; i < match.length; i++) { // Проходимся по ним
                    const key = match[i].replace(/\{\{\s*|\s*\}\}/g, ''); // Получаем имя переменной
                    const value = variables[key] || ''; // Получаем значение переменной из объекта переменных или пустую строку, если значение не задано
                    text = text.replace(match[i], value); // Заменяем переменную в шаблоне на соответствующее значение
                }
            }
        } catch (err) {
            text = 'Page not found';
            status = 404;
        }

        let mimeType = getMimeType(path); // Определяем MIME тип
        response.writeHead(status, {'Content-Type': mimeType}); // Отправляем заголовок с типом контента

        response.write(text); // Отправляем содержимое файла
        response.end(); // Закрываем соединение
    }
}).listen(3000); // Слушаем порт 3000

```

### Задача 3

Реализуйте возможность добавления пользовательских элементов в шаблон сайта.\
При этом эти элементы будут загружаться из отдельных файлов с расширением .html, расположенных в папке elems.\
Команда для добавления элемента: `{% get element 'имя элемента' %}`.

#### Решение

```html
<!DOCTYPE html>
<html>
    <head>
        <title>{% get title '' %}</title>
    </head>
    <body>
        <div id="header">{% get element 'header' %}</div>
        <div id="content">{% get content '' %}</div>
        <div id="footer">{% get element 'footer' %}</div>
    </body>
</html>

```

```js
const http = require('http');
const fs = require('fs');
const path = require('path');

function getMimeType(path) {
    let mimes = {
        html: 'text/html',
        jpeg: 'image/jpeg',
        jpg:  'image/jpeg',
        png:  'image/png',
        svg:  'image/svg+xml',
        json: 'application/json',
        js:   'text/javascript',
        css:  'text/css',
        ico:  'image/x-icon',
    };

    let exts = Object.keys(mimes);
    let extReg = new RegExp('\\.(' + exts.join('|') + ')$');

    let ext = path.match(extReg)[1];

    if (ext) {
        return mimes[ext];
    } else {
        return 'text/plain';
    }
}

function loadTemplate(name, variables) {
    let lpath = path.join(__dirname, 'templates', name);
    let layout = fs.readFileSync(lpath, 'utf8');

    let reg = /\{\{\s*([\w.-]+)\s*\}\}/g;
    layout = layout.replace(reg, (match0, match1) => {
        let value = variables[match1] || '';
        return value;
    });

    return layout;
}

http.createServer(async (request, response) => {
    if (request.url !== '/favicon.ico') {
        let path = 'root' + request.url;
        let status,
            text;
        let variables = {name: 'World'};

        try {
            if ((await fs.promises.stat(path)).isDirectory()) {
                path += '/index.html';
            }

            text = await fs.promises.readFile(path, 'utf8');
            status = 200;

            const reg = /\{\{\s*([\w.-]+)\s*\}\}/g;
            const match = text.match(reg);

            if (match) {
                for (let i = 0; i < match.length; i++) {
                    const key = match[i].replace(/\{\{\s*|\s*\}\}/g, '');
                    const value = variables[key] || '';
                    text = text.replace(match[i], value);
                }
            }

            let mimeType = getMimeType(path);
            response.writeHead(status, {'Content-Type': mimeType});
            response.write(text);
            response.end();
        } catch (err) {
            status = 404;
            let layout = loadTemplate('404.html', {name: 'World'});
            let mimeType = getMimeType('404.html');
            response.writeHead(status, {'Content-Type': mimeType});
            response.write(layout);
            response.end();
        }
    }
}).listen(3000);

```

### Задача 4

Реализуйте возможность подключения стилей и скриптов к страницам сайта. \
Для этого добавьте в шаблон сайта два новых тега: `{% css 'путь к css файлу' %}` и `{% js 'путь к js файлу' %}`. \
Эти теги должны вставлять соответствующие теги **<link/>** и **<script/>** в соответствующие места в head и body страницы.

#### Решение

```html
<!DOCTYPE html>
<html>
    <head>
        <title>{% get title '' %}</title>
        {% css '/css/style.css' %}
    </head>
    <body>
        <div id="header">{% get element 'header' %}</div>
        <div id="content">{% get content '' %}</div>
        {% js '/js/main.js' %}
        <div id="footer">{% get element 'footer' %}</div>
    </body>
</html>

```

```js
const http = require('http');
const fs = require('fs');
const path = require('path');

function getMimeType(path) {
    let mimes = {
        html: 'text/html',
        jpeg: 'image/jpeg',
        jpg:  'image/jpeg',
        png:  'image/png',
        svg:  'image/svg+xml',
        json: 'application/json',
        js:   'text/javascript',
        css:  'text/css',
        ico:  'image/x-icon',
    };

    let exts = Object.keys(mimes);
    let extReg = new RegExp('\\.(' + exts.join('|') + ')$');

    let ext = path.match(extReg)[1];

    if (ext) {
        return mimes[ext];
    } else {
        return 'text/plain';
    }
}

function loadTemplate(name, variables) {
    let lpath = path.join(__dirname, 'templates', name);
    let layout = fs.readFileSync(lpath, 'utf8');

    let reg = /\{\{\s*([\w.-]+)\s*\}\}/g;
    layout = layout.replace(reg, (match0, match1) => {
        let value = variables[match1] || '';
        return value;
    });

    return layout;
}

function parseTags(text, variables) {
    let cssReg = /\{%\s*css\s*'([^']+)'\s*%\}/g;
    let jsReg = /\{%\s*js\s*'([^']+)'\s*%\}/g;

    text = text.replace(cssReg, (match0, match1) => {
        return '<link rel="stylesheet" href="' + match1 + '">';
    });

    text = text.replace(jsReg, (match0, match1) => {
        return '<script src="' + match1 + '"></script>';
    });

    let reg = /\{\{\s*([\w.-]+)\s*\}\}/g;
    let match = text.match(reg);

    if (match) {
        for (let i = 0; i < match.length; i++) {
            const key = match[i].replace(/\{\{\s*|\s*\}\}/g, '');
            const value = variables[key] || '';
            text = text.replace(match[i], value);
        }
    }

    return text;
}

http.createServer(async (request, response) => {
    if (request.url !== '/favicon.ico') {
        let path = 'root' + request.url;
        let status,
            text;
        let variables = {name: 'World'};

        try {
            if ((await fs.promises.stat(path)).isDirectory()) {
                path += '/index.html';
            }

            text = await fs.promises.readFile(path, 'utf8');
            status = 200;

            text = parseTags(text, variables);

            let mimeType = getMimeType(path);
            response.writeHead(status, {'Content-Type': mimeType});
            response.write(text);
            response.end();
        } catch (err) {
            status = 404;
            let layout = loadTemplate('404.html', {name: 'World'});
            let mimeType = getMimeType('404.html');
            response.writeHead(status, {'Content-Type': mimeType});
            response.write(layout);
            response.end();
        }
    }
}).listen(3000);

```



________________________
# 13 Express JS

____________
____________
____________
# Домашнее задание:

Будем работать с разработанным ранее разработанным нами веб-приложением. (part 13 Express JS)

### Задача 1

Добавьте на главную страницу приложения дату и время последнего обновления страницы.

#### Решение

Мы можем добавить дату и время последнего обновления страницы, создав middleware-функцию, которая будет добавлять соответствующий заголовок ответа.\
Для этого создадим файл last-updated.js со следующим содержимым:

```js
function lastUpdated(req, res, next) {
    res.set('Last-Modified', new Date().toUTCString());
    next();
}

module.exports = lastUpdated;

```

Затем мы можем подключить эту middleware-функцию к маршруту главной страницы в файле app.js:

```js
const lastUpdated = require('./middlewares/last-updated');

app.get('/', lastUpdated, (req, res) => {
  res.render('index');
});

```

Теперь при каждом обновлении страницы будет устанавливаться соответствующий заголовок ответа, который будет содержать дату и время последнего обновления страницы.

### Задача 2

Добавьте на страницу пользователя кнопку для удаления его из списка пользователей.

#### Решение

Мы можем добавить на страницу пользователя кнопку для удаления его из списка пользователей, создав новый маршрут для обработки DELETE-запросов и обновив соответствующие шаблоны.

Для этого сначала добавим на страницу пользователя кнопку для удаления пользователя:

```html
<!DOCTYPE html>
<html>
<head>
    <title><%= user.name %></title>
    <link rel="stylesheet" href="/styles.css">
</head>
<body>
<h1><%= user.name %></h1>
<p>ID: <%= user.id %></p>
<p>Name: <%= user.name %></p>
<form method="post" action="/users/<%= user.id %>?_method=DELETE">
    <button type="submit">Delete user</button>
</form>
<a href="/users">Back to users</a>
</body>
</html>

```

Далее добавим новый маршрут для обработки DELETE-запросов в файл users.js:

```js
router.delete('/:id', (req, res) => {
  const userId = req.params.id;
  const userIndex = users.findIndex(user => user.id == userId);
  if (userIndex === -1) {
    res.status(404).send('User not found');
  } else {
    users.splice(userIndex, 1);
    res.redirect('/users');
  }
});

```


Мы определили маршрут для удаления пользователя по ID.\
При получении DELETE-запроса мы находим индекс пользователя в массиве users и удаляем его с помощью метода splice. \
Далее мы перенаправляем пользователя на страницу со списком пользователей.

Теперь, когда мы добавили новый маршрут и обновили соответствующие шаблолоны, мы можем добавить обработку DELETE-запросов на страницу пользователя, добавив middleware-функцию methodOverride в файл app.js:


```js
const methodOverride = require('method-override');
app.use(methodOverride('_method'));

```

Эта middleware-функция позволяет использовать методы PUT и DELETE в HTML-формах, которые не поддерживаются стандартом HTTP.\
Мы указываем символ _method в качестве параметра, чтобы определить, какой метод должен быть использован для обработки запроса.

### Задача 3

Добавьте на страницу со списком пользователей возможность редактирования имени пользователя.

#### Решение

Мы можем добавить на страницу со списком пользователей ссылки для редактирования имени пользователя, а также создать новый маршрут для обработки PUT-запросов на обновление имени пользователя.\
Для этого сначала добавим на страницу со списком пользователей ссылки для редактирования имени пользователя:

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Users</title>
    <link rel="stylesheet" href="/styles.css">
  </head>
  <body>
    <h1>Users</h1>
    <ul>
      <% users.forEach(user => { %>
        <li>
          <a href="/users/<%= user.id %>"><%= user.name %></a>
          <form method="post" action="/users/<%= user.id %>?_method=PUT">
            <input type="text" name="name" value="<%= user.name %>">
            <button type="submit">Update</button>
          </form>
        </li>
      <% }); %>
    </ul>
    <form method="post" action="/users">
      <input type="text" name="name" placeholder="Enter user name">
      <button type="submit">Add user</button>
    </form>
  </body>
</html>

```

Далее добавим новый маршрут для обработки PUT-запросов в файл users.js:


```js
router.put('/:id', (req, res) => {
  const userId = req.params.id;
  const userIndex = users.findIndex(user => user.id == userId);
  if (userIndex === -1) {
    res.status(404).send('User not found');
  } else {
    const newName = req.body.name;
    if (!newName) {
      res.status(400).send('Name is required');
    } else {
      users[userIndex].name = newName;
      res.redirect('/users');
    }
  }
});

```

Мы определили маршрут для обновления имени пользователя по ID. \
При получении PUT-запроса мы находим индекс пользователя в массиве users и обновляем его имя. \
Далее мы перенаправляем пользователя на страницу со списком пользователей. \
Если новое имя не было передано или пустое, мы возвращаем ошибку со статусом 400.

Теперь, когда мы добавили новый маршрут и обновили соответствующие шаблоны, мы можем добавить обработку PUT-запросов на страницу со списком пользователей, добавив middleware-функцию methodOverride в файл app.js:

```js
const methodOverride = require('method-override');
app.use(methodOverride('_method'));

```

Эта middleware-функция позволяет использовать методы PUT и DELETE в HTML-формах, которые не поддерживаются стандартом HTTP. \
Мы указываем символ _method в качестве параметра, чтобы определить, какой метод должен быть использован для обработки запроса.

# Ненадо добавлять эту функцию если вы решили задачу 2

### Задача 4

Добавьте на страницу сообщения возможность добавления и удаления комментариев к сообщению.

#### Решение

Мы можем добавить на страницу сообщения формы для добавления комментариев и ссылки для удаления комментариев, а также создать новый маршрут для обработки POST-запросов на добавление комментариев и DELETE-запросов на удаление комментариев. \
Для этого сначала добавим на страницу сообщения форму для добавления комментариев и ссылки для удаления комментариев:

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Message <%= message.id %></title>
    <link rel="stylesheet" href="/styles.css">
  </head>
  <body>
    <h1>Message <%= message.id %></h1>
    <p>ID: <%= message.id %></p>
    <p>Text: <%= message.text %></p>
    <h2>Comments</h2>
    <ul>
      <% message.comments.forEach(comment => { %>
        <li>
          <%= comment.text %>
          <form method="post" action="/messages/<%= message.id %>/comments/<%= comment.id %>?_method=DELETE">
            <button type="submit">Delete</button>
          </form>
        </li>
      <% }); %>
    </ul>
    <form method="post" action="/messages/<%= message.id %>/comments">
      <input type="text" name="text" placeholder="Enter comment">
      <button type="submit">Add comment</button>
    </form>
    <a href="/messages">Back to messages</a>
  </body>
</html>

```

Затем добавим новый маршрут для обработки POST-запросов на добавление комментариев в файл comments.js:

```js
router.post('/:messageId/comments', (req, res) => {
  const messageId = req.params.messageId;
  const messageIndex = messages.findIndex(message => message.id == messageId);
  if (messageIndex === -1) {
    res.status(404).send('Message not found');
  } else {
    const newComment = { id: uuidv4(), text: req.body.text };
    messages[messageIndex].comments.push(newComment);
    res.redirect(`/messages/${messageId}`);
  }
});

```

Мы определили маршрут для добавления комментария к сообщению по ID. \
При получении POST-запроса мы находим индекс сообщения в массиве messages и добавляем новый комментарий в массив comments данного сообщения. \
Затем мы перенаправляем пользователя на страницу с сообщением.

Далее добавим новый маршрут для обработки DELETE-запросов на удаление комментариев в файл comments.js:

```js
router.delete('/:messageId/comments/:commentId', (req, res) => {
  const messageId = req.params.messageId;
  const commentId = req.params.commentId;
  const messageIndex = messages.findIndex(message => message.id == messageId);
  if (messageIndex === -1) {
    res.status(404).send('Message not found');
  } else {
    const commentIndex = messages[messageIndex].comments.findIndex(comment => comment.id == commentId);
    if (commentIndex === -1) {
      res.status(404).send('Comment not found');
    } else {
      messages[messageIndex].comments.splice(commentIndex, 1);
      res.redirect(`/messages/${messageId}`);
    }
  }
});

```

Мы определили маршрут для удаления комментария к сообщению по ID сообщения и ID комментария. \
При получении DELETE-запроса мы находим индексы сообщения и комментария в массивах messages и comments соответственно и удаляем комментарий с помощью метода splice. \
Далее мы перенаправляем пользователя на страницу с сообщением.

Когда мы добавили новые маршруты и обновили соответствующие шаблоны, мы можем добавить обработку POST- и DELETE-запросов на страницу с сообщением, добавив middleware-функцию methodOverride в файл app.js:

```js
const methodOverride = require('method-override');
app.use(methodOverride('_method'));

```

Эта middleware-функция позволяет использовать методы PUT и DELETE в HTML-формах, которые не поддерживаются стандартом HTTP. Мы указываем символ _method в качестве параметра, чтобы определить, какой метод должен быть использован для обработки запроса.

# Не надо добавлять эту функцию если вы решили задачу 2 или 3 (2 и 3).


________________________
# 14 Шаблонизатор Handlebars в Express
______________________________________
______________________________________
______________________________________

# Домашнее задание:

### Задача 1

Написать интернет-магазин на Node.js с использованием фреймворка Express и шаблонизатора Handlebars. \
Сайт должен иметь несколько страниц: главную, страницу каталога товаров, страницу товара, корзину и страницы авторизации и регистрации пользователей.\
В каталоге товаров должен быть реализован функционал фильтрации товаров по цене и категории с помощью хелперов. \
Все данные о товарах должны храниться в файле `data/products.json.` \
Стили должны быть реализованы с помощью Bootstrap.

## Архитектура приложения:

```
├── app.js
├── data
│   └── products.json
├── helpers.js
├── node_modules
├── package-lock.json
├── package.json
└── views
    ├── catalog.handlebars
    ├── layouts
    │   └── main.handlebars
    └── partials
        ├── footer.handlebars
        └── header.handlebars
```

# решение

`app.js`

```js
const express = require('express');
const hbs = require('hbs');
const path = require('path');
const productsData = require('./data/products.json');

const app = express();

// Устанавливаем путь к папке с шаблонами
app.set('views', path.join(__dirname, 'views'));

// Устанавливаем шаблонизатор Handlebars как движок для рендеринга страниц
app.set('view engine', 'hbs');

// Регистрируем частичные шаблоны для шапки и подвала страницы
hbs.registerPartials(path.join(__dirname, 'views/partials'));

// Регистрируем хелперы для форматирования цены и изображения товара
require('./helpers');

// Определяем маршрут для главной страницы
app.get('/', (req, res) => {
  res.render('index', {
    title: 'Интернет-магазин',
    header: 'Добро пожаловать в наш магазин!',
  });
});

// Определяем маршрут для страницы каталога товаров
app.get('/catalog', (req, res) => {
  const { category, minPrice, maxPrice } = req.query;

  // Фильтруем товары по категории и цене
  const filteredProducts = productsData.filter((product) => {
    if (category && product.category !== category) {
      return false;
    }

    if (minPrice && product.price < minPrice) {
      return false;
    }

    if (maxPrice && product.price > maxPrice) {
      return false;
    }

    return true;
  });

  res.render('catalog', {
    title: 'Каталог',
    products: filteredProducts,
  });
});

// Запускаем сервер
app.listen(3000, () => {
  console.log('Сервер запущен на порту 3000');
});
```

`data/products.json`

```json
[
  {
    "id": 1,
    "name": "Смартфон Apple iPhone 12 Pro",
    "category": "Смартфоны",
    "description": "Новый iPhone 12 Pro с OLED-экраном Super Retina XDR 6,1 дюйма. Новая камера Pro с возможностью съемки в формате Apple ProRAW и LiDAR-сканером для AR-приложений.",
    "image": "/images/iphone12pro.jpg",
    "price": 99990
  },
  {
    "id": 2,
    "name": "Ноутбук Apple MacBook Air",
    "category": "Ноутбуки",
    "description": "Новый MacBook Air с чипом M1 – наш самый популярный ноутбук в новом исполнении. Новый чип M1 обеспечивает потрясающую производительность ЦП и ГП, длительное время автономной работы и невероятную графику в одном компактном корпусе.",
    "image": "/images/macbookair.jpg",
    "price": 109990
  },
  {
    "id": 3,
    "name": "Планшет Apple iPad Pro",
    "category": "Планшеты",
    "description": "Новый iPad Pro с процессором M1, поддержкой 5G и Liquid Retina XDR-дисплеем. Улучшенная камера с возможностью съемки в формате Apple ProRAW и сканер LiDAR для AR-приложений.",
    "image": "/images/ipadpro.jpg",
    "price": 99990
  },
  {
    "id": 4,
    "name": "Умные часы Apple Watch Series 6",
    "category": "Умные часы",
    "description": "Apple Watch Series 6 с датчиком кислорода в крови и Always-On Retina-дисплеем. Можно сделать ЭКГ прямо с руки, отслеживать тренировки и получать уведомления.",
    "image": "/images/applewatch.jpg",
    "price": 49990
  },
  {
    "id": 5,
    "name": "Наушники Apple AirPods Max",
    "category": "Наушники",
    "description": "Новые наушники AirPods Max с акустической системой высшего качества и возможностью активного шумоподавления. Невероятно комфортные наушники с качественной звуковой подачей.",
    "image": "/images/airpodsmax.jpg",
    "price": 79990
  }
]

```

`helpers.js`

```js
const Handlebars = require('handlebars');

// Хелпер для форматирования цены
Handlebars.registerHelper('formatPrice', function (price) {
  return new Intl.NumberFormat('ru-RU', { style: 'currency', currency: 'RUB' }).format(price);
});

// Хелпер для создания списка категорий
Handlebars.registerHelper('categoryList', function (categories, selected) {
  let html = '<option value="">Все категории</option>';
  categories.forEach((category) => {
    html += `<option value="${category}"${category === selected ? ' selected' : ''}>${category}</option>`;
  });
  return html;
});

// Хелпер для выделения выбранного значения в селекте
Handlebars.registerHelper('isSelected', function (value, selected) {
  return value === selected ? ' selected' : '';
});

```

`package.json`

```json
{
  "name": "my-app",
  "version": "1.0.0",
  "description": "Мое приложение",
  "main": "index.js",
  "scripts": {
    "start": "node index.js",
    "dev": "nodemon index.js"
  },
  "dependencies": {
    "express": "^4.17.1",
    "handlebars": "^4.7.7"
  },
  "devDependencies": {
    "nodemon": "^2.0.13"
  }
}

```

Этот файл описывает информацию о проекте, его зависимости и скрипты, которые можно запустить из командной строки с помощью `npm run` Здесь name и version указывают на имя и версию проекта соответственно, а description содержит краткое описание проекта.

`main` указывает на главный файл приложения, который будет запускаться при старте сервера. \
`scripts` позволяет определить команды для запуска приложения или различных скриптов.

`dependencies` содержит список зависимостей, необходимых для работы приложения.\
Мы используем фреймворк `Express` и шаблонизатор `Handlebars`.

`devDependencies` содержит список зависимостей, необходимых только во время разработки
`Nodemon` для автоматической перезагрузки сервера при изменениях в коде.


`catalog.handlebars`

```html
{{#layout 'main'}}
  {{#contentFor 'title'}}Каталог{{/contentFor}}
  <h1>Каталог товаров</h1>
  <form class="form-inline mb-3" method="get" action="/catalog">
    <div class="form-group mr-3">
      <label for="category">Категория:</label>
      <select id="category" name="category" class="form-control">
        {{{categoryList categories category}}}
      </select>
    </div>
    <div class="form-group mr-3">
      <label for="minPrice">Минимальная цена:</label>
      <input type="number" id="minPrice" name="minPrice" class="form-control" value="{{minPrice}}">
    </div>
    <div class="form-group mr-3">
      <label for="maxPrice">Максимальная цена:</label>
      <input type="number" id="maxPrice" name="maxPrice" class="form-control" value="{{maxPrice}}">
    </div>
    <button type="submit" class="btn btn-primary">Применить</button>
  </form>
  <div class="row">
    {{#each products}}
      <div class="col-md-4 mb-4">
        <div class="card">
          <img src="{{image}}" class="card-img-top" alt="{{name}}">
          <div class="card-body">
            <h5 class="card-title">{{name}}</h5>
            <p class="card-text">{{description}}</p>
            <p class="card-text">{{formatPrice price}}</p>
          </div>
        </div>
      </div>
    {{/each}}
  </div>
{{/layout}}

```

`main.handlebars`

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>{{{title}}}</title>
    <link rel="stylesheet" href="/css/bootstrap.min.css">
  </head>
  <body>
    <nav class="navbar navbar-expand-lg navbar-dark bg-dark mb-3">
      <div class="container">
        <a class="navbar-brand" href="/">Мой магазин</a>
        <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarNav" aria-controls="navbarNav" aria-expanded="false" aria-label="Toggle navigation">
          <span class="navbar-toggler-icon"></span>
        </button>
        <div class="collapse navbar-collapse" id="navbarNav">
          <ul class="navbar-nav">
            <li class="nav-item">
              <a class="nav-link" href="/">Главная</a>
            </li>
            <li class="nav-item">
              <a class="nav-link" href="/catalog">Каталог</a>
            </li>
            <li class="nav-item">
              <a class="nav-link" href="/cart">Корзина</a>
            </li>
            <li class="nav-item">
              <a class="nav-link" href="/about">О нас</a>
            </li>
            <li class="nav-item">
              <a class="nav-link" href="/contacts">Контакты</a>
            </li>
          </ul>
        </div>
      </div>
    </nav>
    <div class="container">
      {{> alerts}}
      {{> yield}}
    </div>
    <script src="/js/jquery.min.js"></script>
    <script src="/js/bootstrap.min.js"></script>
  </body>
</html>

```

`footer.handlebars`

```html
<footer class="bg-light py-3">
  <div class="container">
    <p>© 2023 Мой магазин</p>
  </div>
</footer>

```

`header.handlebars`

```html
<header class="bg-white py-3">
  <div class="container">
    <div class="row align-items-center">
      <div class="col-md-3">
        <a class="navbar-brand" href="/">Мой магазин</a>
      </div>
      <div class="col-md-6">
        <form class="form-inline my-2 my-lg-0">
          <input class="form-control mr-sm-2" type="search" placeholder="Поиск товаров" aria-label="Поиск">
          <button class="btn btn-outline-primary my-2 my-sm-0" type="submit">Найти</button>
        </form>
      </div>
      <div class="col-md-3">
        <a href="/cart" class="btn btn-outline-secondary mr-3"><i class="fa fa-shopping-cart"></i> Корзина</a>
        <a href="/login" class="btn btn-primary"><i class="fa fa-user"></i> Войти</a>
      </div>
    </div>
  </div>
</header>

```
____________
____________
____________



________________________
# 15 MySql part1
# Домашнее задание:

## Задача 1

Создайте таблицу `users`

### Решение

```sql
CREATE TABLE users (
    id INT(11) NOT NULL AUTO_INCREMENT,
    name VARCHAR(50) NOT NULL,
    email VARCHAR(50) NOT NULL,
    age INT(11) NOT NULL,
    country VARCHAR(50) NOT NULL,
    balance FLOAT NOT NULL,
    PRIMARY KEY (id)
    );
```

## Задача 2

Сделайте выборку данных из таблицы `users`

### Решение

```sql
SELECT * FROM users;
```

## Задача 3

Добавьте строки в таблицу `users`

### Решение

```sql
INSERT INTO users (name, email, age , country, balance) VALUES
    ('Gleb', 'gleb@exmple.com', 26, 'RUS', 90000.00),
    ('Sergey', 'sergey@example.com', 28, 'RUS', 90000.00),
    ('Andrew', 'andrew@example.com', 24, 'RUS', 95000.00),
    ('Bob', 'bob@example.com', 25, 'USA', 100000.00),
    ('Tom', 'tom@example.com', 29, 'USA', 110000.00);
```

## Задача 4

Обновите данные в таблице `users`

### Решение

```sql
UPDATE users SET name = 'Sergey' WHERE id = 3;
```
____________
____________
____________



________________________
# 16 MySql part2
# Домашнее задание:

## Задача 1

Создайте таблицу `orders` (таблицы `users` и `orders` должны быть взаимосвязаны)

### Решение

```sql
CREATE TABLE orders (
    id INT(11) NOT NULL AUTO_INCREMENT,
    user_id INT(11) NOT NULL,
    product_name VARCHAR(50) NOT NULL,
    price FLOAT NOT NULL,
    quantity INT(11) NOT NULL,
    order_date DATE NOT NULL,
    PRIMARY KEY (id),
    FOREIGN KEY (user_id) REFERENCES users(id)
    );
```

## Задача 2

Добавьте строки в таблицу `orders`

### Решение

```sql
INSERT INTO orders (user_id, product_name, price, quantity, order_date) VALUES
    (2, 'Product B', 20.00, 1, '2023-02-20'),
    (2, 'Product C', 15.00, 3, '2023-02-23'),
    (1, 'Product D', 12.00, 2, '2023-02-25'),
    (2, 'Product E', 25.00, 1, '2023-02-26');
```

## Задача 3

Сделайте выборку используя `LEFT JOIN`

### Решение

```sql
SELECT users.name, users.email, orders.product_name, orders.order_date
    FROM users
    LEFT JOIN orders
    ON users.id = orders.user_id;
```

## Задача 4

Сделайте выборку используя `INNER JOIN`

```sql
SELECT users.name, users.email, orders.product_name, orders.order_date
    FROM users
    INNER JOIN orders
    ON users.id = orders.user_id;
```


____________
____________
____________



________________________
# 17 app $$ test part1
# Домашнее задание

# Задача 1

Создайте модель user

## Решение

```js
const db = require('../config/db');
const bcrypt = require('bcryptjs');

// Создание пользователя
exports.createUser = async (username, email, password) => {
  const hashedPassword = await bcrypt.hash(password, 10);

  return new Promise((resolve, reject) => {
    db.query(
      'INSERT INTO users (username, email, password) VALUES (?, ?, ?)',
      [username, email, hashedPassword],
      (err, results) => {
        if (err) {
          return reject(err);
        }
        resolve(results.insertId);
      }
    );
  });
};


// Получение пользователя по ID
exports.getUserById = (userId) => {
  return new Promise((resolve, reject) => {
    db.query('SELECT * FROM users WHERE id = ?', [userId], (err, results) => {
      if (err) {
        return reject(err);
      }
      const user = results[0];
      if (user) {
        delete user.password;
      }
      resolve(user);
    });
  });
};

// Получение пользователя по email
exports.getUserByEmail = (email) => {
  return new Promise((resolve, reject) => {
    db.query('SELECT * FROM users WHERE email = ?', [email], (err, results) => {
      if (err) {
        return reject(err);
      }
      const user = results[0];
      if (user) {
        delete user.password;
      }
      resolve(user);
    });
  });
};



// Аутентификация пользователя
exports.authenticateUser = (email, password) => {
  return new Promise((resolve, reject) => {
    db.query('SELECT * FROM users WHERE email = ?', [email], (err, results) => {
      if (err) {
        return reject(err);
      }

      if (results.length === 0) {
        return resolve(null);
      }

      const user = results[0];
      bcrypt.compare(password, user.password, (err, passwordMatches) => {
        if (err) {
          return reject(err);
        }

        if (!passwordMatches) {
          return resolve(null);
        }

        resolve(user);
      });
    });
  });
};
```

# Задача 2

Создайте тест модели user

## Решение

```js
const request = require('supertest');
const expect = require('chai').expect;
const { app, startServer, stopServer } = require('../app');
const userRoutes = require('../routes/userRoutes');

const TIMEOUT = 10000; // 10 seconds

describe('User API tests', function () {
  this.timeout(TIMEOUT);

  before(function (done) {
    startServer();
    done();
  });

  after(function (done) {
    stopServer();
    done();
  });

  describe('POST /api/users/register', function () {
    this.timeout(TIMEOUT);

    it('should register a new user', async () => {
      const newUser = {
        username: 'TestUser',
        email: 'testuser@example.com',
        password: 'testpassword',
      };

      const response = await request(app)
        .post('/api/users/register')
        .send(newUser);

      expect(response.status).to.equal(201);
      expect(response.body.message).to.equal('Пользователь успешно создан'); // Исправлено на русский язык
      expect(response.body.userId).to.be.a('number');
    });
  });

  describe('GET /api/users/:id', function () {
    this.timeout(TIMEOUT);

    let userId;
    before(async () => {
      const newUser = {
        username: 'TestUser',
        email: 'testuser@example.com',
        password: 'testpassword',
      };

      const response = await request(app)
        .post('/api/users/register')
        .send(newUser);

      userId = response.body.userId;
    });

    it('should get a user by id', async () => {
      const response = await request(app).get(`/api/users/${userId}`);

      expect(response.status).to.equal(200);
      expect(response.body.id).to.equal(userId);
      expect(response.body.username).to.equal('TestUser');
      expect(response.body.email).to.equal('testuser@example.com');
    });

    it('should return 404 when user not found', async () => {
      const nonExistentUserId = 999;
      const response = await request(app).get(`/api/users/${nonExistentUserId}`);

      expect(response.status).to.equal(404);
      expect(response.body.message).to.equal('User not found');
    });
  });
});

```


добавим тест `user.test.js` в `package.json`


```json
{
  
 "scripts": {
    "start": "node app.js",
    "dev": "nodemon app.js",
    "test-db": "mocha config/db.test.js",
    "test-model-post": "mocha models/post.test.js",
    "test-model-user": "mocha models/user.test.js"
  }
  
}
```

запустим тест

```
npm run test-model-user
```



____________
____________
____________



________________________
# 18 app $$ test part2
# Домашнее задание 

## Задача 1. 

создайте роутер postRouter.js

```js
const express = require('express');
const postController = require('../controllers/postController');

const router = express.Router();

// Получение списка всех постов
router.get('/', postController.getAllPosts);

// Получение поста по ID
router.get('/:id', postController.getPostById);

// Создание нового поста
router.post('/', postController.createPost);

// Обновление поста
router.put('/update-post', postController.updatePost);

// Удаление поста
router.delete('/delete-post', postController.deletePost);

module.exports = router;


```
____________
____________
____________



________________________
# 19 app $$ test part3
____________________________
____________________________
## Домашнее задание

## Задача 1

Напишите тест postController.test.js

## решение

```js
const chai = require('chai');
const sinon = require('sinon');
const postController = require('../controllers/postController');
const postModel = require('../models/post');

const { expect } = chai;

describe('Post Controller', () => {
  afterEach(() => {
    sinon.restore();
  });

  it('should get all posts', async () => {
    const posts = [
      { id: 1, title: 'Post 1', content: 'Content 1' },
      { id: 2, title: 'Post 2', content: 'Content 2' },
    ];

    const req = {};
    const res = {
      status: sinon.stub().returnsThis(),
      json: sinon.stub(),
    };
    const next = sinon.stub();

    sinon.stub(postModel, 'getAllPosts').resolves(posts);

    await postController.getAllPosts(req, res, next);

    expect(res.status.calledOnceWith(200)).to.be.true;
    expect(res.json.calledOnceWith(posts)).to.be.true;
    expect(next.notCalled).to.be.true;
  });

  it('should get a post by id', async () => {
  const post = { id: 1, title: 'Post 1', content: 'Content 1' };

  const req = {
    params: {
      id: post.id,
    },
  };
  const res = {
    status: sinon.stub().returnsThis(),
    json: sinon.stub(),
  };
  const next = sinon.stub();

  sinon.stub(postModel, 'getPostById').resolves(post);

  await postController.getPostById(req, res, next);

  expect(res.status.calledOnceWith(200)).to.be.true;
  expect(res.json.calledOnceWith(post)).to.be.true;
  expect(next.notCalled).to.be.true;
});

it('should return 404 when post not found by id', async () => {
  const req = {
    params: {
      id: 1,
    },
  };
  const res = {
    status: sinon.stub().returnsThis(),
    json: sinon.stub(),
  };
  const next = sinon.stub();

  sinon.stub(postModel, 'getPostById').resolves(null);

  await postController.getPostById(req, res, next);

  expect(res.status.calledOnceWith(404)).to.be.true;
  expect(res.json.calledOnceWith({ message: 'Post not found' })).to.be.true;
  expect(next.notCalled).to.be.true;
});

it('should create a new post', async () => {
  const post = { title: 'New Post', content: 'New Content' };
  const postId = 1;

  const req = {
    body: post,
  };
  const res = {
    status: sinon.stub().returnsThis(),
    json: sinon.stub(),
  };
  const next = sinon.stub();

  sinon.stub(postModel, 'createPost').resolves(postId);

  await postController.createPost(req, res, next);

  expect(res.status.calledOnceWith(201)).to.be.true;
  expect(res.json.calledOnceWith({ message: 'Post created', postId })).to.be.true;
  expect(next.notCalled).to.be.true;
});

it('should update a post', async () => {
  const postId = 1;
  const updatedPost = { title: 'Updated Post', content: 'Updated Content' };

  const req = {
    params: {
      id: postId,
    },
    body: updatedPost,
  };
  const res = {
    status: sinon.stub().returnsThis(),
    json: sinon.stub(),
  };
  const next = sinon.stub();

  sinon.stub(postModel, 'updatePost').resolves(1);

  await postController.updatePost(req, res, next);

  expect(res.status.calledOnceWith(200)).to.be.true;
  expect(res.json.calledOnceWith({ message: 'Post updated', affectedRows: 1 })).to.be.true;
  expect(next.notCalled).to.be.true;
});

  it('should return 404 when updating a non-existent post', async () => {
  const postId = 1;
  const updatedPost = { title: 'Updated Post', content: 'Updated Content' };

  const req = {
    params: {
      id: postId,
    },
    body: updatedPost,
  };
  const res = {
    status: sinon.stub().returnsThis(),
    json: sinon.stub(),
  };
  const next = sinon.stub();

  sinon.stub(postModel, 'updatePost').resolves(0);

  await postController.updatePost(req, res, next);

  expect(res.status.calledOnceWith(404)).to.be.true;
  expect(res.json.calledOnceWith({ message: 'Post not found' })).to.be.true;
  expect(next.notCalled).to.be.true;
});

it('should delete a post', async () => {
  const postId = 1;

  const req = {
    params: {
      id: postId,
    },
  };
  const res = {
    status: sinon.stub().returnsThis(),
    json: sinon.stub(),
  };
  const next = sinon.stub();

  sinon.stub(postModel, 'deletePost').resolves(1);

  await postController.deletePost(req, res, next);

  expect(res.status.calledOnceWith(200)).to.be.true;
  expect(res.json.calledOnceWith({ message: 'Post deleted', affectedRows: 1 })).to.be.true;
  expect(next.notCalled).to.be.true;
});

it('should return 404 when deleting a non-existent post', async () => {
  const postId = 1;

  const req = {
    params: {
      id: postId,
    },
  };
  const res = {
    status: sinon.stub().returnsThis(),
    json: sinon.stub(),
  };
  const next = sinon.stub();

  sinon.stub(postModel, 'deletePost').resolves(0);

  await postController.deletePost(req, res, next);

  expect(res.status.calledOnceWith(404)).to.be.true;
  expect(res.json.calledOnceWith({ message: 'Post not found' })).to.be.true;
  expect(next.notCalled).to.be.true;
});
});

```

## Задача 2

Напшем тест postRouter.test.js

## решение

```js
const request = require('supertest');
const expect = require('chai').expect;
const { app, startServer, stopServer } = require('../app');
const postRoutes = require('../routes/postRoutes');

const TIMEOUT = 10000; // 10 seconds

describe('API tests', function () {
  this.timeout(TIMEOUT);

  before(function (done) {
    startServer();
    done();
  });

  after(function (done) {
    stopServer();
    done();
  });
  // app.use('/api/posts', postRoutes);

  describe('POST /api/posts', function () {
    this.timeout(TIMEOUT);

    it('should create a new post and return its id', async () => {
      const response = await request(app)
          .post('/api/posts')
          .send({title: 'Test Post', content: 'Test Content'});

      expect(response.status).to.equal(201);
      expect(response.body.message).to.equal('Post created');
      expect(response.body.postId).to.be.a('number');
    });
  });

  describe('GET /api/posts', function () {
    this.timeout(TIMEOUT);
    it('should return a list of posts', async () => {
      const response = await request(app).get('/api/posts');

      expect(response.status).to.equal(200);
      expect(response.body).to.be.an('array');
    });
  });

  describe('GET /api/posts/:id', function () {
  this.timeout(TIMEOUT);

  let postId;
  before(async () => {
    const response = await request(app)
      .post('/api/posts')
      .send({ title: 'Test Post', content: 'Test Content' });

    postId = response.body.postId;
  });

  it('should return a post by id', async () => {
    const response = await request(app).get(`/api/posts/${postId}`);

    expect(response.status).to.equal(200);
    expect(response.body.id).to.equal(postId);
  });

    it('should return 404 when post not found', async () => {
      const postId = 999;
      const response = await request(app).get(`/api/posts/${postId}`);

      expect(response.status).to.equal(404);
      expect(response.body.message).to.equal('Post not found');
    });
  });

  describe('PUT /api/posts/:id', function () {
  this.timeout(TIMEOUT);

  let postId;
  before(async () => {
    const response = await request(app)
      .post('/api/posts')
      .send({ title: 'Test Post', content: 'Test Content' });

    postId = response.body.postId;
  });

  it('should update a post and return the number of affected rows', async () => {
    const updatedPost = {title: 'Updated Post', content: 'Updated Content'};
    const response = await request(app)
      .put(`/api/posts/${postId}`)
      .send(updatedPost);

    expect(response.status).to.equal(200);
    expect(response.body.message).to.equal('Post updated');
    expect(response.body.affectedRows).to.equal(1);
  });

    it('should return 404 when updating a non-existent post', async () => {
      const postId = 999;
      const updatedPost = {title: 'Updated Post', content: 'Updated Content'};
      const response = await request(app)
          .put(`/api/posts/${postId}`)
          .send(updatedPost);

      expect(response.status).to.equal(404);
      expect(response.body.message).to.equal('Post not found');
    });
  });

  describe('DELETE /api/posts/:id', function () {
  this.timeout(TIMEOUT);

  let postId;
  before(async () => {
    const response = await request(app)
      .post('/api/posts')
      .send({ title: 'Test Post', content: 'Test Content' });

    postId = response.body.postId;
  });

  it('should delete a post and return the number of affected rows', async () => {
    const response = await request(app).delete(`/api/posts/${postId}`);

    expect(response.status).to.equal(200);
    expect(response.body.message).to.equal('Post deleted');
    expect(response.body.affectedRows).to.equal(1);
  });

    it('should return 404 when deleting a non-existent post', async () => {
      const postId = 999;
      const response = await request(app).delete(`/api/posts/${postId}`);
      expect(response.status).to.equal(404);
      expect(response.body.message).to.equal('Post not found');
    });
  });
});


```



____________
____________
____________



________________________
# 20 app $$ test part4
_____________
_____________
_____________
# Домашнее задание


# Задача 1

`Post.js`, `Post.css`: компонент отдельного поста и его стили.\


```js
import React, { useState, useEffect } from 'react';
import { useParams, useNavigate } from 'react-router-dom';
import './Post.css';

const Post = () => {
  const [post, setPost] = useState(null);
  const { postId } = useParams();
  const navigate = useNavigate();

  useEffect(() => {
    const fetchPost = async () => {
      try {
        const response = await fetch(`http://localhost:5000/api/posts/${postId}`);
        const data = await response.json();

        if (response.ok) {
          setPost(data.post);
        } else {
          alert(`Ошибка загрузки поста: ${data.message}`);
        }
      } catch (error) {
        console.error('Ошибка при загрузке поста:', error);
        alert('Ошибка загрузки поста. Пожалуйста, попробуйте еще раз.');
      }
    };

    fetchPost();
  }, [postId]);

  if (!post) {
    return <div>Загрузка поста...</div>;
  }

  return (
    <div className="post">
      <h2 className="post__title">{post.title}</h2>
      <p className="post__content">{post.content}</p>
      <button className="post__back-btn" onClick={() => navigate(-1)}>Назад</button>
    </div>
  );
};

export default Post;

```

```css
.post {
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 100%;
  max-width: 800px;
  margin: 0 auto;
}

.post__title {
  margin: 0 0 1rem;
}

.post__content {
  margin: 0 0 1rem;
  text-align: justify;
}

.post__back-btn {
  padding: 0.5rem;
  font-size: 1rem;
  background-color: #ccc;
  color: #000;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.2s;
}

.post__back-btn:hover {
  background-color: #999;
}

```
____________
____________
____________



________________________
# 21 app $$ test part5
_______________
_______________

# Домашнее задание

Запустите клинет на 3000 порту и сервер на 5000 порту