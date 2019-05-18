---
title: "Working With Gatsby"
date: "2019-05-18"
meta_title: "Working With Gatsby"
meta_description: "Build a completely custom front-end to your Ghost blog with the power of Gatsby!"
keywords:
  - "headless cms"
  - "gatsby"
  - "static site"
  - "ghost api"
  - "netlify"
---

**Build a completely custom front-end to your Ghost blog with the power of Gatsby!**

Thanks to the Content API, Ghost can now operate as a [completely decoupled headless CMS](https://blog.ghost.org/jamstack/), allowing developers to build their own front-end with modern web technologies, such as [Gatsby.js](https://www.gatsbyjs.org/). This entire docs site is built in the same manner, so you’re looking at proof that it really works!

![](./images/admin-api-gatsby-diagram.png)

The following sections will cover how Gatsby can source content from a Ghost blog, how to create a Gatsby blog using our starter template, and some of the setup required to get you off to the races.

## Sourcing your blog

**As with all content sources for Gatsby, content is fed in by [GraphQL](https://www.gatsbyjs.org/tutorial/part-four/), and it’s no different with Ghost. The official [Gatsby Source Ghost](https://github.com/TryGhost/gatsby-source-ghost) plugin will allow you to pull content from your existing Ghost blog.**

Installing the plugin is just the same as any other Gatsby plugin. Use your CLI tool of choice to navigate to your Gatsby project and a package manager to install it:

``` bash
# yarn users
yarn add gatsby-source-ghost
# npm users
npm install --save gatsby-source-ghost
```

After that, you’ll need to get your blogs’ API URL and Content API Key.

The API URL will be your site URL ending in `.ghost.io`, for example: `myblog.ghost.io`. For Ghost(Pro) customers, your Ghost blog URL will also end in `.ghost.io`. If you’re using a self-hosted version of Ghost, use the URL that allows you to access the admin panel - please ensure that the self-hosted version is served over a https connection. The Content API Key can be found on the Integrations screen of the Ghost Admin.

Open your `gatsby-config.js` file and add the following to the `plugins` section:

``` json
{
  resolve: `gatsby-source-ghost`,
  options: {
    apiUrl: `https://<your-site-subdomain>.ghost.io`,
    contentApiKey: `<your content api key>`
  }
}
```

Restart your local server to apply these configuration changes.

To learn more about the plugin, check out the [documentation within the repo on GitHub](https://github.com/TryGhost/gatsby-source-ghost#how-to-query). If you’re wanting to delve deeper into writing GraphQL queries for Ghost, you can read our [documentation on the Content API](https://docs.ghost.org/api/content/) and find out more on the [official GraphQL docs](https://graphql.org/learn/queries/).

**Finding this all a little overwhelming? Don’t worry! You’re not alone. Thankfully, Ghost have contributed to the ever growing list of Gatsby Starters with a [Gatsby Starter Ghost](https://github.com/TryGhost/gatsby-starter-ghost) template That has pretty much everything to get you started, including the source plugin mentioned above.**

## Gatsby Starter Ghost

**One of the best ways to start a new Gatsby site is with a Gatsby Starter, and in this case, it’s no different.**

Before you begin, you’re going to need the Gatsby CLI tool in order to download a starter. Use the following command in your CLI tool of choice:

``` bash
install -g gatsby-cli
```

Now you can pull down the official [Gatsby Starter Ghost](https://github.com/TryGhost/gatsby-starter-ghost) template directly from GitHub:

``` bash
gatsby new my-gatsby-blog https://github.com/TryGhost/gatsby-starter-ghost.git
```

Navigate into your newly created project and use either `npm install` or `yarn` to install the dependencies. At Ghost, we prefer to use [Yarn](https://yarnpkg.com/en/docs/install#mac-stable).

Before you start customising and developing your Gatsby site, it’s wise to run it to ensure everything has installed correctly. Because you installed the Gatsby CLI tool as part of the previous steps, you can now run `gatsby develop` in the project folder, which will start up the project and run it on your local machine.

Once everything is installed correctly, navigate to http://localhost:8000 in your browser and view your newly created Gatsby blog.

![](./images/gatsby-demo-screenshot.png)

## Making it your own

So, you’ve set up a Gatsby blog, but it’s not **your** blog posts. This is where content sourcing comes into play. You may have seen in a previous section about our [Gatsby Source Ghost](https://github.com/TryGhost/gatsby-source-ghost) plugin, which can pull content into your Gatsby blog from the Ghost Admin via the Content API.

The Gatsby Start Ghost template already has this built in; all it requires is some configuration. Within your project, navigate to the file `.ghost.json` at root level. You’ll see something that looks like this:

``` json
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

We’ve setup this file in order to make environment variables a bit easier to control and edit. Change the `apiUrl` value to the URL of your blog. If you’re a Ghost(Pro) customer, this will be the Ghost URL ending in `.ghost.io`, and for people using the self-hosted version of Ghost, it will be the same URL you use to access the admin panel.

In most cases, you‘ll probably want to change both the `development` and `production` to the same site details. However, if you’re working with a development site, please go ahead and put those details into the `development` json object - it’s very useful if you’re working with a client that’s testing new content. The `production` details should be used when you deploy your blog. After you’ve saved these changes, restart your local server.

**If you’re using [Netlify](https://www.netlify.com/) to host your blog, then you’ll benefit from the `netlify.toml` file that also comes with the starter template. This file with automatically configure your environment details in just the right way.**

## Next steps

The sections above will get you rolling with your new Gatsby blog. We recommend checking out the [tutorials on the official Gatsby site](https://www.gatsbyjs.org/tutorial/) in order to develop with Gatsby.

Deploying your blog to Netlify? Take a look at our [documentation on Integrating with Netlify](https://docs.ghost.org/integrations/netlify/) and using Web Hooks with Ghost.

For community led support about linking and building a Ghost blog with Gatsby, visit [the forum](https://forum.ghost.org/c/themes/).
