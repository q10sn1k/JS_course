# Условный рендеринг в Vue

# Показ по условию в Vue
Директива v-if позволяет показывать или скрывать элементы на странице в зависимости от значения определенного свойства объекта data. \
Если свойство имеет значение true, элемент будет показан, а если false, то скрыт.


рассмотрим пример:
```vue
<template>
  <p v-if="visible">text</p>
</template>

```

Здесь мы определяем абзац с атрибутом v-if, который зависит от свойства visible.\
Если значение свойства visible равно true, то абзац будет показан, а если false, то скрыт.

```vue
data() {
  return {
    visible: true,
  }
}
```
Мы определили свойство visible в объекте data с начальным значением true. \
Если мы изменим его значение на false, то абзац будет скрыт:

```vue
data() {
  return {
    visible: false,
  }
}
```

# Инвертирование условия в Vue
Мы можем инвертировать условие с помощью восклицательного знака:

```vue
<template>
  <p v-if="!visible">text</p>
</template>

```

Здесь абзац будет показан, если значение свойства visible равно false и скрыт, если true.

# Реактивное условие в Vue
Мы можем сделать условие реактивным.\
Например, мы можем скрыть элемент по нажатию на кнопку. \
Для этого добавим кнопку и привязанный метод, который будет менять значение свойства visible на false:

```vue
<template>
  <button @click="hide">hide</button>
  <p v-if="visible">text</p>
</template>

<script>
export default {
  data() {
    return {
      visible: true,
    }
  },
  methods: {
    hide: function() {
      this.visible = false;
    }
  }
}
</script>
```

# Тогглинг элементов в Vue
Мы можем сделать кнопку, которая будет тогглить элемент, то есть по первому клику показывать его, а по второму - скрывать. \
Для этого добавим кнопку и метод toggle, который будет инвертировать значение свойства visible:

```vue
<template>
  <button @click="toggle">toggle</button>
  <p v-if="visible">text</p>
</template>
<script>
export default {
  data() {
    return {
      visible: true,
    }
  },
  methods: {
    hide: function() {
      this.visible = false;
    },
    toggle: function() {
      this.visible = !this.visible;
    },
  }
}
</script>
```

Теперь мы создали два метода: hide и toggle. \
Метод hide скрывает элемент, устанавливая значение свойства visible в false. \
Метод toggle инвертирует значение свойства visible, то есть если элемент был показан, то после вызова этого метода он будет скрыт, и наоборот.

Можно заметить, что оба метода изменяют значение свойства visible. \
Это и есть реактивность, которая позволяет автоматически изменять представление (DOM) в соответствии с изменением данных (свойств объекта data).

# Работа с текстом при тогглинге в Vue

Как изменять текст кнопки при тогглинге элементов в Vue?\
Для этого будем использовать тернарный оператор, который будет проверять значение свойства visible и в зависимости от этого менять текст кнопки на "hide" или "show".
```vue
<template>
  <div>
    <button @click="toggle">{{ buttonLabel }}</button>
    <p v-if="visible">This is the element to toggle</p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      visible: true,
      buttonLabel: 'hide',
    };
  },
  methods: {
    toggle() {
      this.visible = !this.visible;
      this.buttonLabel = this.visible ? 'hide' : 'show';
    },
  },
};
</script>
```




Обратите внимание на использование вычисляемого свойства buttonLabel, которое мы изменяем при каждом тогглинге элемента.

В объекте данных, определены свойства visible и buttonLabel. \
Значение свойства visible установлены в true, чтобы элемент был видимым при первом запуске приложения, а свойство buttonLabel зависит от значения visible.


# Директивы v-if и v-else
Директива v-if позволяет отрисовывать элемент только тогда, когда условие возвращает true. \
Если условие возвращает false, то элемент не будет отображен.

```vue
<template>
  <div>
    <p v-if="isAuth">Вы авторизованы!</p>
  </div>
</template>
<script>
export default {
  data() {
    return {
      isAuth: true,
    }
  },
}
</script>

```

В этом примере мы отрисуем элемент \<p>, только если значение свойства isAuth будет true.\

Если нам нужно показать другой элемент в зависимости от того, истинно ли условие, можно использовать директиву v-else:

```vue
<template>
  <div>
    <p v-if="isAuth">Вы авторизованы!</p>
    <p v-else>Авторизуйтесь, пожалуйста!</p>
  </div>
</template>
<script>
export default {
  data() {
    return {
      isAuth: false,
    }
  },
}
</script>

```

Если isAuth будет true, отрисуется первый элемент \<p>.\
Если isAuth будет false, отрисуется второй элемент \<p>.

# Сложные условия в Vue
Директива v-if также может работать с более сложными условиями.\
В этом случае мы можем использовать операторы && и ||, а также условный оператор ?:

```vue
<template>
  <div>
    <p v-if="isAuth && isAdmin">Вы авторизованы и являетесь администратором!</p>
    <p v-else-if="isAuth && !isAdmin">Вы авторизованы, но не являетесь администратором.</p>
    <p v-else>Авторизуйтесь, пожалуйста!</p>
  </div>
</template>
<script>
export default {
  data() {
    return {
      isAuth: true,
      isAdmin: false,
    }
  },
}
</script>

```

# Директива v-else-if

Директива v-else-if в Vue.js позволяет добавлять несколько условий в один блок кода и проверять их последовательно до тех пор, пока не будет найдено подходящее.\
Если ни одно из условий не подходит, то можно использовать блок v-else для выполнения альтернативного действия.

```vue
<template>
  <div>
    <p v-if="score >= 90">Отличный результат!</p>
    <p v-else-if="score >= 60">Хороший результат.</p>
    <p v-else-if="score >= 40">Средний результат.</p>
    <p v-else>Неудовлетворительный результат.</p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      score: 85,
    };
  },
};
</script>

```
______________________________________________
______________________________________________
# Домашнее задание:

### Задача 1 
Создайте компонент Vue, который содержит кнопку и абзац с текстом "Hello, World!". \
По нажатию на кнопку абзац должен скрываться/показываться.\
Текст на кнопке должен меняться в зависимости от того, скрыт ли абзац или нет.

### Задача 2
Создать форму регистрации с проверкой на корректность введенных данных, используя сложные условия и директивы во Vue.
