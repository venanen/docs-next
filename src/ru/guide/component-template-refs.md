# Ссылки на элементы шаблона

> Подразумевается, что вы уже изучили и разобрались с разделом [Основы компонентов](component-basics.md). Если нет — прочитайте его сначала.

Несмотря на существование входных параметров и событий, иногда может потребоваться прямой доступ к дочернему компоненту в JavaScript. Для этого можно присвоить ID дочернему компоненту или HTML-элементу с помощью атрибута `ref`. Например:

```html
<input ref="input" />
```

К примеру, это может пригодиться для программного выставления фокуса на поле ввода при монтировании компонента:

```js
const app = Vue.createApp({})

app.component('base-input', {
  template: `
    <input ref="input" />
  `,
  methods: {
    focusInput() {
      this.$refs.input.focus()
    }
  },
  mounted() {
    this.focusInput()
  }
})
```

Также, можно добавить ещё один `ref` на сам компонент и использовать его для запуска события `focusInput` из родительского компонента:

```html
<base-input ref="usernameInput"></base-input>
```

```js
this.$refs.usernameInput.focusInput()
```

:::warning ВНИМАНИЕ
Свойство `$refs` заполняется после отрисовки компонента. Они предназначены для исключительных случаев, когда необходимо напрямую управлять дочерними элементами — рекомендуется избегать доступа к `$refs` из шаблонов или вычисляемых свойств.
:::

**См. также**: [Использование ссылок на элементы шаблонов в Composition API](composition-api-template-refs.md#template-refs)
