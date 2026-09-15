---
description: Build a live feed into any HTML page with three attributes. No build step, no backend, no JavaScript to write.
icon: html5
---

# Attributes

The Attributes delivery method lets you build a feed into any HTML page by adding two attributes to your own markup: one on a wrapper element, one on each post. FeedSpring handles data fetching, field injection, and updates automatically.

It is the fastest way to get a live feed on any website. No build step, no backend, no JavaScript to write. If you can edit HTML, you can ship a feed in minutes.

### When to use Attributes

Attributes are the right choice when:

* You are building in Webflow, WordPress, Shopify, Squarespace, Wix, Webstudio, or any tool that exposes HTML and `<head>` access.
* You want full design control without writing a data layer or consuming an API.
* You need the fastest possible setup, with no package installs or build tooling.
* You are prototyping, building marketing pages, or working in a no-code or low-code environment.

Choose a different method if:

* You are building in Framer and want design-time controls — use [Framer components](../build-with/framer.md).
* You need server-side rendering, feed content in the page HTML for SEO, caching, or custom data handling — use the [GraphQL API](../graphql-api/overview.md).
* You are not sure — see [Choose your setup](../getting-started/choose-your-setup.md).

### How it works

FeedSpring injects a small script into your page. On load, the script looks for any element with a `feedspring` attribute, fetches the feed data for that ID, then walks the DOM to find post wrappers and field targets. Your HTML stays yours, the data is layered on top.

The entire system uses three attributes:

| Attribute                   | Placed on                                                     | Purpose                                            |
| --------------------------- | ------------------------------------------------------------- | -------------------------------------------------- |
| `feedspring="YOUR-FEED-ID"` | A wrapper element                                             | Declares a feed container and which feed to render |
| `feedspring="post"`         | A repeating element inside the wrapper                        | Marks the template that repeats for each post      |
| `feed-field="FIELD-NAME"`   | Any element inside a post (or the wrapper for profile fields) | Targets where a piece of content is injected       |

That's the whole model. Everything else, filtering, limits, rendering mode, locale formatting, is configured with one optional `feed-options` attribute.

### A minimal feed

Mark up a container and a single post template. Inside the post, place elements with `feed-field` attributes wherever you want data to appear.

```html
<div feedspring="YOUR-FEED-ID" feed-options="render:dynamic">
  <div feedspring="post">
    <img feed-field="img" alt="" />
    <p feed-field="caption"></p>
    <a feed-field="link">View on Instagram</a>
  </div>
</div>
```

The `feedspring="post"` element acts as a template. In `render:dynamic` mode, FeedSpring clones it for each post in the feed and injects the right data into each copy.


### A complete working example

This is a copy-pasteable Instagram grid. Drop it into any HTML page with the Instagram script installed and replace the feed ID.

```html
<section feedspring="YOUR-FEED-ID" feed-options="render:dynamic|limit:8">
  <div class="grid">
    <article feedspring="post" class="post">
      <img feed-field="img" alt="" class="post-image" />
      <div class="post-meta">
        <time feed-field="timestamp" feed-timestamp="from-now"></time>
        <span feed-field="like-count"></span> likes
      </div>
      <a feed-field="link" target="_blank" rel="noopener" class="post-link">View</a>
    </article>
  </div>
</section>

<style>
  .grid { display: grid; grid-template-columns: repeat(4, 1fr); gap: 16px; }
  .post { position: relative; aspect-ratio: 1 / 1; border-radius: 16px; overflow: hidden; }
  .post-image { width: 100%; height: 100%; object-fit: cover; }
  .post-meta { position: absolute; inset: auto 0 0 0; padding: 12px; color: white; background: linear-gradient(transparent, rgba(0,0,0,0.6)); }
  .post-link { position: absolute; inset: 0; }
  @media (max-width: 768px) { .grid { grid-template-columns: repeat(2, 1fr); } }
</style>
```

### In this section

| Page | What it covers |
| --- | --- |
| [Installation](installation.md) | Scripts for each source, Feed IDs, and platform notes |
| [Fields & Elements](fields-and-elements.md) | The three attributes in detail, element types, post and profile fields |
| [Rendering](rendering.md) | Static and dynamic rendering, combined layouts, `appear` |
| [Feed Options](feed-options.md) | `limit`, `skip` and `lang` |
| [Loading & Events](loading-and-events.md) | Loading placeholders, events, Webflow Interactions |
| [Special Fields](special-fields.md) | Background images, timestamps, stars, verified badges and tags |
| [Troubleshooting](troubleshooting.md) | A checklist, common mistakes and known issues |
| [Attributes Reference](reference.md) | Every attribute, option and field on one page |
