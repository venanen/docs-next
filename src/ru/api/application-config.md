# Конфигурация приложения

`config` — объект, содержащий глобальные параметры Vue. Эти свойства, описанные ниже, можно изменять перед загрузкой приложения:

```js
const app = Vue.createApp({})

app.config = {...}

app.mount(...)
```

## errorHandler

- **Тип:** `Function`

- **По умолчанию:** `undefined`

- **Использование:**

```js
app.config.errorHandler = (err, vm, info) => {
  // Обработка ошибки
  // `info` это Vue-специфичная ошибка
  // например в каком хуке жизненного цикла была найдена ошибка
}
```

Добавление обработчика ошибок, отловленных во время рендеринга компонентов и наблюдателей. Аргументами обработчик получает ошибку и экземпляр приложения.

> Сервисы отслеживания ошибок [Sentry](https://sentry.io/for/vue/) и [Bugsnag](https://docs.bugsnag.com/platforms/browsers/vue/) предоставляют официальную интеграцию с использованием этого свойства.

## warnHandler

- **Тип:** `Function`

- **По умолчанию:** `undefined`

- **Использование:**

```js
app.config.warnHandler = function(msg, vm, trace) {
  // `trace` это трассировка иерархии компонентов
}
```

Определение пользовательского обработчика для предупреждений Vue во время выполнения. Работает только в режиме разработки и игнорируется в production.

## globalProperties

- **Тип:** `[key: string]: any`

- **По умолчанию:** `undefined`

- **Использование:**

```js
app.config.globalProperties.foo = 'bar'

app.component('child-component', {
  mounted() {
    console.log(this.foo) // 'bar'
  }
})
```

Добавление глобального свойства, к которому можно обращаться из любого компонента приложения. В случае конфликта имён, свойства компонента будут приоритетнее.

Данный подход заменяет расширение `Vue.prototype` во Vue 2.x:

```js
// Раньше
Vue.prototype.$http = () => {}

// Сейчас
const app = Vue.createApp({})
app.config.globalProperties.$http = () => {}
```

## isCustomElement

- **Тип:** `(tag: string) => boolean`

- **По умолчанию:** `undefined`

- **Использование:**

```js
// любой элемент, начинающийся с 'ion-' будет считаться пользовательским
app.config.isCustomElement = tag => tag.startsWith('ion-')
```

Определяет метод распознавания пользовательских элементов, определенных вне Vue (например, использование Web Components API). Если компонент совпадает с условием, то его не нужно регистрировать и Vue не выдаст предупреждения `Unknown custom element`.

> Обратите внимание что нет необходимости указывать HTML и SVG теги — парсер Vue определяет их автоматически.

## optionMergeStrategies

- **Тип:** `{ [key: string]: Function }`

- **По умолчанию:** `{}`

- **Использование:**

```js
const app = Vue.createApp({
  mounted() {
    console.log(this.$options.hello)
  }
})

app.config.optionMergeStrategies.hello = (parent, child, vm) => {
  return `Hello, ${child}`
}

app.mixin({
  hello: 'Vue'
})

// 'Hello, Vue'
```

Определение пользовательской функции для слияния опций.

Функция слияния получает первым аргументом значения опций родительского элемента, вторым дочернего элемента
и третьим контекст действующего экземпляра приложения.

- **См. также:** [Пользовательские функции слияния](../guide/mixins.md#custom-option-merge-strategies)

## performance

- **Тип:** `boolean`

- **По умолчанию:** `false`

- **Использование**:

Установка в значение `true` включит отслеживание производительности во время инициализации, компиляции, отрисовки и обновления компонентов в инструментах разработчика браузера. Работает только в режиме разработки в браузерах поддерживающих API [performance.mark](https://developer.mozilla.org/en-US/docs/Web/API/Performance/mark).
