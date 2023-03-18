# Расширенная работа с компонентами

Vue позволяет создавать дочерние компоненты, которые могут быть переиспользованы в различных местах приложения.\
Рассмотрим пример создания дочернего компонента.

Для начала создадим файл с именем ChildComponent.vue


```vue
<template>
  <div>
    <h2>Child Component</h2>
    <p>{{ message }}</p>
  </div>
</template>

<script>
export default {
  props: {
    message: {
      type: String,
      default: "Hello from Child Component",
    },
  },
};
</script>

```

Этот компонент имеет шаблон, в котором выводится сообщение, переданное через свойство message. \
Также компонент имеет определение свойств props, которые могут быть переданы в компонент.

Чтобы использовать этот компонент в другом компоненте, нужно импортировать его и добавить его в определение компонента.


```vue
<template>
  <div>
    <h1>Parent Component</h1>
    <ChildComponent message="Hello from Parent Component" />
  </div>
</template>

<script>
import ChildComponent from "/components/ChildComponent.vue";

export default {
  components: {
    ChildComponent,
  },
};
</script>

```

Этот компонент содержит родительский компонент, который импортирует дочерний компонент и добавляет его в определение компонента. \
Кроме того, родительский компонент передает свойство message в дочерний компонент.

Мы создали дочерний компонент и использовали его в родительском компоненте, передавая свойства и получая данные из дочернего компонента.


### Пропсы
Пропсы (props) в Vue - это механизм передачи данных от родительского компонента к дочернему компоненту. \
Они позволяют создавать переиспользуемые компоненты и делать код более модульным.

Для передачи пропсов из родительского компонента в дочерний, необходимо определить их список в настройках дочернего компонента в свойстве props.\
Например, для передачи имени пользователя и его возраста, определим компонент User со свойством props:

```vue
<template>
  <div>
    <h2>{{ name }}</h2>
    <p>Age: {{ age }}</p>
  </div>
</template>

<script>
  export default {
    props: {
      name: String,
      age: Number
    }
  }
</script>

```

Теперь можно передать пропсы при использовании компонента User в родительском компоненте, используя атрибуты в теге компонента:

```vue
<template>
  <div>
    <User name="Ivan" age="30" />
  </div>
</template>

<script>
import User from './components/User.vue'

export default {
  components: {
    User
  }
}
</script>

```

При передаче пропсов через атрибуты, значения передаются как строки, поэтому для правильной работы с числами или булевыми значениями, необходимо использовать соответствующие преобразования типов.

Передача свойств объекта data в дочерний компонент происходит через пропсы.\
Например, если необходимо передать значение из свойства data родительского компонента в свойство props дочернего компонента, можно использовать следующий синтаксис:


```vue
<template>
  <div>
    <User :name="username" />
  </div>
</template>

<script>
  import User from './components/User.vue'

  export default {
    data() {
      return {
        username: 'Ivan'
      }
    },
    components: {
      User
    }
  }
</script>

```


Мы передаем значение свойства username родительского компонента в свойство name дочернего компонента User. \
Для передачи значения используется директива v-bind или ее сокращенная форма : перед именем пропса.

В самом дочернем компоненте User свойство name будет доступно через props и может использоваться в представлении компонента:

```vue
<template>
  <div>
    <h2>{{ name }}</h2>
  </div>
</template>

<script>
  export default {
    props: {
      name: String
    }
  }
</script>

```


Также можно передавать в компоненты не только отдельные свойства объекта data, но и сам объект целиком. \
Для этого нужно использовать синтаксис v-bind и передать объект в компонент:

```vue
<template>
  <child-component v-bind:dataObject="myDataObject"></child-component>
</template>

<script>
export default {
  data() {
    return {
      myDataObject: {
        prop1: 'value1',
        prop2: 'value2'
      }
    }
  }
}
</script>

```
Затем в компоненте необходимо определить проп dataObject и использовать его в шаблоне:
```vue
<template>
  <div>
    <p>{{ dataObject.prop1 }}</p>
    <p>{{ dataObject.prop2 }}</p>
  </div>
</template>

<script>
export default {
  props: {
    dataObject: {
      type: Object,
      required: true
    }
  }
}
</script>

```

Для упрощения кода можно использовать деструктуризацию объекта для получения доступа к его свойствам в компоненте:

```vue
<template>
  <div>
    <p>{{ prop1 }}</p>
    <p>{{ prop2 }}</p>
  </div>
</template>

<script>
export default {
  props: {
    dataObject: {
      type: Object,
      required: true
    }
  },
  computed: {
    prop1() {
      return this.dataObject.prop1
    },
    prop2() {
      return this.dataObject.prop2
    }
  }
}
</script>

```
## Передача данных в компоненты

Передача данных в компоненты - одна из основных возможностей фреймворка Vue. \
Рассмотрим передачу данных из родительского компонента в дочерний компонент.

### передача строки

В родительском компоненте:

```vue
<template>
  <div>
    <child-component message="Hello, world!"></child-component>
  </div>
</template>

<script>
import ChildComponent from "./ChildComponent.vue";

export default {
  components: {
    ChildComponent
  }
};
</script>

```

В дочернем компоненте:

```vue
<template>
  <div>
    <p>{{ message }}</p>
  </div>
</template>

<script>
export default {
  props: {
    message: String
  }
};
</script>

```

### передача числа

В родительском компоненте:

```vue
<template>
  <div>
    <child-component :number-prop="42"></child-component>
  </div>
</template>

<script>
import ChildComponent from "./ChildComponent.vue";

export default {
  components: {
    ChildComponent
  }
};
</script>

```
В дочернем компоненте:

```vue
<template>
  <div>
    <p>{{ numberProp }}</p>
  </div>
</template>

<script>
export default {
  props: {
    numberProp: Number
  }
};
</script>

```

### Передача массива

В родительском компоненте:

```vue
<template>
  <div>
    <child-component :items="['apple', 'banana', 'cherry']"></child-component>
  </div>
</template>

<script>
import ChildComponent from "./ChildComponent.vue";

export default {
  components: {
    ChildComponent
  }
};
</script>

```

В дочернем компоненте:

```vue
<template>
  <div>
    <ul>
      <li v-for="(item, index) in items" :key="index">{{ item }}</li>
    </ul>
  </div>
</template>

<script>
export default {
  props: {
    items: Array
  }
};
</script>

```


###  передача объекта

В родительском компоненте:

```vue
<template>
  <div>
    <child-component :user="{ name: 'Ivan', age: 30 }"></child-component>
  </div>
</template>

<script>
import ChildComponent from "./ChildComponent.vue";

export default {
  components: {
    ChildComponent
  }
};
</script>

```

В дочернем компоненте:

```vue
<template>
  <div>
    <p>Name: {{ user.name }}</p>
    <p>Age: {{ user.age }}</p>
  </div>
</template>

<script>
export default {
  props: {
    user: Object
  }
};
</script>

```

_____________________
____________________
___________________
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