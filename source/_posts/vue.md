---
title: vue
author: bro wen
date: 2018-06-29 11:01:12
tags: [Vue]
---
1. vue data update
When this data changes, the view will re-render. It should be noted that properties in data are only reactive if they existed when the instance was created. That means if you add a new property, like:

The only exception to this being the use of Object.freeze(), which prevents existing properties from being changed, which also means the reactivity system can’t track changes.

In addition to data properties, Vue instances expose a number of useful instance properties and methods. These are prefixed with $ to differentiate them from user-defined properties.

2. computed properties vs methods

instead of a computed property, we can define the same function as a method instead. For the end result, the two approaches are indeed exactly the same. However, the difference is that computed properties are cached based on their dependencies. A computed property will only re-evaluate when some of its dependencies have changed. This means as long as message has not changed, multiple access to the reversedMessage computed property will immediately return the previously computed result without having to run the function again.

In comparison, a method invocation will always run the function whenever a re-render happens.

Why do we need caching? Imagine we have an expensive computed property A, which requires looping through a huge Array and doing a lot of computations. Then we may have other computed properties that in turn depend on A. Without caching, we would be executing A’s getter many more times than necessary! In cases where you do not want caching, use a method instead.

3. computed property vs watch property
However, it is often a better idea to use a computed property rather than an imperative watch callback. 


4. v-if multiple element

Because v-if is a directive, it has to be attached to a single element. But what if we want to toggle more than one element? In this case we can use v-if on a <template> element, which serves as an invisible wrapper. The final rendered result will not include the <template> element.
```html
<template v-if="ok">
  <h1>Title</h1>
  <p>Paragraph 1</p>
  <p>Paragraph 2</p>
</template>
```

5. v-if and key
Vue tries to render elements as efficiently as possible, often re-using them instead of rendering from scratch. Beyond helping make Vue very fast, this can have some useful advantages.

This isn’t always desirable though, so Vue offers a way for you to say, “These two elements are completely separate - don’t re-use them.” Add a key attribute with unique values:
```html
<template v-if="loginType === 'username'">
  <label>Username</label>
  <input placeholder="Enter your username" key="username-input">
</template>
<template v-else>
  <label>Email</label>
  <input placeholder="Enter your email address" key="email-input">
</template>
```
Now those inputs will be rendered from scratch each time you toggle. 

6. v-if vs v-show

v-if is “real” conditional rendering because it ensures that event listeners and child components inside the conditional block are properly destroyed and re-created during toggles.

v-if is also lazy: if the condition is false on initial render, it will not do anything - the conditional block won’t be rendered until the condition becomes true for the first time.

In comparison, v-show is much simpler - the element is always rendered regardless of initial condition, with CSS-based toggling.

Generally speaking, v-if has higher toggle costs while v-show has higher initial render costs. So prefer v-show if you need to toggle something very often, and prefer v-if if the condition is unlikely to change at runtime.

7. v-for and key

When Vue is updating a list of elements rendered with v-for, by default it uses an “in-place patch” strategy. If the order of the data items has changed, instead of moving the DOM elements to match the order of the items, Vue will patch each element in-place and make sure it reflects what should be rendered at that particular index. 

This default mode is efficient, but only suitable when your list render output does not rely on child component state or temporary DOM state (e.g. form input values).

To give Vue a hint so that it can track each node’s identity, and thus reuse and reorder existing elements, you need to provide a unique key attribute for each item. 
```html
<div v-for="item in items" :key="item.id">
  <!-- content -->
</div>
```
It is recommended to provide a key with v-for whenever possible, unless the iterated DOM content is simple, or you are intentionally relying on the default behavior for performance gains.

7. array mutation methods and non-mutating methods
Mutation Methods

Vue wraps an observed array’s mutation methods so they will also trigger view updates. The wrapped methods are:

    push()
    pop()
    shift()
    unshift()
    splice()
    sort()
    reverse()


In comparison, there are also non-mutating methods, e.g. filter(), concat() and slice(), which do not mutate the original array but always return a new array. When working with non-mutating methods, you can replace the old array with the new one:

example1.items = example1.items.filter(function (item) {
  return item.message.match(/Foo/)
})

8. limit

8.1
Due to limitations in JavaScript, Vue cannot detect the following changes to an array:

    When you directly set an item with the index, e.g. vm.items[indexOfItem] = newValue
    When you modify the length of the array, e.g. vm.items.length = newLength


To overcome caveat 1, both of the following will accomplish the same as vm.items[indexOfItem] = newValue, but will also trigger state updates in the reactivity system:

// Vue.set
Vue.set(vm.items, indexOfItem, newValue)

// Array.prototype.splice
vm.items.splice(indexOfItem, 1, newValue)

You can also use the vm.$set instance method, which is an alias for the global Vue.set:

vm.$set(vm.items, indexOfItem, newValue)

To deal with caveat 2, you can use splice:

vm.items.splice(newLength)

8.2
Again due to limitations of modern JavaScript, Vue cannot detect property addition or deletion.Vue does not allow dynamically adding new root-level reactive properties to an already created instance. However, it’s possible to add reactive properties to a nested object using the Vue.set(object, key, value) method.You can also use the vm.$set instance method, which is an alias for the global Vue.set

Sometimes you may want to assign a number of new properties to an existing object, for example using Object.assign() or _.extend(). In such cases, you should create a fresh object with properties from both objects. 

9. v-for to-do-list

