---
description: Control which items appear in your feed using dashboard filters and the limit and skip options.
icon: filter
---

# Filtering & Limits

Control what shows up in your feed. FeedSpring gives you two places to filter and limit content: the dashboard, and attributes on your feed wrapper.

### The two places to filter

**In your FeedSpring dashboard**, you set up rules that apply to the feed itself. These are server-side and persist across every page the feed appears on.

**In your HTML markup**, you use the `feed-options` attribute to control how many items render and which to skip for this specific feed placement.

Most setups use both. Dashboard filters shape the source data (e.g. hide reviews below 4 stars). Attribute options shape what renders on a specific page (e.g. show the first 4 items here, the next 6 on another page).

### Dashboard filters

Available filters depend on the feed source.

#### Google Reviews

* **High rating only** — exclude reviews below a star threshold you choose
* **Keyword filter** — only include reviews that mention specific words

Set both inside the FeedSpring dashboard under your Google Reviews feed.

#### Instagram, TikTok, Dribbble

Dashboard filtering is not currently available for these feeds. Use attribute options (below) to control rendering.

### Attribute options

`feed-options` is an attribute you place on the feed wrapper. It uses a pipe-delimited format:

```html
<section feedspring="YOUR-FEED-ID" feed-options="render:dynamic|limit:4|skip:2">
```

Order does not matter. Unknown option names are silently ignored (watch for typos).

Without the dynamic attribute, you copy a post template for each item you want to show. With `render:dynamic`, you write one template and FeedSpring clones it for you. In both cases, `limit` caps the number of posts rendered.

#### `limit:N`

`limit` applies in both rendering modes. In dynamic rendering, it caps how many posts are cloned. In static rendering, it caps how many of your post templates render — so if you place 6 post templates but set `limit:4`, only the first 4 render.

```html
<section feedspring="inst_..." feed-options="render:dynamic|limit:6">
```

#### `skip:N`

Skips the first N items before rendering, then `limit` caps how many of the remaining items render. Useful when you want to display a featured item elsewhere on the page and show the rest in a grid.

```html
<section feedspring="inst_..." feed-options="render:dynamic|skip:1|limit:6">
```

### Combining options with rendering options

`feed-options` also controls rendering behaviour (`render:static`, `render:dynamic`, `lang:xx-XX`, etc.). Combine filtering and rendering options with the pipe character:

```html
<section feedspring="inst_..." feed-options="render:dynamic|limit:8|skip:1|lang:en-GB">
```

For rendering options, see [Rendering Behaviour](rendering.md).

### Per-feed filtering examples

#### Showing only 5-star reviews

Set the high rating filter to 5 in the FeedSpring dashboard. Your feed will only return 5-star reviews, so all delivery methods (attributes, React, API) receive pre-filtered data.

#### Showing reviews about a specific product

Set the keyword filter in the dashboard (e.g. "checkout" or "customer service"). Your feed will only include reviews that mention those keywords.

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

### Next steps

* [Rendering Behaviour](rendering.md) — static vs dynamic, `appear`, `lang`, loading states
* [Attributes (HTML)](overview.md) — how to apply these options in HTML
* [Attributes Reference](reference.md) — the full spec
