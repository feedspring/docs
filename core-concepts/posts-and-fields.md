---
icon: database
---

# Posts & Fields

Every FeedSpring feed is made up of posts and fields.

Understanding this structure is the key to building layouts with FeedSpring.

<figure><img src="../.gitbook/assets/main-cta.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/divider-blog.png" alt=""><figcaption></figcaption></figure>

### What is a post?

A post is a single item inside a feed.

Depending on the source, this could be:

* An Instagram post
* A Google review
* A TikTok video
* A Dribbble shot
* A YouTube video

Each feed contains multiple posts.

<figure><img src="../.gitbook/assets/divider-blog.png" alt=""><figcaption></figcaption></figure>

### What is a field?

A field is a piece of data inside a post.

For example:

* An image
* A caption or description
* A link
* A timestamp
* A rating

Fields are what you use to display content.

<figure><img src="../.gitbook/assets/divider-blog.png" alt=""><figcaption></figcaption></figure>

### Example

Here is a simplified example of a post:

```json
{
  "img": "image-url.jpg",
  "caption": "New product launch",
  "link": "https://...",
  "timestamp": "2026-01-01T12:00:00Z"
}
```

Each key is a field.

<figure><img src="../.gitbook/assets/divider-blog.png" alt=""><figcaption></figcaption></figure>

### Fields are different for each feed

Each platform provides different data.

#### Instagram (Preview)

{% hint style="info" %}
Just a preview, view all attributes on the [Instagram](../feeds/instagram.md) feeds page
{% endhint %}

* `img`
* `caption`
* `like-count`
* `comment-count`

#### Google Reviews (Preview)

{% hint style="info" %}
Just a preview, view all attributes on the [Google Review](../feeds/google-reviews.md) feeds page
{% endhint %}

* `review`
* `rating`
* `name`
* `avatar`

#### TikTok (Preview)

{% hint style="info" %}
Just a preview, view all attributes on the [TikTok](../feeds/tiktok.md) feeds page
{% endhint %}

* `img`
* `description`
* `view-count`
* `like-count`

#### Dribbble (Preview)

{% hint style="info" %}
Just a preview, view all attributes on the [Dribbble](../feeds/dribbble.md) feeds page
{% endhint %}

* `img`
* `title`
* `tag`
* `location`

👉 See each feed pages for the full list of fields.

<figure><img src="../.gitbook/assets/divider-blog.png" alt=""><figcaption></figcaption></figure>

### Profile fields vs post fields

Feeds also include profile-level data.

#### Profile fields

These describe the account or source:

* Name
* Avatar
* Bio
* Follower count

#### Post fields

These describe individual items:

* Image or media
* Text content
* Timestamp
* Engagement data

Both can be used when building layouts.

<figure><img src="../.gitbook/assets/divider-blog.png" alt=""><figcaption></figcaption></figure>

### How fields are used

No matter how you use FeedSpring:

* Attributes
* React components
* API

You are always working with the same fields.

Only the syntax changes.

<figure><img src="../.gitbook/assets/divider-blog.png" alt=""><figcaption></figcaption></figure>

### Why this matters

Once you understand posts and fields:

* You can build any layout
* You can switch between delivery methods
* You can combine feeds across platforms

This is the core of how FeedSpring works.

<figure><img src="../.gitbook/assets/divider-blog.png" alt=""><figcaption></figcaption></figure>

### Summary

* A feed contains posts
* Each post contains fields
* Fields are the data you display
* Fields vary by platform
* The structure is always the same
