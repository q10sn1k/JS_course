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