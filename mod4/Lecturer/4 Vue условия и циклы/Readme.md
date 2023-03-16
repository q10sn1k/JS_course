# 4 Vue условия и циклы
# v-if в цикле v-for
Внутри цикла v-for можно использовать директиву v-if для отображения элементов массива, удовлетворяющих определенному условию.

Допустим, у нас есть массив объектов, каждый из которых представляет собой некоторый продукт со свойствами name и price.\
Мы хотим вывести только те продукты, у которых цена меньше 10:

```vue
<template>
  <div>
    <div v-for="product in products" :key="product.id">
      <div v-if="product.price < 10">
        <h2>{{ product.name }}</h2>
        <p>{{ product.price }}</p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      products: [
        { id: 1, name: 'Product 1', price: 5 },
        { id: 2, name: 'Product 2', price: 12 },
        { id: 3, name: 'Product 3', price: 8 },
        { id: 4, name: 'Product 4', price: 15 },
      ],
    };
  },
};
</script>

```

В этом примере мы использовали директиву v-if внутри цикла v-for для отображения только тех элементов массива products, у которых цена меньше 10.

# v-else в цикле v-for
Также внутри цикла v-for можно использовать директиву v-else для отображения альтернативного содержимого, если условие не выполняется.

Выведем цену в скобках, если она больше или равна 10:

```vue
<template>
  <div>
    <div v-for="product in products" :key="product.id">
      <div v-if="product.price < 10">
        <h2>{{ product.name }}</h2>
        <p>{{ product.price }}</p>
      </div>
      <div v-else>
        <h2>{{ product.name }}</h2>
        <p>{{ product.price }} ({{ product.price - 10 }} more)</p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      products: [
        { id: 1, name: 'Product 1', price: 5 },
        { id: 2, name: 'Product 2', price: 12 },
        { id: 3, name: 'Product 3', price: 8 },
        { id: 4, name: 'Product 4', price: 15 },
      ],
    };
  },
};
</script>

```

# v-show в цикле v-for

Директива v-show может быть использована вместе с циклом v-for для отображения элементов массива, удовлетворяющих определенным условиям.

Для использования директивы v-show в цикле v-for сначала необходимо определить условие, которому должны удовлетворять элементы массива. \
Например, мы можем определить условие, что только элементы, значение которых больше 2, должны быть отображены.

```vue
<template>
  <div>
    <p v-for="elem in arr" v-show="elem > 2">{{ elem }}</p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      arr: [1, 2, 3, 4, 5]
    }
  }
}
</script>

```

В этом примере мы определяем массив arr и используем цикл v-for для отображения каждого элемента массива в отдельном теге p. \
Далее мы добавляем директиву v-show, которая скрывает элементы, значение которых меньше или равно 2.

Таким образом, только элементы 3, 4 и 5 будут отображаться на странице.

Важно помнить, что при использовании директивы v-show элементы остаются в DOM, и это может снижать производительность при работе с большими массивами.

Кроме того, можно использовать условие внутри цикла v-for для изменения класса элемента или его стилей, в зависимости от значения элемента массива.

```vue
<template>
  <div>
    <p v-for="elem in arr" :class="{ 'is-even': elem % 2 === 0 }">{{ elem }}</p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      arr: [1, 2, 3, 4, 5]
    }
  }
}
</script>

<style>
.is-even {
  color: red;
}
</style>

```

В этом примере мы определяем массив arr и используем цикл v-for для отображения каждого элемента массива в отдельном теге p. \
Далее мы добавляем условие внутри цикла v-for, которое добавляет класс is-even к элементу, если его значение является четным. \
В результате четные элементы будут иметь красный цвет, заданный в стилях.

# Атрибут key в Vue

Атрибут key в Vue используется для оптимизации рендеринга компонентов. При обновлении компонента Vue не перерисовывает все элементы, а только те, которые изменились.\
Атрибут key позволяет уникально идентифицировать элементы, что позволяет Vue эффективно обновлять только изменившиеся элементы.

Допустим, у нас есть компонент, который отображает список элементов массива:

```vue
<template>
  <div>
    <ul>
      <li v-for="item in items" :key="item.id">{{ item.name }}</li>
    </ul>
  </div>
</template>

<script>
export default {
  data() {
    return {
      items: [
        { id: 1, name: 'Item 1' },
        { id: 2, name: 'Item 2' },
        { id: 3, name: 'Item 3' },
      ],
    };
  },
};
</script>

```

Здесь мы используем директиву v-for для вывода списка элементов массива items.\
Обратите внимание на атрибут key. Мы устанавливаем его равным id элемента, чтобы Vue мог уникально идентифицировать каждый элемент списка.

Теперь, если мы захотим изменить порядок элементов в массиве, например, поменять местами первый и второй элементы, Vue будет знать, что нужно обновить только эти два элемента, а не весь список.

```vue
<template>
  <div>
    <ul>
      <li v-for="item in items" :key="item.id">{{ item.name }}</li>
    </ul>
    <button @click="swapItems">Swap Items 1 and 2</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      items: [
        { id: 1, name: 'Item 1' },
        { id: 2, name: 'Item 2' },
        { id: 3, name: 'Item 3' },
      ],
    };
  },
  methods: {
    swapItems() {
      const temp = this.items[0];
      this.items[0] = this.items[1];
      this.items[1] = temp;
    },
  },
};
</script>

```

Здесь мы добавили кнопку, которая меняет местами первый и второй элементы в массиве. \
Обратите внимание, что мы не меняем атрибут id элементов, поэтому Vue все еще может идентифицировать каждый элемент по его id. Результатом изменения будет только изменение порядка элементов на странице, а не их полное перерисование.


________________________________
_______________________________
______________________________
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