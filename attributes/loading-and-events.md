---
description: Show a placeholder while a feed loads, run code when it has rendered, and keep Webflow Interactions working.
---

# Loading & Events

### Loading placeholder

If you want to show a custom placeholder while the feed loads, wrap it in an element with `feedspring="loading"`. FeedSpring removes this element once feed rendering starts:

```html
<div feedspring="YOUR-FEED-ID" feed-options="render:dynamic">
  <div feedspring="loading">
    <p>Loading posts...</p>
  </div>
  <div feedspring="post">
    <img feed-field="img" alt="" />
  </div>
</div>
```

### Hiding templates until they render

To stop an empty post template flashing before data arrives, add `style="display:none"` to the template. FeedSpring clears it when the post renders. See [`appear`](rendering.md) for other display options.

### Events

FeedSpring dispatches custom events on `document`. Listen for `feedspring:render-complete` to run code after a feed has rendered — for example, to start a slider library once the posts exist:

```js
document.addEventListener('feedspring:render-complete', () => {
  // the feed's posts are now in the page
})
```

| Event | When it fires |
| --- | --- |
| `feedspring:init-complete` | The script has found the feeds on the page. `event.detail.name` is the feed source. |
| `feedspring:load-complete` | Feed data has loaded. |
| `feedspring:render-complete` | A feed has finished rendering. Fires once per rendered feed. |
| `feedspring:error` | Something went wrong loading or rendering a feed. |

### Webflow Interactions

In Webflow, FeedSpring re-initialises Webflow Interactions after a feed renders, so interactions applied to elements inside the post template also work on every rendered post.

### Next steps

* [Rendering](rendering.md)
* [Troubleshooting](troubleshooting.md)
