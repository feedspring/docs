---
description: What a feed is, how FeedSpring keeps it updated, and how often content refreshes on each plan.
icon: arrows-spin
---

# Feeds & Syncing

A feed is a connection between FeedSpring and a source like Instagram, Google Reviews, TikTok or Dribbble. Once connected, FeedSpring keeps your content updated automatically.

<figure><img src="../.gitbook/assets/divider-blog.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/main-cta.png" alt=""><figcaption></figcaption></figure>

### What is a feed?

A feed represents a single source of content.

For example:

* An Instagram account
* A Google Business location
* A TikTok profile
* A Dribbble user

Each feed contains:

* Profile data (name, avatar, follower count)
* Content (posts, reviews, videos, shots)

<figure><img src="../.gitbook/assets/divider-blog.png" alt=""><figcaption></figcaption></figure>

### How syncing works

FeedSpring regularly checks your source and updates your feed.

When new content is published:

* It is fetched by FeedSpring
* Stored and processed
* Made available to your site

You don’t need to manually refresh anything.

<figure><img src="../.gitbook/assets/divider-blog.png" alt=""><figcaption></figcaption></figure>

### When does content update?

Updates depend on your plan and feed type.

In general:

* New content is synced automatically
* Most feeds update within minutes to hours
* Your site always displays the latest available data

<figure><img src="../.gitbook/assets/divider-blog.png" alt=""><figcaption></figcaption></figure>

### What gets synced

FeedSpring syncs structured data from each platform.

This includes:

#### Content

* Images and videos
* Captions and descriptions
* Review text

#### Metadata

* Timestamps
* Likes, views, ratings
* Tags and categories

#### Profile data

* Name
* Avatar
* Follower counts

Each platform provides slightly different data.

👉 See [Plan update rates](#plan-update-rates) below for full details.

<figure><img src="../.gitbook/assets/divider-blog.png" alt=""><figcaption></figcaption></figure>

### Where your data is used

Once synced, your feed can be used anywhere:

* Websites (HTML, Webflow, Framer)
* React applications
* API requests

All delivery methods use the same underlying feed data.

<figure><img src="../.gitbook/assets/divider-blog.png" alt=""><figcaption></figcaption></figure>

### Filtering and control

Feeds generally display content from newest to oldest as provided by the source service. While the sort order cannot typically be changed, you can control which content appears by:

This includes:

* Limiting the number of items
* Skipping specific posts
* Applying keyword filters (Available for Google Reviews)

Some controls are applied:

* In your layout (e.g. limit)
* In your dashboard (e.g. filtering rules)

👉 Learn more in [Filtering & Limits](../attributes/feed-options.md)

<figure><img src="../.gitbook/assets/divider-blog.png" alt=""><figcaption></figcaption></figure>

### Why this matters

FeedSpring separates:

* **Data (feeds)**
* **Display (your layout or components)**

This means:

* You control how your content looks
* FeedSpring handles the data and updates

<figure><img src="../.gitbook/assets/divider-blog.png" alt=""><figcaption></figcaption></figure>

### Plan Update Rates

Discover the post limits and update frequency for your feeds across each platform, based on your plan. This helps you understand how often your content refreshes and how many items are available.

| Platform  | Free             | Personal         | Business        | Enterprise       |
| --------- | ---------------- | ---------------- | --------------- | ---------------- |
| Instagram | 8 posts (24 hrs) | 12 posts (6 hrs) | 12 posts (1 hr) | 12 posts (1 hr)  |
| Google    | 8 posts (24 hrs) | 16 posts (6 hrs) | 32 posts (1 hr) | 200 posts (1 hr) |
| TikTok    | 8 posts (24 hrs) | 16 posts (6 hrs) | 32 posts (1 hr) | 200 posts (1 hr) |
| Dribbble  | 8 posts (24 hrs) | 16 posts (6 hrs) | 32 posts (1 hr) | 200 posts (1 hr) |

{% hint style="info" %}
Instagram is capped at 12 posts on every paid plan. This is a limit of the Instagram API, not of FeedSpring.

Update frequency may vary slightly depending on the platform and availability of new content.
{% endhint %}

<figure><img src="../.gitbook/assets/divider-blog.png" alt=""><figcaption></figcaption></figure>

### Summary

* A feed is your connection to a content source
* FeedSpring keeps it updated automatically
* The same feed can be used across multiple platforms
* You control how it is displayed
