---
description: Send your first GraphQL query and get a FeedSpring feed back as JSON.
---

# GraphQL Quickstart

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


## Next steps

* [Querying Feeds](querying-feeds.md) — how feed types, fragments and `nodes` work
* [Images](images.md) — request image sizes that match your layout
* [Errors](errors.md) — handle errors and empty states
* Queries for each source: [Instagram](../feeds/instagram.md#graphql-api), [Google Reviews](../feeds/google-reviews.md#graphql-api), [TikTok](../feeds/tiktok.md#graphql-api), [Dribbble](../feeds/dribbble.md#graphql-api)
