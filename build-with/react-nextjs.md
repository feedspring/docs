---
description: Add FeedSpring feeds to React and Next.js apps with the GraphQL API or attributes.
icon: react
---

# React & Next.js

There are two ways to add a FeedSpring feed to a React or Next.js app. Both use the same feed data.

|  | GraphQL API | Attributes |
| --- | --- | --- |
| Best for | Server rendering, SEO, caching, custom data handling | Client-rendered pages and the fastest setup |
| Feed content in the page HTML | Yes | No |
| What you write | A query and your own components | JSX with `feedspring` and `feed-field` attributes |
| Works in React Server Components | Yes | No — load the script on the client |

### Option 1 — The GraphQL API

Fetch the feed yourself and render it however you like. This is the right choice when you need server-side rendering, caching, SEO on feed content, or want to transform the data before it reaches your components.

```jsx
async function getFeed(publicKey) {
  const res = await fetch('https://api.feedspring.com/graphql', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({
      query: `
        query Feed($publicKey: String!) {
          feed(publicKey: $publicKey) {
            __typename
            ... on InstagramFeedData {
              profile { username fullName avatar { url(input: { width: 160, height: 160 }) } }
              posts { nodes { id caption url image { url(input: { width: 800 }) } } }
            }
          }
        }
      `,
      variables: { publicKey },
    }),
  })

  const { data, errors } = await res.json()
  if (errors) throw new Error(errors[0].message)
  return data.feed
}

export default async function InstagramGrid() {
  const feed = await getFeed('inst_YOUR-FEED-ID')

  return (
    <div className="grid">
      {feed.posts.nodes.slice(0, 8).map((post) => (
        <a key={post.id} href={post.url} target="_blank" rel="noopener noreferrer">
          <img src={post.image?.url} alt="" />
          <p>{post.caption}</p>
        </a>
      ))}
    </div>
  )
}
```

Four things that catch people out:

* **`feed` returns a union type.** You must select `__typename` and use an inline fragment (`... on InstagramFeedData`) matching your feed source.
* **The collection is named per source** — `posts` for Instagram, `videos` for TikTok, `shots` for Dribbble, `reviews` for Google Reviews.
* **Items sit inside `nodes`.** Query `posts { nodes { … } }` and read the array from `feed.posts.nodes`.
* **Images are objects, not strings.** Request `image { url(input: { width: 800 }) }`, never `image` on its own.

No API key is needed — the Feed ID is the credential and is safe to use in browser code.

See the [GraphQL API](../graphql-api/overview.md) for the full reference, image transforms, and error handling.

### Option 2 — Attributes

Load the attributes script for your feed source and write plain JSX with `feedspring` and `feed-field` attributes. FeedSpring fills them in on the client.

This is the fastest route and needs no data layer, but the feed renders in the browser, so its content is not in the server-rendered HTML.

```jsx
export default function InstagramGrid() {
  return (
    <section
      feedspring="inst_YOUR-FEED-ID"
      feed-options="render:dynamic|limit:8"
    >
      <article feedspring="post">
        <img feed-field="img" alt="" />
        <p feed-field="caption"></p>
        <a feed-field="link" target="_blank" rel="noopener">View post</a>
      </article>
    </section>
  )
}
```

Things to know:

* Load the script client-side only. In Next.js, use `next/script` with `strategy="afterInteractive"`, or load it in a `useEffect`.
* Custom attributes pass through JSX as plain strings — write them exactly as shown.
* Avoid adding the script more than once if the route remounts frequently.
* This renders on the client, so feed content is not present in the server-rendered HTML. If you need the feed indexed for SEO, use the API instead.

See [Attributes](../attributes/overview.md) for the full attribute model.

### Pre-made components

FeedSpring's pre-made components are React components built for Framer, where every setting is exposed as a property control. If you are building in Framer, see [Framer](framer.md).

### Next steps

* [GraphQL API](../graphql-api/overview.md) — the full API reference
* [Attributes](../attributes/overview.md) — the full attribute model
* [Feeds](../feeds/instagram.md) — fields and queries for each source
