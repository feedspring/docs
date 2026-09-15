---
description: Request size and complexity limits, plan view allowances, and how to cache feed responses.
---

# Limits & Caching

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


## Next steps

* [Plans](../core-concepts/plans.md) — view allowances and refresh rates for each plan
* [Errors](errors.md)
