---
description: "Pick the best way to add FeedSpring to your site: pre-made components, attributes, or the GraphQL API."
icon: signs-post
---

# Choose your setup

FeedSpring gives you three ways to put a feed on a site. They all use the same feed data, so you can switch later without reconnecting anything.

| Building in… | Use | Why |
| --- | --- | --- |
| **Framer** | [Pre-made Framer components](../build-with/framer.md) | Every setting is a property control, so you design visually with no code |
| **Webflow** | [Attributes in the Designer](../build-with/webflow.md) | Add attributes through the element settings panel, or start from a pre-made layout |
| **WordPress, Shopify, Squarespace, Wix, Webstudio or plain HTML** | [Attributes](../attributes/overview.md) | One script tag, no build step |
| **React or Next.js** | [GraphQL API or attributes](../build-with/react-nextjs.md) | Use the API for server rendering and SEO, attributes for the fastest client-side setup |
| **A backend, mobile app or build script** | [GraphQL API](../graphql-api/overview.md) | Feed data as JSON, outside the browser |

### Attributes or the GraphQL API?

If you are not using Framer, you are choosing between these two.

**Attributes** suit most marketing sites. You design the feed in your own HTML and FeedSpring fills in the data in the browser. There is no data fetching, error handling or build step to manage.

**The GraphQL API** gives you the feed as structured data to render however you like. Choose it when you need:

* Feed content in the page HTML, so search engines can index it. This matters most for Google Reviews, where review text is often content a business wants found.
* Server-side rendering or caching.
* To transform or combine feed data before it is displayed.
* To use feed data outside a website.

{% hint style="info" %}
Attributes render in the browser, so feed content is not part of the initial page HTML. For an Instagram grid that rarely matters. For a reviews section on a code-based site, consider the GraphQL API.
{% endhint %}

### Next steps

* [Quickstart](quickstart.md) — a live feed with attributes in two minutes
* [GraphQL Quickstart](../graphql-api/quickstart.md) — your first API query
