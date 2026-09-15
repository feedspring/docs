---
description: Build FeedSpring feeds visually in the Webflow Designer using custom attributes and pre-made components.
icon: webflow
---

# Webflow

Webflow is a first-class FeedSpring integration. Feeds are built using the Attributes system, added visually in the Webflow Designer — no code editor required. Add the script once, then apply `feedspring` and `feed-field` attributes to elements through the Designer's settings panel.

Because Webflow is one of our largest and most active customer bases, we provide a growing library of [pre-made components](https://www.feedspring.com/components) you can copy and paste directly into your project to get started in seconds.

### **How it works in Webflow**

FeedSpring uses HTML attributes, and Webflow lets you add custom attributes to any element visually. So the entire attributes workflow applies — you just add the attributes through the Designer instead of typing them into markup.

The three core attributes are the same everywhere:

| Attribute                   | Placed on                              | Purpose                                            |
| --------------------------- | -------------------------------------- | -------------------------------------------------- |
| `feedspring="YOUR-FEED-ID"` | A wrapper element                      | Declares a feed container and which feed to render |
| `feedspring="post"`         | A repeating element inside the wrapper | Marks the template that repeats for each post      |
| `feed-field="FIELD-NAME"`   | Any element inside a post              | Targets where a piece of content is injected       |

For the full attribute model, rendering modes, and field behaviour, see [Attributes](../attributes/overview.md).

### **Quick start**

#### **1. Add the script**

In Webflow, go to **Site Settings → Custom Code → Head Code** and paste the script for your feed source. For a feed on a single page only, use **Page Settings → Custom Code** instead.

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

Only add the script for the feed types you are using.

#### **2. Add the feed wrapper**

Add a `<div>` (or any container) to your canvas. In the element settings panel, open **Custom Attributes** and add:

* Name: `feedspring`
* Value: `YOUR-FEED-ID`

Your feed ID is available on the feed setup page in the [FeedSpring dashboard](https://app.feedspring.com).

To control rendering, add a second attribute:

* Name: `feed-options`
* Value: `render:dynamic|limit:6`

#### **3. Add the post wrapper**

Add a child `<div>` inside the feed wrapper. In its Custom Attributes, add:

* Name: `feedspring`
* Value: `post`

This is the template that repeats for each post.

#### **4. Add field attributes**

Inside the post wrapper, add the elements you want (images, text, links) and give each a custom attribute:

* Name: `feed-field`
* Value: the field name (e.g. `img`, `caption`, `timestamp`)

Remember to place image fields on Image elements and link fields on Link elements — the element type determines the behaviour. See [Fields & Elements](../attributes/fields-and-elements.md) for the full element rules.

#### **5. Publish**

Publish your site and the feed renders on the next page load.

### **Pre-made components**

The fastest way to start is to copy a ready-built component from the [FeedSpring Components library](https://www.feedspring.com/components). Each component is a fully styled Webflow layout with the wrapper, post template, and field attributes already in place. Paste it into your project, swap in your feed ID, and customise the styling to match your site.

#### **Available fields**

Field names depend on the feed source. See the per-source pages for the full list:

* [Instagram fields](../feeds/instagram.md)
* [Google Reviews fields](../feeds/google-reviews.md)
* [TikTok fields](../feeds/tiktok.md)
* [Dribbble fields](../feeds/dribbble.md)

#### **Next steps**

* [Attributes](../attributes/overview.md) — the full attribute model
* [Feed Options](../attributes/feed-options.md) — `limit`, `skip` and `lang`
* [Rendering](../attributes/rendering.md) — static vs dynamic, `appear`
* [Attributes Reference](../attributes/reference.md) — every field, for every source
