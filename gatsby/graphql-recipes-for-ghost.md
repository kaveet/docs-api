---
title: "GraphQL Recipes for Ghost"
date: "2019-05-18"
meta_title: "GraphQL Recipes for Ghost"
meta_description: "A few common examples of using GraphQL to query the Ghost API"
sidebar: "gatsby"
keywords:
- "headless cms"
- "gatsby"
- "static site"
- "graphql"
- "ghost api"
- "netlify"
---

**Here are a few common examples of using GraphQL to query the Ghost API.**

Fetching content from the Ghost API using GraphQL is a modern and performant method of getting content to your front-end web app, such as a Gatsby site. Below are some recipes to retrieve chunks of data from the API that you can use and manipulate for your own needs. More extensive learning can be found in the official [GraphQL documentation](https://graphql.org/graphql-js/passing-arguments/).

## Retrieving posts

This example takes into account a limited amount of posts per page and a ‘skip’ to paginate through those pages of posts:

```js:title=Query
query GhostPostQuery($limit: Int!, $skip: Int!) {
  allGhostPost(
      sort: { order: DESC, fields: [published_at] },
      limit: $limit,
      skip: $skip
  ) {
    edges {
      node {
        ...GhostPostFields
      }
    }
  }
}
```

## Filtering Posts by tag

Filtering posts by tag is a common pattern, but can be tricky with how the query filter is formulated:

```js:title=Query
{
  allGhostPost(filter: {tags: {elemMatch: {slug: {eq: $slug}}}}) {
    edges {
      node {
        slug
        ...
      }
    }
  }
}
```

## Retrieving settings

The Ghost settings node is different to other nodes as it’s a single object - this can be queried like so:

```js:title=Query
{
  allGhostSettings {
    edges {
      node {
        title
        description
        lang
        ...
        navigation {
            label
            url
        }
      }
    }
  }
}
```

More information can be found in the [Ghost API documentation](https://docs.ghost.org/api/content/#settings).

## Retrieving all tags

Getting all tags from a Ghost site could be used to produce a tag cloud or keyword list:

```js:title=Query
{
  allGhostTag(sort: {order: ASC, fields: name}) {
      edges {
          node {
              slug
              url
              postCount
          }
      }
  }
}
```

## Further reading

Many of the GraphQL queries shown above are used within the [Gatsby Starter Ghost](https://github.com/tryghost/gatsby-starter-ghost) template. With a better understanding of how to use queries, customising the starter will become more straightforward.

Additionally, the [Gatsby Source Ghost plugin](https://github.com/TryGhost/gatsby-source-ghost) allows the use of these queries in any existing Gatsby project you may be working on.
