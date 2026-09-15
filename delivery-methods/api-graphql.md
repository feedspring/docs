---
description: Use the FeedSpring GraphQL API to fetch a feed as structured data and render it in any website, application, or backend.
icon: rectangle-api
---

# API (GraphQL)

The API currently supports Instagram, TikTok, Dribbble, and Google Reviews feeds. Each source has its own strongly typed response, and you request only the fields your application needs.

{% hint style="info" %}
Use the GraphQL API when you want complete control over data fetching and rendering. For a ready-made UI, use the React, Framer, Webflow, or HTML delivery methods instead.
{% endhint %}

## Endpoint

Send GraphQL operations as HTTP `POST` requests:

```text
https://api.feedspring.com/graphql
```

Set the request content type to `application/json`. A secret API key or `Authorization` header is not required. The feed is selected using its Feed ID, passed to GraphQL as `publicKey`.

The endpoint accepts GraphQL operations over `POST`. It does not execute operations sent with `GET`.

{% hint style="warning" %}
The Feed ID is public and can be used in browser code. If you configure a domain allow-list for the feed, FeedSpring also checks the request's `Origin` header.
{% endhint %}

## Quickstart

This query returns the first fields needed to render an Instagram feed:

```graphql
query Feed($publicKey: String!) {
	feed(publicKey: $publicKey) {
		__typename
		... on InstagramFeedData {
			profile {
				username
				fullName
				avatar {
					url(input: { width: 160, height: 160 })
				}
			}
			posts {
				nodes {
					id
					caption
					url
					image {
						url(input: { width: 800 })
					}
				}
			}
		}
	}
}
```

Choose a language and replace `inst_YOUR_FEED_ID` with your Feed ID:

{% tabs %}
{% tab title="cURL" %}

```bash
curl --fail-with-body --request POST 'https://api.feedspring.com/graphql' \
  --header 'Content-Type: application/json' \
  --data '{"query":"query Feed($publicKey: String!) { feed(publicKey: $publicKey) { __typename ... on InstagramFeedData { profile { username fullName avatar { url(input: { width: 160, height: 160 }) } } posts { nodes { id caption url image { url(input: { width: 800 }) } } } } } }","variables":{"publicKey":"inst_YOUR_FEED_ID"}}'
```

{% endtab %}

{% tab title="JavaScript" %}

```javascript
const query = `
  query Feed($publicKey: String!) {
    feed(publicKey: $publicKey) {
      __typename
      ... on InstagramFeedData {
        profile {
          username
          fullName
          avatar {
            url(input: { width: 160, height: 160 })
          }
        }
        posts {
          nodes {
            id
            caption
            url
            image {
              url(input: { width: 800 })
            }
          }
        }
      }
    }
  }
`

const response = await fetch('https://api.feedspring.com/graphql', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
  },
  body: JSON.stringify({
    query,
    variables: { publicKey: 'inst_YOUR_FEED_ID' },
  }),
})

const result = await response.json()

if (!response.ok || result.errors?.length) {
  throw new Error(result.errors?.[0]?.message ?? `HTTP ${response.status}`)
}

console.log(result.data.feed)
```

{% endtab %}

{% tab title="Python" %}

Install the `requests` package before running this example.

```python
import requests

query = """
query Feed($publicKey: String!) {
  feed(publicKey: $publicKey) {
    __typename
    ... on InstagramFeedData {
      profile {
        username
        fullName
        avatar {
          url(input: { width: 160, height: 160 })
        }
      }
      posts {
        nodes {
          id
          caption
          url
          image {
            url(input: { width: 800 })
          }
        }
      }
    }
  }
}
"""

response = requests.post(
    "https://api.feedspring.com/graphql",
    json={
        "query": query,
        "variables": {"publicKey": "inst_YOUR_FEED_ID"},
    },
    timeout=10,
)
result = response.json()
errors = result.get("errors") or []

if not response.ok or errors:
    message = errors[0]["message"] if errors else f"HTTP {response.status_code}"
    raise RuntimeError(message)

print(result["data"]["feed"])
```

{% endtab %}

{% tab title="PHP" %}

```php
<?php

$query = <<<'GRAPHQL'
query Feed($publicKey: String!) {
  feed(publicKey: $publicKey) {
    __typename
    ... on InstagramFeedData {
      profile {
        username
        fullName
        avatar {
          url(input: { width: 160, height: 160 })
        }
      }
      posts {
        nodes {
          id
          caption
          url
          image {
            url(input: { width: 800 })
          }
        }
      }
    }
  }
}
GRAPHQL;

$request = curl_init('https://api.feedspring.com/graphql');
curl_setopt_array($request, [
    CURLOPT_POST => true,
    CURLOPT_RETURNTRANSFER => true,
    CURLOPT_HTTPHEADER => ['Content-Type: application/json'],
    CURLOPT_POSTFIELDS => json_encode([
        'query' => $query,
        'variables' => ['publicKey' => 'inst_YOUR_FEED_ID'],
    ], JSON_THROW_ON_ERROR),
]);

$response = curl_exec($request);
if ($response === false) {
    throw new RuntimeException(curl_error($request));
}

$status = curl_getinfo($request, CURLINFO_RESPONSE_CODE);
curl_close($request);
$result = json_decode($response, true, 512, JSON_THROW_ON_ERROR);

if ($status < 200 || $status >= 300 || !empty($result['errors'])) {
    $message = $result['errors'][0]['message'] ?? "HTTP {$status}";
    throw new RuntimeException($message);
}

print_r($result['data']['feed']);
```

