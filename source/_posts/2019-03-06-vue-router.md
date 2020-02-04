---
title: vue-router
author: bro wen
date: 2019-03-06 20:00:21
tags: [Vue, Vue Router]
---
# Vue Router

## What is SPA?

Single Page Application
We hosts our app on index.html, and when we go to other pages, it actually rerender some part of the same page, we don't really go to another page.

The router is the important thing about a single page application, because a single page application  means technically we only have one page, the single page which is the index.html file we load, which hosts  our vuejs application and then we use this single page to simulate navigation for the users, so our  URL does change but we catch all these changes and only re-render this single page.

so that it looks  like as if we were revisiting different pages,  it looks like this to the user but behind the scenes, it's always the same page.

So the big difference to the last lectures is we no longer have application to where we need to reload  our page, where we get different pages from the server and then only embed vuejs to handle parts of  this page.

## What is vue router?

Vue Router is the official router for Vue.js . It deeply integrates with Vue.js core to make building Single Page Applications with Vue.js a breeze

With Vue.js, we are already composing our application with components. When adding Vue Router to the mix, all we need to do is map our components to the routes and let Vue Router know where to render them.

Throughout the docs, we will often use the router instance. Keep in mind that this.$router is exactly the same as using router. The reason we use this.$router is because we don't want to import the router in every single component that needs to manipulate routing.

Notice that a <router-link> automatically gets the .router-link-active class when its target route is matched.

## hash mode and history mode

### hash mode

The default mode for vue-router is hash mode - it uses the URL hash to simulate a full URL so that the page won't be reloaded when the URL changes.

If you have a normal route, normal URL I should say without the hash tag,  then each request once you hit enter on your keyboard gets sent to the server,  this is how the browser and how the web works and which makes sense, it would be hard to visit  webpages if it wouldn't get sent to the server.  

The problem with sending it to the server is that we want to handle the route in our single page application  though, we don't want to get the route to the server,  we want to handle it on our local page here, though if we visit our page for the first time, in this case  we want to get it to the server because we need to get the single page.

but if we for example visit /user without a hash tag, we would like to get our single page, the index.html  file and then take over and handle the rest of the URL with our single page instead with the  browser or the server. 

Well the hash tag allows us to do just that, the part in front of the hash tag is sent to  the server so to say, this will give us back our index.html file  and the part after the hash tag is then handed over to our running Javascript application and may be  handled by that application. so that now we can extract our paths from there.  This works fine and it spares us the issues we might face if we would have to configure our server in a certain way that it kind of gives us back the right thing. 

### history mode

To get rid of the hash, we can use the router's history mode, which leverages the history.pushState API to achieve URL navigation without a page reload:
```js
const router = new VueRouter({
  mode: 'history',
  routes: [...]
})
```
When using history mode, the URL will look "normal," e.g. http://oursite.com/user/id. Beautiful!

Here comes a problem, though: Since our app is a single page client side app, without a proper server configuration, the users will get a 404 error if they access http://oursite.com/user/id directly in their browser. Now that's ugly.

Not to worry: To fix the issue, all you need to do is add a simple catch-all fallback route to your server. If the URL doesn't match any static assets, it should serve the same index.html page that your app lives in. Beautiful, again!

There is a caveat to this: Your server will no longer report 404 errors as all not-found paths now serve up your index.html file. To get around the issue, you should implement a catch-all route within your Vue app to show a 404 page:
```js
const router = new VueRouter({
  mode: 'history',
  routes: [
    { path: '*', component: NotFoundComponent }
  ]
})
```
Alternatively, if you are using a Node.js server, you can implement the fallback by using the router on the server side to match the incoming URL and respond with 404 if no route is matched.

## Dynamic router

