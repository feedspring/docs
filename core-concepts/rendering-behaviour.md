---
description: Learn about the FeedSpring product from a top-level overview.
icon: chart-simple-horizontal
---

# Rendering Behaviour

## Rendering Behaviour

How FeedSpring renders data into your markup: the two rendering modes, how templates appear, locale formatting, and what happens with text content.

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

#### Choosing between them

|                   | Dynamic                 | Static                                  |
| ----------------- | ----------------------- | --------------------------------------- |
| Number of items   | Controlled by `limit`   | Controlled by number of templates       |
| Template count    | One                     | Many                                    |
| Best for          | Grids, sliders, masonry | Featured + grid, magazine-style layouts |
| `limit` supported | Yes                     | No                                      |
| `skip` supported  | Yes                     | Yes                                     |

#### Hybrid layouts

Combine both on the same page by using two wrappers pointing at the same feed. See the featured + grid example in Filtering & Limits.

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

### Loading state

If you want to show a custom placeholder while the feed loads, wrap it in an element with `feedspring="loading"`. FeedSpring removes this element once feed rendering starts:

```html
<div feedspring="inst_YOUR-FEED-ID" feed-options="render:dynamic">
  <div feedspring="loading">
    <p>Loading posts...</p>
  </div>
  <article feedspring="post">
    <img feed-field="img" alt="" />
  </article>
</div>
```

For a shimmer or skeleton, style the `feedspring="loading"` element however you want. The rule is simple: it's in the DOM until the feed renders, then it's gone.

### Locale and number formatting

FeedSpring formats numbers (like, follower, view counts) with `Intl.NumberFormat` in compact notation. The result is locale-aware: `1,234` renders as `1.2K` in `en-US` and `1,2 k` in `fr-FR`.

#### `lang:xx-XX`

Set the locale explicitly:

```html
<section feedspring="inst_..." feed-options="lang:en-GB">
```

Any standard [BCP 47 locale string](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Intl#locales_argument) is accepted.

#### `lang:auto`

Use the browser's runtime locale instead of defaulting to `en-US`:

```html
<section feedspring="inst_..." feed-options="lang:auto">
```

Good for multilingual sites where you want the feed to match each visitor's browser settings.

### Element behaviour by type

The element a `feed-field` sits on controls what happens:

| Element           | Behaviour                                                   |
| ----------------- | ----------------------------------------------------------- |
| `<img>`           | Sets `src` to the field value. Removes `srcset` if present. |
| `<a>`             | Sets `href` to the field value.                             |
| `<iframe>`        | Sets `src` to a derived embed URL (TikTok `video` only).    |
| Any other element | Inserts field value as text or HTML                         |

### Next steps

* Filtering & Limits — `limit`, `skip`, and dashboard filters
* Attributes delivery method — start here if you are building with HTML
* Attribute Reference — the full spec
