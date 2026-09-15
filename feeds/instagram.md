---
description: Display Instagram posts, captions, engagement data and profile information on your site.
icon: square-instagram
---

# Instagram

Use FeedSpring to display Instagram content on your site, posts, captions, engagement data, and profile information, available as structured data across every delivery method.

Feed ID prefix: `inst_...`

### Render Instagram with

* [Attributes](../attributes/overview.md), add Instagram to any HTML page
* [React & Next.js](../build-with/react-nextjs.md), use the GraphQL API or attributes in a React app
* [Framer](../build-with/framer.md), use the Instagram component in Framer
* [GraphQL API](#graphql-api), fetch Instagram data directly — see the query below

### Post fields

In the GraphQL API, post fields are on each item in `posts.nodes`.

| Attribute                    | GraphQL field       | Type      | Description                                                                                                            |
| ---------------------------- | -------------- | --------- | ---------------------------------------------------------------------------------------------------------------------- |
| `feed-field="img"`           | `image.url` | image URL | The post media (image or video). Sets the `src` of an `<img>`.                                                         |
| `feed-field="bg"`            | `image.url` | image URL | Same media URL, but applied as `background-image: url(...)` on any element.                                            |
| `feed-field="link"`          | `url` | URL       | Link to the post on Instagram. Sets the `href` of an `<a>`.                                                            |
| `feed-field="caption"`       | `caption` | string    | Post caption. Written with `innerHTML`, so HTML in the caption is rendered.                                            |
| `feed-field="like-count"`    | `likeCount` | number    | Number of likes, formatted with compact locale notation (e.g. `1.2K`).                                                 |
| `feed-field="comment-count"` | `commentCount` | number    | Number of comments, formatted with compact locale notation.                                                            |
| `feed-field="timestamp"`     | `publishedAt` | date-time | When the post was published. See [timestamp formatting](../attributes/special-fields.md#feed-timestamp). |

### Profile fields

In the GraphQL API, profile fields are on the feed itself.

| Attribute                      | GraphQL field         | Type      | Description                                                      |
| ------------------------------ | ---------------- | --------- | ---------------------------------------------------------------- |
| `feed-field="avatar"`          | `profile.avatar.url` | image URL | Profile avatar image.                                            |
| `feed-field="name"`            | `profile.fullName` | string    | Profile display name.                                            |
| `feed-field="username"`        | `profile.username` | string    | Profile username (the @-handle).                                 |
| `feed-field="bio"`             | `profile.bio` | string    | Profile bio text.                                                |
| `feed-field="follower-count"`  | `profile.followerCount` | number    | Total followers, formatted with compact locale notation.         |
| `feed-field="following-count"` | `profile.followingCount` | number    | Total accounts followed, formatted with compact locale notation. |

{% hint style="info" %}
Profile fields like `avatar`, `name`, and `username` can also be placed _inside_ a post template — for example, to show the account avatar and handle on every card. They resolve to the same account-level value on every post; they are not per-post data.
{% endhint %}

### Instagram-specific notes

A few things worth knowing when building an Instagram feed:

* **Image aspect ratios vary.** Instagram posts can be 1:1, 4:5 portrait, or 1.91:1 landscape. Your layouts should either crop to a fixed ratio (grid-safe) or allow posts to flow at their native dimensions (masonry-safe).
* **Media renders automatically.** Image posts render as images, video posts render as video. No configuration needed.
* **Captions use `innerHTML`.** HTML in captions is rendered, not escaped. Instagram captions rarely contain HTML in practice, but it's worth knowing if you are sanitising user content.
* **`timestamp` may be empty.** In some responses the `timestamp` field is an empty string. Your layout should handle this case, either by using `feed-timestamp="from-now"` (which handles empty values gracefully) or by wrapping the element in a container that gracefully collapses.

### Example

A basic Instagram grid using the attributes delivery method:

```html
<section feedspring="inst_YOUR-FEED-ID" feed-options="render:dynamic|limit:8">
  <div class="grid">
    <article feedspring="post">
      <img feed-field="img" alt="" />
      <div class="meta">
        <span feed-field="like-count"></span> likes
        <time feed-field="timestamp" feed-timestamp="from-now"></time>
      </div>
      <a feed-field="link" target="_blank" rel="noopener">View on Instagram</a>
    </article>
  </div>
</section>
```

### GraphQL API

Fetch Instagram feeds with the [GraphQL API](../graphql-api/overview.md). The feed returns `InstagramFeedData`, with items under `posts.nodes`.

Instagram feeds contain either an account profile or a hashtag, together with a list of posts.

```graphql
query InstagramFeed($publicKey: String!) {
  feed(publicKey: $publicKey) {
    __typename
    ... on InstagramFeedData {
      profile {
        bio
        followerCount
        followingCount
        avatar {
          url(input: { width: 160, height: 160 })
        }
        fullName
        username
      }
      hashtag
      posts {
        nodes {
          id
          mediaType
          image {
            url(input: { width: 1200 })
          }
          url
          likeCount
          commentCount
          publishedAt
          caption
          username
          avatar {
            url(input: { width: 96, height: 96 })
          }
          fullName
          children {
            id
            mediaType
            image {
              url(input: { width: 1200 })
            }
          }
        }
      }
    }
  }
}
```

#### Field notes

| Field | Description |
| --- | --- |
| `profile` | Account-level data. It is `null` for hashtag feeds. |
| `hashtag` | Hashtag represented by the feed. It is `null` for account feeds. |
| `posts.nodes` | Posts in the order configured by FeedSpring. The list is always present and may be empty. |
| `mediaType` | One of `IMAGE`, `VIDEO`, or `CAROUSEL`. |
| `image` | Available display image for the post. |
| `url` | Permalink to the post on Instagram. |
| `publishedAt` | Publication time, or `null` when the source did not provide a valid timestamp. |
| `username`, `avatar`, `fullName` | Optional post-level author data. |
| `children` | Media contained in a carousel. It is an empty list for posts without child media. |

### Typical use cases

* Homepage image grids for brands and creators
* Social proof sections on landing pages
* Product or campaign showcases
* Event highlight feeds

### Next steps

* [Choose your setup](../getting-started/choose-your-setup.md) to render Instagram
* [Feed Options](../attributes/feed-options.md) for `limit` and `skip`, and [Filtering](../core-concepts/filtering.md) for dashboard filters
* [Browse other feed sources](../README.md#what-this-documentation-covers)
