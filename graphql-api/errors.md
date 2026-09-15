---
description: The GraphQL error format and every FeedSpring error code.
---

# Errors

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

## Error codes

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


## Next steps

* [Limits & Caching](limits-and-caching.md) — the limits behind `request_too_large`, `query_too_complex` and `views_limit_reached`
* [Access & Security](access-and-security.md) — the allow-list behind `origin_not_allowed`
