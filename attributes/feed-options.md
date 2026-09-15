---
description: Control how many posts render, which posts to skip, and the locale used for numbers, with the feed-options attribute.
icon: filter
---

# Feed Options

`feed-options` is an attribute you place on the feed wrapper. It uses a pipe-delimited format:

```html
<section feedspring="YOUR-FEED-ID" feed-options="render:dynamic|limit:4|skip:2">
```

Order does not matter. Unknown option names are silently ignored (watch for typos).

Without the dynamic attribute, you copy a post template for each item you want to show. With `render:dynamic`, you write one template and FeedSpring clones it for you. In both cases, `limit` caps the number of posts rendered.

{% hint style="danger" %}
**Review needed (Ilya): `limit` and `skip` behaviour.** The docs and the current script disagree, so this section is not final.

* **Current script:** in dynamic rendering, `limit` is a maximum item *index*, so `skip:2|limit:4` shows 2 posts. In static rendering, `limit` is ignored. With `skip`, the unused template stays on the page as an empty card.
* **Intended behaviour:** `limit` is the number of posts shown, in both rendering modes, so `skip:2|limit:4` shows 4 posts.

Confirm which behaviour the docs should describe (and whether the script is changing), then update this section and remove this callout.
{% endhint %}

### `limit:N`

`limit` applies in both rendering modes. In dynamic rendering, it caps how many posts are cloned. In static rendering, it caps how many of your post templates render — so if you place 6 post templates but set `limit:4`, only the first 4 render.

```html
<section feedspring="inst_..." feed-options="render:dynamic|limit:6">
```

### `skip:N`

Skips the first N items before rendering, then `limit` caps how many of the remaining items render. Useful when you want to display a featured item elsewhere on the page and show the rest in a grid.

```html
<section feedspring="inst_..." feed-options="render:dynamic|skip:1|limit:6">
```

### `lang` — locale and number formatting

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

### Combining options with rendering options

`feed-options` also controls rendering behaviour (`render:static`, `render:dynamic`, `lang:xx-XX`, etc.). Combine filtering and rendering options with the pipe character:

```html
<section feedspring="inst_..." feed-options="render:dynamic|limit:8|skip:1|lang:en-GB">
```

For rendering options, see [Rendering](rendering.md).

### Examples

#### Showing the 4 most recent Instagram posts

Place `feed-options="render:dynamic|limit:4"` on the feed wrapper. Dynamic rendering always returns items in descending chronological order, so `limit:4` gives you the 4 most recent.

#### Featured post plus a grid of the rest

Use two wrappers pointing at the same feed:

```html
<!-- Featured post (first item only) -->
<div feedspring="inst_YOUR-FEED-ID">
  <article feedspring="post" class="hero">
    <img feed-field="img" alt="" />
  </article>
</div>

<!-- Grid of the next 6 posts -->
<div feedspring="inst_YOUR-FEED-ID" feed-options="render:dynamic|skip:1|limit:6">
  <article feedspring="post" class="small">
    <img feed-field="img" alt="" />
  </article>
</div>
```

The first wrapper renders index 0, the second wrapper skips it and renders indexes 1 through 6.

### Dashboard filters

Filtering by star rating or keyword happens in the FeedSpring dashboard, not in `feed-options`. See [Filtering](../core-concepts/filtering.md).

### Next steps

* [Rendering](rendering.md) — static and dynamic rendering
* [Attributes Reference](reference.md) — every option on one page
