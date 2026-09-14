---
icon: react
---

# React Components

Use FeedSpring in React by copying a component into your project and rendering your feed data directly in your app.

This is a good option if you want more control than attributes, while still starting from a ready-made layout.

#### When to use React

Use React components if you are:

* Building with React or Next.js
* Comfortable editing JSX and CSS
* Creating custom layouts inside your frontend
* Looking for more flexibility than attributes

If you want a no-code or visual setup, use Framer components or attributes instead. If you want to render feeds completely from scratch, use the API.

#### How it works

The React component fetches your feed from FeedSpring and renders the results in your app.

You provide a feed ID, the component hits the feed endpoint, and the JSON comes back ready to render. This is the same data as the API, just wrapped in a working component so you do not have to write the fetch and render logic yourself.

#### What you control

Because the component lives in your codebase, every part of it is editable:

* The JSX structure and layout
* Number of items displayed
* Styling and responsive behaviour
* Loading and empty states
* How each field is rendered

#### Example

A minimal Instagram grid, based on the same pattern shipped in the components library:

jsx

```jsx
function InstagramGrid({ feedId, posts = 8 }) {
  const { items, loading } = useFeed(feedId, posts)

  return (
    <section>
      <div className="grid">
        {loading
          ? Array.from({ length: posts }, (_, i) => <div key={i} className="post" />)
          : items.map((item, i) => (
              <a key={item.id ?? i} href={item.permalink} target="_blank" rel="noopener noreferrer">
                <img src={item.mediaUrl} alt="" />
                <p>{item.caption ?? ''}</p>
              </a>
            ))
        }
      </div>
    </section>
  )
}
```

The `useFeed` hook is a small wrapper around a `fetch` to the FeedSpring feed endpoint. You can keep it as a utility, replace it with your own data layer, or move the fetch server-side in a framework like Next.js.

#### Field names in React

The React component works with the **raw JSON field names** from the feed data. These are different from the short `feed-field` names used in the attributes delivery method. For example:

| In React              | In Attributes           |
| --------------------- | ----------------------- |
| `item.mediaUrl`       | `feed-field="img"`      |
| `item.permalink`      | `feed-field="link"`     |
| `item.caption`        | `feed-field="caption"`  |
| `feed.extra.avatar`   | `feed-field="avatar"`   |
| `feed.extra.username` | `feed-field="username"` |

The data is the same, the access pattern is just React-native. See the Posts & fields page for the full mapping across delivery methods.

#### Post fields used in the Instagram component

The default Instagram grid uses the following data from each post:

* `item.mediaUrl` — the post image
* `item.permalink` — link to the original post on Instagram
* `item.caption` — the post caption

#### Profile fields used in the Instagram component

For account-level elements like a header, the component reads from `feed.extra`:

* `feed.extra.avatar` — profile image
* `feed.extra.username` — Instagram handle

Different feed sources (Google Reviews, TikTok, Dribbble) expose different fields. See the Feeds reference for a full list.

#### Notes

* The current components are **copy-paste**, not an NPM package. You own the code and can edit it freely.
* Item limits are applied **client-side** — the component fetches the feed and then slices to the requested count. For server-side limiting, use the API directly with a `limit` parameter.
* Feeds are fetched on the client by default. For server-side rendering or caching, lift the fetch into your framework's data layer.
* The pattern works for any FeedSpring feed type. Swap the Instagram component for Google Reviews, TikTok, or Dribbble variants from the Components library.

#### When to use the API instead

Use the API directly if you want to:

* Fetch feed data yourself and transform or cache it
* Render from scratch without starting from a component
* Use the data outside React, for example in a native app or backend
* Apply server-side filtering, caching, or pagination

#### Next steps

* View available fields for each feed type
* Understand how feeds and fields are structured
* Explore the API for advanced usage
