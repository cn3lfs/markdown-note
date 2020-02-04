---
title: vue_render
author: bro wen
date: 2019-03-16 11:58:35
tags: [Vue]
---

The answer (for anyone else who comes across this), is that render: h => h(App) is shorthand for:

render: function (createElement) {
    return createElement(App);
}

Which can be shortened to:

render (createElement) {
    return createElement(App);
}

Which can again be shortened to (with h being an alias to createElement as noted above):

render (h){
    return h(App);
}

Which is then shortened further to (using ES6 "fat arrow" syntax):

render: h => h(App);