Very often we will need to map routes with the given pattern to the same component. For example we may have a User component which should be rendered for all users but with different user IDs. In vue-router we can use a dynamic segment in the path to achieve that:A dynamic segment is denoted by a colon :. 
```js
const User = {
  template: '<div>User {{ $route.params.id }}</div>'
}

const router = new VueRouter({
  routes: [
    // dynamic segments start with a colon
    { path: '/user/:id', component: User }
  ]
})
```
When a route is matched, the value of the dynamic segments will be exposed as this.$route.params in every component.

A dynamic segment is denoted by a colon :. When a route is matched, the value of the dynamic segments will be exposed as this.$route.params in every component. 

You can have multiple dynamic segments in the same route, and they will map to corresponding fields on $route.params.

In addition to $route.params, the $route object also exposes other useful information such as $route.query (if there is a query in the URL), $route.hash, etc. 

### Reacting to Params Changes

One thing to note when using routes with params is that when the user navigates from /user/foo to /user/bar, the same component instance will be reused. Since both routes render the same component, this is more efficient than destroying the old instance and then creating a new one. However, this also means that the lifecycle hooks of the component will not be called.

To react to params changes in the same component, you can simply watch the $route object:

```js
const User = {
  template: '...',
  watch: {
    '$route' (to, from) {
      // react to route changes...
    }
  }
}
```

Or, use the beforeRouteUpdate navigation guard introduced in 2.2:
```js
const User = {
  template: '...',
  beforeRouteUpdate (to, from, next) {
    // react to route changes...
    // don't forget to call next()
  }
}
```
### Catch all / 404 Not found Route

Regular params will only match characters in between url fragments, separated by /. If we want to match anything, we can use the asterisk (*):

When using asterisk routes, make sure to correctly order your routes so that asterisk ones are at the end. The route { path: '*' } is usually used to 404 client side. If you are using History mode, make sure to correctly configure your server as well.

When using an asterisk, a param named pathMatch is automatically added to $route.params. It contains the rest of the url matched by the asterisk:
```js
// Given a route { path: '/user-*' }
this.$router.push('/user-admin')
this.$route.params.pathMatch // 'admin'

// Given a route { path: '*' }
this.$router.push('/non-existing')
this.$route.params.pathMatch // '/non-existing'
```
### Advanced Matching Patterns

vue-router uses path-to-regexp
as its path matching engine, so it supports many advanced matching patterns such as optional dynamic segments, zero or more / one or more requirements, and even custom regex patterns. 

### Matching Priority

Sometimes the same URL may be matched by multiple routes. In such a case the matching priority is determined by the order of route definition: the earlier a route is defined, the higher priority it gets.

## Nested Routes

Real app UIs are usually composed of components that are nested multiple levels deep. It is also very common that the segments of a URL corresponds to a certain structure of nested components. With vue-router, it is very simple to express this relationship using nested route configurations.

The <router-view> here is a top-level outlet. It renders the component matched by a top level route. Similarly, a rendered component can also contain its own, nested <router-view>.To render components into this nested outlet, we need to use the children option in VueRouter constructor config:

```js
const User = {
  template: `
    <div class="user">
      <h2>User {{ $route.params.id }}</h2>
      <router-view></router-view>
    </div>
  `
}

const router = new VueRouter({
  routes: [
    { path: '/user/:id', component: User,
      children: [
        // UserHome will be rendered inside User's <router-view>
        // when /user/:id is matched
        { path: '', component: UserHome },
        {
          // UserProfile will be rendered inside User's <router-view>
          // when /user/:id/profile is matched
          path: 'profile',
          component: UserProfile
        },
        {
          // UserPosts will be rendered inside User's <router-view>
          // when /user/:id/posts is matched
          path: 'posts',
          component: UserPosts
        }
      ]
    }
  ]
})

```

Note that nested paths that start with / will be treated as a root path. This allows you to leverage the component nesting without having to use a nested URL.

As you can see the children option is just another Array of route configuration objects like routes itself. Therefore, you can keep nesting views as much as you need.

## Named Routes

