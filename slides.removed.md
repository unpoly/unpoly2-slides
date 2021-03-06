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

This is all of `unpoly-bootstrap4.scss`:

```css
up-modal, up-drawer, up-popup {
  .container, .container-fluid {
    padding-left: 0;  /* The overlay box already has padding */
    padding-right: 0; /* The overlay box already has padding */
    max-width: none;  /* The overlay box already has max-width */
  }
}
```



<div class="columns">

Kill boilerplate configuration
------------------------------
- Configure default targets.
- Handle all links/forms without helpers and macros.
- Minimal Bootstrap integration you actually want to use.

New layers
----------
- A new *layer* API replaces modals and popups. The root page is also a layer.
- Layers are now isolated. They cannot accidentally target another layer, or accidentally react to events from other layers.
- Layers may be stacked infinitely.
- Customize layers with fixed sizes, classes, custom dismissability.
- New full-screen overlay mode `cover`.

Sub-interactions
----------------
- Complex interactions may be paused by branching off a sub-interaction into an overlay.
- When opening an overlay you may define a *condition* when the sub-interaction ends.
- Overlay results (selection, finished record creation) are propagated back to parent layer.
- JavaScript may treat overlays as an async operation with a result value.

Navigation intent
------------------
- Not every fragment update means a user navigation (switching screens vs. updating a small fragment).
- Only user navigation scrolls, focuses, falls back to default targets.
- User navigation now aborts earlier requests.

Accessibility
-------------
- New `{ focus }` option lets you control focus after a fragment update.
- Overlays are accessible by trapping focus and removing other layers from the accessibility tree.
- Focus, selection, scroll positions within updated fragments are now preserved.
- Support keyboard navigations for all interactive elements.

Quality of live improvements
----------------------------
- Unified fragment update API.
- Server can emit emits, close overlays, clear the cache, ...
- Event handler may change render options.
- Calmer scrolling.
- Polling.
- New `:closest` selector for targeting self-contained components.
- Preloading eats less bandwidth and server resources

</div>

----
