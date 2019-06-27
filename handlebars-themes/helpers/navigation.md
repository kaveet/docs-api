---
title: "navigation"
date: "2018-10-01"
meta_title: "Ghost Handlebars Theme Helpers: navigation"
meta_description: "Find out how to output a navigation menu in a custom Ghost theme using the navigation helper. Read more about Ghost themes!"
keywords:
    - api
    - handlebars
    - themes
    - helpers
sidebar: "handlebars"
---

Usage: `{{navigation}}`

### Description

`{{navigation}}` is a  template-driven helper which outputs formatted HTML of menu items defined in the Ghost admin panel (Settings > Design > Navigation). By default, the navigation is marked up using a [preset template](https://github.com/TryGhost/Ghost/blob/master/core/server/helpers/tpl/navigation.hbs).

### Default template

By default, the HTML output by including `{{navigation}}` in your theme, looks like the following:

```html
<ul class="nav">
    <li class="nav-home nav-current"><a href="/">Home</a></li>
    <li class="nav-about"><a href="/about">About</a></li>
    <li class="nav-contact"><a href="/contact">Contact</a></li>
    ...
</ul>
```

### Changing The Template

If you want to modify the default markup of the navigation helper, this can be achieved by creating a new file at `./partials/navigation.hbs`. If this file exists, Ghost will load it instead of the default template. Example:

```handlebars
<div class="my-fancy-nav-wrapper">
    <ul class="nav">
        <!-- Loop through the navigation items -->
        {{#foreach navigation}}
        <li class="nav-{{slug}}{{#if current}} nav-current{{/if}}"><a href="{{url absolute="true"}}">{{label}}</a></li>
        {{/foreach}}
        <!-- End the loop -->
    </ul>
</div>
```

The up-to-date default template in Ghost is always available [here](https://github.com/TryGhost/Ghost/blob/master/core/server/helpers/tpl/navigation.hbs).

### List of Attributes

A navigation item has the following attributes which can be used inside your `./partials/navigation.hbs` template file...

* **{{label}}** - The text to display for the link
* **{{url}}** - The URL to link to - see the url helper for more options
* **{{current}}** - Boolean true / false - whether the URL matches the current page
* **{{slug}** - Slugified name of the page, eg `about-us`. Can be used as a class to target specific menu items with CSS or jQuery.

These attributes can only be used inside the `{{#foreach navigation}}` loop inside `./partials/navigation.hbs`. A navigation loop will not work in other partial templates or theme files.

### Examples

The navigation helper doesn't output anything if there are no navigation items to output, so there's no need to wrap it in an `{{#if}}` statement to prevent an empty list. However it's a common pattern, as in Casper, to want to output a link to open the main menu, but only if there are items to show.

The data used by the `{{navigation}}` helper is also stored as a global variable called `@blog.navigation`. You can use this global variable in any theme file to check if navigation items have been added by a user in the Ghost admin panel.

```handlebars
{{#if @blog.navigation}}
    <a class="menu-button" href="#"><span class="word">Menu</span></a>
{{/if}}
```
