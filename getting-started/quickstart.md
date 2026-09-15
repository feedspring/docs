---
description: Get a live FeedSpring feed on your site in under two minutes using HTML attributes.
icon: book
---

# Quickstart

Get a live FeedSpring feed on your site in under 2 minutes.

This example uses the **Attributes (HTML) method**, the fastest way to get started.

FeedSpring works the same across all feed types — Instagram, Google Reviews, TikTok and Dribbble.

<figure><img src="../.gitbook/assets/divider-blog.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/main-cta.png" alt=""><figcaption></figcaption></figure>

### Step 1 — Add the script

Add the script for your feed type to your site `<head>`

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

👉 Only include the script for the feed you want to use.

<figure><img src="../.gitbook/assets/divider-blog.png" alt=""><figcaption></figcaption></figure>

### Step 2 — Add your feed wrapper

Add a wrapper element using your FeedSpring feed ID.

For testing, you can use this Instagram feed ID: `inst_55ZVUQExmdej0j8vygUoP`

```html
<div 
  feedspring="inst_55ZVUQExmdej0j8vygUoP" 
  feed-options="render:dynamic|limit:6|lang:en">
</div>
```

The `feed-options` attribute is optional and allows you to configure how your feed renders.

| Option           | Description                                                             |
| ---------------- | ----------------------------------------------------------------------- |
| `render:dynamic` | Automatically clones a single reusable post template for each feed item |
| `limit:6`        | Limits the number of rendered posts                                     |
| `lang:en`        | Sets the feed language                                                  |

<figure><img src="../.gitbook/assets/divider-blog.png" alt=""><figcaption></figcaption></figure>

### Step 3 — Add a post layout

Add a single reusable post template inside the feed wrapper.

With `render:dynamic` set on the wrapper, FeedSpring clones this one `<div feedspring="post">` template for every post in your feed — so you only design the layout once. Without this attribute, you'll need to duplicate each post structure depending on how many posts you want to display.

```html
<div 
  feedspring="inst_55ZVUQExmdej0j8vygUoP" 
  feed-options="render:dynamic|limit:6|lang:en">

  <div feedspring="post">
    <img feed-field="img" alt="" />
    <p feed-field="caption"></p>
    <a feed-field="link" target="_blank" rel="noopener">
      View post
    </a>
  </div>
</div>
```

<figure><img src="../.gitbook/assets/divider-blog.png" alt=""><figcaption></figcaption></figure>

### Step 4 — Done

Your feed will now render automatically.

<figure><img src="../.gitbook/assets/divider-blog.png" alt=""><figcaption></figcaption></figure>

### Optional — Limit the number of posts

For optimal performance, we recommend setting a post limit in your FeedSpring dashboard. You can also specify a limit directly through the feed options.

```
<div feedspring="YOUR_FEED_ID" feed-options="limit:6">
```

***

### How it works

FeedSpring looks for three things:

* `feedspring="ID"` → connects to your feed
* `feedspring="post"` → repeats for each item
* `feed-field="..."` → inserts data

***

### What next

* Customize your layout with your own styles
* View all available fields
  * [Instagram](../feeds/instagram.md)
  * [Google Reviews](../feeds/google-reviews.md)
  * [TikTok](../feeds/tiktok.md)
  * [Dribbble](../feeds/dribbble.md)
* Learn more about [feed options](../core-concepts/filtering-and-limits.md)

***

### Using React, API, or Framer?

If you're not using Attributes (HTML) or Webflow:

* **React or Next.js** → see [React Components](../delivery-methods/react-components.md)
* **Framer** → see [Framer Components](../delivery-methods/framer-components.md)
* **Raw data access** → see the [API (GraphQL)](../delivery-methods/api-graphql.md)

***

### Plan update rates

Post limits and refresh frequency depend on your plan and feed source.

👉 See [Plan update rates](../core-concepts/feeds-and-syncing.md#plan-update-rates) in Feeds & Syncing for the full breakdown.
