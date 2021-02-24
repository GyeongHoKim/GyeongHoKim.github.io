---
title: "[javaScript] What does '=>' in javaScript?"
excerpt: "Compare traditional functions to arrow functions"
last_modified_at: 2021-02-24T14:50:00+09:00
categories: javaScript
tag: javaScript
toc: true
toc_sticky: true
author_profile: false
---

# What is '=>'?

You might be wondering when you see **=>** in JS code.
It is an arrow function.

If you learned Python, then It is easy to think like Python's lambda function.

# How to use it?

I will change a traditional function to arrow function for example.

```js
function test(a, b) {
	return a + b;
}
```

you should remove 'function', name of function, and '()' like this:

```js
let test = (a, b) => a + b;
```

The End