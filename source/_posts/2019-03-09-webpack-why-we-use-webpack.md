---
title: webpack_why_we_use_webpack
author: bro wen
date: 2019-03-09 23:55:17
tags: [webpack]
---
# Why we use webpack?

In this example, there are implicit dependencies between the <script> tags. Our index.js file depends on lodash being included in the page before it runs. This is because index.js never explicitly declared a need for lodash; it just assumes that the global variable _ exists.

There are problems with managing JavaScript projects this way:

    It is not immediately apparent that the script depends on an external library.
    If a dependency is missing, or included in the wrong order, the application will not function properly.
    If a dependency is included but not used, the browser will be forced to download unnecessary code.
