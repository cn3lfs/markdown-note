---
title: vuex-state
author: bro wen
date: 2019-03-06 22:42:12
tags: [Vue, Vuex]
---
# Vuex

## questions
### what is vuex?
Vuex is a state management pattern + library for Vue.js applications. It serves as a centralized store for all the components in an application, with rules ensuring that the state can only be mutated in a predictable fashion.

What is a "State Management Pattern"?

Let's start with a simple Vue counter app:
```js
new Vue({
  // state
  data () {
    return {
      count: 0
    }
  },
  // view
  template: `
    <div>{{ count }}</div>
  `,
  // actions
  methods: {
    increment () {
      this.count++
    }
  }
})
```
It is a self-contained app with the following parts:

The state, which is the source of truth that drives our app;
The view, which is just a declarative mapping of the state;
The actions, which are the possible ways the state could change in reaction to user inputs from the view.

This is an extremely simple representation of the concept of "one-way data flow":

However, the simplicity quickly breaks down when we have multiple components that share common state:

Multiple views may depend on the same piece of state.
Actions from different views may need to mutate the same piece of state

### what makes vuex store different from plain object?

At the center of every Vuex application is the store. A "store" is basically a container that holds your application state. There are two things that make a Vuex store different from a plain global object:

Vuex stores are reactive. When Vue components retrieve state from it, they will reactively and efficiently update if the store's state changes.

You cannot directly mutate the store's state. The only way to change a store's state is by explicitly committing mutations. This ensures every state change leaves a track-able record, and enables tooling that helps us better understand our applications.

### why don't we change the state directly?

the reason we are committing a mutation instead of changing store.state.count directly, is because we want to explicitly track it. This simple convention makes your intention more explicit, so that you can reason about state changes in your app better when reading the code. In addition, this gives us the opportunity to implement tools that can log every mutation, take state snapshots, or even perform time travel debugging.

## State

###  Single State Tree

Vuex uses a single state tree - that is, this single object contains all your application level state and serves as the "single source of truth". This also means usually you will have only one store for each application. A single state tree makes it straightforward to locate a specific piece of state, and allows us to easily take snapshots of the current app state for debugging purposes.

### retrieve state

Since Vuex stores are reactive, the simplest way to "retrieve" state from it is simply returning some store state from within a computed property.Whenever store.state.count changes, it will cause the computed property to re-evaluate, and trigger associated DOM updates.

However, this pattern causes the component to rely on the global store singleton. When using a module system, it requires importing the store in every component that uses store state, and also requires mocking when testing the component.

Vuex provides a mechanism to "inject" the store into all child components from the root component with the store option (enabled by Vue.use(Vuex))

###  The mapState Helper

When a component needs to make use of multiple store state properties or getters, declaring all these computed properties can get repetitive and verbose. To deal with this we can make use of the mapState helper which generates computed getter functions for us, saving us some keystrokes:

```js
// object syntax
computed: mapState({
    // arrow functions can make the code very succinct!
    count: state => state.count,

    // passing the string value 'count' is same as `state => state.count`
    countAlias: 'count',

    // to access local state with `this`, a normal function must be used
    countPlusLocalState (state) {
      return state.count + this.localCount
    }
  })
// array syntax
computed: mapState([
  // map this.count to store.state.count
  'count'
])
```
Note that mapState returns an object. How do we use it in combination with other local computed properties? Normally, we'd have to use a utility to merge multiple objects into one so that we can pass the final object to computed

```js
computed: {
  localComputed () { /* ... */ },
  // mix this into the outer object with the object spread operator
  ...mapState({
    // ...
  })
}
```

##  Getters

### why we need getters?

Sometimes we may need to compute derived state based on store state, for example filtering through a list of items and counting them. If more than one component needs to make use of this, we have to either duplicate the function, or extract it into a shared helper and import it in multiple places - both are less than ideal.

Vuex allows us to define "getters" in the store. You can think of them as computed properties for stores. Like computed properties, a getter's result is cached based on its dependencies, and will only re-evaluate when some of its dependencies have changed.

###  Property-Style Access and Method-Style Access
1. property-style accesss
The getters will be exposed on the store.getters object, and you access values as properties.
Note that getters accessed as properties are cached as part of Vue's reactivity system.

