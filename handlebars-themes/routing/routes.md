---
title: "Custom Routes"
date: "2019-02-05"
meta_title: "Ghost Themes - Custom Routes"
meta_description: "Custom routes in Ghost themes allow you to map specific URLs to specific template files, so you can use custom static pages in any way you like."
keywords:
    - handlebars
    - themes
    - urls
    - routes
    - routing
    - homepage
    - structure
sidebar: "handlebars"
---

Template routes allow you to map individual URLs to specific template files within a Ghost theme. For example: make `/custom/` load `custom.hbs`

Using template routes is very minimal. There's no default data associated with them, so there isn't any content automatically loaded in from Ghost like there is with posts and pages. Instead, you can write all the custom code you like into a specific file, and then have that file load on the route of your choice.

Custom routes are handy for creating static pages outside of Ghost Admin, when you don't want them to be editable, they use lots of custom code, or you need to create a specific custom URL with more than a basic slug.

Don't worry, we'll go through some examples of all of the above!

---

## Basic routing

The [default routes.yaml file](/api/handlebars-themes/routing/) which comes with Ghost contains an empty section under `routes`, and this is where custom routes can be defined.

Let's say you've got a big **Features** landing page with all sorts of animations and custom HTML. Rather than trying to cram all the code into the Ghost editor and hope for the best, you can instead store the code in a custom template called `features.hbs` - and then point a custom route at it:

```yaml
routes:
  /features/: features
```

The first half is the URL: `site.com/features/` - the second is the template which will be used: `features.hbs` - you leave off the `.hbs` because Ghost takes care of that part. Now you've created a new static page in Ghost, without using the admin!

You can also use custom routes to simulate subdirectories. For example if you want a "Team" page to appear, for navigational purposes, as if it's a subpage of your "About" page.

```yaml
routes:
  /features/: features
  /about/team/: team
```

Now `site.com/about/team/` is a dedicated URL for a static `team.hbs` template within your theme. Routes can be just about anything you like using letters, numbers, slashes, hyphens, and underscores.

---

## Loading data

The downside of using an `/about/team` route to point at a static `team.hbs` template is that it's... well: static.

Unlike the **Features** the team page needs to be updated fairly regularly with a list of team members; so it would be inconvenient to have to do that in code each time. What we really want is to keep the custom route, but have the page still use data stored in Ghost. This is where the `data` property comes in.

```yaml
routes:
  /features/: features
  /about/team/:
    template: team
    data: page.team
```

This will assign all of the data from a Ghost **page** with a slug of `team` to the new route, and it will also automatically redirect the original URL of the content to the new one.

Now, the data from `site.com/team/` will be available inside the `{{#page}}` block helper in a custom `team.hbs` template on `site.com/about/team/` and the old URL will redirect to the new one, to prevent the content being duplicated in two places.

---

## Building feeds & APIs

In the examples used so far, routes have been configured to generate a single page, some data and a template, but that's not all routes can do. You can make a route output just about anything, for instance a custom RSS feed or JSON endpoint.

If you create a custom template file with a [{{#get}}](/api/handlebars-themes/helpers/get/) helper API query loading a list of filtered posts, you can return those posts on a custom route with custom formatting.

```yaml
routes:
  /podcast/rss/:
    template: podcast-feed
    content_type: text/xml
```

Generally routes render HTML, but you can override that by specifying a `content_type` property with a custom mime-type.

For example you might want to build a custom RSS feed to get all posts tagged with `podcast` and deliver them to iTunes. In fact, [here's a full tutorial](https://docs.ghost.org/tutorials/custom-rss-feed/) for how to do that.

Or perhaps you'd like to build your own little public JSON API of breaking news, and provide it to other people to be able to consume your most important updates inside their websites and applications? That's fine too, you'd just pass `json` as the `content_type`.

