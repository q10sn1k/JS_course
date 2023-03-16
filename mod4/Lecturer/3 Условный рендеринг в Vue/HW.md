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