---
description: How many posts each feed holds, how often it refreshes, and the monthly view allowance on each plan.
icon: credit-card
---

# Plans

Your plan decides how many posts each feed holds, how often FeedSpring refreshes it, and how many views your feeds can serve each month. Current prices are on the [pricing page](https://www.feedspring.com/pricing).

### Posts and refresh rates

| Platform  | Free             | Personal         | Business        | Enterprise       |
| --------- | ---------------- | ---------------- | --------------- | ---------------- |
| Instagram | 8 posts (24 hrs) | 12 posts (6 hrs) | 12 posts (1 hr) | 12 posts (1 hr)  |
| Google    | 8 posts (24 hrs) | 16 posts (6 hrs) | 32 posts (1 hr) | 200 posts (1 hr) |
| TikTok    | 8 posts (24 hrs) | 16 posts (6 hrs) | 32 posts (1 hr) | 200 posts (1 hr) |
| Dribbble  | 8 posts (24 hrs) | 16 posts (6 hrs) | 32 posts (1 hr) | 200 posts (1 hr) |

{% hint style="info" %}
Instagram is capped at 12 posts on every paid plan. This is a limit of the Instagram API, not of FeedSpring.

Update frequency may vary slightly depending on the platform and availability of new content.
{% endhint %}

### Monthly views

| Plan | Views per month |
| --- | --- |
| Free | 1,000 |
| Personal | 15,000 |
| Business | 50,000 |
| Enterprise | 1,000,000 |

When the allowance is reached, feeds stop loading until it resets or the plan changes. The GraphQL API returns the `views_limit_reached` error in this case — see [Errors](../graphql-api/errors.md).

### Next steps

* [Feeds & Syncing](feeds-and-syncing.md) — how refreshing works
* [Limits & Caching](../graphql-api/limits-and-caching.md) — request limits for the GraphQL API
