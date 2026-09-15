---
description: Fetch any FeedSpring feed as structured data with GraphQL, and render it in any website, app or backend.
icon: rectangle-api
---

# GraphQL API

The API currently supports Instagram, TikTok, Dribbble, and Google Reviews feeds. Each source has its own strongly typed response, and you request only the fields your application needs.

{% hint style="info" %}
Use the GraphQL API when you want complete control over data fetching and rendering. For a ready-made UI, use [pre-made Framer components](../build-with/framer.md) or [Attributes](../attributes/overview.md) instead — see [Choose your setup](../getting-started/choose-your-setup.md).
{% endhint %}


## When to use the API

Choose the GraphQL API when you need:

* Server-side rendering, or feed content in the page HTML so search engines can index it — especially useful for Google Reviews
* Caching, or one data layer shared across several parts of an application
* To transform, filter or combine feed data before rendering
* To use feed data outside the browser, for example in a backend, mobile app or build script

## Endpoint

Send GraphQL operations as HTTP `POST` requests:

```text
https://api.feedspring.com/graphql
```

Set the request content type to `application/json`. A secret API key or `Authorization` header is not required. The feed is selected using its Feed ID, passed to GraphQL as `publicKey`.

The endpoint accepts GraphQL operations over `POST`. It does not execute operations sent with `GET`.

See [Access & Security](access-and-security.md) for how Feed IDs and domain allow-lists work.

## In this section

| Page | What it covers |
| --- | --- |
| [Quickstart](quickstart.md) | Your first query, in cURL, JavaScript, Python, PHP and Go |
| [Querying Feeds](querying-feeds.md) | Feed types, fragments, `nodes`, nullability and item limits |
| [Images](images.md) | Resizing, formats and responsive `srcset` variants |
| [Access & Security](access-and-security.md) | Public Feed IDs and domain allow-lists |
| [Errors](errors.md) | Error format and error codes |
| [Limits & Caching](limits-and-caching.md) | Request limits, view allowances and caching |
| [Schema Reference](schema-reference.md) | Introspection and downloading the schema |

Complete queries for each source are on the feed pages: [Instagram](../feeds/instagram.md#graphql-api), [Google Reviews](../feeds/google-reviews.md#graphql-api), [TikTok](../feeds/tiktok.md#graphql-api) and [Dribbble](../feeds/dribbble.md#graphql-api).
