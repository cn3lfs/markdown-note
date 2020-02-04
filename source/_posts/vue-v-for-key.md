---
title: vue_v-for_key
author: bro wen
date: 2018-07-22 22:40:34
tags: [Vue]
---

1. [key](https://vuejs.org/v2/guide/list.html#key)

When Vue is updating a list of elements rendered with v-for, by default it uses an “in-place patch” strategy. If the order of the data items has changed, instead of moving the DOM elements to match the order of the items, Vue will patch each element in-place and make sure it reflects what should be rendered at that particular index. This is similar to the behavior of track-by="$index" in Vue 1.x.

This default mode is efficient, but only suitable when your list render output does not rely on child component state or temporary DOM state (e.g. form input values).

To give Vue a hint so that it can track each node’s identity, and thus reuse and reorder existing elements, you need to provide a unique key attribute for each item. An ideal value for key would be the unique id of each item. This special attribute is a rough equivalent to track-by in 1.x, but it works like an attribute, so you need to use v-bind to bind it to dynamic values (using shorthand here):

<div v-for="item in items" :key="item.id">
  <!-- content -->
</div>

It is recommended to provide a key with v-for whenever possible, unless the iterated DOM content is simple, or you are intentionally relying on the default behavior for performance gains.

Since it’s a generic mechanism for Vue to identify nodes, the key also has other uses that are not specifically tied to v-for, as we will see later in the guide.