2. method-style access
You can also pass arguments to getters by returning a function. This is particularly useful when you want to query an array in the store.

Note that getters accessed via methods will run each time you call them, and the result is not cached.
``` js
 getters: {
       // Property-Style Access
       doneTodos: state => {
           return state.todos.filter(todo => todo.done)
       },
       doneTodosCount: (state, getters) => {
           return getters.doneTodos.length
       },
       //  Method-Style Access
       getTodoById: (state) => (id) => {
           return state.todos.find(todo => todo.id === id)
       }
   }
   console.log(store.getters.doneTodos);
   console.log(store.getters.doneTodosCount);
   console.log(store.getters.getTodoById(2));
```

### The mapGetters Helper

The mapGetters helper simply maps store getters to local computed properties:

``` js
computed: {
    // mix the getters into computed with object spread operator
    ...mapGetters([
      'doneTodosCount',
      'anotherGetter',
      // ...
    ])
  }
```

If you want to map a getter to a different name, use an object:

``` js
...mapGetters({
  // map `this.doneCount` to `this.$store.getters.doneTodosCount`
  doneCount: 'doneTodosCount'
})
```

## mutations

### Mutations Follow Vue's Reactivity Rules

Since a Vuex store's state is made reactive by Vue, when we mutate the state, Vue components observing the state will update automatically. This also means Vuex mutations are subject to the same reactivity caveats when working with plain Vue:

Prefer initializing your store's initial state with all desired fields upfront.

When adding new properties to an Object, you should either:

Use Vue.set(obj, 'newProp', 123), or

Replace that Object with a fresh one. For example, using the object spread syntax

we can write it like this:

state.obj = { ...state.obj, newProp: 123 }

## Actions
Actions are similar to mutations, the differences being that:

Instead of mutating the state, actions commit mutations.
Actions can contain arbitrary asynchronous operations.
```js
const store = new Vuex.Store({
  state: {
    count: 0
  },
  mutations: {
    increment (state) {
      state.count++
    }
  },
  actions: {
    increment (context) {
      context.commit('increment')
    }
  }
})
```
Action handlers receive a context object which exposes the same set of methods/properties on the store instance, so you can call context.commit to commit a mutation, or access the state and getters via context.state and context.getters. We can even call other actions with context.dispatch

In practice, we often use ES2015 argument destructuring

to simplify the code a bit (especially when we need to call commit multiple times):
```js
actions: {
  increment ({ commit }) {
    commit('increment')
  }
}
```
### payload

Actions support the same payload format and object-style dispatch:
```js
// dispatch with a payload
store.dispatch('incrementAsync', {
  amount: 10
})

// dispatch with an object
store.dispatch({
  type: 'incrementAsync',
  amount: 10
})
```
### mapActions helper
You can dispatch actions in components with this.$store.dispatch('xxx'), or use the mapActions helper which maps component methods to store.dispatch calls (requires root store injection):

```js
import { mapActions } from 'vuex'

export default {
  // ...
  methods: {
    ...mapActions([
      'increment', // map `this.increment()` to `this.$store.dispatch('increment')`

      // `mapActions` also supports payloads:
      'incrementBy' // map `this.incrementBy(amount)` to `this.$store.dispatch('incrementBy', amount)`
    ]),
    ...mapActions({
      add: 'increment' // map `this.add()` to `this.$store.dispatch('increment')`
    })
  }
}
```

## Modules

### Namespacing
By default, actions, mutations and getters inside modules are still registered under the global namespace - this allows multiple modules to react to the same mutation/action type.

If you want your modules to be more self-contained or reusable, you can mark it as namespaced with namespaced: true. When the module is registered, all of its getters, actions and mutations will be automatically namespaced based on the path the module is registered at. For example:

Namespaced getters and actions will receive localized getters, dispatch and commit. In other words, you can use the module assets without writing prefix in the same module. Toggling between namespaced or not does not affect the code inside the module.

Accessing Global Assets in Namespaced Modules

If you want to use global state and getters, rootState and rootGetters are passed as the 3rd and 4th arguments to getter functions, and also exposed as properties on the context object passed to action functions.

To dispatch actions or commit mutations in the global namespace, pass { root: true } as the 3rd argument to dispatch and commit.