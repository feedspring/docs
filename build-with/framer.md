---
description: Copy a FeedSpring component into Framer and control it entirely through property controls.
icon: square-dashed
---

# Framer

Framer runs React components natively, and FeedSpring components are standard React components. That means the full library of pre-made FeedSpring layouts works inside Framer, with every setting exposed as a property control — no code required.

This is the best option if you want a visual workflow without losing flexibility.

**When to use Framer**

Use Framer components if you are:

* Building in Framer
* Designing visually inside the canvas
* Wanting to control feed settings without writing raw code
* Looking for a component you can copy, style, and extend

If you want complete control in code, use React or the API instead.

**How it works**

The Framer component connects to your FeedSpring feed and renders items using built-in layout and styling controls.

Instead of adding raw attributes to HTML, you edit the component through Framer's property controls — a sidebar of design-time options that match the rest of Framer's editing experience.

**What you can control**

The current component exposes visual controls for:

* Feed ID
* Number of items
* Skip count
* Font settings
* Container settings
* Card settings
* Image sizing
* Radius
* Overlay colour
* Background colour
* Text truncation

**Going beyond the controls**

The property controls cover the common cases, but the component itself is editable code. To customise further — change the structure, add fields, or build a new layout — open the code component in Framer and edit the JSX directly. Everything is JavaScript and React under the hood.

**Example workflow**

1. Copy a FeedSpring component link below into your Framer project
2. Enter your Feed ID
3. Choose how many items to show
4. Adjust skip, typography, container, and card styles
5. Publish your site

**Available components**

Browse the full library at [feedspring.com/components](https://www.feedspring.com/components). A selection of components to copy and paste into Framer:

**Google Reviews**

* [https://framer.com/m/feedspring-google-reviews-basic-grid-V6Zp.js](https://framer.com/m/feedspring-google-reviews-basic-grid-V6Zp.js)
* [https://framer.com/m/feedspring-google-reviews-center-grid-hyBB.js](https://framer.com/m/feedspring-google-reviews-center-grid-hyBB.js)
* [https://framer.com/m/feedspring-google-reviews-basic-slider-WFXd.js](https://framer.com/m/feedspring-google-reviews-basic-slider-WFXd.js)

**Instagram**

* [https://framer.com/m/feedspring-instagram-image-grid-sBsr.js](https://framer.com/m/feedspring-instagram-image-grid-sBsr.js)
* [https://framer.com/m/feedspring-instagram-highlight-grid-NQGf.js](https://framer.com/m/feedspring-instagram-highlight-grid-NQGf.js)
* [https://framer.com/m/feedspring-instagram-basic-slider-P3qR.js](https://framer.com/m/feedspring-instagram-basic-slider-P3qR.js)

For the complete and most up-to-date set, visit [feedspring.com/components](https://www.feedspring.com/components).

**Post fields used in the Instagram component**

The default Instagram Framer component reads the following from each post:

* Post image
* Post caption
* Post permalink
* Like count

**Profile fields used in the Instagram component**

For account-level elements, the component reads from the feed's profile data:

* Profile avatar
* Profile username

Different feed sources (Google Reviews, TikTok, Dribbble) expose different fields. See the [Attributes Reference](../attributes/reference.md) for the full list.

**Building a fully custom component**

If you need a layout the property controls can't reach, open a code component in Framer and write it yourself. You have two ways to get feed data into it:

* **[Attributes](../attributes/overview.md)** — load the attributes script and use `feedspring` and `feed-field` attributes in your JSX. Fastest, no data layer.
* **[GraphQL API](../graphql-api/overview.md)** — fetch the feed yourself and render from raw data. Use this when you need to transform or combine data, or share one data layer across systems.

See [React & Next.js](react-nextjs.md) for worked examples of both.

**Next steps**

* [React & Next.js](react-nextjs.md) — building your own components with the GraphQL API or attributes
* [Attributes Reference](../attributes/reference.md) — every available field, for every feed source
* [Posts & Fields](../core-concepts/posts-and-fields.md) — how feeds and fields are structured
* [GraphQL API](../graphql-api/overview.md) — fetching feed data directly
