---
title: "Working With Gatsby"
date: "2019-05-18"
meta_title: "Working With Gatsby"
meta_description: "Build a completely custom front-end to your Ghost site with the power of Gatsby!"
sidebar: "gatsby"
keywords:
- "headless cms"
- "gatsby"
- "static site"
- "graphql"
- "ghost api"
- "netlify"
---

**Build a completely custom front-end for your Ghost site with the power of Gatsby.**

Thanks to the Content API, Ghost can operate as a [completely decoupled headless CMS](https://blog.ghost.org/jamstack/), allowing developers to build their own front-end with modern web technologies, such as [Gatsby.js](https://www.gatsbyjs.org/). The Ghost documentation site is built in the same manner, so you’re looking at proof that it really works!

![Diagram of content API to Gatsby](images/admin-api-gatsby-diagram.png)

The following sections cover how Gatsby can source content from a Ghost site, how to create a Gatsby site using our starter template, and some of the setup required to get you off to the races.

---

## Gatsby Starter Ghost

**One of the best ways to start a new Gatsby site is with a Gatsby Starter, and in this case, it’s no different.**

**Prerequisites**

To use Gatsby Starters, and indeed Gatsby itself, the [Gatsby CLI](https://www.gatsbyjs.org/docs/quick-start/) tool is required. Additionally, a [Ghost account](https://ghost.org/pricing/) is needed to source content and get site related credentials.

**Getting started**

To begin, generate a new project using the [Gatsby Starter Ghost](https://github.com/TryGhost/gatsby-starter-ghost) template with the following CLI command:

```bashhtml:title=Terminal
gatsby new my-gatsby-site https://github.com/TryGhost/gatsby-starter-ghost.git
```

Navigate into the newly created project and use either npm install or yarn to install the dependencies. The Ghost team prefer to use [Yarn](https://yarnpkg.com/en/docs/install#mac-stable).

Before customising and developing in this new Gatsby site, it’s wise to give it a test run to ensure everything is installed correctly. Use the following command to run the project:

```bashhtml:title=Terminal
gatsby develop
```

Then navigate to http://localhost:8000 in a browser and view the newly created Gatsby site.

![Gatsby demo screenshot](images/gatsby-demo-screenshot.png)

## Making it your own

**So, you’ve set up a Gatsby site, but it’s not showing the right content.** This is where content sourcing comes into play. Gatsby uses [GraphQL](https://graphql.org/) as a method of pulling content from a number of APIs, including Ghost. Sourcing content from Ghost in the Gatsby Starter Ghost template is made possible with the [Gatsby Source Ghost](https://github.com/TryGhost/gatsby-source-ghost) plugin.

Configuring the plugin can be done within the template files. Within the project, navigate to and open the file named .ghost.json, which is found at root level:

```json:title=.ghost.json
{
  "development": {
    "apiUrl": "https://gatsby.ghost.io",
    "contentApiKey": "9cc5c67c358edfdd81455149d0"
  },
  "production": {
    "apiUrl": "https://gatsby.ghost.io",
    "contentApiKey": "9cc5c67c358edfdd81455149d0"
  }
}
```

This json file is set up to make environment variables a bit easier to control and edit. Change the apiUrl value to the URL of the site. For Ghost(Pro) customers, this is the Ghost URL ending in .ghost.io, and for people using the self-hosted version of Ghost, it’s the same URL used to view the admin panel.

In most cases, it’s best to change both the development and production to the same site details. Use the respective environment objects when using production and development content; this is ideal if you’re working with clients and test content. After saving these changes, restart the local server.

**Using [Netlify](https://www.netlify.com/) to host your site? If so, the netlify.toml file that comes with the starter template provides the deployment configuration straight out of the box.**

## Next steps

[The official Gatsby docs](https://www.gatsbyjs.org/docs/gatsby-project-structure/) is a great place to learn more about how typical Gatsby projects are structured and how it can be extended.

Gaining a greater understanding of how data and content can be sourced from the Ghost API with GraphQL will help with extending aforementioned starter project for more specific use cases.

There’s also a guide for setting up a new static site, such as Gatsby, [with the hosting platform Netlify](https://docs.ghost.org/integrations/netlify/). 

For community led support about linking and building a Ghost site with Gatsby, [visit the forum](https://forum.ghost.org/c/themes/).
