---
title: "Further Reading"
date: "2019-02-05"
meta_title: "Ghost Themes - Dynamic Routing Tips"
meta_description: "Advanced tips, resources and reading material to help you get more out of using dynamic routing in Ghost to build custom site structures."
keywords:
    - handlebars
    - themes
    - urls
    - architecture
    - structure
    - navigation
    - channels
    - routing
    - routes
sidebar: "handlebars"
---

Where you go from here is up to you, the world is your router<sup><a name="top" href="#footnote">1</a></sup>

Ghost's dynamic routing system is an extremely powerful way to build advanced structures for your site, and it's hard to document every possible example of what can be done with it in comprehensive detail.

## Detailed tutorials

While these docs cover simple examples and broad use-cases, you'll find more detailed and specific use-cases of how to build different types of publication in these tutorials:

- [Build a multi-language site with Ghost](https://docs.ghost.org/tutorials/multi-language-content/)
- [How to make an iTunes Podcast RSS feed with Ghost](https://docs.ghost.org/tutorials/custom-rss-feed/)
- [Set up a business website with Ghost](https://docs.ghost.org/tutorials/custom-home-page/)
- [How to build specialised content hubs for Ghost](https://docs.ghost.org/tutorials/building-a-content-hub/)
- [Create an ongoing story with chronological posts](https://docs.ghost.org/tutorials/chronological-posts/)

Head over to the [Ghost tutorials](https://docs.ghost.org/tutorials/) section to find even more tutorials about how to build different types of theme and website with Ghost.

---

## Limitations & troubleshooting

As you work further with dynamic routing it's worth keeping in mind that there are some limitations to what you're able to do with it. Here are a few of the most common areas where you'll find the edges of what's possible:

#### Slugs can conflict

Dynamic routing has no concept of what slugs are used in Ghost, and vice-versa. So if you create a route called `/about/` and a page in Ghost called `about` then one of them is going to work, but not both. You'll need to manage this manually.

#### Collections must be unique

If you have a collection filtering for posts tagged with `camera` and another filtering for posts tagged with `news` - then you will run into problems if a post is tagged with both `camera` and `news`. You should either trust your authors to use the correct tags, or base collections on properties which are always unique, like `primary_tag`.

#### Trailing slashes are required

You probably noticed that all the examples here use trailing slashes on routes, which is because these are required for dynamic routing to function correctly.

---

###### Footnotes

<strong><a name="footnote" href="#top">1</a></strong>: I'm so sorry
