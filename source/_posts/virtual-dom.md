---
title: virtual_dom
author: bro wen
date: 2019-01-22 23:23:21
tags: [JavaScript]
---
[React: The Virtual DOM](https://www.codecademy.com/articles/react-virtual-dom)

The Problem

DOM manipulation is the heart of the modern, interactive web. Unfortunately, it is also a lot slower than most JavaScript operations.

This slowness is made worse by the fact that most JavaScript frameworks update the DOM much more than they have to.
To address this problem, the people at React popularized something called the virtual DOM.

The Virtual DOM

In React, for every DOM object, there is a corresponding "virtual DOM object." A virtual DOM object is a representation of a DOM object, like a lightweight copy.

A virtual DOM object has the same properties as a real DOM object, but it lacks the real thing's power to directly change what's on the screen.

Manipulating the DOM is slow. Manipulating the virtual DOM is much faster, because nothing gets drawn onscreen. Think of manipulating the virtual DOM as editing a blueprint, as opposed to moving rooms in an actual house.


[Vue: The Virtual DOM](https://vuejs.org/v2/guide/render-function.html#The-Virtual-DOM)

Vue accomplishes this by building a virtual DOM to keep track of the changes it needs to make to the real DOM. Taking a closer look at this line:

return createElement('h1', this.blogTitle)

What is createElement actually returning? It’s not exactly a real DOM element. It could perhaps more accurately be named createNodeDescription, as it contains information describing to Vue what kind of node it should render on the page, including descriptions of any child nodes. We call this node description a “virtual node”, usually abbreviated to VNode. “Virtual DOM” is what we call the entire tree of VNodes, built by a tree of Vue components.



[What’s The Deal With Vue’s Virtual DOM?](https://medium.com/js-dojo/whats-the-deal-with-vue-s-virtual-dom-3ed4fc0dbb20)

Reminding ourselves what the DOM actually is

We tend think of the DOM as the HTML document it represents. But actually the DOM is a tree-like data structure that comes into existence once an HTML document has been parsed by the browser.

The browser paints the DOM to the screen and will repaint it in response to user actions (e.g. mouse clicks) and updates via its API from your Javascript scripts e.g. document.createElement.