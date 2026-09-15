---
description: How FeedSpring Feed IDs work as public keys, and how to restrict a feed to your own domains.
---

# Access & Security

## No API key

A secret API key or `Authorization` header is not required. The feed is selected using its Feed ID, passed to GraphQL as `publicKey`.

{% hint style="warning" %}
The Feed ID is public and can be used in browser code. If you configure a domain allow-list for the feed, FeedSpring also checks the request's `Origin` header.
{% endhint %}

## Domain allow-list

You can restrict a feed to origins configured in the FeedSpring dashboard. When the allow-list is enabled, the request's `Origin` header must exactly match an allowed origin, including its scheme and port when applicable.

Examples of different origins:

```text
https://example.com
https://www.example.com
http://localhost:3000
```

Browsers add `Origin` automatically to cross-origin requests. A backend, build script, or command-line client must set it explicitly when accessing a restricted feed:

```bash
curl --request POST 'https://api.feedspring.com/graphql' \
  --header 'Content-Type: application/json' \
  --header 'Origin: https://example.com' \
  --data '{"query":"query Feed($publicKey: String!) { feed(publicKey: $publicKey) { __typename } }","variables":{"publicKey":"YOUR_FEED_ID"}}'
```

{% hint style="warning" %}
An origin allow-list prevents unintended browser use, but it is not a replacement for secret authentication. Non-browser clients can set the `Origin` header themselves.
{% endhint %}


## Next steps

* [Errors](errors.md) — including `origin_not_allowed`
* [Limits & Caching](limits-and-caching.md)