{% endtab %}

{% tab title="Go" %}

```go
package main

import (
	"bytes"
	"encoding/json"
	"fmt"
	"net/http"
	"time"
)

const query = `
query Feed($publicKey: String!) {
  feed(publicKey: $publicKey) {
    __typename
    ... on InstagramFeedData {
      profile {
        username
        fullName
        avatar {
          url(input: { width: 160, height: 160 })
        }
      }
      posts {
        nodes {
          id
          caption
          url
          image {
            url(input: { width: 800 })
          }
        }
      }
    }
  }
}`

func main() {
	payload, err := json.Marshal(map[string]any{
		"query":     query,
		"variables": map[string]string{"publicKey": "inst_YOUR_FEED_ID"},
	})
	if err != nil {
		panic(err)
	}

	client := &http.Client{Timeout: 10 * time.Second}
	response, err := client.Post(
		"https://api.feedspring.com/graphql",
		"application/json",
		bytes.NewReader(payload),
	)
	if err != nil {
		panic(err)
	}
	defer response.Body.Close()

	var result struct {
		Data struct {
			Feed json.RawMessage `json:"feed"`
		} `json:"data"`
		Errors []struct {
			Message string `json:"message"`
		} `json:"errors"`
	}
	if err := json.NewDecoder(response.Body).Decode(&result); err != nil {
		panic(err)
	}
	if response.StatusCode < 200 || response.StatusCode >= 300 || len(result.Errors) > 0 {
		if len(result.Errors) > 0 {
			panic(result.Errors[0].Message)
		}
		panic(fmt.Sprintf("HTTP %d", response.StatusCode))
	}

	fmt.Println(string(result.Data.Feed))
}
```

{% endtab %}
{% endtabs %}

A successful response has the standard GraphQL shape:

```json
{
  "data": {
    "feed": {
      "__typename": "InstagramFeedData",
      "profile": {
        "username": "feedspring",
        "fullName": "FeedSpring",
        "avatar": {
          "url": "https://images.feedspring.com/..."
        }
      },
      "posts": {
        "nodes": [
          {
            "id": "17895695668004550",
            "caption": "Build social feeds your way.",
            "url": "https://www.instagram.com/p/example/",
            "image": {
              "url": "https://images.feedspring.com/..."
            }
          }
        ]
      }
    }
  }
}
```

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

## Instagram

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

### Instagram field notes

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

## TikTok

TikTok feeds contain profile information and a list of videos.

```graphql
query TikTokFeed($publicKey: String!) {
  feed(publicKey: $publicKey) {
    __typename
    ... on TikTokFeedData {
      profile {
        id
        avatar {
          url(input: { width: 160, height: 160 })
        }
        displayName
        bio
        url
        followingCount
        likeCount
        isVerified
        followerCount
      }
      videos {
        nodes {
          id
          url
          embedUrl
          embedHtml
          cover {
            url(input: { width: 800 })
          }
          title
          description
          publishedAt
          viewCount
          likeCount
          shareCount
          commentCount
          durationSeconds
        }
      }
    }
  }
}
```

### TikTok field notes

| Field | Description |
| --- | --- |
| `profile` | Profile represented by the feed. |
| `videos.nodes` | Videos in the order configured by FeedSpring. The list is always present and may be empty. |
| `url` | Public TikTok URL for the profile or video. |
| `embedUrl` | URL intended for embedding the video. |
| `embedHtml` | Embed markup supplied for the video. Treat it as third-party HTML before inserting it into a page. |
| `cover` | Optional video cover image. |
| `publishedAt` | Publication time, or `null` when unavailable. |
| `durationSeconds` | Video duration in seconds. |

## Dribbble

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

### Dribbble field notes

| Field | Description |
| --- | --- |
| `profile` | Designer profile represented by the feed. |
| `shots.nodes` | Shots in the order configured by FeedSpring. The list is always present and may be empty. |
| `url` | Public Dribbble URL for the profile, team, or shot. |
| `websiteUrl` | Optional external website configured on the profile or team. |
| `image` | Optional shot image. |
| `tags` | Tags attached to the shot. The list is always present and may be empty. |
| `team` | Team that published the shot, or `null` for shots without a team. |

## Google Reviews

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

### Google Reviews field notes

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

## Images

Image fields do not expose third-party source URLs directly. Instead, request a delivery URL using the `url` field:

```graphql
image {
  url
}
```

With no input, FeedSpring returns the image at its source dimensions in `WEBP` format.

### Resize an image

Pass a width, height, or both:

```graphql
image {
  url(input: {
    width: 800
    height: 600
    format: WEBP
  })
}
```

