---
description: "The feedspring and feed-field attributes in detail: wrappers, post templates, element types, and post versus profile fields."
---

# Fields & Elements

### `feedspring="YOUR-FEED-ID"`, the feed wrapper

Placed on any element that will contain the feed. Typically a `<div>` or `<section>`.

Each Feed ID starts with a source prefix, such as `inst_` for Instagram. See [Installation](installation.md) for the full list.

Example:

```html
<section feedspring="inst_55ZVUQExmdej0j8vygUoP">
```

* Each page can have multiple wrappers, including multiple feeds from different sources.
* The wrapper is where you also add `feed-options` to control rendering.
* Any `feed-field` attribute placed on or inside the wrapper but outside a post template is treated as a profile-level field. Examples: follower count, average rating, business name.

### `feedspring="post"`, the post template

The repeating element. FeedSpring finds the elements inside the wrapper with `feedspring="post"` and uses them to render per-item content.

* All `feed-field` attributes inside a post are post-level and receive the data for that specific post.
* Any styling, hover states, animations, or interactions you add to the template are preserved in each rendered copy.
* `feed-field="item"` is a supported alias for `feedspring="post"`, both mark an element as a post template.

### `feed-field="FIELD-NAME"`, the data target

Placed on the element that should display a piece of data. The element type matters:

| Element           | Behaviour                                                                      |
| ----------------- | ------------------------------------------------------------------------------ |
| `<img>`           | The `src` attribute is set to the field value. `srcset` is removed if present. |
| `<a>`             | The `href` attribute is set to the field value.                                |
| `<iframe>`        | The `src` is set to the derived embed URL (used for TikTok video playback).    |
| Any other element | The field value is injected as text or HTML content.                           |

Some fields are written with `innerHTML` rather than `innerText`, which means HTML in the value is rendered, not escaped. This is true for:

* Instagram `caption`
* TikTok `title`, `description`, `name`, `bio`
* Dribbble `bio`

For every other field, values are written as plain text.

Available field names depend on the feed source. See the per-source pages for the full list:

* [Instagram fields](../feeds/instagram.md)
* [Google Reviews fields](../feeds/google-reviews.md)
* [TikTok fields](../feeds/tiktok.md)
* [Dribbble fields](../feeds/dribbble.md)

### Post fields and profile fields

Where you place a `feed-field` decides which data it receives:

* **Inside a post template** — the field is filled with data for that post, such as an image, caption or rating.
* **Inside the wrapper, outside any post template** — the field is filled with account-level data, such as follower count, average rating or profile name.

Profile fields can also be placed inside a post template, for example to show the account avatar on every card. They show the same account-level value on every post.

### Next steps

* [Special Fields](special-fields.md) — fields that behave differently
* [Rendering](rendering.md) — how post templates are repeated
