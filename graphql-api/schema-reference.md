---
description: Discover the FeedSpring GraphQL schema through introspection or download it as an SDL file.
---

# Schema Reference

GraphQL introspection is enabled, so GraphQL clients and code-generation tools can discover the schema directly from the endpoint.

The endpoint does not provide a browser GraphQL Playground. Send introspection and application operations with an HTTP `POST` client such as GraphQL Code Generator, Apollo tooling, `curl`, or your own application.

## Download the schema

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
