---
description: Filter which content a feed contains in the FeedSpring dashboard, for every delivery method at once.
icon: filter
---

# Filtering

Filtering decides which content a feed contains. Because it happens in the FeedSpring dashboard, a filter applies everywhere the feed is used.

### The two places to filter

**In your FeedSpring dashboard**, you set up rules that apply to the feed itself. These are server-side and persist across every page the feed appears on.

**Where you display the feed**, you control how many items show and which to skip for that placement — with [`feed-options`](../attributes/feed-options.md) for attributes, or in your own code with the [GraphQL API](../graphql-api/querying-feeds.md).

Most setups use both. Dashboard filters shape the source data (e.g. hide reviews below 4 stars). Display options shape what renders on a specific page (e.g. show the first 4 items here, the next 6 on another page).

### Dashboard filters

Available filters depend on the feed source.

#### Google Reviews

* **High rating only** — exclude reviews below a star threshold you choose
* **Keyword filter** — only include reviews that mention specific words

Set both inside the FeedSpring dashboard under your Google Reviews feed.

#### Instagram, TikTok, Dribbble

Dashboard filtering is not currently available for these feeds. Control what shows where the feed is displayed instead.

### Examples

#### Showing only 5-star reviews

Set the high rating filter to 5 in the FeedSpring dashboard. Your feed will only return 5-star reviews, so all delivery methods (attributes, Framer components and the GraphQL API) receive pre-filtered data.

#### Showing reviews about a specific product

Set the keyword filter in the dashboard (e.g. "checkout" or "customer service"). Your feed will only include reviews that mention those keywords.

### Next steps

* [Feed Options](../attributes/feed-options.md) — `limit` and `skip` with attributes
* [Querying Feeds](../graphql-api/querying-feeds.md) — item limits with the GraphQL API
