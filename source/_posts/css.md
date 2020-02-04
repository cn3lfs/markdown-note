---
title: css
author: bro wen
date: 2018-01-19 11:01:12
tags: [CSS]
---
To-do list
Section

In this example we will create a simple to-do list using pseudo-elements. This method can often be used to add small touches to the UI and improve user experience.
HTML
```html
<ul>
  <li>Buy milk</li>
  <li>Take the dog for a walk</li>
  <li>Exercise</li>
  <li>Write code</li>
  <li>Play music</li>
  <li>Relax</li>
</ul>

CSS
<style>
li {
  list-style-type: none;
  position: relative;
  margin: 2px;
  padding: 0.5em 0.5em 0.5em 2em;
  background: lightgrey;
  font-family: sans-serif;
}

li.done {
  background: #CCFF99;
}

li.done::before {
  content: '';
  position: absolute;
  border-color: #009933;
  border-style: solid;
  border-width: 0 0.3em 0.25em 0;
  height: 1em;
  top: 1.3em;
  left: 0.6em;
  margin-top: -1em;
  transform: rotate(45deg);
  width: 0.5em;
}
</style>

JavaScript
<script type="text/javascript">
var list = document.querySelector('ul');
list.addEventListener('click', function(ev) {
  if (ev.target.tagName === 'LI') {
     ev.target.classList.toggle('done'); 
  }
}, false);
</script>
```
Here is the above code example running live. Note that there are no icons used, and the check-mark is actually the ::before that has been styled in CSS. Go ahead and get some stuff done.