Sometimes it is more convenient to identify a route with a name, especially when linking to a route or performing navigations. You can give a route a name in the routes options while creating the Router instance:
```js
const router = new VueRouter({
  routes: [
    {
      path: '/user/:userId',
      name: 'user',
      component: User
    }
  ]
})
```
To link to a named route, you can pass an object to the router-link component's to prop:
```html
<router-link :to="{ name: 'user', params: { userId: 123 }}">User</router-link>
```
This is the exact same object used programatically with router.push():
```js
router.push({ name: 'user', params: { userId: 123 }})
```
In both cases, the router will navigate to the path /user/123.

## Named Views

Sometimes you need to display multiple views at the same time instead of nesting them, e.g. creating a layout with a sidebar view and a main view. This is where named views come in handy. Instead of having one single outlet in your view, you can have multiple and give each of them a name. A router-view without a name will be given default as its name.
```html
<router-view class="view one"></router-view>
<router-view class="view two" name="a"></router-view>
<router-view class="view three" name="b"></router-view>
```
A view is rendered by using a component, therefore multiple views require multiple components for the same route. Make sure to use the components (with an s) option:
```js
const router = new VueRouter({
  routes: [
    {
      path: '/',
      components: {
        default: Foo,
        a: Bar,
        b: Baz
      }
    }
  ]
})
```

### Nested Named Views

It is possible to create complex layouts using named views with nested views. When doing so, you will also need to name nested router-view components used. 

## Redirect and Alias
### Redirect

Redirecting is also done in the routes configuration. To redirect from /a to /b:
```js
const router = new VueRouter({
  routes: [
    { path: '/a', redirect: '/b' }
  ]
})
```
The redirect can also be targeting a named route:
```js
const router = new VueRouter({
  routes: [
    { path: '/a', redirect: { name: 'foo' }}
  ]
})
```
Or even use a function for dynamic redirecting:
```js
const router = new VueRouter({
  routes: [
    { path: '/a', redirect: to => {
      // the function receives the target route as the argument
      // return redirect path/location here.
    }}
  ]
})
```
Note that Navigation Guards are not applied on the route that will be redirected, only on its target. In the example below, adding a beforeEnter or beforeLeave guard to the /a route would not have any effect.

### Alias

A redirect means when the user visits /a, and URL will be replaced by /b, and then matched as /b. But what is an alias?

An alias of /a as /b means when the user visits /b, the URL remains /b, but it will be matched as if the user is visiting /a.

The above can be expressed in the route configuration as:
```js
const router = new VueRouter({
  routes: [
    { path: '/a', component: A, alias: '/b' }
  ]
})
```
An alias gives you the freedom to map a UI structure to an arbitrary URL, instead of being constrained by the configuration's nesting structure.

## Scroll Behavior

When using client-side routing, we may want to scroll to top when navigating to a new route, or preserve the scrolling position of history entries just like real page reload does. vue-router allows you to achieve these and even better, allows you to completely customize the scroll behavior on route navigation.