Images use `fit` resizing and are not cropped. If you specify only one dimension, the other dimension is calculated from the original aspect ratio.

Supported formats:

| Value | Output format |
| --- | --- |
| `WEBP` | WebP, the default |
| `JPEG` | JPEG |

### Responsive images

Use `srcset` to generate several variants in one field:

```graphql
image {
  url(input: { width: 1200 })
  srcset(input: [
    { key: "small", width: 480 }
    { key: "medium", width: 768 }
    { key: "large", width: 1200 }
  ]) {
    key
    url
  }
}
```

The response preserves the keys and order from the input:

```json
{
  "url": "https://images.feedspring.com/...",
  "srcset": [
    {
      "key": "small",
      "url": "https://images.feedspring.com/..."
    },
    {
      "key": "medium",
      "url": "https://images.feedspring.com/..."
    },
    {
      "key": "large",
      "url": "https://images.feedspring.com/..."
    }
  ]
}
```

`key` is an application-defined label. You can use values such as `480w`, `tablet`, or `large` and map them to HTML `srcset` descriptors in your rendering code.

Calling `srcset` without input returns an empty list:

```graphql
image {
  srcset {
    key
    url
  }
}
```

### Image limits

| Limit | Value |
| --- | --- |
| Width | `1` to `4096` pixels |
| Height | `1` to `4096` pixels |
| Requested image area | When both dimensions are set, `width x height` must not exceed `16,777,216` pixels |
| Variants per `srcset` field | Up to `3` |
| `srcset` key length | `1` to `32` characters |
| Allowed key characters | `A-Z`, `a-z`, `0-9`, `_`, `-` |

Keys in the same `srcset` input must be unique. Invalid dimensions, keys, or duplicate keys return the `invalid_image_transform` error code. Unsupported enum values are rejected during normal GraphQL input validation.

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

## Errors

The API uses the standard GraphQL error format. Application-specific error codes are available in `errors[].extensions.code`:

```json
{
  "data": {
    "feed": null
  },
  "errors": [
    {
      "message": "Feed not found",
      "path": ["feed"],
      "extensions": {
        "code": "feed_not_found"
      }
    }
  ]
}
```

Always check the `errors` property, even when the HTTP status is `200`.

### Error codes

| Code | Meaning |
| --- | --- |
| `feed_not_found` | No feed exists for the supplied Feed ID. |
| `feed_not_active` | The feed exists but is not active. |
| `origin_not_allowed` | The request origin is not in the feed's allow-list. |
| `views_limit_reached` | The feed owner's monthly view allowance has been reached. |
| `invalid_image_transform` | An image transform or `srcset` input is invalid. |
| `image_unavailable` | The requested image has no available source. |
| `request_too_large` | The HTTP request body exceeds the allowed size. |
| `query_too_complex` | The operation exceeds the public API complexity policy. |
| `internal_error` | FeedSpring could not complete the operation. |

GraphQL may also return standard parse and validation errors for malformed operations, unknown fields, missing variables, or invalid input values.

## Request limits

Public GraphQL requests have the following limits:

| Limit | Behaviour |
| --- | --- |
| Request body | Maximum `64 KiB`; larger requests return HTTP `413` with `request_too_large`. |
| Feed lookups | One `feed` lookup per operation. Repeating it with aliases is rejected. |
| Query complexity | Excessively complex operations return HTTP `422` with `query_too_complex`. |

Keep queries focused on the fields used by the current page or component. This reduces response size and avoids unnecessary image variants.

Feed requests are also subject to the feed owner's plan limits. Once the monthly view allowance is reached, the API returns `views_limit_reached` until the allowance resets or the plan changes.

## Caching

Feed responses include a strict `no-store` cache policy. Browsers, CDNs, and reverse proxies should not store the HTTP response automatically.

You can keep the returned data in your application's memory or state for the lifetime of the current page. If you introduce persistent application-side caching, ensure that its lifetime matches how frequently the feed should update and that it does not bypass FeedSpring plan or access controls.

## Schema discovery

GraphQL introspection is enabled, so GraphQL clients and code-generation tools can discover the schema directly from the endpoint.

The endpoint does not provide a browser GraphQL Playground. Send introspection and application operations with an HTTP `POST` client such as GraphQL Code Generator, Apollo tooling, `curl`, or your own application.

### Download the schema

**Schema URL**

```text
https://api.feedspring.com/graphql/schema.graphql
```

[Download the GraphQL schema](https://api.feedspring.com/graphql/schema.graphql) as an SDL `.graphql` file.

You can also download it from the command line:

```bash
curl --output feedspring-schema.graphql \
  'https://api.feedspring.com/graphql/schema.graphql'
```

## Next steps

* Create and configure a feed in the FeedSpring dashboard
* Review the source-specific guides for [Instagram](/introduction/feeds/instagram.md), [TikTok](/introduction/feeds/tiktok.md), [Dribbble](/introduction/feeds/dribbble.md), and [Google Reviews](/introduction/feeds/google-reviews.md)
* Use responsive image variants to match your layout
* Add loading, empty, and GraphQL error states to your application
