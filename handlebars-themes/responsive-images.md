---
title: "Responsive Images"
date: "2018-12-18"
meta_title: "How to use responsive images in Ghost themes"
meta_description: "Ghost themes support automatic image resizing, allowing you to use a minimal handlebars helper to output different image sizes."
keywords:
    - handlebars
    - themes
    - images
    - media
    - responsive
    - performance
sidebar: "handlebars"
---

Optimise the performance of your site by outputting images at different sizes depending on where they appear

## Overview

So you upload glorious 2000px feature images to all your posts to appear in the giant hero/header on individual articles and things look great. On your home page, though, you're displaying those feature images as 250px thumbnails for every single post. And there are a lot of them. Suddenly, those big beautiful 2000px jpgs are no longer ideal and your site performance slows right down.

Ghost's dynamic image sizes feature solves this, by allowing you to use scaled down images or build out responsive image srcsets for your theme.


## Configuration

First, in your theme's `package.json` - you'll need to set up which sizes you'd like to use. You can change these sizes at any time and Ghost will automatically regenerate copies of your images at the specified sizes, so treat this more like a cache than anything else. Generally speaking, less is better. Try to not have more than 10 image sizes so your media storage doesn't grow out of control.

Here's a sample of [the image sizes in Ghost's default Casper theme](https://github.com/TryGhost/Casper/blob/master/package.json).

```json:title=package.json
"config": {
    "image_sizes": {
        "xxs": {
            "width": 30
        },
        "xs": {
            "width": 100
        },
        "s": {
            "width": 300
        },
        "m": {
            "width": 600
        },
        "l": {
            "width": 1000
        },
        "xl": {
            "width": 2000
        }
    }
}
```


## Using image sizes

Once your image sizes are defined, you can pass a `size` parameter to the [{{img_url}}](/api/handlebars-themes/helpers/img_url/) helper in your theme to output an image at a particular size.

```handlebars
<img src="{{img_url feature_image size="s"}}">
```

If you want to build out [full responsive images](https://medium.freecodecamp.org/a-guide-to-responsive-images-with-ready-to-use-templates-c400bd65c433), then create your html srcsets passing in multiple image sizes, and let the browser do the rest.

Here's an [example from Ghost default Casper theme](https://github.com/TryGhost/Casper/blob/master/partials/post-card.hbs) implementation:

```handlebars:title=index.hbs
<img class="post-image"
    srcset="{{img_url feature_image size="s"}} 300w,
            {{img_url feature_image size="m"}} 600w,
            {{img_url feature_image size="l"}} 1000w,
            {{img_url feature_image size="xl"}} 2000w"
    sizes="(max-width: 1000px) 400px, 700px"
    src="{{img_url feature_image size="m"}}"
    alt="{{title}}"
/>
```

## Compatibility

Ghost image sizes will be automatically generated for all images uploaded directly _to_ Ghost, and will regenerated as needed automatically whenever you change an image, a list of sizes, or the theme being used. Unlike other platforms, there's no manual work needed to manage image sizes, it's all done in the background for you.

Dynamic image sizes are _not_ compatible with externally hosted images. If you insert images from [Unsplash](https://ghost.org/integrations/unsplash/) or you store your image files on a [third party storage adapter](https://ghost.org/integrations/storage/) then the image url returned will be determined by the external source.
