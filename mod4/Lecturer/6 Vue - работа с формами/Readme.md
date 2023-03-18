# Двусторонняя привязка данных к инпутам в Vue:

Vue - это фреймворк JavaScript для создания интерфейсов, который позволяет связывать данные и элементы интерфейса, такие как инпуты.\
Для связывания данных и инпутов в Vue используется директива v-model.

```vue
<template>
  <div>
    <input v-model="message" type="text">
    <p>{{ message }}</p>
  </div>
</template>
<script>
export default {
  data() {
    return {
      message: ''
    }
  }
}
</script>
```

Мы создаем инпут и связываем его с данными, используя директиву v-model.\
Когда пользователь вводит данные в инпут, связанные данные также обновляются и отображаются в абзаце.

# Получение данных формы по событию в Vue:

В Vue данные формы могут быть получены по событию, например, когда пользователь нажимает кнопку. \
Для этого используется директива v-on:click.

```vue
<template>
  <div>
    <input v-model="num" type="number">
    <button v-on:click="calc">work</button>
    <p>{{ res }}</p>
  </div>
</template>
<script>
export default {
  data() {
    return {
      num: 0,
      res: 0
    }
  },
  methods: {
    calc: function() {
      this.res = this.num ** 2;
    }
  }
}
</script>
```

Мы создаем инпут, кнопку и абзац; связываем инпут с данными, используя директиву v-model. \
Когда пользователь нажимает на кнопку, запускается метод calc, который вычисляет квадрат введенного числа и записывает его в свойство res. \
Значение свойства res также отображается в абзаце.

# Работа с textarea в Vue:

Textarea - это элемент формы, который позволяет пользователю вводить многострочный текст.\
В Vue данные textarea могут быть связаны с помощью директивы v-model также, как и с инпутами.

```vue
<template>
  <div>
    <textarea v-model="text"></textarea>
    <p>{{ text }}</p>
  </div>
</template>
<script>
export default {
  data() {
    return {
      text: ''
    }
  }
}
</script>
```

Создаем textarea и связываем его с данными, используя директиву v-model.\
Когда пользователь вводит текст в textarea, связанные данные также обновляются и отображаются в абзаце.

# Работа с чекбоксами в Vue:

Чекбоксы - это элементы формы, которые позволяют пользователю выбрать одно или несколько значений из заданного списка.\
В Vue значения чекбоксов могут быть связаны с помощью директивы v-model.

```vue
<template>
  <div>
    <input type="checkbox" v-model="checked">
    <p>{{ checked ? 'yes' : 'no' }}</p>
  </div>
</template>
<script>
export default {
  data() {
    return {
      checked: false
    }
  }
}
</script>
```

Создаем чекбокс и связываем его значение с данными, используя директиву v-model.\
Когда пользователь отмечает или снимает отметку с чекбокса, связанные данные также обновляются.\
Мы используем тернарный оператор, чтобы отобразить текст "yes" или "no" в зависимости от значения связанных данных.

# Массив значений чекбоксов в Vue:

Группа чекбоксов - это несколько чекбоксов, которые имеют общее свойство, такое как название или значение.\
В Vue значения отмеченных чекбоксов могут быть связаны с массивом данных.

```vue
<template>
  <div>
    <input type="checkbox" v-model="arr" value="v1">
    <input type="checkbox" v-model="arr" value="v2">
    <input type="checkbox" v-model="arr" value="v3">
    <p>Selected values: {{ arr }}</p>
  </div>
</template>
<script>
export default {
  data() {
    return {
      arr: []
    }
  }
}
</script>
```

Создаем группу чекбоксов и связываем их значения с массивом данных, используя директиву v-model и атрибут value.\
Когда пользователь отмечает или снимает отметку с чекбокса, значение атрибута value добавляется или удаляется из массива данных. Значение массива данных также отображается в абзаце.


# Работа с радиокнопками в Vue

Радиокнопки - это элементы формы, которые позволяют пользователю выбрать один из нескольких взаимоисключающих вариантов.\
В Vue работа с радиокнопками происходит также, как и с чекбоксами, через директиву v-model.

```vue
<template>
  <div>
    <input type="radio" id="male" value="male" v-model="gender">
    <label for="male">М</label><br>
    <input type="radio" id="female" value="female" v-model="gender">
    <label for="female">Ж</label><br>
    <p>Вы выбрали: {{ gender }}</p>
  </div>
</template>
<script>
export default {
  data() {
    return {
      gender: ''
    }
  }
}
</script>
```

# Работа с селектами в Vue:

Селект - это элемент формы, который предоставляет пользователю список вариантов для выбора. \
В Vue для работы со списками используется директива v-model, которая связывает выбранный вариант с определенным свойством данных в экземпляре Vue.

```vue
<template>
  <div>
    <label for="fruit-select">Выберете фрукт:</label>
    <select id="fruit-select" v-model="selectedFruit">
      <option value="">________</option>
      <option value="apple">Яблоко</option>
      <option value="banana">Банан</option>
      <option value="orange">Апельсин</option>
    </select>
    <p>Вы выбрали: {{ selectedFruit }}</p>
  </div>
</template>
<script>
export default {
  data() {
    return {
      selectedFruit: ''
    }
  }
}
</script>
```

# Блокировка элементов в Vue:

Если возникает необходимость временно блокировать элементы формы, например, чтобы предотвратить повторное нажатие на кнопку до завершения операции или чтобы защитить форму от изменения до ее сохранения, то в Vue для блокировки элементов формы используется атрибут disabled.

```vue
<template>
  <div>
    <button v-on:click="submitForm" :disabled="isDisabled">Submit</button>
  </div>
</template>
<script>
export default {
  data() {
    return {
      isDisabled: true
    }
  },
  methods: {
    submitForm() {
      // отправка формы 
      this.isDisabled = true // блокируем кнопку после отправки формы
    }
  }
}
</script>
```

Можно использовать вычисляемые свойства для управления состоянием элементов формы, для проверки корректности заполнения формы.

Например используем вычисляемое свойство для блокировки кнопки, если поле ввода пустое:

```vue
<template>
  <div>
    <input type="text" v-model="inputValue">
    <button :disabled="isDisabled">Submit</button>
  </div>
</template>
<script>
export default {
  data() {
    return {
      inputValue: ''
    }
  },
  computed: {
    isDisabled() {
      return this.inputValue === ''
    }
  }
}
</script>
```
________________________________
_______________________________
______________________________
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