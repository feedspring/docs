---
description: Display Instagram posts, captions, engagement data and profile information on your site.
icon: square-instagram
---

# Instagram

Use FeedSpring to display Instagram content on your site, posts, captions, engagement data, and profile information, available as structured data across every delivery method.

Feed ID prefix: `inst_...`

### Render Instagram with

* [Attributes](../attributes/overview.md), add Instagram to any HTML page
* [React](../build-with/react-nextjs.md), drop a component into your React app
* [Framer](../build-with/framer.md), use the Instagram component in Framer
* [API](../graphql-api/overview.md), fetch Instagram data directly

### Post fields

| Attribute                    | JSON key       | Type      | Description                                                                                                            |
| ---------------------------- | -------------- | --------- | ---------------------------------------------------------------------------------------------------------------------- |
| `feed-field="img"`           | `mediaUrl`     | image URL | The post media (image or video). Sets the `src` of an `<img>`.                                                         |
| `feed-field="bg"`            | `mediaUrl`     | image URL | Same media URL, but applied as `background-image: url(...)` on any element.                                            |
| `feed-field="link"`          | `permalink`    | URL       | Link to the post on Instagram. Sets the `href` of an `<a>`.                                                            |
| `feed-field="caption"`       | `caption`      | string    | Post caption. Written with `innerHTML`, so HTML in the caption is rendered.                                            |
| `feed-field="like-count"`    | `likeCount`    | number    | Number of likes, formatted with compact locale notation (e.g. `1.2K`).                                                 |
| `feed-field="comment-count"` | `commentCount` | number    | Number of comments, formatted with compact locale notation.                                                            |
| `feed-field="timestamp"`     | `timestamp`    | date-time | When the post was published. See [timestamp formatting](../attributes/overview.md#feed-timestamp). |

### Profile fields

| Attribute                      | JSON key         | Type      | Description                                                      |
| ------------------------------ | ---------------- | --------- | ---------------------------------------------------------------- |
| `feed-field="avatar"`          | `avatar`         | image URL | Profile avatar image.                                            |
| `feed-field="name"`            | `fullName`       | string    | Profile display name.                                            |
| `feed-field="username"`        | `username`       | string    | Profile username (the @-handle).                                 |
| `feed-field="bio"`             | `bio`            | string    | Profile bio text.                                                |
| `feed-field="follower-count"`  | `followersCount` | number    | Total followers, formatted with compact locale notation.         |
| `feed-field="following-count"` | `followingCount` | number    | Total accounts followed, formatted with compact locale notation. |

{% hint style="info" %}
Profile fields like `avatar`, `name`, and `username` can also be placed _inside_ a post template — for example, to show the account avatar and handle on every card. They resolve to the same account-level value on every post; they are not per-post data.
{% endhint %}

### Instagram-specific notes

A few things worth knowing when building an Instagram feed:

* **Image aspect ratios vary.** Instagram posts can be 1:1, 4:5 portrait, or 1.91:1 landscape. Your layouts should either crop to a fixed ratio (grid-safe) or allow posts to flow at their native dimensions (masonry-safe).
* **Media renders automatically.** Image posts render as images, video posts render as video. No configuration needed.
* **Captions use `innerHTML`.** HTML in captions is rendered, not escaped. Instagram captions rarely contain HTML in practice, but it's worth knowing if you are sanitising user content.
* **`timestamp` may be empty.** In some responses the `timestamp` field is an empty string. Your layout should handle this case, either by using `feed-timestamp="from-now"` (which handles empty values gracefully) or by wrapping the element in a container that gracefully collapses.

### Example

A basic Instagram grid using the attributes delivery method:

```html
<section feedspring="inst_YOUR-FEED-ID" feed-options="render:dynamic|limit:8">
  <div class="grid">
    <article feedspring="post">
      <img feed-field="img" alt="" />
      <div class="meta">
        <span feed-field="like-count"></span> likes
        <time feed-field="timestamp" feed-timestamp="from-now"></time>
      </div>
      <a feed-field="link" target="_blank" rel="noopener">View on Instagram</a>
    </article>
  </div>
</section>
```

### Typical use cases

* Homepage image grids for brands and creators
* Social proof sections on landing pages
* Product or campaign showcases
* Event highlight feeds

### Next steps

* [Pick a delivery method](../README.md#where-to-start) to render Instagram
* [Filtering & Limits](../attributes/feed-options.md) for limit, skip, and dashboard filters
* [Browse other feed sources](../README.md#what-this-documentation-covers)
