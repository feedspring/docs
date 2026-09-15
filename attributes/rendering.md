---
description: Static and dynamic rendering, combined layouts, and how rendered posts appear.
icon: chart-simple-horizontal
---

# Rendering

How FeedSpring turns your post templates into rendered posts: the two rendering modes, combining them in one layout, and how rendered posts appear.

### Rendering modes

FeedSpring renders feed data in one of two modes: **static** or **dynamic**. Static is the default — if you don't set a render option, you copy a post template for each item you want to show. Dynamic clones a single template for you. Both respect `limit`.

#### Static rendering (default)

`render:static` fills already-present post templates in document order. You place each template by hand, FeedSpring fills them in with data from the 1st, 2nd, 3rd post, and so on.

```html
<section feedspring="inst_...">
  <article feedspring="post" class="hero">
    <img feed-field="img" alt="" />
    <h2 feed-field="caption"></h2>
  </article>

  <div class="secondary-grid">
    <article feedspring="post" class="small">
      <img feed-field="img" alt="" />
    </article>
    <article feedspring="post" class="small">
      <img feed-field="img" alt="" />
    </article>
    <article feedspring="post" class="small">
      <img feed-field="img" alt="" />
    </article>
  </div>
</section>
```

This renders 4 posts total: the first fills the hero, the next 3 fill the small cards.

Use static when:

* You want different-shaped templates for different positions (hero + grid, featured + sidebar)
* The layout is hand-designed and each slot has a specific purpose
* You want pixel-perfect control over every card individually

`limit` still applies in static mode: it caps how many of your placed templates render. If you place 6 templates but set `limit:4`, only the first 4 render.

#### Dynamic rendering

`render:dynamic` clones a single post template for each returned item. Design one post card, FeedSpring duplicates it.

```html
<section feedspring="inst_..." feed-options="render:dynamic|limit:6">
  <article feedspring="post">
    <img feed-field="img" alt="" />
    <p feed-field="caption"></p>
  </article>
</section>
```

This renders 6 copies of the `<article>` template, each populated with a different post.

Use dynamic when:

* You want a grid, slider, or list where every item has the same structure
* The number of items is driven by your feed, not by your layout
* You want consistent design across every post

This is the right choice for about 90% of feeds.

{% hint style="danger" %}
**Review needed (Ilya): `limit` and `skip` behaviour.** The docs and the current script disagree, so this section is not final.

* **Current script:** in dynamic rendering, `limit` is a maximum item *index*, so `skip:2|limit:4` shows 2 posts. In static rendering, `limit` is ignored. With `skip`, the unused template stays on the page as an empty card.
* **Intended behaviour:** `limit` is the number of posts shown, in both rendering modes, so `skip:2|limit:4` shows 4 posts.

Confirm which behaviour the docs should describe (and whether the script is changing), then update this section and remove this callout.
{% endhint %}

#### Choosing between them

|                   | Dynamic                 | Static                                  |
| ----------------- | ----------------------- | --------------------------------------- |
| Number of items   | Controlled by `limit`   | Controlled by number of templates       |
| Template count    | One                     | Many                                    |
| Best for          | Grids, sliders, masonry | Featured + grid, magazine-style layouts |
| `limit` supported | Yes                     | No                                      |
| `skip` supported  | Yes                     | Yes                                     |

#### Hybrid layouts

Combine both on the same page by using two wrappers pointing at the same feed. See the featured + grid example in [Filtering & Limits](feed-options.md).

### The `appear` option

Controls how the post template reveals itself when rendered. Placed on the `feedspring="post"` element (not the wrapper).

#### `appear:display-none` (default)

If the post template has `style="display:none"`, that inline display value is cleared when rendering. This lets you hide the template from view while the feed loads, so an empty skeleton doesn't flash before the real content arrives.

```html
<article feedspring="post" style="display:none">
  <img feed-field="img" alt="" />
</article>
```

This is the default behaviour. No configuration needed.

#### `appear:display-block`

Forces the rendered template to `display: block`.

```html
<article feedspring="post" feed-options="appear:display-block">
```

#### `appear:display-flex`

Forces the rendered template to `display: flex`. Useful when the template uses flexbox and you need to ensure the layout applies after FeedSpring renders.

```html
<article feedspring="post" feed-options="appear:display-flex">
```

### Next steps

* [Feed Options](feed-options.md) — `limit`, `skip` and `lang`
* [Loading & Events](loading-and-events.md) — placeholders while the feed loads
