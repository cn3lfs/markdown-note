---
title: VueJS:when to use computed/watch
date: 2018-07-20 17:57:18
tags: [Vue]
---

1. coming from vuejs guide
However, it is often a better idea to use a computed property rather than an imperative watch callback.
```html
<div id="demo">{{ fullName }}</div>
```
```js
var vm = new Vue({
  el: '#demo',
  data: {
    firstName: 'Foo',
    lastName: 'Bar',
    fullName: 'Foo Bar'
  },
  watch: {
    firstName: function (val) {
      this.fullName = val + ' ' + this.lastName
    },
    lastName: function (val) {
      this.fullName = this.firstName + ' ' + val
    }
  }
})
```
The above code is imperative and repetitive. Compare it with a computed property version:
```js
var vm = new Vue({
  el: '#demo',
  data: {
    firstName: 'Foo',
    lastName: 'Bar'
  },
  computed: {
    fullName: function () {
      return this.firstName + ' ' + this.lastName
    }
  }
})
```

2. my own thinking

watch是在vue instantiation的时候添加的，对于watch的对象，如果需要同时watch多个对象，并且这些对象同时作用于一个新的对象，使用watch会产生很多重复的代码，而且更加影响性能。因为可能多个对象之间会互相影响，而使用computed property只需写一次代码，更加整洁，更加maintainable。

3. coming from internet

When to use methods

    To react on some event happening in the DOM
    To call a function when something happens in your component. You can call a methods from computed properties or watchers.

When to use computed properties

    You need to compose new data from existing data sources
    You have a variable you use in your template that’s built from one or more data properties
    You want to reduce a complicated, nested property name to a more readable and easy to use one, yet update it when the original property changes
    You need to reference a value from the template. In this case, creating a computed property is the best thing because it’s cached.
    You need to listen to changes of more than one data property

When to use watchers

    You want to listen when a data property changes, and perform some action
    You want to listen to a prop value change
    You only need to listen to one specific property (you can’t watch multiple properties at the same time)
    You want to watch a data property until it reaches some specific value and then do something

[click](https://flaviocopes.com/vue-methods-watchers-computed-properties/)