```html
<div id="todo-list-example">
  <form v-on:submit.prevent="addNewTodo">
    <label for="new-todo">Add a todo</label>
    <input
      v-model="newTodoText"
      id="new-todo"
      placeholder="E.g. Feed the cat"
    >
    <button>Add</button>
  </form>
  <ul>
    <li
      is="todo-item"
      v-for="(todo, index) in todos"
      v-bind:key="todo.id"
      v-bind:title="todo.title"
      v-on:remove="todos.splice(index, 1)"
    ></li>
  </ul>
</div>

```

```js
Vue.component('todo-item', {
  template: '\
    <li>\
      {{ title }}\
      <button v-on:click="$emit(\'remove\')">Remove</button>\
    </li>\
  ',
  props: ['title']
})

new Vue({
  el: '#todo-list-example',
  data: {
    newTodoText: '',
    todos: [
      {
        id: 1,
        title: 'Do the dishes',
      },
      {
        id: 2,
        title: 'Take out the trash',
      },
      {
        id: 3,
        title: 'Mow the lawn'
      }
    ],
    nextTodoId: 4
  },
  methods: {
    addNewTodo: function () {
      this.todos.push({
        id: this.nextTodoId++,
        title: this.newTodoText
      })
      this.newTodoText = ''
    }
  }
})
```


10. event handler v-on

Sometimes we also need to access the original DOM event in an inline statement handler. You can pass it into a method using the special $event variable:
```html
<button v-on:click="warn('Form cannot be submitted yet.', $event)">
  Submit
</button>
```

```js
// ...
methods: {
  warn: function (message, event) {
    // now we have access to the native event
    if (event) event.preventDefault()
    alert(message)
  }
}
```

11. Event Modifiers
    .stop
    .prevent
    .capture
    .self
    .once
    .passive

The .passive modifier is especially useful for improving performance on mobile devices.

12. Key Modifiers

When listening for keyboard events, we often need to check for specific keys. Vue allows adding key modifiers for v-on when listening for key events:

<!-- only call `vm.submit()` when the `key` is `Enter` -->
<input v-on:keyup.enter="submit">

You can directly use any valid key names exposed via KeyboardEvent.key as modifiers by converting them to kebab-case.

<input v-on:keyup.page-down="onPageDown">

In the above example, the handler will only be called if $event.key is equal to 'PageDown'.



System Modifier Keys

You can use the following modifiers to trigger mouse or keyboard event listeners only when the corresponding modifier key is pressed:

    .ctrl
    .alt
    .shift
    .meta

<!-- Ctrl + Click -->
<div @click.ctrl="doSomething">Do something</div>


.exact Modifier

The .exact modifier allows control of the exact combination of system modifiers needed to trigger an event.

<!-- this will fire even if Alt or Shift is also pressed -->
<button @click.ctrl="onClick">A</button>

<!-- this will only fire when Ctrl and no other keys are pressed -->
<button @click.ctrl.exact="onCtrlClick">A</button>

<!-- this will only fire when no system modifiers are pressed -->
<button @click.exact="onClick">A</button>


Mouse Button Modifiers

    .left
    .right
    .middle

13. v-model


Modifiers
.lazy

By default, v-model syncs the input with the data after each input event (with the exception of IME composition as stated above). You can add the lazy modifier to instead sync after change events:

<!-- synced after "change" instead of "input" -->
<input v-model.lazy="msg" >

.number

If you want user input to be automatically typecast as a number, you can add the number modifier to your v-model managed inputs:

<input v-model.number="age" type="number">

This is often useful, because even with type="number", the value of HTML input elements always returns a string. If the value cannot be parsed with parseFloat(), then the original value is returned.
.trim

If you want whitespace from user input to be trimmed automatically, you can add the trim modifier to your v-model-managed inputs:

<input v-model.trim="msg">

14. component

data Must Be a Function

When we defined the <button-counter> component, you may have noticed that data wasn’t directly provided an object, like this:

data: {
  count: 0
}

Instead, a component’s data option must be a function, so that each instance can maintain an independent copy of the returned data object:

data: function () {
  return {
    count: 0
  }
}

If Vue didn’t have this rule, clicking on one button would affect the data of all other instances, like below:




Props


Props are custom attributes you can register on a component. When a value is passed to a prop attribute, it becomes a property on that component instance. To pass a title to our blog post component, we can include it in the list of props this component accepts, using a props option:

Vue.component('blog-post', {
  props: ['title'],
  template: '<h3>{{ title }}</h3>'
})

A component can have as many props as you’d like and by default, any value can be passed to any prop. In the template above, you’ll see that we can access this value on the component instance, just like with data.



every component must have a single root element. 

<blog-post
  v-for="post in posts"
  v-bind:key="post.id"
  v-bind:post="post"
></blog-post>

Vue.component('blog-post', {
  props: ['post'],
  template: `
    <div class="blog-post">
      <h3>{{ post.title }}</h3>
      <div v-html="post.content"></div>
    </div>
  `
})



Dynamic Components


<!-- Component changes when currentTabComponent changes -->
<component v-bind:is="currentTabComponent"></component>

In the example above, currentTabComponent can contain either:

    the name of a registered component, or
    a component’s options object



DOM Template Parsing Caveats

Some HTML elements, such as <ul>, <ol>, <table> and <select> have restrictions on what elements can appear inside them, and some elements such as <li>, <tr>, and <option> can only appear inside certain other elements.

This will lead to issues when using components with elements that have such restrictions. For example:

<table>
  <blog-post-row></blog-post-row>
</table>

The custom component <blog-post-row> will be hoisted out as invalid content, causing errors in the eventual rendered output. Fortunately, the is special attribute offers a workaround:

<table>
  <tr is="blog-post-row"></tr>
</table>
It should be noted that this limitation does not apply if you are using string templates from one of the following sources:

    String templates (e.g. template: '...')
    Single-file (.vue) components
    <script type="text/x-template">
