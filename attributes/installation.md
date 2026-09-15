---
description: Add the FeedSpring script for your feed source, find your Feed ID, and install on any platform.
---

# Installation

Attributes need two things on the page: the script for your feed source, and your Feed ID.

### 1. Add the script

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

### 2. Find your Feed ID

Your Feed ID is on the feed setup page in the [FeedSpring dashboard](https://app.feedspring.com). Each feed ID starts with a source prefix so the script knows which source to render:

| Prefix         | Source         |
| -------------- | -------------- |
| `inst_...`     | Instagram      |
| `google_...`   | Google Reviews |
| `tiktok_...`   | TikTok         |
| `dribbble_...` | Dribbble       |

The Feed ID prefix, the script, and the field names you use must all belong to the same source. A `google_` Feed ID with the Instagram script renders nothing.

### 3. Add your markup and publish

Add a feed wrapper and a post template to your page — see [A minimal feed](overview.md#a-minimal-feed) and [Fields & Elements](fields-and-elements.md). Replace `YOUR-FEED-ID` with the ID from your FeedSpring dashboard and publish the page. The feed renders on the next page load.

### Platform notes

The script and markup are the same everywhere. The only thing that changes is where you paste the script.

**Webflow.** See [Webflow](../build-with/webflow.md) for a step-by-step guide. In short: open your project, go to Site Settings → Custom Code and paste the `<script>` tag into the head code. For a feed on a single page only, use Page Settings → Custom Code instead. Add your HTML in a Webflow Embed element, or build the wrapper and post natively in Designer and add the `feedspring` and `feed-field` attributes via the element settings panel.

**Plain HTML, Shopify, WordPress, or any CMS.** Paste the `<script>` tag inside `<head>`. Add your markup anywhere in the `<body>`. No build step, no package install.

**React, Vue, Svelte, or any modern framework.** The Attributes method works inside a framework as long as the script has run before your component mounts. If you need server-side rendering, use the [GraphQL API](../graphql-api/overview.md) instead. [React & Next.js](../build-with/react-nextjs.md) covers both approaches.

### Next steps

* [Fields & Elements](fields-and-elements.md) — the three attributes in detail
* [Rendering](rendering.md) — static and dynamic rendering
