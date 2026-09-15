---
description: Add a live feed to any HTML page with two attributes. No build step, no backend, no JavaScript to write.
icon: html5
---

# Attributes (HTML)

The Attributes delivery method lets you build a feed into any HTML page by adding two attributes to your own markup: one on a wrapper element, one on each post. FeedSpring handles data fetching, field injection, and updates automatically.

It is the fastest way to get a live feed on any website. No build step, no backend, no JavaScript to write. If you can edit HTML, you can ship a feed in minutes.

### When to use Attributes

Attributes is the right choice when:

* You are building in Webflow, WordPress, Shopify, Squarespace, Wix, Webstudio, or any tool that exposes HTML and `<head>` access.
* You want full design control without writing React or consuming an API.
* You need the fastest possible setup with no package installs or build tooling.
* You are prototyping, building marketing pages, or working in a no-code or low-code environment.

Choose a different delivery method if:

* You are working inside a React, Next.js, or similar framework, use React components.
* You are building in Framer and want design-time controls, use Framer components.
* You need server-side rendering, custom caching, or fully custom data handling, use the API.

### How it works

FeedSpring injects a small script into your page. On load, the script looks for any element with a `feedspring` attribute, fetches the feed data for that ID, then walks the DOM to find post wrappers and field targets. Your HTML stays yours, the data is layered on top.

The entire system uses three attributes:

| Attribute                   | Placed on                                                     | Purpose                                            |
| --------------------------- | ------------------------------------------------------------- | -------------------------------------------------- |
| `feedspring="YOUR-FEED-ID"` | A wrapper element                                             | Declares a feed container and which feed to render |
| `feedspring="post"`         | A repeating element inside the wrapper                        | Marks the template that repeats for each post      |
| `feed-field="FIELD-NAME"`   | Any element inside a post (or the wrapper for profile fields) | Targets where a piece of content is injected       |

That's the whole model. Everything else, filtering, limits, rendering mode, locale formatting, is configured with one optional `feed-options` attribute.

### Quick install

Three steps from a fresh account to a live feed.

#### 1. Add the script for your feed source

Paste the script for your feed type into your site's `<head>`. In Webflow, that's Site Settings → Custom Code → Head Code. In plain HTML, inside `<head>`.

```html
<!-- Instagram -->
<script src="https://scripts.feedspring.com/instagram-attrs.js" async defer></script>

<!-- Google Reviews -->
<script src="https://scripts.feedspring.com/google-reviews-attrs.js" async defer></script>

<!-- TikTok -->
<script src="https://scripts.feedspring.com/tiktok-attrs.js" async defer></script>

<!-- Dribbble -->
<script src="https://scripts.feedspring.com/dribbble-attrs.js" async defer></script>
```

Only load the scripts for feed types you are actually using.

#### 2. Build your wrapper and post

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

#### 3. Publish

Replace `YOUR-FEED-ID` with the ID from your FeedSpring dashboard and publish the page. The feed renders on the next page load.

### The three attributes in detail

#### `feedspring="YOUR-FEED-ID"`, the feed wrapper

Placed on any element that will contain the feed. Typically a `<div>` or `<section>`.

Each feed ID starts with a source prefix so the script knows which source to render:

| Prefix         | Source         |
| -------------- | -------------- |
| `inst_...`     | Instagram      |
| `google_...`   | Google Reviews |
| `tiktok_...`   | TikTok         |
| `dribbble_...` | Dribbble       |

Example:

```html
<section feedspring="inst_55ZVUQExmdej0j8vygUoP">
```

* Each page can have multiple wrappers, including multiple feeds from different sources.
* The wrapper is where you also add `feed-options` to control rendering.
* Any `feed-field` attribute placed on or inside the wrapper but outside a post template is treated as a profile-level field. Examples: follower count, average rating, business name.

#### `feedspring="post"`, the post template

The repeating element. FeedSpring finds the elements inside the wrapper with `feedspring="post"` and uses them to render per-item content.

