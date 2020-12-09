---
marp: true
title: Unpoly 2
theme: unpoly2-slides
---


🥋 Targeting self-contained components
======================================

🎥 *Demo: DeskBot cards*

TODO: Implement self-contained components in the layers demo app, e.g. rating widget


**💡 We sometimes have multiple self-contained components on the same page.**

In Unpoly 2 the position of a clicked link may be considered when deciding which element to replace.

---

🥋 Example
----------

Let's say we have two links that replace `.card`:

```html
<div class="card" id="card1">
  Card #1 preview
  <a href="/cards/1" up-target=".card">Show full card #1</a>
</div>

<div class="card" id="card2">
  Card #2 preview
  <a href="/cards/2" up-target=".card">Show full card #2</a>
</div>
```

When clicking "Sow full card #2", Unpoly 1 would have replaced `.card`, matching the first card.

This makes it hard to use interactive components more than once in the same page.

---


🥋 Introducing `:closest`
-------------------------

In Unpoly 2, links may refer to the closest ancestor with the experimental `:closest` pseudo class:

```html
<div class="card" id="card1">
  Card #1 preview
  <a href="/cards/1" up-target=".card:closest">Show full card #1</a>
</div>

<div class="card" id="card2">
  Card #2 preview
  <a href="/cards/2" up-target=".card:closest">Show full card #2</a>
</div>
```

When clicking *"Show full card #2"*, Unpoly 2 will replace `#card2` matching the second card.

While the card container still requires a unique selector (e.g. `[id=card1]`), none of the content elements do need to know about it.

---

This also works with descendant selectors:

```html
<div class="card" id="card1">
  <a href="/cards/1/links" up-target=".card:closest .card-links">Show card #2 links</a>
  <div class="card-links"></div>
</div>

<div class="card" id="card2">
  <a href="/cards/2/links" up-target=".card:closest .card-links">Show card #2 links</a>
  <div class="card-links"></div>
</div>
```

When clicking *"Show card #2 links"*, Unpoly 2 will replace `#card2 .card-links`.
