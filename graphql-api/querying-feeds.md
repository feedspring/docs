---
description: "How the feed query works: feed types, inline fragments, collections under nodes, nullability and item limits."
---

# Querying Feeds

Every request uses the same `feed` query. What comes back depends on the source connected to the Feed ID.

## Feed types

The `feed` field returns the `FeedData` union. Its concrete type depends on the source connected to the Feed ID:

| Source | GraphQL type |
| --- | --- |
| Instagram | `InstagramFeedData` |
| TikTok | `TikTokFeedData` |
| Dribbble | `DribbbleFeedData` |
| Google Reviews | `GoogleReviewsFeedData` |

Select `__typename` to identify the returned source, then use an inline fragment to select source-specific fields:

```graphql
query FeedType($publicKey: String!) {
  feed(publicKey: $publicKey) {
    __typename

    ... on InstagramFeedData {
      posts {
        nodes {
          id
        }
      }
    }

    ... on TikTokFeedData {
      videos {
        nodes {
          id
        }
      }
    }

    ... on DribbbleFeedData {
      shots {
        nodes {
          id
        }
      }
    }

    ... on GoogleReviewsFeedData {
      reviews {
        nodes {
          id
        }
      }
    }
  }
}
```


## Queries for each source

Each feed page has a complete query and notes on every field:

* [Instagram](../feeds/instagram.md#graphql-api) — `InstagramFeedData`, items in `posts.nodes`
* [Google Reviews](../feeds/google-reviews.md#graphql-api) — `GoogleReviewsFeedData`, items in `reviews.nodes`
* [TikTok](../feeds/tiktok.md#graphql-api) — `TikTokFeedData`, items in `videos.nodes`
* [Dribbble](../feeds/dribbble.md#graphql-api) — `DribbbleFeedData`, items in `shots.nodes`

## Scalars and nullability

The API uses these GraphQL scalars:

| Scalar | Description |
| --- | --- |
| `ID` | Identifier serialized as a string. |
| `Time` | ISO 8601 date and time value. |
| `Int64` | 64-bit integer used for engagement and aggregate counters. |

Fields marked with `!` in the schema are non-null. Image objects and some source fields are nullable because providers do not always supply them.

Connection fields `posts`, `videos`, `shots`, and `reviews` are always present. Their `nodes` arrays return `[]` instead of `null` when no items are available. Nested collection fields such as `children` and `tags` also always return an array.


## Filtering and item limits

Provider collections use connection-style objects, with the current items available under `nodes`. The API returns the items currently configured and available for the feed. The `feed` query does not accept pagination, `limit`, or `skip` arguments.

Use the FeedSpring dashboard to configure source filters and the number of synced items. If one application needs fewer items, select them after receiving the response:

```javascript
const visiblePosts = result.data.feed.posts.nodes.slice(0, 6)
```


## Next steps

* [Images](images.md)
* [Errors](errors.md)
* [Limits & Caching](limits-and-caching.md)