* All `feed-field` attributes inside a post are post-level and receive the data for that specific post.
* Any styling, hover states, animations, or interactions you add to the template are preserved in each rendered copy.
* `feed-field="item"` is a supported alias for `feedspring="post"`, both mark an element as a post template.

#### `feed-field="FIELD-NAME"`, the data target

Placed on the element that should display a piece of data. The element type matters:

| Element           | Behaviour                                                                      |
| ----------------- | ------------------------------------------------------------------------------ |
| `<img>`           | The `src` attribute is set to the field value. `srcset` is removed if present. |
| `<a>`             | The `href` attribute is set to the field value.                                |
| `<iframe>`        | The `src` is set to the derived embed URL (used for TikTok video playback).    |
| Any other element | The field value is injected as text or HTML content.                           |

Some fields are written with `innerHTML` rather than `innerText`, which means HTML in the value is rendered, not escaped. This is true for:

* Instagram `caption`
* TikTok `title`, `description`, `name`, `bio`
* Dribbble `bio`

For every other field, values are written as plain text.

Available field names depend on the feed source. See the per-source pages for the full list:

* [Instagram fields](../feeds/instagram.md)
* [Google Reviews fields](../feeds/google-reviews.md)
* [TikTok fields](../feeds/tiktok.md)
* [Dribbble fields](../feeds/dribbble.md)

### The `bg` field

In addition to the element-type rules above, the `bg` field is a special case: applied to any element with `feed-field="bg"`, it sets `background-image: url(...)` rather than `src`. Useful for card backgrounds and hero covers where you want the image as a CSS background.

Currently Instagram only.

### Modifier attributes

Some rendering behaviours are controlled with additional attributes that sit alongside `feed-field`. These modify how the field is rendered.

#### `feed-timestamp`

Applied to an element with `feed-field="timestamp"`. Controls how the timestamp is formatted.

```html
<time feed-field="timestamp" feed-timestamp="from-now"></time>
```

* `from-now`, renders as a relative time: "2 days ago", "a month ago"
* Any other value is passed to [Day.js format strings](https://day.js.org/docs/en/display/format), for example `MMMM D, YYYY`, `DD/MM/YY`, or `HH:mm`
* If omitted, the default format is `MMMM D, YYYY`

Works on all feeds that expose a `timestamp` field: Instagram, Google Reviews, TikTok, Dribbble.

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

### Loading state

If you want to show a custom placeholder while the feed loads, wrap it in an element with `feedspring="loading"`. FeedSpring removes this element once feed rendering starts:

```html
<div feedspring="YOUR-FEED-ID" feed-options="render:dynamic">
  <div feedspring="loading">
    <p>Loading posts...</p>
  </div>
  <div feedspring="post">
    <img feed-field="img" alt="" />
  </div>
</div>
```

### Install in specific platforms

The script and markup are the same everywhere. The only thing that changes is where you paste the script.

**Webflow.** Open your project, go to Site Settings → Custom Code and paste the `<script>` tag into the head code. For a feed on a single page only, use Page Settings → Custom Code instead. Add your HTML in a Webflow Embed element, or build the wrapper and post natively in Designer and add the `feedspring` and `feed-field` attributes via the element settings panel.

**Plain HTML, Shopify, Wordpress, or any CMS.** Paste the `<script>` tag inside `<head>`. Add your markup anywhere in the `<body>`. No build step, no package install.

**React, Vue, Svelte, or any modern framework.** The Attributes method works inside a framework as long as the script has run before your component mounts. For most frameworks we recommend the dedicated [React component delivery method](../build-with/react-nextjs.md) instead, which is designed for component-based environments.

### Next steps

* See which fields are available for your feed in the [Feeds reference](../README.md#what-this-documentation-covers)
* Tune filtering and limits with [Filtering & Limits](feed-options.md)
* Understand [Rendering Behaviour](rendering.md) (static vs dynamic, `appear`, locale)
* If you are an AI agent building a feed, see the [full attribute reference](reference.md)