Note: this feature only works if the browser supports history.pushState.
```js
const router = new VueRouter({
  routes: [...],
  scrollBehavior (to, from, savedPosition) {
    // return desired position
  }
})
```
The scrollBehavior function receives the to and from route objects. The third argument, savedPosition, is only available if this is a popstate navigation (triggered by the browser's back/forward buttons).

The function can return a scroll position object. The object could be in the form of:
```js
{ x: number, y: number }
{ selector: string, offset? : { x: number, y: number }} //(offset only supported in 2.6.0+)
```
If a falsy value or an empty object is returned, no scrolling will happen.

Returning the savedPosition will result in a native-like behavior when navigating with back/forward buttons:
```js
scrollBehavior (to, from, savedPosition) {
  if (savedPosition) {
    return savedPosition
  } else {
    return { x: 0, y: 0 }
  }
}
```
If you want to simulate the "scroll to anchor" behavior:
```js
scrollBehavior (to, from, savedPosition) {
  if (to.hash) {
    return {
      selector: to.hash
      // , offset: { x: 0, y: 10 }
    }
  }
}
```

### Async Scrolling
You can also return a Promise that resolves to the desired position descriptor:
```js
scrollBehavior (to, from, savedPosition) {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve({ x: 0, y: 0 })
    }, 500)
  })
}
```
## Navigation Guards

As the name suggests, the navigation guards provided by vue-router are primarily used to guard navigations either by redirecting it or canceling it. There are a number of ways to hook into the route navigation process: globally, per-route, or in-component.

Remember that params or query changes won't trigger enter/leave navigation guards. You can either watch the $route object to react to those changes, or use the beforeRouteUpdate in-component guard.

### guard
Every guard function receives three arguments:

* to: Route: the target Route Object being navigated to.

* from: Route: the current route being navigated away from.

* next: Function: this function must be called to resolve the hook. The action depends on the arguments provided to next:

    - next(): move on to the next hook in the pipeline. If no hooks are left, the navigation is confirmed.

   -  next(false): abort the current navigation. If the browser URL was changed (either manually by the user or via back button), it will be reset to that of the from route.

   -  next('/') or next({ path: '/' }): redirect to a different location. The current navigation will be aborted and a new one will be started. You can pass any location object to next, which allows you to specify options like replace: true, name: 'home' and any option used in router-link's to prop or router.push

    - next(error): (2.4.0+) if the argument passed to next is an instance of Error, the navigation will be aborted and the error will be passed to callbacks registered via router.onError().

Make sure to always call the next function, otherwise the hook will never be resolved.

```js
const router = new VueRouter({ ... })

router.beforeEach((to, from, next) => {
  // ...
})
```
### Global Guards
#### Global Before Guards
#### Global Resolve Guards
#### Global After Hooks


### Per-Route Guard

You can define beforeEnter guards directly on a route's configuration object:

### In-Component Guards

Finally, you can directly define route navigation guards inside route components (the ones passed to the router configuration) with the following options:

beforeRouteEnter
beforeRouteUpdate
beforeRouteLeave

The beforeRouteEnter guard does NOT have access to this, because the guard is called before the navigation is confirmed, thus the new entering component has not even been created yet.

However, you can access the instance by passing a callback to next. The callback will be called when the navigation is confirmed, and the component instance will be passed to the callback as the argument:
```js
beforeRouteEnter (to, from, next) {
  next(vm => {
    // access to component instance via `vm`
  })
}
```
Note that beforeRouteEnter is the only guard that supports passing a callback to next.
### The Full Navigation Resolution Flow

Navigation triggered.
Call leave guards in deactivated components.
Call global beforeEach guards.
Call beforeRouteUpdate guards in reused components.
Call beforeEnter in route configs.
Resolve async route components.
Call beforeRouteEnter in activated components.
Call global beforeResolve guards.
Navigation confirmed.
Call global afterEach hooks.
DOM updates triggered.
Call callbacks passed to next in beforeRouteEnter guards with instantiated instances.

## Lazy Loading Routes

When building apps with a bundler, the JavaScript bundle can become quite large, and thus affect the page load time. It would be more efficient if we can split each route's components into a separate chunk, and only load them when the route is visited.

Combining Vue's async component feature
and webpack's code splitting feature , it's trivially easy to lazy-load route components.

```js
const Foo = () => import('./Foo.vue')

//Nothing needs to change in the route config, just use Foo as usual:

const router = new VueRouter({
  routes: [
    { path: '/foo', component: Foo }
  ]
})
```

### Grouping Components in the Same Chunk

Sometimes we may want to group all the components nested under the same route into the same async chunk. To achieve that we need to use named chunks

by providing a chunk name using a special comment syntax (requires webpack > 2.4):
```js
const Foo = () => import(/* webpackChunkName: "group-foo" */ './Foo.vue')
const Bar = () => import(/* webpackChunkName: "group-foo" */ './Bar.vue')
const Baz = () => import(/* webpackChunkName: "group-foo" */ './Baz.vue')
```
webpack will group any async module with the same chunk name into the same async chunk.