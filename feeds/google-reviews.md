---
description: Get started building a beautiful feed today!
icon: google
---

# Google Reviews

## Google Reviews

Use FeedSpring to display Google Reviews on your site, star ratings, reviewer information, and aggregate data like total review count and average rating.

Feed ID prefix: `google_...`

### Render Google Reviews with

* [Attributes](https://claude.ai/delivery-methods/attributes), add Google Reviews to any HTML page
* [React](https://claude.ai/delivery-methods/react), drop a component into your React app
* [Framer](https://claude.ai/delivery-methods/framer), use the Google Reviews component in Framer
* [API](https://claude.ai/delivery-methods/api), fetch Google Reviews data directly

### Post fields

| Attribute                    | JSON key          | Type      | Description                                                                                                           |
| ---------------------------- | ----------------- | --------- | --------------------------------------------------------------------------------------------------------------------- |
| `feed-field="review"`        | `comment`         | string    | The review text itself.                                                                                               |
| `feed-field="name"`          | `author.name`     | string    | Reviewer's name.                                                                                                      |
| `feed-field="avatar"`        | `author.photoUrl` | image URL | Reviewer's Google profile photo.                                                                                      |
| `feed-field="rating"`        | `rating.number`   | number    | Numeric rating (1 to 5).                                                                                              |
| `feed-field="rating-string"` | `rating.string`   | string    | Rating as a word ("five", "four", etc.). Useful for human-readable output.                                            |
| `feed-field="star"`          | `rating.number`   | repeater  | Active star template. FeedSpring clones this element once per rating point.                                           |
| `feed-field="star-inactive"` | `rating.number`   | repeater  | Inactive star template. FeedSpring clones this element `5 - rating` times.                                            |
| `feed-field="timestamp"`     | `createdAt`       | date-time | When the review was posted. See [timestamp formatting](https://claude.ai/delivery-methods/attributes#feed-timestamp). |

### Profile fields

| Attribute                     | JSON key        | Type   | Description                                                                     |
| ----------------------------- | --------------- | ------ | ------------------------------------------------------------------------------- |
| `feed-field="average-rating"` | `averageRating` | number | Average rating across all reviews, formatted to one decimal place (e.g. `5.0`). |
| `feed-field="total"`          | `total`         | number | Total number of reviews.                                                        |

### How star ratings work

Stars are a little different from other fields. Instead of a single attribute, you provide two star elements, one active, one inactive, and FeedSpring fills in the correct count for each review:

```html
<div feedspring="post">
  <div class="stars">
    <svg feed-field="star" class="star-active">...</svg>
    <svg feed-field="star-inactive" class="star-inactive">...</svg>
  </div>
  <p feed-field="review"></p>
  <span feed-field="name"></span>
</div>
```

For a 4-star review, FeedSpring renders 4 active stars and 1 inactive star. For 5 stars, all 5 active. You design both states, FeedSpring handles the logic.

### Google Reviews-specific notes

* **There is no `link` attribute.** Google does not expose individual URLs per review, so there is no way to link to a specific review. You can still link to your overall Google Business listing, but that's a static URL, not per-review data.
* **Ratings are always 1 to 5.** Google does not expose half-stars or decimal ratings at the review level. The average rating is a decimal.
* **`rating.string` returns English words.** "five", "four", "three", etc. Useful if you want prose like "Five stars" without formatting the number yourself.
* **Review text is written with `innerText`.** Unlike Instagram captions, review text is inserted as plain text, so any HTML in reviews is escaped.

### Example

A carousel card using the attributes delivery method:

```html
<section feedspring="google_YOUR-FEED-ID" feed-options="render:dynamic|limit:6">
  <header>
    <span feed-field="average-rating"></span>
    out of 5
    (<span feed-field="total"></span> reviews)
  </header>

  <article feedspring="post">
    <div class="stars">
      <svg feed-field="star"><!-- filled star --></svg>
      <svg feed-field="star-inactive"><!-- empty star --></svg>
    </div>
    <p feed-field="review"></p>
    <footer>
      <img feed-field="avatar" alt="" />
      <span feed-field="name"></span>
      <time feed-field="timestamp" feed-timestamp="from-now"></time>
    </footer>
  </article>
</section>
```

### Typical use cases

* Carousel of 5-star reviews under a landing page hero
* Dedicated reviews page with full review grid
* Small trust badge in a sidebar or footer showing average rating and review count
* Product page testimonial with a single featured review

### Next steps

* [Pick a delivery method](https://claude.ai/delivery-methods) to render Google Reviews
* [Filtering & Limits](https://claude.ai/core-concepts/filtering-and-limits) for limit, skip, and dashboard filters
* [Browse other feed sources](https://claude.ai/feeds)
