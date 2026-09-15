---
description: Complete reference of every FeedSpring attribute, option and field. Written for developers and AI coding agents.
icon: book-bookmark
---

# Attributes Reference

Complete reference of every attribute used by the FeedSpring attributes delivery method.&#x20;

This page exists as a single source of truth for humans and AI coding agents.

### Wrapper attributes

| Attribute                   | Purpose                                                                       |
| --------------------------- | ----------------------------------------------------------------------------- |
| `feedspring="YOUR-FEED-ID"` | Outer feed wrapper. Declares which feed to render.                            |
| `feedspring="post"`         | Post template. Marks an element inside the wrapper as the repeating template. |
| `feed-field="item"`         | Alias for `feedspring="post"`. Both work identically.                         |
| `feedspring="loading"`      | Optional loading placeholder. Removed once feed rendering starts.             |

### Feed ID prefixes

| Prefix      | Source         |
| ----------- | -------------- |
| `inst_`     | Instagram      |
| `google_`   | Google Reviews |
| `tiktok_`   | TikTok         |
| `dribbble_` | Dribbble       |

### Script URLs

Load scripts inside the `<head>` using `async defer`.

Only load the scripts for feed types you are using.

| Source         | URL                                                 |
| -------------- | --------------------------------------------------- |
| Instagram      | `https://scripts.feedspring.com/instagram-attrs.js` |
| Google Reviews | `https://scripts.feedspring.com/google-reviews-attrs.js`    |
| TikTok         | `https://scripts.feedspring.com/tiktok-attrs.js`    |
| Dribbble       | `https://scripts.feedspring.com/dribbble-attrs.js`  |

### Rendering Modes

FeedSpring supports two rendering modes.

#### Dynamic Rendering

Recommended for most use cases.

Enable dynamic rendering with:

```html
feed-options="render:dynamic"
```

FeedSpring will automatically clone a single reusable post for each feed item:

```html
<div feedspring="post">
```

Example:

```html
<div 
  feedspring="inst_55ZVUQExmdej0j8vygUoP"
  feed-options="render:dynamic|limit:6">

  <div feedspring="post">
    <img feed-field="img" alt="" />
    <p feed-field="caption"></p>
  </div>

</div>
```

This automatically renders 6 posts.

***

#### Static Rendering

Static rendering is the default mode.

If `render:dynamic` is not enabled, FeedSpring only fills the existing:

```html
<div feedspring="post">
```

elements already in the DOM.

This gives you full control over:

* the number of rendered posts
* custom layouts
* advanced positioning structures

Example:

```html
<div feedspring="inst_55ZVUQExmdej0j8vygUoP">

  <div feedspring="post"></div>
  <div feedspring="post"></div>
  <div feedspring="post"></div>

</div>
```

This renders 3 posts.

### Feed options

Placed on the feed wrapper, as pipe-delimited pairs:

```
feed-options="render:dynamic|limit:6|lang:en"
```

