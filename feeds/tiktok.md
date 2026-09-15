---
description: Display TikTok videos with thumbnails, inline playback, engagement counts and profile data.
icon: tiktok
---

# TikTok

Use FeedSpring to display TikTok videos on your site, thumbnails, playable video, engagement counts, and profile information.

Feed ID prefix: `tiktok_...`

### Render TikTok with

* [Attributes](../delivery-methods/attributes-html.md), add TikTok to any HTML page
* [React](../delivery-methods/react-components.md), drop a component into your React app
* [Framer](../delivery-methods/framer-components.md), use the TikTok component in Framer
* [API](../delivery-methods/api-graphql.md), fetch TikTok data directly

### Post fields

| Attribute                    | JSON key        | Type              | Description                                                                                                             |
| ---------------------------- | --------------- | ----------------- | ----------------------------------------------------------------------------------------------------------------------- |
| `feed-field="img"`           | `coverImageUrl` | image URL         | The video thumbnail. Sets the `src` of an `<img>`.                                                                      |
| `feed-field="video"`         | `id`            | derived embed URL | Inline video player. Sets the `src` of an `<iframe>` to the TikTok embed URL.                                           |
| `feed-field="link"`          | `shareUrl`      | URL               | Canonical TikTok link. Sets the `href` of an `<a>`.                                                                     |
| `feed-field="title"`         | `title`         | string            | Video title. Written with `innerHTML`.                                                                                  |
| `feed-field="description"`   | `description`   | string            | Video description. Written with `innerHTML`.                                                                            |
| `feed-field="duration"`      | `duration`      | number            | Video length.                                                                                                           |
| `feed-field="view-count"`    | `viewCount`     | number            | View count, formatted with compact locale notation (e.g. `1.2K`).                                                       |
| `feed-field="like-count"`    | `likeCount`     | number            | Like count, formatted with compact locale notation.                                                                     |
| `feed-field="comment-count"` | `commentCount`  | number            | Comment count, formatted with compact locale notation.                                                                  |
| `feed-field="share-count"`   | `shareCount`    | number            | Share count.                                                                                                            |
| `feed-field="timestamp"`     | `createTime`    | date-time         | When the video was published. See [timestamp formatting](../delivery-methods/attributes-html.md#feed-timestamp). |

### Profile fields

| Attribute                      | JSON key         | Type         | Description                                                                                                                        |
| ------------------------------ | ---------------- | ------------ | ---------------------------------------------------------------------------------------------------------------------------------- |
| `feed-field="avatar"`          | `avatarUrl`      | image URL    | Profile avatar.                                                                                                                    |
| `feed-field="name"`            | `displayName`    | string       | Profile display name. Written with `innerHTML`.                                                                                    |
| `feed-field="bio"`             | `bio`            | string       | Profile bio. Written with `innerHTML`.                                                                                             |
| `feed-field="follower-count"`  | `followerCount`  | number       | Total followers, formatted with compact locale notation.                                                                           |
| `feed-field="following-count"` | `followingCount` | number       | Total accounts followed, formatted with compact locale notation.                                                                   |
| `feed-field="total-likes"`     | `likesCount`     | number       | Total likes across all the account's videos, formatted with compact locale notation.                                               |
| `feed-field="verified"`        | `isVerified`     | boolean gate | Element is kept in the DOM if the account is verified, removed entirely if not. Use this to conditionally render a verified badge. |

### Inline video playback

TikTok has a dedicated `video` attribute that renders the video inline using TikTok's embed player. Apply it to an `<iframe>`:

```html
<div feedspring="post">
  <iframe feed-field="video" allowfullscreen></iframe>
</div>
```

FeedSpring sets the `src` to `https://www.tiktok.com/embed/v2/{video-id}`. The TikTok player handles all controls, audio, and fullscreen behaviour.

If you prefer to link out instead of embedding, use `feed-field="link"` on an `<a>` and show the thumbnail with `feed-field="img"`. This is lighter-weight and faster to render.

### Verified badge

The `verified` attribute works as a conditional gate. Place it on any element that should only show for verified accounts, and FeedSpring removes the element for unverified ones:

```html
<header>
  <img feed-field="avatar" alt="" />
  <span feed-field="name"></span>
  <svg feed-field="verified"><!-- blue checkmark icon --></svg>
</header>
```

### TikTok-specific notes

* **9:16 is native.** TikTok videos are always portrait 9:16. Layouts that crop to square or landscape will look wrong. Respect the aspect ratio in your CSS.
* **`title` and `description` may be identical.** TikTok uses both fields, often populated with the same value (the video caption). Most layouts use one or the other.
* **Inline video loads the TikTok player.** Using `feed-field="video"` pulls in TikTok's embed script, which adds weight to the page. For performance-sensitive pages, use thumbnails with click-through links instead.
* **`duration` is a raw number.** Format it yourself if you want `0:15` style output.

### Example

A video grid using the attributes delivery method:

```html
<section feedspring="tiktok_YOUR-FEED-ID" feed-options="render:dynamic|limit:8">
  <header>
    <img feed-field="avatar" alt="" />
    <span feed-field="name"></span>
    <span feed-field="follower-count"></span> followers
  </header>

  <div class="grid">
    <article feedspring="post">
      <img feed-field="img" alt="" />
      <div class="meta">
        <span feed-field="view-count"></span> views
      </div>
      <a feed-field="link" target="_blank" rel="noopener">Watch on TikTok</a>
    </article>
  </div>
</section>
```

### Typical use cases

* Creator homepage video grids
* Brand campaign landing pages
* Product reveal pages with inline playback
* Event or launch highlight feeds

### Next steps

* [Pick a delivery method](../README.md#where-to-start) to render TikTok
* [Filtering & Limits](../core-concepts/filtering-and-limits.md) for limit, skip, and dashboard filters
* [Browse other feed sources](../README.md#what-this-documentation-covers)
