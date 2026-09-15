---
description: "Fields that do more than insert a value: background images, timestamp formatting, star ratings, verified badges and tags."
---

# Special Fields

Most fields insert a value into an element. The fields on this page behave differently.

### `bg`

In addition to the element-type rules above, the `bg` field is a special case: applied to any element with `feed-field="bg"`, it sets `background-image: url(...)` rather than `src`. Useful for card backgrounds and hero covers where you want the image as a CSS background.

Currently Instagram only.

### `feed-timestamp`

Applied to an element with `feed-field="timestamp"`. Controls how the timestamp is formatted.

```html
<time feed-field="timestamp" feed-timestamp="from-now"></time>
```

* `from-now`, renders as a relative time: "2 days ago", "a month ago"
* Any other value is passed to [Day.js format strings](https://day.js.org/docs/en/display/format), for example `MMMM D, YYYY`, `DD/MM/YY`, or `HH:mm`
* If omitted, the default format is `MMMM D, YYYY`

Works on all feeds that expose a `timestamp` field: Instagram, Google Reviews, TikTok, Dribbble.


### `star` and `star-inactive`

Google Reviews star ratings are built from two templates you design: one filled star and one empty star. FeedSpring repeats the filled star once for each rating point and the empty star for the rest, up to five.

```html
<div class="stars">
  <svg feed-field="star"><!-- filled star --></svg>
  <svg feed-field="star-inactive"><!-- empty star --></svg>
</div>
```

For a 4-star review, FeedSpring renders 4 filled stars and 1 empty star. Keep both elements in the template, even if every review in your feed is 5 stars.

{% hint style="warning" %}
**Known issue:** with `render:dynamic`, stars currently multiply on every review after the first. Until a fix is released, use static rendering for Google Reviews. See [Google Reviews](../feeds/google-reviews.md).
{% endhint %}

### `verified`

The TikTok `verified` field is a condition, not a value. Place it on any element that should only show for verified accounts: FeedSpring keeps the element if the account is verified and removes it if not.

```html
<svg feed-field="verified"><!-- verified badge --></svg>
```

### `tag`

The Dribbble `tag` field repeats its element once for each tag on a shot.

```html
<div class="tags">
  <span feed-field="tag"></span>
</div>
```

{% hint style="warning" %}
**Known issue:** `tag` currently leaves out the last tag on each shot. See [Dribbble](../feeds/dribbble.md).
{% endhint %}

### Next steps

* [Fields & Elements](fields-and-elements.md)
* [Troubleshooting](troubleshooting.md)
