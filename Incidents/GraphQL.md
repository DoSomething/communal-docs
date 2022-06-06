# GraphQL Troubleshooting

Here's some common issues we might see on our [GraphQL](https://github.com/DoSomething/graphql/) application:

### Rate-limiting errors

```
INFO    [warning] Rate limit error occurred. Waiting for 1699 ms before retrying...
INFO    [warning] Rate limit error occurred. Waiting for 1642 ms before retrying...
INFO    [warning] Rate limit error occurred. Waiting for 1546 ms before retrying...
```

These errors usually occur because Contentful's Preview API is rather aggressively [rate-limited](https://www.contentful.com/developers/docs/references/content-preview-api/#/introduction/api-rate-limits). If we make a lot of changes at once, we may briefly these errors pop up (from [preview.dosomething.org](https://preview.dosomething.org)). They should clear up within a few minutes.
