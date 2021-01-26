---
marp: true
title: Unpoly 2
theme: unpoly2-slides
---


-----
<!-- _class: pro -->

# Accessible JavaScript is hard

Is this accessible for visually impaired users?\
<https://codepen.io/triskweline/pen/abmZZJb>


----
<!-- _class: pro -->

Issues with this code
---------------------

- The *Show details* and *Hide details* buttons cannot be focused
- The *Show details* and *Hide details* buttons cannot be activated with the keyboard
- When the modal is opened, the modal should be focused.
- When the modal is closed, the link that opened the modal should be focused.
- While the modal is open, background elements should be removed from the accessibility tree.
- While the modal is open, focus should be trapped in a cycle within the modal.


---


You can customize this with `up.fragment.config.autoFocus`.

----
<!-- _class: pro -->


```js
up.fragment.config.autoFocus // returns ['keep', 'autofocus', 'target']

// Ignore [autofocus] elements
up.fragment.config.autofocus = ['keep', 'target']


THIS EXAMPLE IS NOT GOOD


// Focus form errors
up.fragment.config.autoFocus = function(fragment) {
  if (fragment.matches('form') && fragment.querySelector('.errors')) {
    return '.errors'
  else
    // If we don't have a form with errors, focus the new fragment
    return fragment  
  }
}
```





---
<!-- _class: pro -->

CSS selector extensions
==========================

| Target | What will be updated |
|--------|-------------------------------------|
| `.foo`                | An element matching `.foo`.      |
| `.foo:has(.bar)`      | An element matching `.foo` with a descendant matching `.bar`. |
| `:none`               | Nothing, but we still make a server request. |
| `:layer`              | The first element in the current layer. |
| `:main`               | Any element matching a main selector (default `main, [up-main]`).
| `.foo:closest`        | The activated element's closest ancestor matching `.foo`. |
| `.foo:closest .child `| An element matching `.child`  within the activated element's closest ancestor matching `.foo`. |




---

<!-- _class: pro -->


Setting a default transition
============================

I'm not a big fan of animating *every* fragment update.

**But since some of you do this**, here is how to set a default transition:

```js
up.fragment.config.navigateOptions.transition = 'cross-fade'
```

We're going to learn more about `up.fragment.config.navigateOptions` in a minute.







---
<!-- _class: pro -->

Remembering the current layer
=============================

Like most functions, `up.layer.dismiss()` will affect the "current" layer,\
so it's an alias for `up.layer.current.dismiss()`.

`up.layer.current` is set to the right layer in compilers and most events, even if that layer is not the "front" layer. E.g. if you're compiling a fragment for a background layer, `up.layer.current` will be
the background layer during compilation.


If you have async code, the current layer may change when your callback is called.\
You may also retrieve the current layer for later reference:

```js
function dismissCurrentIn(seconds) {
  let savedLayer = up.layer.current // returns an up.Layer object
  let dismiss = () => savedLayer.dismiss()
  setTimeout(dismiss, seconds * 1000)
}

dismissCurrentIn(10) // 
```



---
<!-- _class: pro -->

Return value contains information about the request
--------------------------------------------------

```js
let request = up.request('/api/v3/foo')
console.log(request.method) // logs "GET"

// The request is also a promise for its response.
let response = await request
console.log(response.text)
```



---
<!-- _class: pro -->

Content Security Policy
=======================

By choosing a strict CSP, you also decide against using event handlers in your HTML.

Handlers like `[up-on-accepted]` cannot work with CSP:

```html
<a href="/contacts/new"
  up-layer="new"
  up-accept-location="/contacts/$id"
  up-on-accepted="up.reload('.table')">
  ...
</a>  
```

You will need to move your JavaScript code into your JavaScript sources.

---
<!-- _class: pro -->

This is easier now that event handlers can change render options:

```html
<a href="/contacts/new" class="add-contact" up-follow>...</a>
```

```js
up.on('up:link:follow', '.add-contact', function(event) {
  event.renderOptions.layer = 'new'
  event.renderOptions.acceptLocation = '/contacts/$id'
  event.renderOptions.onAccepted = (event) => up.reload('.table')
})
```
