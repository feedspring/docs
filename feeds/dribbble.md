---
description: Display Dribbble shots and designer profile information as a live portfolio feed.
icon: dribbble
---

# Dribbble

Use FeedSpring to display Dribbble shots on your site, portfolio images, titles, and profile information. Ideal for designers, studios, and agencies building a live portfolio.

Feed ID prefix: `dribbble_...`

### Render Dribbble with

* [Attributes](../attributes/overview.md), add Dribbble to any HTML page
* [React & Next.js](../build-with/react-nextjs.md), use the GraphQL API or attributes in a React app
* [Framer](../build-with/framer.md), use the Dribbble component in Framer
* [GraphQL API](#graphql-api), fetch Dribbble data directly — see the query below

### Post fields

In the GraphQL API, post fields are on each item in `shots.nodes`.

| Attribute                | GraphQL field      | Type      | Description                                                                                                            |
| ------------------------ | ------------- | --------- | ---------------------------------------------------------------------------------------------------------------------- |
| `feed-field="img"`       | `image.url` | image URL | The shot image. Sets the `src` of an `<img>`.                                                                          |
| `feed-field="link"`      | `url` | URL       | Link to the shot on Dribbble. Sets the `href` of an `<a>`.                                                             |
| `feed-field="title"`     | `title` | string    | Shot title.                                                                                                            |
| `feed-field="tag"`       | `tags` | repeater  | Shot tags. The element is repeated once per tag. See the known issue below.                                             |
| `feed-field="timestamp"` | `publishedAt` | date-time | When the shot was published. See [timestamp formatting](../attributes/special-fields.md#feed-timestamp). |

### Profile fields

In the GraphQL API, profile fields are on the feed itself.

| Attribute                   | GraphQL field         | Type      | Description                                                   |
| --------------------------- | ---------------- | --------- | ------------------------------------------------------------- |
| `feed-field="avatar"`       | `profile.avatar.url` | image URL | Profile avatar.                                               |
| `feed-field="name"`         | `profile.name` | string    | Profile display name.                                         |
| `feed-field="bio"`          | `profile.bio` | string    | Profile bio. Written with `innerHTML`.                        |
| `feed-field="location"`     | `profile.location` | string    | Profile location.                                             |
| `feed-field="profile-link"` | `profile.url` | URL       | Link to the profile on Dribbble. Sets the `href` of an `<a>`. |
| `feed-field="followers"`    | `profile.followerCount` | number    | Total followers, formatted with compact locale notation.      |

{% hint style="warning" %}
**Known issue:** `feed-field="tag"` currently leaves out the last tag on each shot, so a shot with three tags shows two. A fix is in progress.
{% endhint %}

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

### GraphQL API

Fetch Dribbble feeds with the [GraphQL API](../graphql-api/overview.md). The feed returns `DribbbleFeedData`, with items under `shots.nodes`.

Dribbble feeds contain a designer profile and a list of shots.

```graphql
query DribbbleFeed($publicKey: String!) {
  feed(publicKey: $publicKey) {
    __typename
    ... on DribbbleFeedData {
      profile {
        id
        avatar {
          url(input: { width: 160, height: 160 })
        }
        bio
        createdAt
        followerCount
        url
        websiteUrl
        location
        username
        name
        isPro
      }
      shots {
        nodes {
          id
          url
          title
          publishedAt
          updatedAt
          image {
            url(input: { width: 1200 })
          }
          tags
          team {
            id
            avatar {
              url(input: { width: 160, height: 160 })
            }
            bio
            createdAt
            followerCount
            url
            websiteUrl
            location
            username
            name
            isPro
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
| `profile` | Designer profile represented by the feed. |
| `shots.nodes` | Shots in the order configured by FeedSpring. The list is always present and may be empty. |
| `url` | Public Dribbble URL for the profile, team, or shot. |
| `websiteUrl` | Optional external website configured on the profile or team. |
| `image` | Optional shot image. |
| `tags` | Tags attached to the shot. The list is always present and may be empty. |
| `team` | Team that published the shot, or `null` for shots without a team. |

### Typical use cases

* Live portfolio section on a designer's homepage
* Studio case study feed that updates as new work is posted
* Agency "recent work" block on a services page
* Designer profile page with live shot grid

### Next steps

* [Choose your setup](../getting-started/choose-your-setup.md) to render Dribbble
* [Feed Options](../attributes/feed-options.md) for `limit` and `skip`, and [Filtering](../core-concepts/filtering.md) for dashboard filters
* [Browse other feed sources](../README.md#what-this-documentation-covers)
