---
icon: book-bookmark
---

# Attributes Reference

## Attribute Reference

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
| Google Reviews | `https://scripts.feedspring.com/google-attrs.js`    |
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

### Feed Options (feed-options)

Placed on the feed wrapper.

Format:

```
feed-options="name:value|name:value"
```

Example:

```
feed-options="render:dynamic|limit:6|lang:en"
```

### Feed-Level Options

| Option           | Description                                   |
| ---------------- | --------------------------------------------- |
| `render:dynamic` | Automatically clones a reusable post template |
| `limit`          | Limits the number of rendered posts           |
| `skip`           | Skips posts from the beginning of the feed    |
| `lang`           | Controls localization and number formatting   |

### All `feed-field` values

#### Instagram post fields

| Attribute    | Attribute       | Type                              |
| ------------ | --------------- | --------------------------------- |
| `feed-field` | `img`           | image URL                         |
| `feed-field` | `bg`            | image URL (as `background-image`) |
| `feed-field` | `link`          | URL                               |
| `feed-field` | `caption`       | string (innerHTML)                |
| `feed-field` | `like-count`    | number (compact)                  |
| `feed-field` | `comment-count` | number (compact)                  |
| `feed-field` | `timestamp`     | date-time                         |
| `feed-field` | `avatar`        | image URL                         |
| `feed-field` | `name`          | string                            |
| `feed-field` | `username`      | string                            |

#### Instagram profile fields

| JSON key     | Attribute         | Type             |
| ------------ | ----------------- | ---------------- |
| `feed-field` | `avatar`          | image URL        |
| `feed-field` | `name`            | string           |
| `feed-field` | `username`        | string           |
| `feed-field` | `bio`             | string           |
| `feed-field` | `follower-count`  | number (compact) |
| `feed-field` | `following-count` | number (compact) |

#### Google Reviews post fields

| Attribute       | JSON key          | Type                          |
| --------------- | ----------------- | ----------------------------- |
| `review`        | `comment`         | string                        |
| `name`          | `author.name`     | string                        |
| `avatar`        | `author.photoUrl` | image URL                     |
| `rating`        | `rating.number`   | number (1-5)                  |
| `rating-string` | `rating.string`   | string ("five", "four", etc.) |
| `star`          | `rating.number`   | repeater (active stars)       |
| `star-inactive` | `rating.number`   | repeater (inactive stars)     |
| `timestamp`     | `createdAt`       | date-time                     |

#### Google Reviews profile fields

| Attribute        | JSON key        | Type               |
| ---------------- | --------------- | ------------------ |
| `average-rating` | `averageRating` | number (1 decimal) |
| `total`          | `total`         | number             |

#### TikTok post fields

| Attribute       | JSON key          | Type               |
| --------------- | ----------------- | ------------------ |
| `img`           | `coverImageUrl`   | image URL          |
| `video`         | derived from `id` | iframe embed URL   |
| `link`          | `shareUrl`        | URL                |
| `title`         | `title`           | string (innerHTML) |
| `description`   | `description`     | string (innerHTML) |
| `duration`      | `duration`        | number             |
| `view-count`    | `viewCount`       | number (compact)   |
| `like-count`    | `likeCount`       | number (compact)   |
| `comment-count` | `commentCount`    | number (compact)   |
| `share-count`   | `shareCount`      | number             |
| `timestamp`     | `createTime`      | date-time          |

#### TikTok profile fields

| Attribute         | JSON key         | Type               |
| ----------------- | ---------------- | ------------------ |
| `avatar`          | `avatarUrl`      | image URL          |
| `name`            | `displayName`    | string (innerHTML) |
| `bio`             | `bio`            | string (innerHTML) |
| `follower-count`  | `followerCount`  | number (compact)   |
| `following-count` | `followingCount` | number (compact)   |
| `total-likes`     | `likesCount`     | number (compact)   |
| `verified`        | `isVerified`     | boolean gate       |

#### Dribbble post fields

| Attribute   | JSON key      | Type      |
| ----------- | ------------- | --------- |
| `img`       | `image`       | image URL |
| `link`      | `url`         | URL       |
| `title`     | `title`       | string    |
| `timestamp` | `publishedAt` | date-time |

#### Dribbble profile fields

| Attribute      | JSON key         | Type               |
| -------------- | ---------------- | ------------------ |
| `avatar`       | `avatarUrl`      | image URL          |
| `name`         | `name`           | string             |
| `bio`          | `bio`            | string (innerHTML) |
| `location`     | `location`       | string             |
| `profile-link` | `url`            | URL                |
| `followers`    | `followersCount` | number (compact)   |

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

Attributes that work the same across feeds, mapping to different JSON keys:

| Attribute         | Works on                    | Maps to                                               |
| ----------------- | --------------------------- | ----------------------------------------------------- |
| `img`             | Instagram, TikTok, Dribbble | `mediaUrl`, `coverImageUrl`, `image`                  |
| `link`            | Instagram, TikTok, Dribbble | `permalink`, `shareUrl`, `url`                        |
| `timestamp`       | All                         | `timestamp`, `createdAt`, `createTime`, `publishedAt` |
| `avatar`          | All                         | `avatar`, `author.photoUrl`, `avatarUrl`              |
| `name`            | All                         | `fullName`, `author.name`, `displayName`, `name`      |
| `bio`             | Instagram, TikTok, Dribbble | `bio`                                                 |
| `like-count`      | Instagram, TikTok           | `likeCount`                                           |
| `comment-count`   | Instagram, TikTok           | `commentCount`                                        |
| `follower-count`  | Instagram, TikTok           | `followersCount` / `followerCount`                    |
| `following-count` | Instagram, TikTok           | `followingCount`                                      |

### Source-specific attributes

| Source         | Attributes only available here                                                             |
| -------------- | ------------------------------------------------------------------------------------------ |
| Instagram      | `bg`, `caption`, `username`                                                                |
| Google Reviews | `review`, `rating`, `rating-string`, `star`, `star-inactive`, `average-rating`, `total`    |
| TikTok         | `video`, `description`, `duration`, `view-count`, `share-count`, `total-likes`, `verified` |
| Dribbble       | `location`, `followers`, `profile-link`                                                    |

### Gotchas and behaviours worth knowing

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

* [Attributes delivery method guide](delivery-methods/attributes-html.md)
* [Per-feed field pages](README.md#what-this-documentation-covers)
* [Feed options reference](core-concepts/filtering-and-limits.md)
* [Rendering modes](core-concepts/rendering-behaviour.md)
