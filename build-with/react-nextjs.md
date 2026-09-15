---
description: Pre-made React components with exposed props, plus how to build your own.
icon: react
---

# React Components

FeedSpring components are standard React components. That matters for two audiences: Framer users, because Framer runs React components natively, and React or Next.js developers, who can drop the same components into an app.

Every component ships with its props exposed, so most people never write any code at all.

### Pre-made components

Browse the full library at [feedspring.com/components](https://www.feedspring.com/components). Each one is a complete, styled layout — grid, slider, highlight, carousel — built for a specific feed source.

Because the props are exposed, you can change how a component looks and behaves without touching the code:

* Feed ID
* Number of items and skip count
* Font settings
* Container and card settings
* Image sizing and radius
* Overlay and background colour
* Text truncation

In Framer these appear as property controls in the right-hand sidebar. In a React app they are ordinary component props.

{% hint style="info" %}
If you are building in Framer, start here. See [Framer Components](framer-components.md) for the step-by-step.
{% endhint %}

### Building your own

If a pre-made component isn't the right shape and you want to write the layout yourself, you have two options. Both give you the same feed data.

#### Option 1 — Attributes

Load the attributes script for your feed source and write plain JSX with `feedspring` and `feed-field` attributes. FeedSpring fills them in on the client.

This is the fastest route and needs no data layer.

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

See [Attributes (HTML)](attributes-html.md) for the full attribute model.

#### Option 2 — The GraphQL API

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

See [API (GraphQL)](api-graphql.md) for the full reference, image transforms, and error handling.

### Which option to choose

|                          | Pre-made component | Attributes        | GraphQL API      |
| ------------------------ | ------------------ | ----------------- | ---------------- |
| Code required            | None               | Markup only       | Yes              |
| Works in Framer          | Yes                | No                | Via a code component |
| Custom layout            | Within the props   | Full              | Full             |
| Server-side rendering    | No                 | No                | Yes              |
| Feed content in page HTML for SEO | No        | No                | Yes              |
| Data transformation      | No                 | No                | Yes              |

### Next steps

* [Framer Components](framer-components.md) — using these components in Framer
* [Attributes (HTML)](attributes-html.md) — the full attribute model
* [API (GraphQL)](api-graphql.md) — fetching feed data directly
* [Attributes Reference](../attributes-reference.md) — every field, for every source
