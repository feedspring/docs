---
description: Get started building a beautiful feed today!
icon: dribbble
---

# Dribbble

## Dribbble

Use FeedSpring to display Dribbble shots on your site, portfolio images, titles, and profile information. Ideal for designers, studios, and agencies building a live portfolio.

Feed ID prefix: `dribbble_...`

### Render Dribbble with

* [Attributes](https://claude.ai/delivery-methods/attributes), add Dribbble to any HTML page
* [React](https://claude.ai/delivery-methods/react), drop a component into your React app
* [Framer](https://claude.ai/delivery-methods/framer), use the Dribbble component in Framer
* [API](https://claude.ai/delivery-methods/api), fetch Dribbble data directly

### Post fields

| Attribute                | JSON key      | Type      | Description                                                                                                            |
| ------------------------ | ------------- | --------- | ---------------------------------------------------------------------------------------------------------------------- |
| `feed-field="img"`       | `image`       | image URL | The shot image. Sets the `src` of an `<img>`.                                                                          |
| `feed-field="link"`      | `url`         | URL       | Link to the shot on Dribbble. Sets the `href` of an `<a>`.                                                             |
| `feed-field="title"`     | `title`       | string    | Shot title.                                                                                                            |
| `feed-field="timestamp"` | `publishedAt` | date-time | When the shot was published. See [timestamp formatting](https://claude.ai/delivery-methods/attributes#feed-timestamp). |

### Profile fields

| Attribute                   | JSON key         | Type      | Description                                                   |
| --------------------------- | ---------------- | --------- | ------------------------------------------------------------- |
| `feed-field="avatar"`       | `avatarUrl`      | image URL | Profile avatar.                                               |
| `feed-field="name"`         | `name`           | string    | Profile display name.                                         |
| `feed-field="bio"`          | `bio`            | string    | Profile bio. Written with `innerHTML`.                        |
| `feed-field="location"`     | `location`       | string    | Profile location.                                             |
| `feed-field="profile-link"` | `url`            | URL       | Link to the profile on Dribbble. Sets the `href` of an `<a>`. |
| `feed-field="followers"`    | `followersCount` | number    | Total followers, formatted with compact locale notation.      |

### Dribbble-specific notes

* **Follower count uses `followers`, not `follower-count`.** This differs from Instagram and TikTok which use `follower-count`. Use `feed-field="followers"` specifically for Dribbble.
* **Shot aspect ratio is 4:3.** Dribbble shots are cropped to 4:3 by Dribbble. Your layouts should respect this ratio to avoid distortion.
* **No engagement metrics.** Dribbble shots don't expose like or view counts through FeedSpring.
* **Some shots are posted by teams.** In the underlying JSON, each shot has an associated team. This is not currently exposed as an attribute.

### Example

A portfolio grid using the attributes delivery method:

```html
<section feedspring="dribbble_YOUR-FEED-ID" feed-options="render:dynamic|limit:9">
  <header>
    <img feed-field="avatar" alt="" />
    <div>
      <h2 feed-field="name"></h2>
      <p feed-field="location"></p>
      <a feed-field="profile-link" target="_blank" rel="noopener">
        <span feed-field="followers"></span> followers on Dribbble
      </a>
    </div>
  </header>

  <div class="grid">
    <article feedspring="post">
      <a feed-field="link" target="_blank" rel="noopener">
        <img feed-field="img" alt="" />
      </a>
      <h3 feed-field="title"></h3>
      <time feed-field="timestamp" feed-timestamp="from-now"></time>
    </article>
  </div>
</section>

<style>
  .grid { display: grid; grid-template-columns: repeat(3, 1fr); gap: 24px; }
  article img { aspect-ratio: 4 / 3; width: 100%; object-fit: cover; }
</style>
```

### Typical use cases

* Live portfolio section on a designer's homepage
* Studio case study feed that updates as new work is posted
* Agency "recent work" block on a services page
* Designer profile page with live shot grid

### Next steps

* [Pick a delivery method](https://claude.ai/delivery-methods) to render Dribbble
* [Filtering & Limits](https://claude.ai/core-concepts/filtering-and-limits) for limit, skip, and dashboard filters
* [Browse other feed sources](https://claude.ai/feeds)