👉 Every available option is listed under [`feed-options`](#feed-options) below.

### All `feed-field` values

The GraphQL field column gives the matching field in the [GraphQL API](../graphql-api/overview.md). Post fields are on each item in the feed's collection `nodes`; profile fields are on the feed.

#### Instagram post fields

| Attribute       | GraphQL field       | Type                              |
| --------------- | -------------- | --------------------------------- |
| `img`           | `image.url` | image URL                         |
| `bg`            | `image.url` | image URL (as `background-image`) |
| `link`          | `url` | URL                               |
| `caption`       | `caption` | string (innerHTML)                |
| `like-count`    | `likeCount` | number (compact)                  |
| `comment-count` | `commentCount` | number (compact)                  |
| `timestamp`     | `publishedAt` | date-time                         |

#### Instagram profile fields

| Attribute         | GraphQL field         | Type             |
| ----------------- | ---------------- | ---------------- |
| `avatar`          | `profile.avatar.url` | image URL        |
| `name`            | `profile.fullName` | string           |
| `username`        | `profile.username` | string           |
| `bio`             | `profile.bio` | string           |
| `follower-count`  | `profile.followerCount` | number (compact) |
| `following-count` | `profile.followingCount` | number (compact) |

{% hint style="info" %}
Profile fields can also be placed _inside_ a post template — for example, to show the account avatar and handle on every card. They resolve to the same account-level value on every post.
{% endhint %}

#### Google Reviews post fields

| Attribute       | GraphQL field          | Type                          |
| --------------- | ----------------- | ----------------------------- |
| `review`        | `comment` | string                        |
| `name`          | `author.name` | string                        |
| `avatar`        | `author.photo.url` | image URL                     |
| `rating`        | `rating.value` | number (1-5)                  |
| `rating-string` | `rating.label` | string ("five", "four", etc.) |
| `star`          | `rating.value` | repeater (active stars)       |
| `star-inactive` | `rating.value` | repeater (inactive stars)     |
| `timestamp`     | `createdAt` | date-time                     |

#### Google Reviews profile fields

| Attribute        | GraphQL field        | Type               |
| ---------------- | --------------- | ------------------ |
| `average-rating` | `averageRating` | number (1 decimal) |
| `total`          | `reviewCount` | number             |

#### TikTok post fields

| Attribute       | GraphQL field          | Type               |
| --------------- | ----------------- | ------------------ |
| `img`           | `cover.url` | image URL          |
| `video`         | `embedUrl` | iframe embed URL   |
| `link`          | `url` | URL                |
| `title`         | `title` | string (innerHTML) |
| `description`   | `description` | string (innerHTML) |
| `duration`      | `durationSeconds` | number             |
| `view-count`    | `viewCount` | number (compact)   |
| `like-count`    | `likeCount` | number (compact)   |
| `comment-count` | `commentCount` | number (compact)   |
| `share-count`   | `shareCount` | number             |
| `timestamp`     | `publishedAt` | date-time          |

#### TikTok profile fields

| Attribute         | GraphQL field         | Type               |
| ----------------- | ---------------- | ------------------ |
| `avatar`          | `profile.avatar.url` | image URL          |
| `profile-link`    | `profile.url` | URL (known issue)  |
| `name`            | `profile.displayName` | string (innerHTML) |
| `bio`             | `profile.bio` | string (innerHTML) |
| `follower-count`  | `profile.followerCount` | number (compact)   |
| `following-count` | `profile.followingCount` | number (compact)   |
| `total-likes`     | `profile.likeCount` | number (compact)   |
| `verified`        | `profile.isVerified` | boolean gate       |

#### Dribbble post fields

| Attribute   | GraphQL field      | Type      |
| ----------- | ------------- | --------- |
| `img`       | `image.url` | image URL |
| `link`      | `url` | URL       |
| `title`     | `title` | string    |
| `tag`       | `tags` | repeater (known issue) |
| `timestamp` | `publishedAt` | date-time |

#### Dribbble profile fields

| Attribute      | GraphQL field         | Type               |
| -------------- | ---------------- | ------------------ |
| `avatar`       | `profile.avatar.url` | image URL          |
| `name`         | `profile.name` | string             |
| `bio`          | `profile.bio` | string (innerHTML) |
| `location`     | `profile.location` | string             |
| `profile-link` | `profile.url` | URL                |
| `followers`    | `profile.followerCount` | number (compact)   |

### Modifier attributes

#### `feed-timestamp`

Applied to an element with `feed-field="timestamp"`. Controls how the timestamp renders.

| Value                    | Behaviour                                                                          |
| ------------------------ | ---------------------------------------------------------------------------------- |
| `from-now`               | Relative time: "2 days ago"                                                        |
| Any Day.js format string | Passed directly to `dayjs().format(...)`, e.g. `MMMM D, YYYY`, `DD/MM/YY`, `HH:mm` |
| _(omitted)_              | Default format `MMMM D, YYYY`                                                      |

Works on all feeds that implement a `timestamp` field.

### `feed-options`

{% hint style="danger" %}
**Review needed (Ilya): `limit` and `skip` behaviour.** The docs and the current script disagree, so this section is not final.

* **Current script:** in dynamic rendering, `limit` is a maximum item *index*, so `skip:2|limit:4` shows 2 posts. In static rendering, `limit` is ignored. With `skip`, the unused template stays on the page as an empty card.
* **Intended behaviour:** `limit` is the number of posts shown, in both rendering modes, so `skip:2|limit:4` shows 4 posts.

Confirm which behaviour the docs should describe (and whether the script is changing), then update this section and remove this callout.
{% endhint %}

Placed on the feed wrapper. Pipe-delimited pairs: `name:value|name:value`.

#### Feed-level options

| Option           | Description                                           | Applies to |
| ---------------- | ----------------------------------------------------- | ---------- |
| `render:dynamic` | Clone a single post template for each item            | All feeds  |
| `render:static`  | Fill already-present post templates by index          | All feeds  |
| `limit:N`        | Maximum source index in dynamic mode (`0` = no limit) | All feeds  |
| `skip:N`         | Skip first N items                                    | All feeds  |
| `lang:xx-XX`     | Locale for number formatting                          | All feeds  |
| `lang:auto`      | Use runtime locale                                    | All feeds  |

#### Post-level options

Placed on the `feedspring="post"` element.

| Option                          | Description                               |
| ------------------------------- | ----------------------------------------- |
| `appear:display-none` (default) | Clear inline `display:none` when rendered |
| `appear:display-block`          | Force `display: block` after render       |
| `appear:display-flex`           | Force `display: flex` after render        |

### Element behaviour

The element a `feed-field` sits on controls what happens with the value:

| Element           | Behaviour                                                   |
| ----------------- | ----------------------------------------------------------- |
| `<img>`           | Sets `src` to the field value. Removes `srcset` if present. |
| `<a>`             | Sets `href` to the field value.                             |
| `<iframe>`        | Sets `src` to derived embed URL (TikTok `video` only).      |
| Any other element | Inserts field value as text or HTML                         |

### Fields written with `innerHTML` (HTML rendered, not escaped)

* Instagram: `caption`
* TikTok: `title`, `description`, `name`, `bio`
* Dribbble: `bio`

All other text fields use plain text insertion.

### Cross-source normalization

Attributes that work the same across feeds, and the GraphQL fields they correspond to:

| Attribute         | Works on                         | GraphQL field                                                          |
| ----------------- | -------------------------------- | ---------------------------------------------------------------------- |
| `img`             | Instagram, TikTok, Dribbble      | `image.url`, `cover.url`, `image.url`                                  |
| `link`            | Instagram, TikTok, Dribbble      | `url`                                                                  |
| `timestamp`       | All                              | `publishedAt` (Instagram, TikTok, Dribbble), `createdAt` (Google)      |
| `avatar`          | All                              | `profile.avatar.url`, or `author.photo.url` for Google reviewers       |
| `name`            | All                              | `profile.fullName`, `author.name`, `profile.displayName`, `profile.name` |
| `bio`             | Instagram, TikTok, Dribbble      | `profile.bio`                                                          |
| `profile-link`    | TikTok, Dribbble                 | `profile.url`                                                          |
| `like-count`      | Instagram, TikTok                | `likeCount`                                                            |
| `comment-count`   | Instagram, TikTok                | `commentCount`                                                         |
| `follower-count`  | Instagram, TikTok                | `profile.followerCount` (Dribbble uses `followers`)                    |
| `following-count` | Instagram, TikTok                | `profile.followingCount`                                               |

### Source-specific attributes

| Source         | Attributes only available here                                                             |
| -------------- | ------------------------------------------------------------------------------------------ |
| Instagram      | `bg`, `caption`, `username`                                                                |
| Google Reviews | `review`, `rating`, `rating-string`, `star`, `star-inactive`, `average-rating`, `total`    |
| TikTok         | `video`, `description`, `duration`, `view-count`, `share-count`, `total-likes`, `verified` |
| Dribbble       | `location`, `followers`, `tag`                                                             |

### Known issues

See [Troubleshooting](troubleshooting.md) for current known issues and workarounds.

### Gotchas and behaviours worth knowing

{% hint style="danger" %}
**Review needed (Ilya): `limit` and `skip` behaviour.** The docs and the current script disagree, so this section is not final.

* **Current script:** in dynamic rendering, `limit` is a maximum item *index*, so `skip:2|limit:4` shows 2 posts. In static rendering, `limit` is ignored. With `skip`, the unused template stays on the page as an empty card.
* **Intended behaviour:** `limit` is the number of posts shown, in both rendering modes, so `skip:2|limit:4` shows 4 posts.

Confirm which behaviour the docs should describe (and whether the script is changing), then update this section and remove this callout.
{% endhint %}

**`limit` and `skip` interaction.** In dynamic mode, `limit` is a max source index, not a max count. `skip:2|limit:4` renders items 2 and 3 only (2 items total). To render 4 items starting from index 2, use `skip:2|limit:6`.

**`limit` has no effect in static mode.** Static mode renders one post per `feedspring="post"` template, regardless of `limit`.

**Unknown options are silently ignored.** Typos in `feed-options` names don't produce errors. Double-check spelling.

**`innerHTML` vs `innerText`.** Several fields use `innerHTML` (listed above). HTML in the value is rendered, not escaped. For user-generated content concerns, plain text fields are safer.

**`verified` is a conditional gate, not a value.** Place on any element. If `isVerified` is true, the element is kept in the DOM. If false, it is removed entirely.

**Stars are a repeater pattern.** `feed-field="star"` and `feed-field="star-inactive"` are templates, not value targets. FeedSpring clones them based on the rating.

**Number formatting is locale-aware.** The shared compact formatter uses `Intl.NumberFormat(..., { notation: 'compact' })`. A number like `1234` renders as `1.2K` in `en-US`, `1,2 k` in `fr-FR`. Control this with `feed-options="lang:xx-XX"` or `lang:auto`.

**Follower count naming is inconsistent.** Instagram and TikTok use `follower-count`. Dribbble uses `followers`. Both map to the same concept but the attribute name differs.

**Some fields may be empty.** Instagram's `timestamp` is sometimes an empty string. Design layouts to handle missing values gracefully, or use `feed-timestamp="from-now"` which handles empty timestamps.

### Next steps

* [Attributes overview](overview.md)
* [Per-feed field pages](../README.md#what-this-documentation-covers)
* [Feed options reference](feed-options.md)
* [Rendering modes](rendering.md)
