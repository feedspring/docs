---
description: Request resized, reformatted and responsive images from any FeedSpring image field.
---

# Images

Image fields do not expose third-party source URLs directly. Instead, request a delivery URL using the `url` field:

```graphql
image {
  url
}
```

With no input, FeedSpring returns the image at its source dimensions in `WEBP` format.

## Resize an image

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

## Responsive images

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

## Image limits

| Limit | Value |
| --- | --- |
| Width | `1` to `4096` pixels |
| Height | `1` to `4096` pixels |
| Requested image area | When both dimensions are set, `width x height` must not exceed `16,777,216` pixels |
| Variants per `srcset` field | Up to `3` |
| `srcset` key length | `1` to `32` characters |
| Allowed key characters | `A-Z`, `a-z`, `0-9`, `_`, `-` |

Keys in the same `srcset` input must be unique. Invalid dimensions, keys, or duplicate keys return the `invalid_image_transform` error code. Unsupported enum values are rejected during normal GraphQL input validation.
