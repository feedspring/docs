---
description: Display Google Reviews with star ratings, reviewer details, and aggregate rating data.
icon: google
---

# Google Reviews

Use FeedSpring to display Google Reviews on your site, star ratings, reviewer information, and aggregate data like total review count and average rating.

Feed ID prefix: `google_...`

### Render Google Reviews with

* [Attributes](../attributes/overview.md), add Google Reviews to any HTML page
* [React & Next.js](../build-with/react-nextjs.md), use the GraphQL API or attributes in a React app
* [Framer](../build-with/framer.md), use the Google Reviews component in Framer
* [GraphQL API](#graphql-api), fetch Google Reviews data directly — see the query below

### Post fields

In the GraphQL API, post fields are on each item in `reviews.nodes`.

| Attribute                    | GraphQL field          | Type      | Description                                                                                                           |
| ---------------------------- | ----------------- | --------- | --------------------------------------------------------------------------------------------------------------------- |
| `feed-field="review"`        | `comment` | string    | The review text itself.                                                                                               |
| `feed-field="name"`          | `author.name` | string    | Reviewer's name.                                                                                                      |
| `feed-field="avatar"`        | `author.photo.url` | image URL | Reviewer's Google profile photo.                                                                                      |
| `feed-field="rating"`        | `rating.value` | number    | Numeric rating (1 to 5).                                                                                              |
| `feed-field="rating-string"` | `rating.label` | string    | Rating as a word ("five", "four", etc.). Useful for human-readable output.                                            |
| `feed-field="star"`          | `rating.value` | repeater  | Active star template. FeedSpring clones this element once per rating point.                                           |
| `feed-field="star-inactive"` | `rating.value` | repeater  | Inactive star template. FeedSpring clones this element `5 - rating` times.                                            |
| `feed-field="timestamp"`     | `createdAt` | date-time | When the review was posted. See [timestamp formatting](../attributes/special-fields.md#feed-timestamp). |

### Profile fields

In the GraphQL API, profile fields are on the feed itself.

| Attribute                     | GraphQL field        | Type   | Description                                                                     |
| ----------------------------- | --------------- | ------ | ------------------------------------------------------------------------------- |
| `feed-field="average-rating"` | `averageRating` | number | Average rating across all reviews, formatted to one decimal place (e.g. `5.0`). |
| `feed-field="total"`          | `reviewCount` | number | Total number of reviews.                                                        |

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

{% hint style="warning" %}
**Known issue:** with `render:dynamic`, stars currently multiply on every review after the first (a 5-star review shows 25 stars). Until a fix is released, use static rendering for Google Reviews: repeat the post template once for each review you want to show, and leave out `render:dynamic`.
{% endhint %}


### Google Reviews-specific notes

* **There is no `link` attribute.** Google does not expose individual URLs per review, so there is no way to link to a specific review. See [Linking to your Google listing](#linking-to-your-google-listing) below for the recommended alternative.
* **Ratings are always 1 to 5.** Google does not expose half-stars or decimal ratings at the review level. The average rating is a decimal.
* **`rating.string` returns English words.** "five", "four", "three", etc. Useful if you want prose like "Five stars" without formatting the number yourself.
* **Review text is written with `innerText`.** Unlike Instagram captions, review text is inserted as plain text, so any HTML in reviews is escaped.

### Linking to your Google listing

Because there is no per-review link, we recommend linking to your Google Business Profile instead. Visitors can read your other reviews there and leave one of their own.

Add it as a normal `<a>` in your markup, outside the post template, using your own Place ID:

```html
<a href="https://search.google.com/local/reviews?placeid=YOUR_PLACE_ID" target="_blank" rel="noopener">
  Read all reviews on Google
</a>

<a href="https://search.google.com/local/writereview?placeid=YOUR_PLACE_ID" target="_blank" rel="noopener">
  Leave us a review
</a>
```

This is a static link you control, not feed data, so it stays the same for every review.

### Example

A review grid using the attributes delivery method. It uses static rendering, repeating the review card, because of the known star issue above:

```html
<section feedspring="google_YOUR-FEED-ID">
  <header>
    <span feed-field="average-rating"></span>
    out of 5
    (<span feed-field="total"></span> reviews)
  </header>

  <!-- Repeat this card once per review you want to show -->
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

  <article feedspring="post">
    <!-- same markup as the card above -->
  </article>
</section>
```

### GraphQL API

Fetch Google Reviews feeds with the [GraphQL API](../graphql-api/overview.md). The feed returns `GoogleReviewsFeedData`, with items under `reviews.nodes`.

Google Reviews feeds contain business information, aggregate rating data, and reviews.

```graphql
query GoogleReviewsFeed($publicKey: String!) {
  feed(publicKey: $publicKey) {
    __typename
    ... on GoogleReviewsFeedData {
      business {
        name
      }
      location {
        name
        address
        placeId
        mapId
      }
      reviewCount
      averageRating
      reviews {
        nodes {
          id
          comment
          reply {
            comment
            updatedAt
          }
          author {
            name
            photo {
              url(input: { width: 128, height: 128 })
            }
            isAnonymous
          }
          rating {
            label
            value
          }
          location {
            name
            address
            placeId
            mapId
          }
          createdAt
          updatedAt
        }
      }
    }
  }
}
```

#### Field notes

| Field | Description |
| --- | --- |
| `business` | Business represented by the feed. |
| `location` | Feed-level Google location, or `null` when the feed is not tied to one location. |
| `reviewCount` | Total review count reported for the business. |
| `averageRating` | Average rating reported for the business. |
| `reviews.nodes` | Reviews available in the feed. The list is always present and may be empty. |
| `reply` | Business reply to the review, or `null` when there is no reply. |
| `author.photo` | Optional author photo. |
| `author.isAnonymous` | Whether Google marked the reviewer as anonymous. |
| `rating.label` | Human-readable rating label. |
| `rating.value` | Numeric rating value. |
| `review.location` | Location associated with an individual review, when available. |

### Typical use cases

* Carousel of 5-star reviews under a landing page hero
* Dedicated reviews page with full review grid
* Small trust badge in a sidebar or footer showing average rating and review count
* Product page testimonial with a single featured review

### Next steps

* [Choose your setup](../getting-started/choose-your-setup.md) to render Google Reviews
* [Feed Options](../attributes/feed-options.md) for `limit` and `skip`, and [Filtering](../core-concepts/filtering.md) for dashboard filters
* [Browse other feed sources](../README.md#what-this-documentation-covers)
