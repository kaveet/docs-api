---
title: "GraphQL, Gatsby & the Ghost API"
date: "2019-05-18"
meta_title: "GraphQL, Gatsby & the Ghost API"
meta_description: "Using GraphQL to feed content to Gatsby from the Ghost API"
sidebar: "gatsby"
keywords:
- "headless cms"
- "gatsby"
- "static site"
- "graphql"
- "ghost api"
- "netlify"
---

**As with all content sources for Gatsby, content is fed in by [GraphQL](https://www.gatsbyjs.org/tutorial/part-four/), and it’s no different with Ghost. The official [Gatsby Source Ghost](https://github.com/TryGhost/gatsby-source-ghost) plugin allows you to pull content from your existing Ghost site.**

## Getting started

Installing the plugin is the same as any other Gatsby plugin. Use your CLI tool of choice to navigate to your Gatsby project and a package manager to install it:

```bashhtml:title=Terminal
# yarn users
yarn add gatsby-source-ghost
# npm users
npm install --save gatsby-source-ghost
```

After that, the next step is to get the API URL and Content API Key of the Ghost site. The API URL is domain used to access the Ghost Admin. For Ghost(Pro) customers, this is the `.ghost.io`, for example: `mysite.ghost.io`. For self-hosted versions of Ghost, use the admin panel access URL and ensure that the self-hosted version is served over a https connection. The Content API Key can be found on the Integrations screen of the Ghost Admin.

Open the `gatsby-config.js` file and add the following to the `plugins` section:

```js:title=gatsby-config.js
{
  resolve: `gatsby-source-ghost`,
  options: {
    apiUrl: `https://<your-site-subdomain>.ghost.io`,
    contentApiKey: `<your content api key>`
  }
}
```

Restart the local server to apply these configuration changes.

## Querying Graph with GraphQL

The Ghost API provides 5 types of nodes:
- Post
- Page
- Author
- Tag
- Settings

These nodes match with the endpoints shown in the [API endpoints documentation](https://docs.ghost.org/api/content/#endpoints). Querying these node with GraphQL can be done like so:

```js:title=Query
{
  allGhostPost(sort: { order: DESC, fields: [published_at] }) {
    edges {
      node {
        id
        slug
        title
        html
        published_at
      }
    }
  }
}
```

The above example is retrieving all posts in descending order of the ‘published at’ field. The posts will each come back with an id, slug, title, the content (html) and the ‘published at’ date.

## Next steps

GraphQL is a very powerful tool to query the Ghost API with. This is why we’ve documented a few recipes that will get you started.

To learn more about the plugin itself, check out the [documentation within the repo on GitHub](https://github.com/TryGhost/gatsby-source-ghost#how-to-query). There’s also plenty of documentation on what the Ghost API has to offer when making queries. To learn more about GraphQL as a language, head over to the [official GraphQL docs](https://graphql.org/learn/queries/).
