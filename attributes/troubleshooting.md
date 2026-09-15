---
description: Why an attributes feed isn't rendering, and how to fix it: a checklist, common mistakes and known issues.
---

# Troubleshooting

Attributes fail silently. A wrong field name, element or option produces no error — the element just stays empty. Work through this checklist first.

### Checklist

* The script matches the feed source, and only that script is loaded.
* The Feed ID prefix matches the source and the script.
* The wrapper has `feedspring="YOUR-FEED-ID"`, and a post template inside it has `feedspring="post"`.
* Field names are checked against the page for your source, not guessed.
* `feed-field="link"` and any profile link are on `<a>` elements.
* `feed-field="img"` and `feed-field="avatar"` are on `<img>` elements.
* TikTok `feed-field="video"` is on an `<iframe>`.
* With `render:dynamic`, there is exactly one post template.
* Profile fields sit inside the wrapper but outside the post template.

### Common mistakes

* **Using `caption` outside Instagram.** Google Reviews uses `review`, TikTok uses `title` or `description`, and Dribbble uses `title`.
* **Linking to a Google review.** Google Reviews has no `link` field. Link to your Google Business listing instead.
* **Dribbble followers.** The field is `followers`, not `follower-count`.
* **Typos in `feed-options`.** Unknown option names are ignored without an error.
* **Wrong element type.** A field on the wrong element is skipped.

### Known issues

These are bugs in the current attribute scripts. Fixes are in progress.

| Issue | Workaround |
| ----- | ---------- |
| Google Reviews: with `render:dynamic`, stars multiply on every review after the first. | Use static rendering: repeat the post template once per review. |
| Counts that are exactly `0` render as blank instead of `0`. | Hide the empty element and its label with CSS, e.g. `:has(> [feed-field]:empty)`. |
| TikTok: `feed-field="profile-link"` does not match, because the field is registered with a trailing space. | Use a static link to the profile. |
| Dribbble: `feed-field="tag"` leaves out the last tag on each shot. | None yet. |

### Still stuck?

Ask in [our Discord community](https://discord.gg/B7p7Qzb6Br) or email hello@feedspring.com.
